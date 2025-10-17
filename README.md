# NYC-TLC-January-2023-Analysis

## Deskripsi Proyek

Proyek ini merupakan analisis komprehensif terhadap data pemesanan taxi di New York City yang dikelola oleh NYC TLC (New York City Taxi and Limousine Commission). Analisis berfokus pada pola peak hours dan peak days untuk memberikan rekomendasi strategis dalam pengerahan armada dan optimasi layanan.

## Dataset

Dataset mencakup data perjalanan taxi di New York City bulan Januari 2023 dari dua vendor utama:
- Creative Mobile
- VeriFone

Total records setelah data cleaning: 67,896 trips

## Latar Belakang Bisnis

NYC TLC ingin menganalisis pola pemesanan taxi untuk mengetahui:
- Kapan (jam/hari) terjadi puncak pemesanan (peak hours dan peak days)
- Lokasi mana yang paling banyak digunakan sebagai pickup dan dropoff
- Rekomendasi pengerahan armada ke lokasi dan waktu yang strategis
- Tingkat kepuasan pelanggan melalui analisis data tip

## Tahapan Proyek

### 1. Data Cleaning

Data cleaning dilakukan untuk memastikan kualitas data dengan langkah-langkah berikut:

**Penanganan Missing Values:**
- Drop kolom `ehail_fee` (100% missing)
- Fill `store_and_fwd_flag` dengan modus (N)
- Fill `RatecodeID` missing values dengan 99 (Unknown)
- Fill `passenger_count` dengan rata-rata (1)
- Fill `payment_type` dan `trip_type` dengan 99
- Fill `congestion_surcharge` dengan 0 (dihitung dari total_amount)

**Pembersihan Data Temporal:**
- Konversi format datetime ke tipe datetime64
- Filter data hanya tahun 2023 (ada data dari 2009 dan 2022)
- Fokus pada bulan Januari 2023 (data paling lengkap)

**Penghapusan Outliers:**
- Hapus trip dengan jarak > 30 miles (63 data tidak signifikan)
- Hapus trip dengan total_amount <= 0 (248 voided trips dan refunds)

**Enrichment Data:**
- Merge dengan lookup table taxi zones untuk mendapatkan Borough dan Zone information
- Map kode menjadi label deskriptif (VendorID, RatecodeID, PaymentType, TripType)

**Hasil Akhir:**
- Total records: 67,896 valid trips
- No missing values
- Data siap untuk analisis

### 2. Exploratory Data Analysis (EDA)

#### Peak Days Analysis
Analisis menunjukkan pola pemesanan berdasarkan hari dalam seminggu:
- Peak pada hari kerja (Senin-Jumat), terutama akhir minggu (Jumat)
- Drop signifikan pada awal minggu (Minggu-Senin)
- Total trips berkisar 1,460-2,661 per hari
- Pola konsisten untuk kedua vendor

#### Peak Hours Analysis
Analisis menunjukkan pola pemesanan berdasarkan jam dalam sehari:
- Off-peak: Tengah malam (00:00-06:00) dengan 426-1,125 trips/jam
- Ramp-up: Pagi (07:00-09:00) dengan peningkatan signifikan
- Peak: Siang-Sore (10:00-18:00) dengan 3,616-5,196 trips/jam
- Peak tertinggi: Jam 16:00-18:00 (around 5,195 trips/jam)
- Decline: Malam (19:00-23:00) dengan 1,499-4,210 trips/jam

**Peak Hours per Vendor:**
- Creative Mobile: Peak di jam 15:00 (754 trips)
- VeriFone: Peak di jam 18:00 (4,565 trips)

#### Analisis Lokasi

**Pickup Locations (Top 5):**
1. East Harlem North: 13,240 trips
2. East Harlem South: 9,085 trips
3. Central Harlem: 4,041 trips
4. Morningside Heights: 3,874 trips
5. Forest Hills: 3,826 trips

**Borough Distribution (Pickup):**
- Manhattan: 39,350 trips (58%)
- Queens: 17,825 trips (26%)
- Brooklyn: 9,293 trips (14%)
- Bronx: 1,222 trips (2%)
- Others: 206 trips (<1%)

**Dropoff Locations:**
- Manhattan: 39,529 trips (58%)
- Queens: 17,511 trips (26%)
- Brooklyn: 7,808 trips (11%)
- Bronx: 2,372 trips (3%)

#### Analisis Vendor

**Market Share:**
- VeriFone mendominasi dengan ~88% dari total trips
- Creative Mobile hanya ~12% market share

**Performa Komparatif:**
| Metrik | Creative Mobile | VeriFone |
|--------|---|---|
| Rata-rata Tip | $1.67 | $2.22 |
| Rata-rata Fare | $16.40 | $16.66 |
| Rata-rata Jarak Trip | 2.23 miles | 2.73 miles |

#### Analisis Kepuasan Pelanggan (Tip Analysis)

**Statistik Umum:**
- 58% penumpang memberikan tip
- Distribusi tip sangat miring ke kanan (skewness: 10.22)
- Median tip berkisar $1.60

**Tip Berdasarkan Lokasi (Top 10):**
- Lenox Hill West: $25.88 rata-rata
- Newark Airport: $19.00 rata-rata
- Times Sq/Theatre District: $12.03 rata-rata

**Tip Berdasarkan Jam:**
- Tip tertinggi: Tengah malam (00:00-02:00)
- Tip terendah: Sore hari (15:00-17:00)
- VeriFone konsisten mendapat tip lebih tinggi di semua jam

**Insight Statistik:**
- Mann-Whitney U test menunjukkan perbedaan signifikan antara tip vendor (p < 0.05)
- Pada jarak trip yang sama, VeriFone mendapat tip lebih besar
- VeriFone beroperasi di zona yang memberikan tip lebih tinggi

## Key Findings

**1. Peak Period Identification**
- Peak Days: Senin-Jumat, terutama Jumat (2,300+ trips)
- Peak Hours: 10:00-18:00 dengan puncak 16:00-18:00
- Off-Peak: Tengah malam (00:00-06:00)

**2. Geographic Distribution**
- Dominasi trip di Manhattan (58%)
- East Harlem North sebagai hotspot pickup utama
- Perjalanan intra-borough dominan (terutama Manhattan)

**3. Vendor Performance**
- VeriFone mendominasi pasar dengan market share 88%
- VeriFone memiliki trip distance lebih panjang
- Perbedaan tip signifikan antara kedua vendor

**4. Customer Satisfaction**
- Tipping rate 58% menunjukkan kepuasan cukup baik
- Lokasi premium (Times Square, Airport) menghasilkan tip lebih tinggi
- VeriFone memiliki rating kepuasan lebih tinggi secara konsisten

## Rekomendasi

### 1. Rekomendasi Operasional

**Pengerahan Armada:**
- Perbanyak armada pada hari kerja (Senin-Jumat), khususnya Jumat
- Konsentrasikan armada pada jam 10:00-18:00 dengan fokus 14:00-18:00
- Kurangi armada pada tengah malam (00:00-06:00) dan awal minggu

**Alokasi Geografis:**
- Prioritaskan borough Manhattan, Queens, dan Brooklyn
- Fokus zone East Harlem North dan sekitarnya untuk pickup
- Persiapkan capacity untuk dropoff di Manhattan

### 2. Rekomendasi untuk Creative Mobile

**Strategi Kompetitif:**
- Fokuskan operasi di zona dengan trip terbanyak untuk mencapai economies of scale
- Tarik armada dari zona sepi dan konsentrasikan di East Harlem North

**Peningkatan Revenue & Kepuasan:**
- Investasi dalam training customer service untuk meningkatkan tipping rate
- Operasikan lebih banyak di zona premium (Times Square, Airport) dengan tip tinggi
- Targetkan trips dengan jarak lebih jauh untuk meningkatkan revenue per trip

**Perbaikan Rating:**
- Tingkatkan kualitas layanan driver melalui program training
- Monitor kepuasan pelanggan lebih ketat
- Buat incentive program untuk driver berdasarkan rating

### 3. Rekomendasi untuk Pricing & Service

- Dynamic pricing pada peak hours (10:00-18:00) di hari kerja
- Special rates untuk off-peak period untuk meningkatkan utilization
- Service improvement di jam-jam dengan tip rendah

## Tools & Technology

**Data Processing:**
- Python 3.x
- Pandas: Data manipulation dan aggregation
- NumPy: Numerical computations

**Statistical Analysis:**
- SciPy: Mann-Whitney U test dan statistical testing
- Scipy.stats: Normality testing dan hypothesis testing

**Visualization:**
- Matplotlib: Line plots, bar charts
- Seaborn: Advanced statistical plots, heatmaps
- Plotly: Interactive visualizations (optional)

**Data Integration:**
- CSV files untuk data source
- Taxi Zone Lookup table untuk enrichment

## Struktur File

```
├── NYC TLC Trip Record.csv          # Raw data
├── taxi_zone_lookup.csv             # Zone reference data
├── nyc_tlc_analysis.ipynb           # Main analysis notebook
└── NYC TLC January 2023.csv         # Cleaned data
```

## Metodologi Statistik

**Hypothesis Testing:**
- Mann-Whitney U test untuk membandingkan tip antara vendor
- Significance level: α = 0.05

**Time Series Analysis:**
- Aggregation per jam dan per hari
- Trend identification menggunakan line plot
- Seasonal pattern detection

## Limitations & Considerations

- Data hanya mencakup Januari 2023 (satu bulan)
- Tidak ada data tentang demand yang tidak terpenuhi
- External factors (holiday, events) tidak dianalisis
- Hasil mungkin berbeda untuk periode/musim lain

## Kesimpulan

Analisis menunjukkan pola pemesanan taxi yang jelas dengan peak hours pada siang-sore (10:00-18:00) dan peak days pada hari kerja. VeriFone memiliki dominasi pasar yang signifikan dengan kepuasan pelanggan lebih tinggi. Rekomendasi fokus pada optimasi pengerahan armada berdasarkan peak times dan zones, serta strategi kompetitif untuk Creative Mobile melalui peningkatan kualitas layanan dan targeting zona strategis.

link tableau: https://public.tableau.com/app/profile/axel.alexander2273/viz/NYCTLCCapstone2_17606226397600/Dashboard1
