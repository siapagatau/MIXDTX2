# Porting DTX -> Mixer (v2.1)

Prinsip: pakai kode DTX apa adanya, hanya ganti isi "pad sampler" dengan engine mixer.

| File DTX | Di mixer | Catatan |
|---|---|---|
| ThemeState.kt | ThemeState.kt | identik (prefs `mixer_theme`) |
| ColorPickerDialog.kt | ColorPickerDialog.kt | identik: warna, background gambar, transparansi, visual sentuhan (ukuran, kedip) |
| BackgroundState.kt | BackgroundState.kt | identik (prefs `mixer_background`) |
| TouchFx.kt | TouchFx.kt | identik; overlay + `dispatchTouchEvent` di MainActivity (global, juga di atas skin) |
| MainActivity: pemilih gambar, muat disk | MainActivity.kt | sama dengan DTX |
| skin/ (Skin Engine) | skin/ | sudah diport sebelumnya + tipe baru toggle/slider/meter |
| config/skin-repo.json + SkinRepository | sama | skin hanya dari repo; cache offline setelah diunduh |

Beda yang disengaja:
- Transparansi Pad -> **Transparansi Panel** (kartu UI klasik), karena mixer tidak punya pad.
- Bila user memilih background gambar saat skin aktif, `layer-bg` skin dibuang saat render sehingga gambar tampil di belakang
  kontrol (cache render dibedakan lewat flag `transparentBg`). Tanpa background gambar, skin tampil utuh seperti biasa.
- Tidak ada skin bawaan. Pemakaian pertama: UI klasik tampil, skin pertama repo diunduh otomatis lalu dipakai
  (offline -> dicoba lagi di peluncuran berikutnya). Pilihan lama `default` otomatis dibersihkan.
- URL repo mencoba `main`, lalu `master` bila gagal.
