# Revenue Analysis and Customer-Level Aggregation on UK E-Commerce Data — Hesabat

## 1. Data Təmizləmə Addımları
- **Datanın Yüklənməsi:** İlkin UK e-ticarət transaksiya datası  uğurla yükləndi. Dataset tam olaraq **541,909 sətir və 8 sütundan** ibarətdir (Dekabr 2010 – Dekabr 2011 dövrü).
- **Missing Value İdarəsi:** `Description` sütununda 1,454, `CustomerID` sütununda **135,080** boşluq aşkar edildi. `CustomerID` boş olan sətirlər yalnız müştəri-səviyyəli aqreqasiya üçün çıxarıldı — ölkə/gəlir analizlərinə daxil edilməyə davam etdi (çünki `Country` və `Revenue` məlumatı mövcuddur).
- **Legv Edilmiş Sifarişlərin Ayrılması:** `InvoiceNo` "C" ilə başlayan **9,288 sətir (3,836 unikal invoice)** ləğv edilmiş sifariş kimi ayrıca saxlanıldı və return-rate analizi üçün istifadə edildi, gəlir hesablamalarına daxil edilmədi.
- **Mənfi Quantity Sətirləri:** Ləğv olunmamış sətirlər arasında **1,336 mənfi Quantity qeydi** aşkar edildi — bunların hamısında `UnitPrice = 0` və `Description` boşdur, real satış/iadə deyil, stok düzəliş qeydləridir; gəlir hesablamasına təsiri yoxdur (0 qiymət), lakin miqdar əsaslı analizlərdən çıxarıldı.
- **Gizli Ləğv Cütlərinin Aşkarlanması (əlavə keyfiyyət yoxlanışı):** Bəzi sifarişlər "C"-prefiksli cavab olmadan, **tamamilə ayrı invoice nömrəsi ilə** ləğv olunur (məs. sifariş #581483 ilə 80,995 ədəd alınır, sonra tamamilə fərqli #C581484 invoysu ilə eyni miqdar geri qaytarılır). Bu cür qeydlər sadə "C" filtrindən keçmir və süni şəkildə şişirdilmiş satış rəqəmləri yaradır. Statistik ekstremal (Quantity > 99.9-cu percentil, ~480 ədəd) sətirlər arasında tam uyğun (CustomerID + StockCode + UnitPrice + miqdar) ləğv qeydi olan **26 sətir** aşkarlanıb çıxarıldı — bunlardan biri "PAPER CRAFT, LITTLE BIRDIE" məhsuluna aid 80,995 ədədlik tək sifariş idi, digəri isə müştəri #12346-nın 74,215 ədədlik sifarişi idi (hər ikisi tam geri qaytarılmışdı).
- **Revenue Sütununun Yaradılması:** `Revenue = Quantity × UnitPrice` düsturu ilə hesablanıb. Təmizlənmiş dataset (`df_clean`, revenue/məhsul analizi üçün) **530,078 sətirdən** ibarətdir, ümumi gəlir **£10,372,228.75**.

## 2. Əsas Statistik Göstəricilər

- **Ölkə üzrə Bazar Payı:** `United Kingdom` gəlirin **84.17%-ni** təşkil edir (18,015 invoice); ardınca `Netherlands` (2.75%), `EIRE` (2.73%), `Germany` (2.21%) və `France` (2.02%) gəlir.
- **Müştəri-Səviyyəli Göstəricilər:** 397,858 sətir keçərli `CustomerID`-yə malikdir. Ən yüksək gəlirli müştəri **#14646** — £280,206.02 (73 sifariş), ardınca **#18102** (£259,657.30, 60 sifariş) gəlir. Bütün müştərilər üzrə orta sifariş dəyəri **£380.59**.
- **Top 10 Müştəri Payı:** Ən yüksək gəlirli 10 müştəri ümumi gəlirin **~16.3%-ni** təşkil edir — bu, B2B/topdansatış xarakterli bir neçə iri müştərinin mövcudluğuna işarədir.
- **Aylıq Gəlir Trendi:** **Noyabr 2011** ən yüksək gəlir ayıdır (**£1,509,496.33**) — Milad mövsümünə hazırlıq effekti aydın görünür (Sentyabr-Noyabr davamlı artım), Dekabrda isə kəskin azalma (bayram qabağı sifarişlərin artıq tamamlanması).
- **Top Məhsullar (miqdara görə, təmizlənmiş):** `WORLD WAR 2 GLIDERS ASSTD DESIGNS` (55,047 ədəd) və `JUMBO BAG RED RETROSPOT` (48,474 ədəd) liderdir. `WHITE HANGING HEART T-LIGHT HOLDER` (35,461 ədəd) — brendin ən tanınan məhsulu — 6-cı yerdədir.
- **Return Rate (Ölkə üzrə, minimum 20 sifariş):** `Japan` (32.1%), `Italy` (30.9%) və `Switzerland` (27.0%) ən yüksək ləğv nisbətinə malikdir. `United Kingdom` isə həcminin böyüklüyünə baxmayaraq nisbətən aşağı səviyyədədir (14.35%).
- **Basket Size (Bonus):** `RSA` (57.0), `Lebanon` (45.0) və `Cyprus` (37.4) ən yüksək orta unikal-məhsul-sayı/invoice göstəricisinə malikdir — bu bazarlarda sifarişlər çox güman ki topdansatış xarakterlidir.

## 3. Business Insights 

1. **UK bazarı praktiki olaraq təkbaşına biznesdir (~84% gəlir).** Qalan bütün beynəlxalq bazarlar hər biri 3%-dən az paya malikdir. Beynəlxalq genişlənmə üçün resurs planlaması bu reallığı nəzərə almalıdır — hazırkı halda kənar bazarlar "niche" statusundadır.

2. **Noyabr ayı ilin ən güclü satış dövrüdür — Milad hazırlıq effekti aydın görünür.** Gəlir Sentyabrdan başlayaraq davamlı artır və Noyabrda pikə çatır, Dekabrda isə kəskin azalır. İnventar və marketinq büdcəsi Sentyabr-Noyabr dövrünə əvvəlcədən yönəldilməlidir ki, pik tələbat qarşılana bilsin.

3. **Data keyfiyyəti xüsusi diqqət tələb edir — gizli ləğv cütləri metrikalari təhrif edə bilər.** Fərqli invoice nömrələri ilə edilən tam-ləğv əməliyyatları (məs. 80,995 ədədlik "PAPER CRAFT" sifarişi) sadə "C-prefiks" filtri ilə aşkarlanmır və top-məhsul/top-müştəri siyahılarını ciddi şəkildə təhrif edə bilər. Analitika komandası mütəmadi olaraq bu cür uyğunsuz cütləri yoxlamalı, hesabat pipeline-ına daimi keyfiyyət yoxlanışı əlavə edilməlidir.

4. **Return rate ölkələr arasında kəskin fərqlənir — Yaponiya, İtaliya və İsveçrə "high-return" bazarlarıdır (>27%).** Bu ölkələrdə məhsul təsviri dəqiqliyi, ölçü/uyğunluq məlumatı və çatdırılma keyfiyyəti araşdırılmalıdır. Yüksək ləğv nisbəti uzunmüddətli perspektivdə logistika xərclərini artırır və müştəri məmnuniyyətinə mənfi təsir edir.

5. **Top 10 müştəri gəlirin 16%-ni təşkil edir — bu, sabit lakin diqqət tələb edən konsentrasiyadır.** Bu iri müştərilər (çox güman ki kiçik pərakəndə satıcılar /topdansatış tərəfdaşları) üçün fərdi hesab meneceri təyinatı və sadiqlik proqramları gəlir sabitliyini artıra bilər, eyni zamanda bu qrupun itirilməsi riskindən qorunmaq üçün diversifikasiya strategiyası da nəzərdən keçirilməlidir.




