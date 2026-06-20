# MiniGPT 
Proyek ini adalah peningkatan dari model **TinyGPT** dengan tambahan parameter yang dilatih khusus menggunakan korpus teks berbahasa Indonesia mengenai Teori Relativitas dari Wikipedia. Tujuan utama proyek ini adalah untuk mempelajari bagaimana model transformer berskala kecil dapat mengenal dan menghasilkan teks dalam topik spesifik meskipun hanya menggunakan data yang terbatas.

## Struktur Proyek

1.  **Library Utama**: Menggunakan library Python yang diperlukan seperti `torch` dan `sentencepiece`.

2.  **Scraping Data**: 
    *   Data dikumpulkan dari beberapa halaman Wikipedia berbahasa Indonesia yang berkaitan dengan 'Teori Relativitas', 'Relativitas Khusus', 'Relativitas Umum', dan konsep-konsep lain yang berhubungan.
    *   Library `requests` dan `BeautifulSoup` digunakan untuk mengambil dan mem-parsing konten halaman Wikipedia.
    *   Konten teks dibersihkan dari elemen-elemen non-teks dan referensi sitasi.
    *   Korpus teks yang terkumpul disimpan dalam file `corpus.txt`.

3.  **Blok Transformer**: 
    *   Mendefinisikan komponen inti dari arsitektur transformer:
        *   **SelfAttentionHead**: Mekanisme perhatian yang memungkinkan model untuk menimbang pentingnya kata-kata yang berbeda dalam urutan input.
        *   **MultiHeadAttention**: Menggabungkan beberapa kepala perhatian untuk menangkap berbagai aspek informasi.
        *   **FeedForward**: Jaringan saraf feed-forward sederhana yang diterapkan pada setiap posisi dalam urutan input.
        *   **Block**: Menggabungkan mekanisme perhatian dan feed-forward, dilengkapi dengan normalisasi lapisan, membentuk satu blok transformer.

4.  **Setup Device & Hyperparameter**: 
    *   Menentukan perangkat komputasi (`cuda` jika tersedia, `cpu` jika tidak).
    *   Mengatur berbagai hyperparameter penting untuk pelatihan model, seperti `block_size`, `embedding_dim`, `n_heads`, `n_layers`, `lr`, `epochs`, `batch_size`, dan `vocab_size`.

5.  **Definisi Model MiniGPT**: 
    *   Membangun kelas `MiniGPT` yang mengintegrasikan blok-blok transformer.
    *   Model ini menggunakan embedding token dan embedding posisi.
    *   Fungsi `forward` menghitung logits dan loss.
    *   Fungsi `generate` memungkinkan model untuk menghasilkan teks baru berdasarkan prompt awal, dengan dukungan untuk `temperature` dan `top_k` sampling.
    *   Model diinisialisasi dan optimizer `AdamW` dikonfigurasi.

6.  **Pelatihan MiniGPT**: 
    *   Melakukan pelatihan model menggunakan dua jenis tokenizer: **BPE (Byte Pair Encoding)** dan **Unigram**.
    *   Setiap tokenizer dilatih pada `corpus.txt` dan disimpan bersama model MiniGPT yang dilatihnya.
    *   Proses pelatihan mencakup iterasi batch dan perhitungan loss.
    *   Model dan tokenizer disimpan di folder `drive/MyDrive/results`.

7.  **Eksekusi Eksperimen**: 
    *   Memuat model dan tokenizer yang telah dilatih (BPE dan Unigram).
    *   Menerima prompt teks, jumlah token yang akan dihasilkan, `temperature`, dan `top_k` sebagai parameter input.
    *   Menghasilkan teks baru berdasarkan prompt menggunakan kedua model dan menampilkan hasilnya.

8.  **Visualisasi Hasil**: 
    *   **Perbandingan Loss Tokenizer**: Membandingkan loss pelatihan akhir antara model yang dilatih dengan tokenizer BPE dan Unigram.
    *   **Tingkat Efisiensi Tokenisasi**: Menganalisis efisiensi kedua tokenizer dalam mengkodekan korpus, termasuk total token dan rata-rata karakter per token.
    *   **Distribusi Panjang Subword**: Memvisualisasikan distribusi panjang subword (dalam karakter) untuk token yang dihasilkan oleh tokenizer BPE dan Unigram.

## Hasil dan Analisis

Proyek ini menunjukkan bahwa model MiniGPT, bahkan dengan skala yang relatif kecil, dapat mempelajari pola bahasa dari korpus spesifik dan menghasilkan teks yang relevan. Perbandingan antara tokenizer BPE dan Unigram memberikan wawasan tentang bagaimana pilihan tokenizer dapat memengaruhi efisiensi tokenisasi dan kinerja model. Visualisasi loss dan efisiensi tokenisasi membantu dalam memahami karakteristik masing-masing metode tokenisasi.