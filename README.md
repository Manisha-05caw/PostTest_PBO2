Sistem dibagi menjadi tiga package utama:
•	Package penitipan_hewan.main
1.	Menyimpan kode utama (MainApp.java) yang berfungsi sebagai antarmuka menu untuk user.
2.	Menampilkan pilihan menu seperti tambah data, lihat data, ubah, hapus, cari, dan keluar.
<img width="670" height="417" alt="image" src="https://github.com/user-attachments/assets/8c70e062-6be9-457b-87b6-e6935d8b9e10" />

- package penitipan_hewan.main;
Menentukan namespace file. Struktur folder harus mengikuti paket ini: penitipan_hewan/main/MainApp.java.

public class MainApp {
Deklarasi kelas utama.

public static void main(String[] args) {
Titik masuk program Java. Saat program dijalankan, method ini dipanggil.

try (Scanner scanner = new Scanner(System.in)) {
Membuat satu Scanner yang membaca dari System.in. Dipakai try-with-resources supaya scanner otomatis ditutup ketika program selesai (good practice, mencegah kebocoran resource).

CrudService crud = new CrudService(scanner);
Membuat objek service. Scanner disuntikkan (dependency injection) agar semua submenu/operasi di CrudService memakai scanner yang sama, menghindari konflik kalau ada banyak Scanner untuk System.in.

boolean running = true;
Flag untuk mengendalikan loop menu.

while (running) { ... }
Perulangan yang menampilkan menu terus-menerus sampai running diubah menjadi false.

<img width="727" height="432" alt="image" src="https://github.com/user-attachments/assets/22828b74-b8f5-428e-b33e-5576e87713d9" />

Jika pengguna memilih "1", aplikasi menampilkan layar “CRUD HEWAN” dengan opsi Tambah/Lihat/Ubah/Hapus, lalu membaca pilihan kedua ke variabel c. Pilihan c diproses lewat switch bersarang: "1" memanggil crud.tambahHewan(), "2" memanggil crud.lihatHewan(), "3" memanggil crud.ubahHewan(), "4" memanggil crud.hapusHewan(), sedangkan input lain menampilkan pesan “Pilihan tidak tersedia.” Semua pekerjaan CRUD-nya didelegasikan ke objek CrudService bernama crud. Input dibaca dengan scanner.nextLine() agar aman terhadap sisa newline, dan setelah suatu aksi selesai, kontrol kembali ke loop menu utama.

•	Package penitipan_hewan.model
Berisi kelas (class) yang mendefinisikan struktur data.
Contoh class:
    => Hewan → atribut: nama, jenis, umur.

<img width="762" height="452" alt="image" src="https://github.com/user-attachments/assets/acf25ad8-ddad-46a1-a864-d4633ae39b47" />

"1" menjalankan crud.tambahHewan(), "2" menjalankan crud.lihatHewan(), "3" menjalankan crud.ubahHewan(), dan "4" menjalankan crud.hapusHewan(). Jika input tidak sesuai, cabang default menampilkan pesan “Pilihan tidak tersedia.”

   => Pemilik → atribut: nama, alamat, no telepon.
<img width="698" height="505" alt="image" src="https://github.com/user-attachments/assets/abdd7658-2046-40b0-a0e5-005d29d9048e" />

   Field private: nama, alamat, noHp. Dibuat private untuk enkapsulasi agar hanya bisa diakses lewat method.

Konstruktor Pemilik(String nama, String alamat, String noHp) menginisialisasi ketiga field (pakai this.field = arg).

Getter/Setter:
- getNama() / setNama(String)
- getAlamat() / setAlamat(String)
- getNoHp() / setNoHp(String)
  
Method ini jadi “pintu” baca/tulis nilai field dengan aman.
toString() di-override untuk menghasilkan teks rapi saat objek dicetak 

  => Petugas → atribut: nama, no telepon, shift.


<img width="724" height="482" alt="image" src="https://github.com/user-attachments/assets/fa382f30-794d-4206-9a6e-7becbcd226d9" />

- Field privat:
nama (String), jenisKelamin (String), umur (int). Disembunyikan (encapsulation) agar hanya bisa diakses lewat method.

- Konstruktor
public Petugas(String nama, String jenisKelamin, int umur) mengisi ketiga field dengan nilai dari argumen (this.field = arg).

- Getter/Setter
getNama()/setNama(), getJenisKelamin()/setJenisKelamin(), getUmur()/setUmur() untuk baca/tulis tiap field. Tipe int pada umur memastikan nilai numerik.

- toString() override
Mengembalikan string ringkas berisi seluruh atribut, sehingga ketika objek dicetak (System.out.println(petugas)

Transaksi → atribut: id transaksi, hewan, pemilik, petugas, tanggal masuk, tanggal keluar.


<img width="736" height="494" alt="image" src="https://github.com/user-attachments/assets/0c8ba129-dbcd-4490-80b9-8f75c5856804" />

•	Package penitipan_hewan.service

<img width="777" height="276" alt="image" src="https://github.com/user-attachments/assets/f6f2ea1f-10ba-4473-842b-a09c47e65f10" />

  => Menyimpan logika CRUD dan pencarian.
  => Class CrudService berisi method untuk:
- Tambah data (Create)
- Lihat daftar data (Read)
- Ubah data (Update)
- Hapus data (Delete)
- Cari data (Search)



<img width="734" height="544" alt="image" src="https://github.com/user-attachments/assets/9a94941a-3209-4afa-b0ac-c65a2e47abe5" />


<img width="758" height="419" alt="image" src="https://github.com/user-attachments/assets/83c23761-b947-4514-8d7e-6af0670fb561" />


<img width="796" height="685" alt="image" src="https://github.com/user-attachments/assets/593758f1-035e-468f-974a-a26cbb3568e5" />


<img width="703" height="391" alt="image" src="https://github.com/user-attachments/assets/54231dae-6946-4ed8-8082-ce36ec38833f" />


<img width="698" height="94" alt="image" src="https://github.com/user-attachments/assets/4e6fef85-440c-4bc5-b0ea-b04a2e625a7f" />

tambahHewan() meminta tiga input: nama, jenis, dan umur. Umur dibaca dengan readIntSafe() supaya kalau pengguna salah ketik (bukan angka) program tidak crash—fungsi itu akan mengulang sampai input valid. Setelah dapat nilainya, method membuat objek Hewan baru dan menambahkannya ke daftarHewan, lalu mencetak pesan sukses.

lihatHewan() menampilkan daftar hewan. Jika list kosong, ia menulis “Belum ada hewan” dan berhenti. Kalau tidak, ia melakukan loop dari indeks 0 hingga daftarHewan.size()-1 dan mencetak nomor urut (i+1) beserta isi objek (mengandalkan toString() di kelas Hewan).

ubahPemilik() memulai dengan memanggil lihatPemilik() agar pengguna melihat nomor urut data. Bila list kosong ia langsung return. Pengguna lalu mengisi nomor yang ingin diubah; nomor dibaca dengan readIntSafe() dan dicek memakai validIndex(idx, ukuran), yang memastikan nomor 1…size. Jika valid, diambil objek target p = daftarPemilik.get(idx-1) (karena list 0-based, tampilan 1-based). Selanjutnya ada tiga prompt: nama/alamat/noHP baru. Tiap prompt menerima string; jika pengguna menekan Enter (kosong), field tidak diubah; kalau ada isi, setter dipanggil untuk memperbarui. Di akhir, program mengonfirmasi bahwa data berhasil diubah.

cariPetugas() membaca kata kunci, menurunkannya ke huruf kecil, lalu menelusuri daftarPetugas. Untuk setiap Petugas p, ia cek apakah p.getNama() atau p.getJenisKelamin() (keduanya diturunkan hurufnya) mengandung kata kunci. Jika cocok, objek dicetak dan flag ditemukan diubah menjadi true. Setelah loop, jika tidak ada yang cocok, muncul “Petugas tidak ditemukan.”

cariTransaksi() bekerja mirip, tetapi objek yang dicari Transaksi. Kata kunci dibandingkan ke tiga sisi: nama Pemilik, nama Hewan, atau nama Petugas di dalam transaksi. Jika salah satu mengandung kata kunci, transaksi dicetak; kalau tidak ada hasil, dicetak “Transaksi tidak ditemukan.”

readIntSafe() dan readDoubleSafe() adalah helper untuk validasi input numerik. Keduanya menjalankan while(true): baca satu baris, coba Integer.parseInt(...) atau Double.parseDouble(...) atas line.trim(). Jika parsing gagal (lempar Exception), tampilkan pesan “Input harus angka…” dan ulangi meminta input hingga valid. Ini mencegah NumberFormatException menghentikan program.

validIndex(int idx, int size) mengembalikan true jika idx berada pada rentang 1…size.


# Alur Kerja Sistem
- Program dijalankan → user melihat menu utama.
- User memilih menu sesuai kebutuhan: tambah, lihat, ubah, hapus, cari, atau keluar.
- Jika tambah data → sistem meminta input atribut sesuai class terkait.
- Data disimpan dalam struktur ArrayList.
- User dapat melakukan perubahan data melalui menu ubah atau menghapus data tertentu.
- User dapat mencari data menggunakan menu pencarian.
- Saat user memilih keluar, program akan berhenti.


