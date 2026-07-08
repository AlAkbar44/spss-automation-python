# 📊 Statistical Data Analysis Automation with Python (Google Colab Version)

Proyek ini merupakan sistem otomatisasi analisis data kuantitatif menggunakan **Python** yang dirancang untuk membantu peneliti melakukan seluruh proses analisis statistik secara otomatis di **Google Colab**. Program mampu membaca data kuesioner dalam format CSV, melakukan pembersihan data, menghitung statistik deskriptif, menguji validitas dan reliabilitas instrumen, melakukan uji asumsi klasik, hingga menghasilkan analisis regresi linear berganda beserta pengujian hipotesis.

Seluruh proses dilakukan hanya dengan menjalankan **satu file Python**, sehingga dapat menjadi alternatif otomatisasi analisis selain menggunakan SPSS.

---

## 🚀 Fitur Utama & Penjelasan Kode

Berikut merupakan alur utama program beserta penjelasan dan potongan kode (*code snippet*) pada setiap bagian sub-bab:

### 📂 1. Membaca Dataset & Validasi File
Program memanfaatkan library internal Google Colab agar pengguna bisa langsung mengunggah file CSV kuesioner dari komputer secara interaktif saat program dijalankan.

```python
from google.colab import files
uploaded = files.upload()
NAMA_FILE_DATA = list(uploaded.keys())[0]
df = pd.read_csv(NAMA_FILE_DATA, sep=";")
```

📖 **Penjelasan:** Tahapan ini digunakan untuk memastikan dataset tersedia sebelum analisis dijalankan. Jika file tidak ditemukan atau proses dibatalkan, program akan menghentikan proses sehingga mencegah terjadinya *error* saat pembacaan data.

---

### 🧹 2. Data Cleaning & Persiapan Variabel
Program membersihkan data dengan menghapus spasi pada nama kolom, mengubah seluruh nilai menjadi numerik, serta menghapus data kosong (*missing value*).

```python
df.columns = df.columns.str.strip()
semua_kolom = k_X1 + k_X2 + k_X3 + k_Y
df_all = df[semua_kolom].apply(pd.to_numeric, errors="coerce").dropna()
```

🧹 **Penjelasan:** Tahap ini bertujuan menghasilkan dataset yang siap dianalisis. Seluruh data yang tidak valid atau kosong akan dihapus sehingga hasil analisis menjadi lebih akurat.

---

### 📊 3. Analisis Statistik Deskriptif
Program menghitung statistik dasar setiap variabel penelitian untuk memberikan gambaran umum data.

```python
deskriptif_data.append({
    "Variabel": v,
    "N": n,
    "Minimum": int(df_clean[v].min()),
    "Maximum": int(df_clean[v].max()),
    "Mean": round(df_clean[v].mean(), 3),
    "Std. Deviation": round(df_clean[v].std(), 3)
})
```

📊 **Penjelasan:** Statistik deskriptif digunakan untuk memberikan gambaran umum mengenai karakteristik data penelitian. Output yang dihasilkan meliputi jumlah responden ($N$), nilai minimum, nilai maksimum, mean, dan standar deviasi.

---

### ✅ 4. Uji Validitas (Pearson Product Moment)
Program menguji apakah setiap indikator mampu mengukur variabel yang seharusnya diukur menggunakan korelasi Pearson.

```python
r_hitung, p_val = stats.pearsonr(df_data[col], df_data[total_col])
status = "VALID ✅" if r_hitung > r_tabel_valid and p_val < 0.05 else "TIDAK VALID ❌"
```

✅ **Penjelasan:** Uji validitas dilakukan untuk mengetahui apakah setiap butir pertanyaan pada kuesioner layak digunakan. Kriteria keputusan dinyatakan valid jika $r_{hitung} > r_{tabel}$ dan $\text{Sig.} < 0.05$.

---

### 🔒 5. Uji Reliabilitas (Cronbach's Alpha)
Program menghitung nilai *Cronbach's Alpha* untuk mengetahui konsistensi jawaban responden pada instrumen penelitian.

```python
def cronbach_alpha(df_data, item_cols):
    df_items = df_data[item_cols]
    k_items = df_items.shape[1]
    item_variances = df_items.var(ddof=1).sum()
    total_variance = df_items.sum(axis=1).var(ddof=1)
    return (k_items / (k_items - 1)) * (1 - (item_variances / total_variance))
```

🔒 **Penjelasan:** Uji reliabilitas digunakan untuk mengetahui apakah instrumen penelitian memiliki tingkat konsistensi yang baik jika pengujian dilakukan berulang. Kriteria andal terikat pada nilai $\text{Alpha} > 0.60$.

---

### 📈 6. Analisis Regresi Linear Berganda
Model regresi dibangun menggunakan library **Statsmodels** untuk memproses data hubungan antar variabel.

```python
X_with_const = sm.add_constant(X_centered)
model = sm.OLS(Y_final, X_with_const).fit()
```

📈 **Penjelasan:** Analisis ini digunakan untuk mengetahui pengaruh kelompok variabel independen terhadap variabel dependen. Output meliputi Koefisien Regresi ($B$), Beta, Standard Error, Nilai $t$, dan Nilai Signifikansi.

---

### 📉 7. Uji Normalitas Residual
Normalitas residual diuji menggunakan metode non-parametrik *One-Sample Kolmogorov-Smirnov Test*.

```python
residual_std_test = (residual_raw - mean_res) / std_res
ks_stat, p_value_ks = stats.kstest(residual_std_test, "norm")
```

📉 **Penjelasan:** Uji normalitas bertujuan mengetahui apakah nilai residual dalam model regresi berdistribusi secara normal. Model dinyatakan normal jika nilai $\text{Sig.} > 0.05$.

---

### 📌 8. Uji Multikolinearitas
Program menghitung nilai *Tolerance* dan *Variance Inflation Factor* (VIF) untuk memeriksa hubungan korelasi antar variabel independen.

```python
tabel_vif_only = tabel_spss_master[["Model", "Tolerance", "VIF"]]
```

📌 **Penjelasan:** Uji ini digunakan untuk memastikan bahwa tidak terjadi hubungan linear yang terlalu tinggi antar variabel independen. Kriteria lolos uji adalah $\text{Tolerance} > 0.10$ dan $\text{VIF} < 10$.

---

### 📉 9. Uji Heteroskedastisitas (Glejser Test)
Program mendeteksi kesamaan varians residual menggunakan metode **Uji Glejser** dengan meregresikan nilai absolut residual.

```python
abs_resid = np.abs(model.resid)
model_glejser = sm.OLS(abs_resid, X_with_const).fit()
```

📉 **Penjelasan:** Uji heteroskedastisitas dilakukan untuk memastikan varians residual bersifat konstan (homoskedastisitas). Model dinyatakan lolos jika nilai $\text{Sig.} > 0.05$.

---

### 🎯 10. Uji Hipotesis Parsial (Uji t)
Program melakukan uji signifikansi koefisien individu dengan membandingkan parameter $t$-hitung terhadap nilai kritis tabel.

```python
t_hit = row_data["t"].values[0]
kesimpulan = "Berpengaruh Signifikan ✅" if abs(t_hit) > t_tabel_val and p_sig < 0.05 else "Tidak Berpengaruh ❌"
```

🎯 **Penjelasan:** Uji t digunakan untuk mengetahui apakah masing-masing variabel independen secara parsial atau individu memiliki pengaruh signifikan terhadap variabel dependen.

---

### 📊 11. Uji Hipotesis Simultan (Uji F)
Program menghitung parameter pengujian signifikansi kelompok model regresi secara bersama-sama.

```python
f_hit = model.fvalue
p_f_sig = model.f_pvalue
```

📊 **Penjelasan:** Uji F digunakan untuk mengetahui apakah seluruh variabel independen secara simultan berpengaruh signifikan terhadap variabel dependen dengan kriteria $F_{hitung} > F_{tabel}$ dan $\text{Sig.} < 0.05$.

---

### 📈 12. Koefisien Determinasi ($R^2$)
Program mengekstrak ringkasan performa model dari hasil evaluasi ringkasan variabilitas objek.

```python
print(model.rsquared)
print(model.rsquared_adj)
```

📈 **Penjelasan:** Koefisien determinasi digunakan untuk mengetahui seberapa besar variasi variabel dependen dapat dijelaskan oleh model regresi melalui nilai *R Square* dan *Adjusted R Square*.

---

## 🛠️ Cara Menggunakan di Google Colab

1. Salin seluruh kode program Python (yang berada di kotak kode paling bawah dokumen ini).
2. Buka [Google Colab](https://colab.research.google.com/), buat Notebook Baru (*New Notebook*).
3. Tempelkan kode ke dalam cell Google Colab, lalu jalankan cell tersebut (`Ctrl + Enter`).
4. Jendela program akan memunculkan tombol **"Choose Files"**. Klik tombol tersebut lalu unggah file kuesioner Anda (format `.csv` dengan pemisah/delimiter titik koma `;`).
5. Program akan otomatis memproses data dan menampilkan 7 tahapan output pengujian statistik ala SPSS langsung di layar Colab.

---

## 🤝 Kontribusi

Jika Anda ingin mengembangkan proyek ini lebih lanjut (misalnya menambahkan fitur visualisasi grafik otomatis menggunakan `matplotlib` atau `seaborn`), silakan ikuti langkah berikut:

1. Lakukan **Fork** pada repositori ini.
2. Buat branch fitur baru Anda (`git checkout -b fitur-baru-keren`).
3. Lakukan commit pada perubahan Anda (`git commit -m 'Menambahkan fitur X'`).
4. Push ke branch tersebut (`git push origin fitur-baru-keren`).
5. Buat sebuah **Pull Request**.

---

## 📜 Lisensi

Proyek ini dilisensikan di bawah **MIT License** - lihat file [LICENSE](LICENSE) untuk detail lebih lanjut. Bebas digunakan untuk keperluan akademik, tugas akhir, maupun riset independen.

---

## 📚 Daftar Pustaka (References)

Analisis mekanis dan struktur logika perhitungan statistik dalam program ini disesuaikan dengan standar metodologi penelitian kuantitatif berikut:

* Ghozali, I. (2018). *Aplikasi Analisis Multivariate dengan Program IBM SPSS 25*. Badan Penerbit Universitas Diponegoro.
* Hair, J. F., Black, W. C., Babin, B. J., & Anderson, R. E. (2019). *Multivariate Data Analysis* (8th ed.). Cengage Learning.
* Sugiyono. (2019). *Metode Penelitian Kuantitatif, Kualitatif, dan R&D*. Alfabeta.


