# CBT Informatika Kelas 9 — Microsoft Word & Excel

Aplikasi ujian berbasis komputer (CBT) statis: 30 PG, 5 PG kompleks, 5 menjodohkan (4 HOTS = 10%, 36 soal lain level C2–C4; 20 soal dari modul PDF dan 20 soal sesuai CP), skor total 100. Setengah soal bersumber dari modul "Latihan Excel dan Laporan Word Kelas 9", setengahnya dari Capaian Pembelajaran.

## Isi repositori
- `index.html` — aplikasi lengkap (satu file; pengaturan & soal ada di bagian paling atas blok `<script>` pertama)
- `README.md` — panduan ini

## Publikasi lewat GitHub Pages
1. Buat repositori baru di GitHub, unggah ketiga file di atas.
2. **Settings → Pages → Build and deployment → Source: Deploy from a branch**, pilih `main` / `(root)` → Save.
3. Tunggu ±1 menit; alamat siswa: `https://USERNAME.github.io/NAMA-REPO/`

## Sebelum dipakai
Ubah di bagian atas `index.html`: `sekolah`, `guru`, **`passwordGuru`**, `garam`, dan (opsional) `kodeUjian`.

## Fitur anti-curang
- 3 kesempatan: pindah tab/aplikasi, keluar layar penuh, memuat ulang halaman, atau membuka alat pengembang = 1 pelanggaran. Pelanggaran ke-3 → ujian berhenti & jawaban dinilai.
- Urutan soal, urutan opsi, dan urutan pasangan menjodohkan diacak berbeda untuk tiap siswa.
- Salin/tempel/klik kanan/cetak dinonaktifkan; satu siswa hanya satu kali mengerjakan.

**Batas teknis (jujur):** web statis tidak bisa mencegah siswa memakai HP/perangkat kedua, dan kunci jawaban terbaca bagi yang membuka source file. Pengawasan guru di kelas tetap diperlukan. Panel guru memakai kata sandi sisi-klien (pengaman ringan).

## Rekap nilai — pilih cara pengumpulan
Nilai disimpan di **browser perangkat tempat siswa mengerjakan**. Panel Guru (tautan kecil di bawah halaman awal) menampilkan tabel nilai, tombol **Unduh Excel (.xlsx)**, **Salin Tabel** (tempel langsung ke Excel/Word/Sheets), Cetak, dan Hapus (agar siswa bisa mengulang).

1. **Lab komputer (paling mudah):** semua siswa memakai komputer yang sama → buka Panel Guru di komputer tersebut.
2. **Perangkat berbeda, tanpa server:** setelah selesai siswa melihat *Kode hasil*; kirim ke guru (WA/Classroom), lalu guru tempel di Panel Guru → **Impor Kode Hasil**.
3. **Otomatis ke Google Sheets (disarankan):**
   1. Buat Google Sheet baru → **Extensions → Apps Script**, tempel:
   ```js
   function doPost(e){
     var d=JSON.parse(e.postData.contents);
     var lock=LockService.getScriptLock(); lock.waitLock(20000);
     var ss=SpreadsheetApp.getActiveSpreadsheet();
     var sh=ss.getSheetByName('Rekap Nilai')||ss.insertSheet('Rekap Nilai');
     if(sh.getLastRow()===0) sh.appendRow(d.header);
     sh.appendRow(d.baris);
     lock.releaseLock();
     return ContentService.createTextOutput('OK');
   }
   ```
   2. **Deploy → New deployment → Web app**; *Execute as:* Me, *Who has access:* Anyone → Deploy, salin URL `/exec`.
   3. Tempel URL ke `sheetsUrl` di `CONFIG` pada `index.html`. Nilai masuk ke Sheet begitu siswa selesai (dikirim ulang otomatis bila internet putus).

## Mengubah soal
Edit bagian `SOAL_PG`, `SOAL_KOMPLEKS`, `SOAL_JODOH` di `index.html`. PG: `a` = nomor opsi benar (mulai 0). Kompleks: `a` = daftar nomor benar. Menjodohkan: pasangan pada urutan yang sama adalah pasangan benar. Tambah `hots: true` + `stim` untuk studi kasus.
