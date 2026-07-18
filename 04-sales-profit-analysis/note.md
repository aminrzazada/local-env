# Superstore Satış və Mənfəət Datası üzrə Çoxölçülü Analiz Hesabatı

## 1. Data Təmizləmə Addımları
- **Datanın Yüklənməsi:** İlkin Superstore satış datası uğurla yükləndi. Dataset tam olaraq **9,994 sətir və 21 sütundan** ibarətdir.
- **Tarix Formatının Düzəldilməsi:** `Order Date` və `Ship Date` sütunları mətn (object) tipindən standart Python `datetime` formatına uğurla çevrildi (dataset **US formatındadır — MM/DD/YYYY**, `dayfirst` parametri istifadə edilməməlidir, əks halda sətirlərin ~37%-də ay/gün yerdəyişir). İllik trendləri analiz etmək üçün bu sütundan əlavə olaraq `Order Year` parametri çıxarıldı.
- **Çatdırılma Müddətinin Hesablanması:** `Ship Date` ilə `Order Date` arasındakı fərq olaraq `days_to_ship` sütunu yaradıldı. Orta çatdırılma müddəti **3.96 gün**  təşkil edir.
- **Boş Xanaların İdarə Olunması (Missing Values):** Dataset daxilindəki bütün sütunlar üzrə boşluq yoxlanışı aparıldı. Heç bir buraxılmış və ya boş xana aşkar edilməmişdir — data tam təmizdir.
- **Dublikatların Yoxlanılması:** Dataset daxilində tam təkrarlanan sətirlər yoxlanıldı (`df.duplicated()`) və heç bir dublikat qeyd qeydə alınmadı.
- **Çoxölçülü Aqreqasiya:** Region × Category × Sub-Category üzrə Total Sales və Total Profit hesablanaraq bütün sonrakı analizlərin əsasını təşkil etdi.

## 2. Əsas Statistik Göstəricilər
- **Region Bölgüsü:** Dataset 4 regionu (Central, East, South, West) əhatə edir. Satış baxımından **West** ($725.5k) və **East** ($678.8k) aparıcı iki regiondur; **South** ($391.7k) ən aşağı satışa malikdir. Orta əməliyyat məbləği isə əksinə — **South** ($241.8) ən yüksək, **Central** ($215.8) ən aşağıdır.
- **Kateqoriya üzrə Liderlər:** Ümumi satış bazasında ən yüksək gəlir **$836.2k** ilə `Technology` kateqoriyasına məxsusdur, dərhal ardınca **$742.0k** ilə `Furniture` və **$719.0k** ilə `Office Supplies` gəlir. Lakin mənfəət marjasında mənzərə fərqlidir: `Technology` (**17.40%**) və `Office Supplies` (**17.04%**) yüksək marjalıdır, `Furniture` isə cəmi **2.49%** marja ilə əhəmiyyətli dərəcədə geri qalır.
- **Zərərli Sub-Kateqoriya:** `Tables` sub-kateqoriyası mənfi ümumi mənfəətdədir (**-$17.7k**), ardınca `Bookcases` (**-$3.5k**) və `Supplies` (**-$1.2k**) gəlir — bu üçü Furniture və Office Supplies kateqoriyalarının aşağı marjasının əsas səbəbidir.
- **Ən Gəlirli Sub-Kateqoriya:** `Copiers` — cəmi 68 sifariş (234 ədəd) ilə **$55.6k** mənfəət gətirir; ardınca `Phones` ($44.5k) və `Accessories` ($41.9k) gəlir. Bu, aşağı-həcm/yüksək-marja modelinin uğurlu nümunəsidir.
- **Endirim–Mənfəət Əlaqəsi:** Discount və Profit arasında orta səviyyəli mənfi korrelyasiya var (**r ≈ -0.22**). Endirim 0.4-dən yuxarı olan sifarişlərin orta mənfəəti **-$106.7**, endirim 0.4 və aşağı olanların isə **+$42.6** təşkil edir — yəni yüksək endirim demək olar ki, sistematik olaraq zərərə çevrilir.
- **Segment Bölgüsü:** `Consumer` seqmenti əməliyyatların əksəriyyətini (5,191 sifariş, $1.16M gəlir) təşkil edir, ardınca `Corporate` (3,020 sifariş, $706k) və `Home Office` (1,783 sifariş, $430k) gəlir.
- **Çatdırılma Metodu:** Müştərilərin **59.7%-i Standard Class**, 19.5%-i Second Class, 15.4%-i First Class, 5.4%-i isə Same Day çatdırılmadan istifadə edir.
- **İllik Gəlir:** **2017-ci il** ($733.2k) ümumi satış baxımından ən yüksək il olub, 2016-cı ilə ($609.2k) nisbətən **~20.4%** artımla. 2015-ci il isə 2014-ə görə **~2.8%** azalma göstərib — yəni 2015-2017 aralığında davamlı və sürətlənən böyümə trendi müşahidə olunur.

## 3. Biznes Anlayışları
1. **Tables Sub-Kateqoriyasında Endirim Siyasətinin Sərtləşdirilməsi:** `Tables` ən böyük zərər mənbəyidir (-$17.7k) və bu, əsasən yüksək endirimlərlə satılan sifarişlərdən qaynaqlanır. Bu sub-kateqoriya üçün maksimum icazə verilən endirim həddi (məsələn ≤20%) təyin edilməli və ya qiymət strukturuna yenidən baxılmalıdır.
2. **Endirim Səlahiyyətinin Məhdudlaşdırılması:** Endirim 0.4-dən yuxarı olan demək olar ki, bütün sifarişlər zərərlə nəticələnir (orta -$106.7). Satış nümayəndələrinə verilən endirim təyin etmə səlahiyyəti məhdudlaşdırılmalı, xüsusilə aşağı-marjalı `Furniture` kateqoriyasında əlavə təsdiq mexanizmi tətbiq edilməlidir.
3. **Furniture Kateqoriyasında Qiymətləndirmə Strategiyasının Yenidən Nəzərdən Keçirilməsi:** `Furniture` satış həcminə görə ikinci yerdə olsa da ($742k), marjası ($18.5k, ~2.5%) `Technology` və `Office Supplies`-dən qat-qat aşağıdır. Bu, əsasən `Tables` və `Bookcases`-in zərəri ilə izah olunur — bu iki sub-kateqoriya çıxarılsa, kateqoriyanın real marjası xeyli yüksəkdir.
4. **Yüksək-Marjalı Məhsullara İnvestisiyanın Artırılması:** `Copiers` kimi aşağı-həcm/yüksək-marja məhsullara investisiya (inventar, marketinq) artırıla bilər — xüsusilə ən yüksək performanslı `West` və `East` regionlarında, çünki bu regionlar həm ən yüksək satış həcminə, həm də sabit müştəri bazasına malikdir.
5. **Böyümə Trendinin Davam Etdirilməsi:** 2015-2017 aralığında sürətlənən artım (2016: +29.5%, 2017: +20.4%) müsbət siqnaldır. Lakin bu artımın hansı kateqoriya/regiondan qaynaqlandığı (xüsusilə `Technology`-nin payı) daha detallı seqmentləşdirilmiş təhlillə təsdiqlənməli və gələcək büdcə planlamasında nəzərə alınmalıdır.

