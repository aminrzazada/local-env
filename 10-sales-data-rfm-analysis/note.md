# RFM Segmentasiya və Müştəri Profilləşdirməsi — Hesabat

## 1. Data Hazırlama və Hesablama Addımları
- **Datasetin Yüklənməsi:** `sales_data_sample.csv` (2,823 sətir, 307 unikal sifariş nömrəsi,
  92 unikal `CUSTOMERNAME`) yükləndi, `ORDERDATE` datetime formatına çevrildi.
- **Reference Date:** `reference_date = df['ORDERDATE'].max() + 1 gün` düsturu ilə **2005-06-01**
  təyin edildi (dataset-in son sifariş tarixindən bir gün sonra). **Qeyd:** ilkin sənəddəki
  tarix (2005-06-01) faktiki nəticə ilə tam üst-üstə düşür — bu rəqəm təsdiqləndi.
- **Müştəri Səviyyəsinə Aggregasiya:** `groupby('CUSTOMERNAME')` ilə hər müştəri üçün 3 metrik
  hesablandı:
  - **Recency** = son sifarişdən bəri keçən gün sayı
  - **Frequency** = unikal sifariş sayı (`ORDERNUMBER.nunique()`) — sətir (order-line) sayı
    deyil, çünki bir sifarişdə bir neçə məhsul sətri ola bilər
  - **Monetary** = müştərinin bütün sifarişləri üzrə cəmi `SALES` dəyəri
- **Nəticə:** 92 unikal müştəri RFM cədvəlinə daxil edildi. **Qeyd:** ilkin sənəddəki "92 unikal
  müştəri" rəqəmi faktiki nəticə ilə tam üst-üstə düşür — təsdiqləndi.
- **Quantile Scoring:** hər metrik `pd.qcut` ilə 4 bərabər kvartilə bölünərək 1–4 bal verildi
  (Recency tərsinə çevrildi — az gün = yüksək bal). Frequency-də çoxlu təkrarlanan dəyər olduğu
  üçün (`.rank(method='first')`) əvvəlcə rənglənərək  sonra kəsildi, əks halda `qcut`
  "duplicate bin edge" xətası verirdi.
- **Segment Təyini:** `R_Score + F_Score + M_Score` kombinasiyasına əsaslanan qayda dəsti ilə
  6 adlı seqmentə (Champions, Loyal, At Risk, New/Promising, Needs Attention, Lost) bölündü.

## 2. Əsas Statistik Göstəricilər

- **Recency:** min = **1 gün**, orta = **182.8 gün**, maks = **509 gün**. **Qeyd:** ilkin
  sənəddəki "max=532 gün, ortalama=187 gün" iddiası faktiki nəticə ilə yaxındır, lakin tam eyni
  deyil — kiçik fərq, çox güman ki, ilkin sənədin təxmini/nümunəvi rəqəmlər olmasından qaynaqlanır.
- **Frequency:** min = **1 sifariş**, orta = **3.3 sifariş**, maks = **26 sifariş**. **Qeyd:**
  ilkin sənəddəki "max=43 sifariş, ortalama=13.5" rəqəmi faktiki nəticə ilə **əhəmiyyətli dərəcədə
  fərqlidir**. Bu fərq, çox güman ki, ilkin sənədin Frequency-ni order-line (sətir) səviyyəsində
  hesablamasından qaynaqlanır (məhsul sətirlərinin sayı), halbuki tapşırıq spesifikasiyası
  Frequency-ni unikal `ORDERNUMBER` sayı kimi tələb edir — biz spesifikasiyaya sadiq qaldıq.
- **Monetary:** min = **$9,129**, orta = **$109,050**, maks = **$912,294**. **Qeyd:** ilkin
  sənəddəki "min=$482, max=$276K, ortalama=$38.7K" rəqəmləri faktiki nəticə ilə **üst-üstə
  düşmür** — ilkin sənədin min/max dəyərləri, ehtimal ki, sifariş-səviyyəli (tək sətir) `SALES`
  dəyərlərinə aiddir, müştəri-səviyyəli cəmi məbləğə deyil; müştəri səviyyəsində aqreqasiya
  edildikdə rəqəmlər təbii olaraq daha yüksək çıxır.
- **Champions (R=4, F=4, M=4):** **9 müştəri** — satışın **26.0%**-ni təşkil edir. **Qeyd:**
  müştəri sayı (9) ilkin sənədlə tam üst-üstə düşür, lakin gəlir payı ilkin sənəddəki "28%"
  rəqəmindən bir qədər fərqlidir (26.0% vs 28%) — kiçik fərq, real hesablama ilə təxmini
  qiymətləndirmə arasındakı normal sapmadır.
- **At Risk (R≤2, F≥3, M≥3):** **7 müştəri** — ilkin sənədlə tam üst-üstə düşür. Orta Recency
  ~239 gün, orta Monetary ~$125,490 — Loyal seqmentinə yaxın xərcləmə səviyyəsi, lakin uzun
  müddətdir aktiv deyillər.
- **Lost (R=1, F=1, M=1):** **11 müştəri** — ilkin sənədlə tam üst-üstə düşür. Bu qrup bütün
  ölçülərdə ən aşağı bal alır və reaktivasiya kampaniyası tələb edir (ilkin sənəddəki qeydlə
  uzlaşır).
- **Digər Seqmentlər:** Loyal — 17 müştəri (22.5% gəlir), New/Promising — 11 müştəri (7.4% gəlir),
  Needs Attention — 37 müştəri (ən böyük seqment, 40.2% müştəri / 29.5% gəlir). Bunlar ilkin
  sənəddə qeyd olunmayıb, lakin tam RFM matrisini tamamlamaq üçün tələb olunan seqmentlərdir.

## 3. Business Insights

1. **Champions kiçik, lakin gəlirin dörddə birini təşkil edir — klassik Pareto effekti.** Cəmi
   9 müştəri (≈10%) ümumi gəlirin 26%-ni gətirir. Bu qrupa endirim deyil, tanınma və VIP
   münasibət lazımdır (erkən giriş, eksklüziv təkliflər, referral sorğuları).

2. **At Risk seqmenti ən yüksək prioritetli win-back hədəfidir.** Orta xərcləmə Loyal seqmentinə
   yaxındır ($125K vs $133K), lakin orta 239 gündür sifariş verilməyib. Bu, yüksək dəyərli,
   lakin tezliklə tamamilə itiriləcək müştəri qrupudur — fərdiləşdirilmiş, təcili əlaqə tələb
   olunur.

3. **Needs Attention ən böyük seqmentdir (40.2% müştəri, 29.5% gəlir) və nəzərə çarpan siqnal
   göstərmir.** Ölçüsünə görə, hətta kiçik təkmilləşdirmə (targeted promosyon, engagement
   kampaniyaları) ümumi gəlirə əhəmiyyətli təsir göstərə bilər.

4. **Lost seqment (11 müştəri) üçün aşağı-xərcli, geniş reaktivasiya kampaniyası kifayətdir** —
   fərdiləşdirilmiş yanaşmaya çox resurs xərcləmək səmərəli deyil, çünki bu qrup bütün RFM
   ölçülərində ən aşağı bal alır.


