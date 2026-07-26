# Seasonal Patterns and Cancellation Analysis in Hotel Booking Data — Hesabat

## 1. Data Təmizləmə Addımları
- **Datanın Yüklənməsi:**  Dataset **119,390 sətir və 32 sütundan** ibarətdir.
- **Missing Value İdarəsi:** `company` sütununda **112,593 (94.31%)**, `agent` sütununda **16,340 (13.69%)**, `country` sütununda **488 (0.41%)** və `children` sütununda **4** boşluq aşkar edildi. Tapşırıqda göstərilən qayda üzrə: `country` → **"Unknown"**, `agent` və `company` → **0** ilə dolduruldu (0 dəyəri "agent/company vasitəsilə edilməyib" mənasını verir); `children`-dəki 4 sətir əhəmiyyətsiz say olduğu üçün 0 ilə dolduruldu.
- **arrival_date Sütununun Yaradılması:** `arrival_date_year`, `arrival_date_month` (mətn formatında ay adı) və `arrival_date_day_of_month` sütunları birləşdirilərək tam `datetime` formatında `arrival_date` sütunu yaradıldı (`%Y-%B-%d` formatı ilə parse edildi). Ayrıca `arrival_date_month` sütunu xronoloji sıralama üçün ordered categorical tipinə çevrildi.
- **Digər Qeydlər:** Datasetdə `is_canceled` (0/1) sütunu birbaşa iptal indikatoru kimi mövcuddur, ayrıca ləğv qeydlərinin ayrılmasına ehtiyac olmadı.

## 2. Əsas Statistik Göstəricilər

- **İptal Nisbəti (Hotel üzrə):** `City Hotel` — **41.7%**, `Resort Hotel` — **27.8%**. City Hotel-in iptal nisbəti Resort Hotel-dən əhəmiyyətli dərəcədə yüksəkdir.
- **Pivot Cədvəl (Ay × Hotel Tipi):** Ən yüksək tələbat ayı **Avqust** (**13,877 rezervasiya**: City 8,983 + Resort 4,894), ən aşağı isə **Yanvar** (**5,929 rezervasiya**: City 3,736 + Resort 2,193). **Qeyd:** ilkin sənəddəki "Resort Hotel yayda City Hotel-i üstələyir" fərziyyəsi təsdiqlənmədi — pivot cədvələ görə **City Hotel bütün 12 ayda, o cümlədən yay aylarında da (İyun-Avqust) Resort Hotel-dən daha çox rezervasiyaya malikdir** (məs. Avqust: City 8,983 vs Resort 4,894).
- **Aylıq İptal Nisbəti:** Hər iki hotel tipində iptal nisbəti ilin ortasında (Aprel-İyun) və Sentyabr-Oktyabr aylarında pikə çatır; City Hotel üçün ən yüksək **Aprel (46.3%)**, Resort Hotel üçün isə **İyun (33.1%)**.
- **Lead Time Müqayisəsi (İptal vs Aktiv):** İptal edilmiş rezervasiyaların orta gözləmə müddəti **144.8 gün** (median 113), aktiv qalan rezervasiyalarınkı isə **80.0 gün** (median 45) — fərq statistik olaraq böyükdür və trendi təsdiqləyir: uzun lead_time ilə gələn rezervasiyaların iptal ehtimalı əhəmiyyətli dərəcədə yüksəkdir. Hotel tipi üzrə ümumi orta lead_time: **City Hotel 109.7 gün, Resort Hotel 92.7 gün**.
- **Top 10 Ölkə (rezervasiya sayına görə):** **Portuqaliya (PRT)** açıq liderdir — **48,590 rezervasiya**, iptal nisbəti isə **56.6%** (ortalamadan xeyli yüksək). Ardınca `GBR` (12,129, iptal 20.2%), `FRA` (10,415, iptal 18.6%), `ESP` (8,568, iptal 25.4%), `DEU` (7,287, iptal 16.7%). **Qeyd:** ilkin sənəddə PRT üçün göstərilən "28.2k" rəqəmi faktiki nəticə ilə (48.6k) üst-üstə düşmür — bu, real hesablama əsasında düzəldilib.
- **Market Segment üzrə Bölgü:** `Online TA` ən böyük seqmentdir — rezervasiyaların **47.3%-i** (56,477 ədəd), iptal nisbəti **36.7%**. Ardınca `Offline TA/TO` (20.3%, iptal 34.3%), `Groups` (16.6%, iptal **61.1%** — ən yüksək), `Direct` (10.6%, iptal 15.3%), `Corporate` (4.4%, iptal 18.7%).
- **Ən Aşağı İptal Nisbətinə Malik Seqment (Bonus):** `Complementary` seqmenti ən aşağı iptal nisbətinə malikdir — **13.1%** (cəmi 743 rezervasiya, əsasən pulsuz/promo tipli sifarişlər). Əsas seqmentlər arasında isə `Direct` (15.3%) ən sabit seqmentdir.
- **ADR (Bonus, Ay × Hotel):** Resort Hotel-in ADR-i yay aylarında (İyun-Avqust) kəskin artır və Avqustda **~186.8 EUR**-a çatır, City Hotel-in ADR-i isə nisbətən sabit qalır və May-da **~121.6 EUR** pikinə çatır. Bu, Resort Hotel-in mövsümi qiymətləndirmə strategiyasının daha aqressiv olduğunu göstərir.
- **Müştəri Tipi (əlavə qeyd):** `customer_type` sütununa görə **"Transient"** seqmenti rezervasiyaların **75.1%-ni** təşkil edir (ilkin sənəddəki rəqəmlə üst-üstə düşür) — lakin bu, `market_segment` deyil, ayrı `customer_type` sütunudur; tapşırıqda tələb olunan `market_segment` bölgüsündə ən böyük seqment `Online TA`-dır.

## 3. Business Insights

1. **City Hotel üçün daha sərt ləğv/depozit siyasəti tətbiq edilməlidir.** İptal nisbəti (41.7%) Resort Hotel-dən (27.8%) əhəmiyyətli dərəcədə yüksəkdir. Uzaq tarixli (uzun lead_time) City Hotel rezervasiyaları üçün qabaqcadan ödəniş və ya qeyri-geri-qaytarılan tarif seçimləri tətbiq edilməsi gəlir itkisini azalda bilər.

2. **Avqust pik tələbat ayıdır, Yanvar isə ən sakit — qiymətləndirmə strategiyası buna uyğunlaşdırılmalıdır.** Yay aylarında (xüsusən Resort Hotel-də) ADR artımı davam etdirilməli, eyni zamanda yüksək tələbat mövsümündə gözlənilən iptal faizini kompensasiya etmək üçün overbooking buferi nəzərdən keçirilməlidir.

3. **Uzun lead_time ilə gələn rezervasiyalar daha çox iptal riski daşıyır (144.8 gün vs 80.0 gün).** Rezervasiya tarixi ilə gəliş tarixi arasındakı fasilə artdıqca, dinamik depozit tələbləri (məs. 90+ gün əvvəldən edilən sifarişlər üçün icbari qabaqcadan ödəniş) tətbiq edilməsi spekulyativ/az-öhdəlikli rezervasiyaların sayını azalda bilər.

4. **Online TA seqmenti ən böyük həcmi gətirir, lakin həm də yüksək iptal riski daşıyır; Groups seqmenti isə ən yüksək iptal nisbətinə (61.1%) malikdir.** Gəlir idarəetməsi Direct və Corporate kanallarına  investisiya yönəltməli — sadiqlik proqramları və birbaşa rezervasiya təşviqləri vasitəsilə tələbatı yüksək-iptal OTA/Groups kanallarından uzaqlaşdırmaq mümkündür.


