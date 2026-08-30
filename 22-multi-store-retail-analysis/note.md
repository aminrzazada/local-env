# Multi-Store Retail Analytics with Holiday and External Factor Impact — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları
- **Datasetin Yüklənməsi:** `Walmart.csv` — 6435 sətir, 45 Store × 143 həftə, 2010-02-05 –
  2012-10-26 aralığı (təxminən 2 il 9 ay). Hər store üçün eyni sayda (143) həftəlik qeyd var,
  yəni dataset balanslıdır və boş həftə yoxdur.
- **Date Sütununun Çevrilməsi:** `Date` sütunu `DD-MM-YYYY` mətn formatındaydı,
  `pd.to_datetime(..., format='%d-%m-%Y')` ilə datetime tipinə çevrildi, sonra `Store` və
  `Date` üzrə sıralandı.
- **Year/Month Sütunlarının Əlavəsi:** `Year` (2010/2011/2012) və `Month` (period, YYYY-MM)
  sütunları törəmə göstəricilər üçün əlavə edildi.
- **Boş Dəyər Yoxlanışı:** heç bir sütunda boş (`NaN`) dəyər aşkar edilmədi — əlavə imputasiya
  tələb olunmadı.
- **Holiday Tarixlərinin Eyniləşdirilməsi:** `Holiday_Flag = 1` olan sətirlər 450-dir (45 store
  × 10 unikal tarix), bu 10 tarix 4 bayram tipinə uyğundur: Super Bowl (fevral, 3 dəfə:
  2010/2011/2012), Labor Day (sentyabr, 3 dəfə), Thanksgiving (noyabr, 2 dəfə: 2010/2011,
  dataset oktyabr 2012-də bitdiyi üçün 2012-ci il Thanksgiving-i yoxdur) və Christmas
  (dekabr, 2 dəfə: 2010/2011). Qeyd: Christmas üçün flag-lənən tarix faktiki 24/25 dekabr yox,
  31 dekabr/30 dekabr həftəsidir — bu, orijinal Walmart dataset-inin məlum xüsusiyyətidir.

## 2. Əsas Statistik Göstəricilər

- **Store Tiering (ümumi satışa görə):** Top 10 / Mid 25 / Bottom 10 seqmentləşdirməsi 45
  store-un hamısını əhatə edir.
  - Ən yüksək kümülatif satış: **Store 20 ($301.4M)**, ardınca Store 4 ($299.5M), Store 14
    ($289.0M), Store 13 ($286.5M), Store 2 ($275.4M).
  - Ən aşağı kümülatif satış: **Store 33 ($37.2M)**, ardınca Store 44 ($43.3M), Store 5
    ($45.5M), Store 36 ($53.4M), Store 38 ($55.2M).
  - Top store (20) ilə bottom store (33) arasında ~8.1x fərq var — store ölçüsü/format
    fərqinin satışa təsiri çox böyükdür.

- **Holiday Impact:**
  - Bayram həftələrinin ortalama satışı: **$1.123M**; normal həftələr: **$1.041M** →
    ümumi uplift **+7.8%**.
  - Bayram üzrə uplift (normal ortalamaya nisbətən):
    - **Thanksgiving: +41.3%** — ən yüksək spike, Black Friday effekti ilə üst-üstə düşür.
    - Super Bowl: +3.6%
    - Labor Day: +0.1% (praktiki olaraq neytral)
    - **Christmas: −7.7%** — mənfi, çünki flag-lənən həftə faktiki bayram bayramından sonrakı
      (31 dekabr) həftədir və əsas alış-veriş dalğası bundan əvvəlki (flag-lənməyən)
      həftələrdə baş verir.
  - Nəticə: **Thanksgiving həftəsi ən güclü satış hərəkətverici bayramdır**, Christmas həftəsi
    isə tarix seçimi səbəbindən aldadıcı şəkildə "aşağı" görünür.

- **Monthly Trend (2010–2012):** Aylıq aqreqat satışın zirvəsi **dekabr aylarındadır** —
  2010-12 ($288.8M) və 2011-12 ($288.1M) demək olar bərabərdir və bütün dövrün ən yüksək
  iki ayıdır (bayram alış-veriş mövsümü: Thanksgiving/Black Friday-dən sonrakı davamlı
  yüksək tələb). İkinci pik qrupu iyul/aprel/iyun aylarında müşahidə olunur (yay
  mövsümü/məktəb başlanğıcı effekti ehtimal olunur).

- **Store × Year Pivot / YoY Growth:**
  - 2010→2011 arası ortalama YoY artım: **+7.3%** (45 store-un yalnız 3-ü — Store 18, 35, 36 —
    geriləmə göstərib).
  - 2011→2012 YoY göstəricisi bütün store-larda mənfi görünür (ortalama **−17.7%**), lakin
    bu **məlumat artefaktıdır**: 2012 məlumatı yalnız oktyabra qədərdir (Noyabr/Dekabr —
    ilin ən güclü satış ayları — 2012 üçün datasetdə yoxdur), ona görə 2011 vs 2012 tam illik
    müqayisə deyil və birbaşa "geriləmə" kimi şərh edilməməlidir.

- **Correlation (Weekly_Sales ilə):**
  - Unemployment: r = **−0.106** (zəif mənfi)
  - CPI: r = **−0.073** (praktiki əlaqə yoxdur)
  - Temperature: r = **−0.064** (praktiki əlaqə yoxdur)
  - Fuel_Price: r = **+0.009** (əlaqə yoxdur)
  - Bu 4 makroiqtisadi göstəricinin heç biri store səviyyəsində satışı əhəmiyyətli dərəcədə
    izah etmir — satış dəyişkənliyi əsasən store-spesifik amillərdən (ölçü, format,
    lokasiya) və bayram təqviminə bağlıdır.
  - **Unemployment-ə həssaslıq store üzrə fərqlənir:** Store 38 (r = −0.79) və Store 44
    (r = −0.78) işsizlik artdıqca satışın ən çox düşdüyü store-lardır; əksinə Store 36
    (r = +0.83) və Store 35 (r = +0.48) işsizliklə pozitiv əlaqə göstərir — bu, yerli
    demoqrafiya/rəqabət mühitindən qaynaqlanan store-spesifik effekt ola bilər.

- **Bonus — Satış Volatilliyi (CV = Std/Mean):** Ən qeyri-sabit store-lar: Store 35
  (CV 23.0%), Store 7 (19.7%), Store 15 (19.3%), Store 29 (18.4%), Store 23 (18.0%). Ən
  sabit store-lar: Store 37 (4.2%), Store 30 (5.2%), Store 43 (6.4%). Qeyd: xam std ilə
  sıralasaq ən yüksək mütləq volatillik böyük store-larda (14, 10, 20) görünür, çünki
  onların satış həcmi də böyükdür — nisbi (CV) ölçü daha ədalətli müqayisə verir.

- **Bonus — YoY Geriləmə + Pozitiv Bayram Həftəsi (2011):** Store 18 (YoY −3.1%, bayram
  uplifti +14.1%) və Store 35 (YoY −15.5%, bayram uplifti +33.7%) — hər iki store 2011-ci
  ildə bayram həftələrində güclü satış artımı göstərsə də, ilin ümumi satışı 2010-a nisbətən
  azalıb. Bu, bayram performansının yaxşı olmasının ilin qalan hissəsindəki struktur
  problemi (məs. bağlanan rəqiblərin təsiri, yerli iqtisadi tənəzzül) kompensasiya
  etmədiyini göstərir — investigasiya üçün flag-lənməlidir.

## 3. Business/Policy Insights

1. **Thanksgiving həftəsi bütün bayramlar arasında ən güclü satış hərəkətvericisidir
   (+41.3% normal ortalamaya nisbətən), Super Bowl (+3.6%) və Labor Day (+0.1%) isə
   praktiki olaraq adi həftələrdən fərqlənmir.** Bayram üzrə işçi qüvvəsi/stok planlaması
   resurslarının böyük hissəsi Thanksgiving/Black Friday həftəsinə yönəldilməli, digər
   bayramlar üçün standart əməliyyat rejimi kifayətdir.

2. **Christmas həftəsinin mənfi görünməsi (−7.7%) real satış zəifliyi deyil, dataset-də
   flag-lənən tarixin (31 dekabr) faktiki bayram pikindən (Noyabr sonu – 24 dekabr) sonra
   olması ilə izah olunur.** Aylıq trend göstərir ki, dekabr ayı bütövlükdə ilin ən güclü
   ayıdır (2010 və 2011-də ~$288M) — bayram effektini qiymətləndirərkən yalnız
   `Holiday_Flag` sütununa deyil, tam aylıq konteksə baxmaq lazımdır.

3. **Makroiqtisadi göstəricilər (Unemployment, CPI, Temperature, Fuel_Price) store
   səviyyəsində satışı praktiki olaraq izah etmir (bütün |r| < 0.11).** Ona görə region
   səviyyəli qiymət/təchizat strategiyası makro göstəricilərə deyil, store-spesifik
   performans göstəricilərinə (tier, tarixi trend, bayram davranışı) əsaslanmalıdır.
   Bununla belə, Store 38 və Store 44 kimi işsizliyə güclü mənfi həssaslıq göstərən
   store-lar üçün yerli iqtisadi indikatorlar izlənməyə davam edilməlidir.

4. **2011→2012 YoY azalması (−17.7% ortalama) yanlış siqnaldır — dataset 2012-ni oktyabrda
   kəsir, yəni ilin ən güclü satış ayları (noyabr/dekabr) 2012 üçün məlumatda yoxdur.**
   Hesabatlarda YoY göstəricisi tam təqvim ili ilə müqayisə edilməli, yarımçıq illər üçün
   "annualized" və ya eyni aylıq pəncərəyə əsaslanan müqayisə istifadə olunmalıdır.

5. **Store 18 və Store 35 kimi "yaxşı bayram, pis il" nümunələri göstərir ki, bayram
   performansı tək başına store sağlamlığının göstəricisi deyil.** Bu store-lar üçün
   il boyu satış trendi, yerli rəqabət mühiti və mağaza formatı ayrıca araşdırılmalı,
   yalnız bayram nəticələrinə əsaslanan qərar qəbul edilməməlidir. Ümumilikdə, Top 10
   store-lar (Store 20, 4, 14, 13, 2 və s.) ən yüksək satış həcminə malikdir və bu
   store-ların əməliyyat modelini (stok, saat, işçi qüvvəsi) Bottom 10 store-lara tətbiq
   etmək — format/lokasiya fərqləri nəzərə alınmadan — səmərəsiz ola bilər.
