# TUGAS ARTIKEL PEMROGRAMAN WEB
|Nama|NIM|Kelas|Matkul|
|-----|-----|-----|-----|
|KEMASRAFIRAMADHAN|312310346|TI 23A.4|Pemog Web|

## ESKOERIMEN
Eksperimen: Perbandingan Kinerja Perhitungan Fibonacci
Untuk memahami perbedaan kinerja antara JavaScript dan WebAssembly, saya melakukan eksperimen sederhana: membandingkan waktu eksekusi fungsi Fibonacci rekursif pada kedua platform. Fungsi Fibonacci rekursif dipilih karena merupakan contoh klasik dari beban komputasi yang berat dan eksponensial.
Implementasi JavaScript
Pertama, saya mengimplementasikan fungsi Fibonacci rekursif dalam JavaScript:

## CODINGAN
javascript
function fibonacciJS(n) {
  if (n <= 1) return n;
  return fibonacciJS(n - 1) + fibonacciJS(n - 2);
}
Implementasi WebAssembly dengan Rust
Untuk WebAssembly, saya menggunakan Rust sebagai bahasa sumber karena dukungan toolingnya yang baik:
rust
// fibonacci.rs
#[no_mangle]
pub fn fibonacci(n: i32) -> i32 {
  if n <= 1 {
    return n;
  }
  fibonacci(n - 1) + fibonacci(n - 2)
}
Kemudian saya mengkompilasi kode Rust menjadi WebAssembly menggunakan perintah:
bash
rustc --target wasm32-unknown-unknown -O fibonacci.rs -o fibonacci.wasm
Integrasi WebAssembly dengan JavaScript
Untuk menggunakan modul WebAssembly dalam aplikasi web, saya membuat fungsi loader:
javascript
async function loadWasm() {
  const response = await fetch('fibonacci.wasm');
  const buffer = await response.arrayBuffer();
  const wasmModule = await WebAssembly.instantiate(buffer);
  return wasmModule.instance.exports;
}

// Penggunaan
loadWasm().then(wasm => {
  console.time('Wasm Fibonacci');
  const result = wasm.fibonacci(40);
  console.timeEnd('Wasm Fibonacci');
  console.log('Result:', result);
});

## HASIL 
![img 1](screenshot/hasilnya.png)

## KESIMPULAN
Ini menunjukkan peningkatan kinerja sekitar 7x dengan WebAssembly dibandingkan JavaScript murni!
Analisis dan Observasi
Dari eksperimen ini, saya mengobservasi beberapa hal menarik:
1.	Perbedaan Kinerja Signifikan: WebAssembly menunjukkan keunggulan kinerja yang signifikan untuk operasi komputasi intensif seperti fungsi Fibonacci rekursif.
2.	Overhead Startup: Saat menjalankan fungsi sederhana atau dengan input kecil, JavaScript terkadang lebih cepat karena WebAssembly memiliki overhead kompilasi awal.
3.	Kompleksitas Development: Mengembangkan dengan WebAssembly membutuhkan pengetahuan tambahan dan toolchain yang lebih kompleks, namun hasilnya sepadan untuk aplikasi yang membutuhkan kinerja tinggi.
4.	Keterbatasan Akses DOM: WebAssembly tidak dapat mengakses DOM secara langsung dan masih membutuhkan JavaScript sebagai perantara, yang perlu dipertimbangkan dalam desain aplikasi.
Kasus Penggunaan Ideal
Berdasarkan eksperimen, WebAssembly sangat cocok untuk:
•	Pemrosesan gambar dan video
•	Game browser dengan grafis intensif
•	Aplikasi komputasi ilmiah
•	Encoding/decoding dan enkripsi
•	Emulasi dan virtualisasi

