# Multi-Store Retail Analytics with Holiday and External Factor Impact — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları
- **Datasetin Yüklənməsi:** `Walmart.csv` — 6435 sətir, 45 mağaza × 143 həftə, 2010-02-05 –
  2012-10-26 aralığı (təxminən 2 il 9 ay). Hər mağaza üçün eyni sayda (143) həftəlik qeyd var,
  yəni dataset balanslıdır və boş həftə yoxdur.
- **Date Sütununun Çevrilməsi:** `Date` sütunu `DD-MM-YYYY` mətn formatında idi,
  `pd.to_datetime(..., format='%d-%m-%Y')` ilə datetime tipinə çevrildi, sonra `Store` və
  `Date` üzrə sıralandı.
- **Year/Month Sütunlarının Əlavəsi:** `Year` (2010/2011/2012) və `Month` (YYYY-MM formatında)
  sütunları əlavə edildi.
- **Boş Dəyər Yoxlanışı:** heç bir sütunda boş (`NaN`) dəyər tapılmadı, əlavə təmizləmə tələb
  olunmadı.
- **Bayram Tarixlərinin Müəyyənləşdirilməsi:** `Holiday_Flag = 1` olan sətirlər 450-dir (45
  mağaza × 10 unikal tarix). Bu 10 tarix 4 bayrama uyğun gəlir: Super Bowl (fevral, 3 dəfə —
  2010/2011/2012), Labor Day (sentyabr, 3 dəfə), Thanksgiving (noyabr, 2 dəfə — 2010/2011;
  dataset oktyabr 2012-də bitdiyi üçün 2012-ci ilin Thanksgiving-i yoxdur) və Christmas
  (dekabr, 2 dəfə — 2010/2011). Qeyd edək ki, Christmas üçün işarələnən tarix əslində 24/25
  dekabr yox, 31 dekabr/30 dekabr həftəsidir — bu, Walmart dataset-inin məlum xüsusiyyətidir.

## 2. Əsas Statistik Göstəricilər

- **Mağazaların Səviyyələnməsi (Ümumi Satışa Görə):** Top 10 / Orta 25 / Son 10
  seqmentləşdirməsi 45 mağazanın hamısını əhatə edir.
  - Ən yüksək ümumi satış: **20-ci mağaza ($301.4M)**, ardınca 4-cü ($299.5M), 14-cü
    ($289.0M), 13-cü ($286.5M), 2-ci ($275.4M) mağazalar gəlir.
  - Ən aşağı ümumi satış: **33-cü mağaza ($37.2M)**, ardınca 44-cü ($43.3M), 5-ci
    ($45.5M), 36-cı ($53.4M), 38-ci ($55.2M) mağazalar gəlir.
  - Ən yüksək (20) və ən aşağı (33) satışlı mağazalar arasında ~8.1 dəfə fərq var — mağaza
    ölçüsü və formatının satışa təsiri çox böyükdür.

- **Bayramın Satışa Təsiri:**
  - Bayram həftələrinin orta satışı: **$1.123M**; adi həftələrdə: **$1.041M** →
    ümumi artım **+7.8%**.
  - Bayram növünə görə artım (adi orta göstəriciyə nisbətən):
    - **Thanksgiving: +41.3%** — ən böyük sıçrayış, Black Friday ilə üst-üstə düşür.
    - Super Bowl: +3.6%
    - Labor Day: +0.1% (demək olar heç bir fərq yoxdur)
    - **Christmas: −7.7%** — mənfidir, çünki işarələnən həftə əslində bayramın özündən
      sonrakı (31 dekabr) həftədir, əsas alış-veriş isə bundan əvvəlki, işarələnməmiş
      həftələrdə baş verir.
  - Nəticə: **Thanksgiving satışa ən çox təsir edən bayramdır**, Christmas isə tarix
    seçimindən ötrü yanlışlıqla "aşağı" görünür.

- **Aylıq Satış Trendi (2010–2012):** Ən yüksək aylıq satış **dekabr aylarına** düşür —
  2010-12 ($288.8M) və 2011-12 ($288.1M) demək olar bərabərdir və bütün dövrün ən yüksək
  iki ayıdır (bayram alış-veriş mövsümü — Thanksgiving/Black Friday-dən sonra davam edən
  yüksək tələb). İkinci pik iyul, aprel və iyun aylarında görünür (yay mövsümü və ya
  məktəb başlanğıcı ilə bağlı ola bilər).

- **Mağaza × İl Pivot Cədvəli və İllik Artım:**
  - 2010→2011 arası orta illik artım: **+7.3%** (45 mağazadan yalnız 3-ü — 18, 35, 36 —
    azalma göstərib).
  - 2011→2012 illik göstərici bütün mağazalarda mənfi görünür (orta hesabla **−17.7%**), amma
    bunun səbəbi **məlumatın özündədir**: 2012-ci il məlumatı yalnız oktyabra qədərdir
    (noyabr/dekabr — ilin ən güclü satış ayları — 2012 üçün dataset-də yoxdur). Ona görə
    2011 ilə 2012-ni tam illik müqayisə etmək düzgün deyil və bunu real azalma kimi
    şərh etmək olmaz.

- **Korrelyasiya (Həftəlik Satışla):**
  - Unemployment (işsizlik): r = **−0.106** (zəif mənfi)
  - CPI: r = **−0.073** (əlaqə demək olar yoxdur)
  - Temperature (temperatur): r = **−0.064** (əlaqə demək olar yoxdur)
  - Fuel_Price (yanacaq qiyməti): r = **+0.009** (əlaqə yoxdur)
  - Bu 4 makroiqtisadi göstəricinin heç biri mağaza səviyyəsində satışı ciddi şəkildə izah
    etmir — satışdakı fərqlər əsasən mağazanın özünəməxsus xüsusiyyətlərindən (ölçü, format,
    yerləşdiyi ərazi) və bayram təqviminə bağlıdır.
  - **İşsizliyə həssaslıq mağazadan-mağazaya dəyişir:** 38-ci (r = −0.79) və 44-cü
    (r = −0.78) mağazalarda işsizlik artdıqca satış ən çox azalır; əksinə 36-cı (r = +0.83)
    və 35-ci (r = +0.48) mağazalarda işsizliklə satış arasında müsbət əlaqə var — bu, yerli
    demoqrafiya və ya rəqabət mühiti ilə bağlı ola bilər.

- **Əlavə: Satışın Dəyişkənliyi (CV = Std/Orta):** Ən dəyişkən (proqnozlaşdırılması çətin)
  mağazalar: 35-ci (CV 23.0%), 7-ci (19.7%), 15-ci (19.3%), 29-cu (18.4%), 23-cü (18.0%).
  Ən sabit mağazalar: 37-ci (4.2%), 30-cu (5.2%), 43-cü (6.4%). Qeyd edək ki, xam standart
  sapmaya görə sıralasaq, ən "dəyişkən" görünən böyük mağazalar (14, 10, 20) çıxır, çünki
  onların satış həcmi də böyükdür — nisbi göstərici (CV) daha ədalətli müqayisə verir.

- **Əlavə: İllik Azalma + Müsbət Bayram Nəticəsi (2011):** 18-ci mağaza (illik dəyişmə
  −3.1%, bayram artımı +14.1%) və 35-ci mağaza (illik dəyişmə −15.5%, bayram artımı
  +33.7%) — hər iki mağaza 2011-ci ildə bayram həftələrində güclü satış artımı göstərsə də,
  ilin ümumi satışı 2010-a nisbətən azalıb. Bu onu göstərir ki, bayram nəticələrinin yaxşı
  olması ilin qalan hissəsindəki problemi (məsələn, yaxınlıqda açılan rəqib mağaza, yerli
  iqtisadi tənəzzül) örtə bilmir — bu mağazalar ayrıca araşdırılmalıdır.

## 3. Əməliyyat və Siyasət Baxımından Tövsiyələr

1. **Thanksgiving satışa ən çox təsir edən bayramdır (+41.3%), Super Bowl (+3.6%) və
   Labor Day (+0.1%) isə adi həftələrdən demək olar fərqlənmir.** Bayram dövründə işçi
   qüvvəsi və stok planlamasının böyük hissəsi Thanksgiving/Black Friday həftəsinə
   yönəldilməli, digər bayramlar üçün adi iş rejimi yetərlidir.

2. **Christmas həftəsinin mənfi görünməsi (−7.7%) real satış zəifliyi demək deyil** —
   səbəb, dataset-də işarələnən tarixin (31 dekabr) əsl bayram pikindən (noyabrın sonu –
   24 dekabr) sonra olmasıdır. Aylıq trend göstərir ki, dekabr ayı bütövlükdə ilin ən güclü
   ayıdır (2010 və 2011-də ~$288M). Bayramın satışa təsirini qiymətləndirərkən yalnız
   `Holiday_Flag` sütununa deyil, ümumi aylıq mənzərəyə də baxmaq lazımdır.

3. **Makroiqtisadi göstəricilər (işsizlik, CPI, temperatur, yanacaq qiyməti) mağaza
   səviyyəsində satışı praktiki olaraq izah etmir (bütün göstəricilərdə |r| < 0.11).**
   Bunun üçün regional qiymət və təchizat strategiyası makro göstəricilərə deyil, hər
   mağazanın özünəməxsus göstəricilərinə (səviyyə qrupu, tarixi trend, bayram davranışı)
   əsaslanmalıdır. Buna baxmayaraq, 38-ci və 44-ci mağazalar kimi işsizliyə güclü həssaslıq
   göstərən mağazalarda yerli iqtisadi vəziyyət izlənməyə davam etməlidir.

4. **2011→2012 illik azalma göstəricisi (orta −17.7%) yanlış təsəvvür yaradır** — dataset
   2012-ci ili oktyabrda kəsir, yəni ilin ən güclü satış ayları (noyabr/dekabr) həmin il
   üçün mövcud deyil. Hesabatlarda illik müqayisə tam təqvim ili üzrə aparılmalı, yarımçıq
   illər üçün isə "annualized" göstərici və ya eyni aylıq dövrün müqayisəsi istifadə
   olunmalıdır.

5. **18-ci və 35-ci mağazaların nümunəsi göstərir ki, bir mağazanın bayram həftələrindəki
   yaxşı nəticəsi onun ilin qalan hissəsindəki vəziyyətini əks etdirmir.** Bu mağazalar
   üçün il boyu satış trendi, yerli rəqabət mühiti və mağaza formatı ayrıca
   araşdırılmalı, təkcə bayram nəticələrinə əsaslanaraq qərar verilməməlidir. Ümumilikdə,
   Top 10 mağazalar (20, 4, 14, 13, 2 və s.) ən yüksək satış həcminə malikdir və bu
   mağazaların əməliyyat modelini (stok, iş saatları, işçi sayı) format və yer fərqlərini
   nəzərə almadan Son 10 mağazalara tətbiq etmək səmərəsiz ola bilər.
