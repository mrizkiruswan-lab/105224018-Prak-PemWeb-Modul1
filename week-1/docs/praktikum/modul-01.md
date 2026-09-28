Dokumen Teknis Modul 1 --- Lingkungan Pengembangan, Git, dan Lalu Lintas HTTP

Nama/NIM: M. Rizki Ruswan / 105224018
Repositori: https://github.com/mrizkiruswan-lab/105224018-Prak-PemWeb-Modul1.git

Dokumen ini disusun berdasarkan hasil praktikum dan bukti pengujian
yang tersedia. Bagian yang belum memiliki bukti pada tangkapan layar
diberi keterangan agar tidak mengada-ada hasil praktikum.

1. Lingkungan Pengembangan

Lingkungan pengembangan yang digunakan adalah Windows 11, Visual Studio
Code, Node.js, npm, dan Git. Modul menetapkan Node.js 24 LTS sebagai
versi acuan serta menggunakan Git dan Visual Studio Code untuk
pengembangan aplikasi web. 

Komponen                Hasil yang digunakan    Bukti

Sistem Operasi          Windows 11 Home Single  assets/04-windows-version.png
Language, Version 25H2
(OS Build 26200.9550)

Visual Studio Code      Version 1.139.1 (User   assets/06-vscode-version.png
Setup), Electron
38.6.0, Node.js 24.20.0
pada runtime VS Code

Node.js                 v26.7.0                 assets/10-node-npm-git-version.png

npm                     11.6.0                  assets/10-node-npm-git-version.png

Git                     2.55.0.windows.3        assets/10-node-npm-git-version.png

Catatan: Modul menggunakan Node.js 24 LTS sebagai versi acuan,
sedangkan hasil terminal yang saya gunakan menunjukkan Node.js
v26.7.0. Karena itu, hasil aktual dicatat apa adanya dan tidak
disamakan dengan versi acuan modul. Modul juga menjelaskan bahwa
pemeriksaan akhir dilakukan dengan node -v, npm -v, dan
git --version.
Menjalankan aplikasi

Aplikasi Next.js dijalankan pada server pengembangan lokal melalui
npm run dev dan diakses menggunakan http://localhost:3000. Modul
memang meminta aplikasi dijalankan pada alamat tersebut sebelum
pengamatan HTTP dilakukan. 

Bukti dari curl.exe -v http://localhost:3000 menunjukkan koneksi
berhasil ke localhost:3000 dan server memberikan respons
HTTP/1.1 200 OK.



Bukti lain pada DevTools menunjukkan request dokumen utama ke
http://localhost:3000/ memperoleh status 200 OK dengan
Content-Type: text/html; charset=utf-8 dan X-Powered-By: Next.js.



2. Alur Kerja Git

Modul menjelaskan bahwa Git menyimpan perubahan sebagai commit,
sedangkan branch digunakan untuk mengerjakan perubahan secara terpisah
sebelum digabungkan melalui merge. 

Riwayat commit

Hasil git log --oneline --graph yang tersedia menunjukkan:

* 1d82d75 (HEAD -> DoKtek, origin/main, origin/HEAD, main) Week 1



Dari bukti tersebut, branch lokal main dan branch remote origin/main
berada pada commit yang sama, yaitu 1d82d75 dengan pesan Week 1.

Catatan: Bukti yang tersedia saat penyusunan dokumen ini baru
menunjukkan satu commit. Modul menetapkan luaran repositori minimal tiga
commit bermakna dan satu pull request yang telah digabungkan.
fileciteturn0file0L32-L35 Bukti pull request merged dan tiga commit
belum disertakan pada kumpulan tangkapan layar yang diberikan, sehingga
bagian tersebut tidak saya nyatakan sudah selesai.

Branch, merge, dan konflik

Modul meminta latihan membuat branch latihan/konflik, melakukan
perubahan yang berbeda pada baris yang sama, lalu melakukan merge
sehingga terjadi konflik. Konflik diselesaikan dengan memilih isi akhir,
menghapus penanda konflik, kemudian melakukan commit merge.

Bukti tangkapan layar yang diberikan belum memperlihatkan proses
git switch, git merge, pesan CONFLICT, atau hasil commit merge.
Oleh karena itu, hasil konflik dan alasan pemilihan isi akhir belum
dapat dicatat sebagai hasil yang sudah diverifikasi, tetapi bisa di lihat di repo saya nantinya.

Repositori GitHub

Repositori GitHub digunakan sebagai tempat penyimpanan remote untuk proyek dan riwayat perubahan.

Tautan repositori: https://github.com/mrizkiruswan-lab/105224018-Prak-PemWeb-Modul1.git

Catatan: pada dokumentasi ini, bukti GitHub dicantumkan berupa tautan repositori sesuai dokumentasi yang dikumpulkan.

3. Pengamatan Lalu Lintas HTTP

Modul meminta pengamatan dilakukan melalui panel Network pada
DevTools dan terminal menggunakan curl. Komponen yang diamati meliputi
URL, metode, kode status, Content-Type, dan header lainnya.

3.1 Lembar kerja pengamatan

No         URL                                                  Metode     Kode Status Content-Type                 Header lain yang diamati                      Bukti

1          http://localhost:3000/                             GET        200 OK      text/html; charset=utf-8   Cache-Control: no-cache, must-revalidate,   assets/08-devtools-document-200.png
Connection: keep-alive,
X-Powered-By: Next.js,
Transfer-Encoding: chunked

2          http://localhost:3000/_next/hmr?...                GET        101         ---                          Connection: Upgrade, Upgrade: websocket,  assets/02-devtools-websocket-101.png
Switching                                Sec-WebSocket-Version: 13
Protocols

3          http://localhost:3000/_next/static/chunks/...css   GET        304 Not     ---                          Cache-Control: no-cache, must-revalidate,   assets/05-devtools-css-304.png
Modified                                 ETag, Last-Modified

4          http://localhost:3000/ melalui curl.exe -I       HEAD       200 OK      text/html; charset=utf-8   Cache-Control: no-cache, must-revalidate,   assets/07-curl-head-200.png
X-Powered-By: Next.js,
Keep-Alive: timeout=5

3.2 Analisis kode status

200 OK

Request dokumen utama ke http://localhost:3000/ menghasilkan 200
OK. Kode 200 termasuk kelas 2xx yang menunjukkan bahwa permintaan
berhasil. Modul juga mencantumkan 200 OK sebagai contoh kode status
kelas 2xx. 



304 Not Modified

Pada salah satu berkas CSS dari localhost terlihat status 304 Not
Modified. Respons tersebut disertai ETag dan Last-Modified.



Status 304 termasuk kelas 3xx dan digunakan saat browser melakukan
validasi ulang salinan cache. Modul menjelaskan bahwa sumber daya yang
menggunakan cache dapat muncul sebagai 304 apabila browser memvalidasi
ulang salinan cache ke server. fileciteturn0file0L451-L460

101 Switching Protocols

Request /_next/hmr?... terlihat menggunakan status 101 Switching
Protocols dengan header Connection: Upgrade dan
Upgrade: websocket.



Status 101 termasuk kelas 1xx. Pada pengamatan ini, request tersebut
digunakan untuk koneksi WebSocket yang mendukung komunikasi Hot Module
Reloading pada lingkungan pengembangan.

3.3 Analisis curl -I

Perintah yang digunakan:

curl.exe -I http://localhost:3000

Hasil menunjukkan:

HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
X-Powered-By: Next.js



Opsi -I pada curl digunakan untuk hanya menampilkan header respons.
Modul menjelaskan bahwa -I mengirim permintaan dengan metode HEAD,
sehingga metode yang dicatat pada pengamatan curl -I adalah HEAD,
bukan GET. 

3.4 Analisis curl -v

Perintah yang digunakan:

curl.exe -v http://localhost:3000

Pada keluaran terlihat bagian request yang diawali tanda >:

> GET / HTTP/1.1
> Host: localhost:3000
> User-Agent: curl/8.21.0
> Accept: */*

Kemudian server memberikan respons yang diawali tanda <:

< HTTP/1.1 200 OK



Opsi -v menampilkan detail komunikasi request dan response. Modul
menjelaskan bahwa baris request ditandai > sedangkan baris respons
ditandai <. fileciteturn0file0L461-L471

3.5 Perbandingan cache

Bukti yang tersedia menunjukkan request CSS dengan status 304 Not
Modified, sedangkan dokumen utama memperoleh 200 OK. Pada request
CSS terdapat ETag dan Last-Modified, yang merupakan informasi yang
dapat digunakan browser untuk validasi cache.



Modul meminta perbandingan pemuatan dengan dan tanpa cache. Saat cache
digunakan, sumber daya dapat muncul sebagai (memory cache),
(disk cache), atau 304 setelah validasi ulang.


Catatan: tangkapan layar yang diberikan belum menunjukkan pasangan
hasil yang sama-sama didokumentasikan secara eksplisit untuk kondisi
Disable cache aktif dan Disable cache nonaktif. Karena itu,
perbandingan ukuran lengkap belum dapat disimpulkan dari bukti yang
tersedia.

4. Kendala dan Penyelesaian

4.1 Perbedaan versi Node.js

Modul menggunakan Node.js 24 LTS sebagai versi acuan, sedangkan hasil
terminal menunjukkan:

node -v
v26.7.0

Perintah yang digunakan:

node -v
npm -v
git --version

Hasil:

v26.7.0
11.6.0
git version 2.55.0.windows.3



Perbedaan ini dicatat apa adanya agar dokumentasi sesuai dengan
lingkungan yang benar-benar digunakan.

4.2 Penggunaan curl.exe pada Windows PowerShell

Pada Windows PowerShell digunakan curl.exe, bukan hanya curl. Hal
ini sesuai dengan modul karena PowerShell dapat menggunakan curl
sebagai alias Invoke-WebRequest, sehingga keluaran dapat berbeda dari
program curl asli. 

Perintah yang digunakan:

curl.exe -I http://localhost:3000
curl.exe -v http://localhost:3000

4.3 DevTools menampilkan WebSocket

Pada Network terdapat request /_next/hmr?... dengan status 101
Switching Protocols. Request ini bukan halaman HTML biasa, tetapi
koneksi WebSocket yang digunakan oleh lingkungan pengembangan Next.js.

5. Catatan Pemanfaatan AI

AI digunakan sebagai alat bantu penyusunan dan pengecekan Dokumen Teknis
Modul 1.

Alat: ChatGPT.

Bagian yang digunakan: - Membantu menyusun struktur dokumen Markdown
sesuai kerangka Bagian H pada Modul 1. - Membantu menjelaskan hasil
pengamatan HTTP dari bukti curl dan DevTools. - Membantu merapikan
tabel dan penjelasan teknis berdasarkan hasil praktikum yang tersedia.

Cara verifikasi: - Versi Node.js, npm, dan Git diverifikasi langsung
melalui terminal. - Status HTTP, URL, metode, dan header diverifikasi
melalui DevTools. - Hasil curl diverifikasi dari keluaran terminal. -
Riwayat Git diverifikasi melalui git log --oneline --graph. -
Informasi mengenai format Dokumen Teknis dan komponen yang harus dicatat
dibandingkan dengan Modul 1.

Saya tetap memeriksa kembali hasil yang ditulis terhadap bukti praktikum
dan tidak menuliskan hasil yang belum terlihat pada bukti yang tersedia.

Kesimpulan

Praktikum Modul 1 berhasil menunjukkan lingkungan pengembangan web yang
digunakan, menjalankan aplikasi Next.js pada localhost:3000, memeriksa
versi Node.js/npm/Git, melihat riwayat Git, serta mengamati lalu lintas
HTTP melalui DevTools dan curl.exe.

Dari pengamatan HTTP ditemukan beberapa jenis respons, yaitu 200 OK,
304 Not Modified, dan 101 Switching Protocols. Pengamatan juga
menunjukkan perbedaan antara metode GET pada curl.exe -v dan
HEAD pada curl.exe -I.

Untuk melengkapi dokumen sesuai seluruh ketentuan Modul 1, bukti yang
masih perlu ditambahkan adalah minimal tiga commit bermakna, pull
request berstatus Merged, bukti proses konflik/merge, serta perbandingan
cache dengan kondisi Disable cache aktif dan nonaktif.