# 📦 CLI Binary Lite Distribution & Compression Architecture

> **Studi Kasus:** Arsitektur distribusi biner `x2mail` oleh **@__r17x** (`x2mail lite distribution v1.0.3`).  
> **Hasil:** Memangkas ukuran total artefak rilis dari **317 MB $\rightarrow$ 116 MB (hemat 63% / -201 MB)** untuk 4 target platform (*darwin-arm64, darwin-x64, linux-arm64, linux-x64*).

---

## 🎯 1. Masalah: Fat Binaries pada Modern Single-Executable Runtime

Saat kita meng-compile aplikasi JavaScript/TypeScript menjadi biner tunggal mandiri (menggunakan `bun build --compile --minify` atau Node.js Single Executable Application / SEA):
* Runtime (engine JavaScript, garbage collector, standard library) di-*bundle* langsung ke dalam executable.
* Ukuran tiap biner melonjak drastis:
  * macOS: **~60 MB – 65 MB**
  * Linux: **~96 MB**
* Jika mendistribusikan ke 4 platform utama, total unduhan mencapai **>300 MB**. Ini memberatkan pengguna, memperlambat proses instalasi CLI, dan boros bandwidth CDN/CI/CD.

---

## ⚙️ 2. Arsitektur Kompresi Ganda (Dual-Method Compression)

Tidak ada satu solusi kompresi yang dapat dipakai di semua sistem operasi karena perbedaan kernel dan kebijakan keamanan. Oleh karena itu, digunakan **pendekatan bercabang dua**:

```
                              [ Kompilasi Raw Binary ]
                     (bun build --compile --minify - 4 targets)
                                         │
                    ┌────────────────────┴────────────────────┐
                    ▼                                         ▼
            [ Target: Linux ]                         [ Target: macOS ]
        (linux-arm64, linux-x64)                  (darwin-arm64, darwin-x64)
                    │                                         │
                    ▼                                         ▼
           Metode 1: UPX Packing                    Metode 2: SFX Gzip Wrapper
           (Direct in-place binary)                  (.github/sfx-wrap.sh)
                    │                                         │
                    ▼                                         ▼
           x2mail-linux-*-lite                       x2mail-darwin-*-lite
           (96 MB -> 34-36 MB)                       (60-65 MB -> 22-24 MB)
```

### Tabel Performa Kompresi (Benchmark Nyata)

| Target OS / Arch | Raw Size | Lite Size | Ratio | Metode Kompresi | Keterangan |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `darwin-arm64` (Apple Silicon) | 60 MB | **22 MB** | 37% | SFX gzip | Self-extracting stub ke `~/.cache/` |
| `darwin-x64` (Intel Mac) | 65 MB | **24 MB** | 37% | SFX gzip | Self-extracting stub ke `~/.cache/` |
| `linux-arm64` (ARM64 Server) | 96 MB | **34 MB** | 35% | UPX | Direct executable compression |
| `linux-x64` (Standard x86 Linux) | 96 MB | **36 MB** | 37% | UPX | `patchelf` + direct UPX |
| **TOTAL (4 Binaries)** | **317 MB** | **116 MB** | **37%** | — | **-201 MB download saved!** |

---

## 🍎 3. Kenapa macOS Tidak Bisa Pakai UPX? (AppleSystemPolicy Trap)

### Akar Masalah:
* Sejak **macOS Monterey**, sistem keamanan kernel Apple memperkenalkan pemeriksaan ketat pada binary Mach-O via `AppleSystemPolicy`.
* UPX bekerja dengan menyematkan *decompression stub* di header biner yang melakukan *raw syscall* langsung ke kernel saat dieksekusi.
* Pada macOS modern, binary yang melakukan *raw syscall* tanpa melewati `libSystem.dylib` (atau yang dicurigai memodifikasi halaman memori executable) akan langsung dibunuh oleh kernel dengan sinyal `SIGKILL` / `Killed: 9`.
* Akibatnya: **Biner hasil UPX di macOS 100% crash/rusak saat dijalankan pengguna.**

### Solusi Elegan: SFX Gzip Wrapper (`sfx-wrap.sh`)
Alih-alih memodifikasi struktur internal Mach-O, bungkus biner dengan skrip *Self-Extracting Executable (SFX)*:
1. Biner asli di-compress menggunakan `gzip -9`.
2. Biner terkompresi digabungkan dengan skrip wrapper bash/POSIX portabel (menggunakan *heredoc template*).
3. **Mekanisme Eksekusi:**
   * **First Run (Jalankan Pertama Kali):** Wrapper mengekstrak biner asli ke direktori *cache* pengguna: `~/.cache/x2mail/<version>/x2mail`.
   * **Subsequent Runs (Jalankan Berikutnya):** Wrapper mendeteksi bahwa biner versi tersebut sudah ada di cache, lalu langsung melakukan `exec` terhadap file biner di cache.
   * **Hasil:** Biner yang dieksekusi adalah biner Mach-O asli yang 100% identik (*byte-for-byte identical, SHA-256 verified*), bebas dari intervensi `AppleSystemPolicy`, dan *overhead* dekompresi hanya terjadi satu kali saat run pertama.

---

## 🐧 4. Metode Linux: `patchelf` + UPX

Pada platform Linux:
1. `patchelf linux-x64`: Menyesuaikan *interpreter* dan *RPATH/RUNPATH* jika diperlukan untuk kompatibilitas glibc.
2. `upx linux-arm64` & `upx linux-x64`:
   * Mengompresi binary secara langsung (*in-place*).
   * Waktu kompresi sangat cepat (~10 detik di CI).
   * Berjalan *natively* tanpa membutuhkan folder cache.

---

## 🔄 5. Otomasi CI/CD & Semantic Release Pipeline

Integrasikan pembuatan artefak raw dan lite ke dalam alur rilis otomatis:

```
semantic-release
├── 1. commit-analyzer (conventionalcommits)
├── 2. release-notes-generator
├── 3. changelog
├── 4. npm.prepare (bump version)
├── 5. exec.prepare
│   ├── bun run build
│   │   ├── tsc (lib -> dist/)
│   │   └── bun build --compile --minify (4 targets: raw binaries)
│   ├── patchelf linux-x64
│   ├── upx linux-arm64  -> release/x2mail-linux-arm64-lite [NEW]
│   ├── upx linux-x64    -> release/x2mail-linux-x64-lite   [NEW]
│   └── .github/sfx-wrap.sh ${nextRelease.version}          [NEW]
│       ├── heredoc stub + sed VERSION
│       ├── gzip -9 darwin-arm64 -> release/x2mail-darwin-arm64-lite
│       └── gzip -9 darwin-x64   -> release/x2mail-darwin-x64-lite
├── 6. git (commit package.json & CHANGELOG.md)
└── 7. github release (publish 8 assets: 4 raw + 4 lite)
```

### File Konfigurasi Terkait:
1. **`flake.nix`**: Tambahkan `pkgs.upx` dan `pkgs.patchelf` ke dalam CI `devShell` agar toolchain selalu tersedia secara deterministik.
2. **`.releaserc.json`**: Tambahkan script eksekusi di `prepareCmd` dan daftarkan 8 aset rilis (4 biner mentah + 4 varian `-lite`).

---

## 💡 6. Template Skrip SFX Wrapper (`.github/sfx-wrap.sh`)

Contoh struktur sederhana untuk mengimplementasikan SFX wrapper:

```bash
#!/usr/bin/env bash
set -euo pipefail

VERSION="${1:-dev}"
APP_NAME="myapp"
TARGET_DIR="release"

wrap_darwin() {
  local arch="$1"
  local raw_bin="${TARGET_DIR}/${APP_NAME}-darwin-${arch}"
  local lite_bin="${TARGET_DIR}/${APP_NAME}-darwin-${arch}-lite"
  local gz_payload="/tmp/${APP_NAME}-${arch}.gz"

  echo "Compressing ${raw_bin} with gzip -9..."
  gzip -9 -c "${raw_bin}" > "${gz_payload}"

  echo "Creating SFX stub for darwin-${arch}..."
  cat << 'EOF' > "${lite_bin}"
#!/usr/bin/env bash
set -euo pipefail

APP="APP_NAME_PLACEHOLDER"
VERSION="VERSION_PLACEHOLDER"
CACHE_DIR="${XDG_CACHE_HOME:-$HOME/.cache}/${APP}/${VERSION}"
BIN_PATH="${CACHE_DIR}/${APP}"

if [ ! -f "${BIN_PATH}" ]; then
  mkdir -p "${CACHE_DIR}"
  PAYLOAD_LINE=$(awk '/^__PAYLOAD_BELOW__/ {print NR + 1; exit 0; }' "$0")
  tail -n +"${PAYLOAD_LINE}" "$0" | gzip -d > "${BIN_PATH}"
  chmod +x "${BIN_PATH}"
fi

exec "${BIN_PATH}" "$@"
__PAYLOAD_BELOW__
EOF

  sed -i.bak "s/APP_NAME_PLACEHOLDER/${APP_NAME}/g" "${lite_bin}"
  sed -i.bak "s/VERSION_PLACEHOLDER/${VERSION}/g" "${lite_bin}"
  rm -f "${lite_bin}.bak"

  # Lampirkan binary gzip di akhir skrip
  cat "${gz_payload}" >> "${lite_bin}"
  chmod +x "${lite_bin}"
  rm -f "${gz_payload}"
  echo "Created: ${lite_bin}"
}

wrap_darwin "arm64"
wrap_darwin "x64"
```

---

## 📌 Kesimpulan & Best Practice
1. **Untuk Linux:** Gunakan `upx` (opsional `patchelf` jika dynamic linking). Cepat, efisien, dan tanpa dependensi runtime tambahan.
2. **Untuk macOS:** Hindari `upx` pada platform macOS modern (Monterey ke atas). Gunakan pola **SFX Gzip Wrapper** yang dekompresi ke `~/.cache/<app>/<version>` pada pemanggilan pertama.
3. **Distribusi Ganda:** Terbitkan versi *raw* (untuk debugging/environment strict) dan versi *-lite* (untuk download cepat pengguna harian).
