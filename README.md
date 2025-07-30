# Perbandingan Topic Modeling: LDA vs. BERTopic pada Dataset Artikel Ilmiah ArXiv

Repositori ini berisi analisis perbandingan antara dua algoritma *topic modeling* populer, yaitu Latent Dirichlet Allocation (LDA) dan BERTopic. Studi kasus ini menggunakan dataset abstrak artikel ilmiah dari [arXiv.org](https://arxiv.org/) untuk mengidentifikasi dan mengevaluasi kualitas topik yang dihasilkan oleh masing-masing metode.

## Latar Belakang

Volume publikasi ilmiah yang sangat besar menyulitkan para peneliti untuk mengikuti perkembangan terbaru. *Topic modeling* hadir sebagai solusi untuk mengelompokkan jutaan dokumen secara otomatis ke dalam tema-tema utama, sehingga memungkinkan identifikasi tren dan eksplorasi literatur yang efisien. Proyek ini bertujuan untuk membandingkan metode klasik (LDA) dengan metode modern berbasis *transformer* (BERTopic) untuk melihat mana yang lebih efektif dalam menangani teks ilmiah.

## 🎯 Pertanyaan Penelitian

1.  Topik apa saja yang berhasil ditemukan oleh masing-masing metode dari kumpulan abstrak artikel?
2.  Berapa jumlah topik optimal untuk LDA, dan model mana yang memberikan kualitas topik terbaik berdasarkan interpretabilitas?
3.  Seberapa sesuai hasil topik dari masing-masing metode dengan label kategori asli dari artikel?

## 💾 Dataset

Dataset yang digunakan adalah **[ArXiv Dataset](https://www.kaggle.com/datasets/Cornell-University/arxiv)** dari Kaggle. Untuk analisis ini, dilakukan *sampling* acak sebanyak **10.000 artikel** yang dipublikasikan dari tahun **2021 ke atas**. Teks utama yang dianalisis adalah gabungan dari judul dan abstrak setiap artikel.

## ⚙️ Metodologi

Analisis dilakukan melalui beberapa tahapan utama yang terdokumentasi dalam notebook Jupyter:

1.  **Pra-pemrosesan Teks**: Teks dibersihkan melalui serangkaian proses standar yang diterapkan sebelum pemodelan LDA:
    * *Case Folding* (mengubah teks menjadi huruf kecil).
    * Tokenisasi.
    * *Stopword Removal* (menghilangkan kata-kata umum).
    * Lemmatisasi (mengubah kata ke bentuk dasarnya).
    * Filtering kata-kata yang tidak penting (misalnya, terlalu pendek).

2.  **Penerapan Algoritma**:
    * **Latent Dirichlet Allocation (LDA)**: Diimplementasikan menggunakan *library* `gensim`. Jumlah topik optimal ditentukan menggunakan metrik *Coherence Score*, yang menghasilkan **20 topik** sebagai konfigurasi terbaik. Model ini dilatih pada teks yang sudah diproses.
    * **BERTopic**: Diimplementasikan menggunakan *library* `bertopic` dengan model *embedding* `all-MiniLM-L6-v2`. Sesuai praktik terbaiknya, BERTopic dilatih pada teks mentah untuk memanfaatkan kemampuannya dalam menangkap konteks semantik.

3.  **Evaluasi dan Perbandingan**:
    * **Kuantitatif**: Menghitung *Coherence Score* untuk menemukan jumlah topik LDA yang optimal.
    * **Kualitatif**: Membandingkan topik-topik yang dihasilkan secara berdampingan berdasarkan keterbacaan, koherensi, dan relevansi dengan domainnya.
    * **Validasi Eksternal**: Memeriksa komposisi kategori asli artikel dalam setiap topik yang dihasilkan LDA untuk memvalidasi relevansinya.

## 📊 Hasil dan Analisis Utama

Hasil analisis menunjukkan bahwa untuk dataset artikel ilmiah ini, **LDA secara signifikan mengungguli BERTopic** dalam menghasilkan topik yang bersih, spesifik, dan mudah diinterpretasikan.

-   **LDA** berhasil mengidentifikasi klaster topik yang sangat koheren dan relevan dengan bidang ilmu seperti:
    -   *Computer Vision & Machine Learning* (Topik 14)
    -   Fisika Kuantum (Topik 16)
    -   Teori Graf & Matematika (Topik 10)
    -   Linguistik Komputasional & NLP (Topik 18)

-   **BERTopic**, dalam konfigurasi standarnya, cenderung menghasilkan topik yang "tercemar" oleh kata-kata umum (mirip *stopwords*), sehingga sulit untuk diinterpretasikan dan kurang memberikan wawasan yang mendalam.

| Tema | LDA (Topik 14) - Hasil Jernih | BERTopic (Topik 4) - Hasil Tercemar |
| :--- | :--- | :--- |
| **Fisika Kuantum**| `['quantum', 'state', 'theory', 'classical', 'circuit', ...]` | `['quantum', 'the', 'of', 'to', 'and', 'in', ...]` |

## 💡 Kesimpulan

Meskipun BERTopic adalah teknologi yang lebih modern, studi kasus ini menunjukkan bahwa **pendekatan klasik seperti LDA, jika diterapkan dengan metodologi yang tepat (pra-pemrosesan dan tuning), masih bisa menjadi alat yang lebih kuat dan efektif** untuk dataset teks yang terstruktur seperti abstrak ilmiah.

Kelemahan BERTopic dalam konfigurasi standar menyoroti pentingnya penyesuaian lebih lanjut (misalnya, representasi topik) untuk mendapatkan hasil yang optimal, sementara LDA memberikan hasil yang solid setelah pra-pemrosesan.