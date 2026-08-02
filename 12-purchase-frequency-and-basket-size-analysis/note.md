# Purchase Frequency and Basket Size Analysis — Hesabat

## 1. Data Təmizləmə Addımları
- **Datasetin Yüklənməsi:** `Groceries_dataset.csv` (38,765 sətir, 3 sütun: `Member_number`,
  `Date`, `itemDescription`) yükləndi, `Date` `DD-MM-YYYY` formatından datetime-a çevrildi.
  
- **Unikal Müştəri/Məhsul Sayı:** **3,898 unikal müştəri** (`Member_number`), **167 unikal
  məhsul** (`itemDescription`). 
- **Basket Konstruksiyası:** basket unikal `(Member_number, Date)` kombinasiyası kimi təyin
  edildi; basket ölçüsü həmin gündə alınan məhsul sətirlərinin sayıdır.
- **Ziyarət Tezliyi:** hər müştəri üçün unikal alış-veriş günü sayı hesablanıb, 4 qrupa
  bölündü: 1 ziyarət, 2-5, 6-10, 10+.
- **Co-occurrence Hesablaması (Bonus):** hər basket-dəki unikal məhsulların bütün cüt
  kombinasiyaları (`itertools.combinations`) sayılaraq ən çox birgə alınan cütlər tapıldı.

## 2. Əsas Statistik Göstəricilər

- **Unikal Basket Sayı:** **14,963**. **Qeyd:** ilkin sənəddəki "9835 unikal basket" rəqəmi
  faktiki nəticə ilə **üst-üstə düşmür** — mümkün səbəb, ilkin sənədin basket tərifini fərqli
  tətbiq etməsidir (məs. məhsul sətirlərinin əvvəlcə deduplikasiya edilməsi).
- **Orta Basket Ölçüsü:** **2.59 məhsul**, maksimum **11 məhsul**. **Qeyd:** ilkin sənəddəki
  "orta 3.95, maksimum 14" rəqəmləri faktiki nəticə ilə **üst-üstə düşmür** — eyni metodologiya
  fərqindən qaynaqlanan ümumi bir kənarlaşma nümunəsidir.
- **Basket Ölçüsü = 1 (İmpuls Alışlar):** real datada **0%** — minimum basket ölçüsü 2-dir, heç
  bir tək-məhsullu basket yoxdur. **Qeyd:** ilkin sənəddəki "basket size 1 = 17%" iddiası faktiki
  nəticə ilə **tam üst-üstə düşmür** — bu, ən böyük fərqlərdən biridir, yəqin ki, fərqli dataset
  snapshot-u və ya basket tərifi ilə bağlıdır.
- **Top 3 Məhsul:** whole milk (**2,502**), other vegetables (**1,898**), rolls/buns
  (**1,716**). **Qeyd:** ilkin sənəddəki "whole milk 2513, other vegetables 1903, rolls/buns
  1809" rəqəmlərinə **çox yaxındır** (0.4-5% fərq, sıralama tam eynidir) — praktiki olaraq
  təsdiqləndi.
- **Ziyarət Tezliyi Seqmentləri:** 1 ziyarət = **8.95%** (349 müştəri), 2-5 ziyarət = **72.32%**
  (2,819 müştəri), 6-10 ziyarət = **18.60%** (725 müştəri), 10+ ziyarət = **0.13%** (5 müştəri).
  **Qeyd:** ilkin sənəddəki "müştərilərin 68%-i ayda 1 dəfədən az alış-veriş edir" iddiasını
  dəqiq eyni metodologiya ilə yenidən yaratmaq mümkün olmadı (aylıq tezlik tərifi ilkin sənəddə
  göstərilməyib) — alternativ hesablama üsulları 70-90% aralığında nəticə verir, istiqamət
  eynidir (əksər müştəri nadir alış-veriş edir), lakin dəqiq rəqəm doğrulanmadı.
- **Həftənin Günü:** **Cümə axşamı (Thursday)** ən çox tranzaksiyaya malikdir (5,620 sətir,
  2,188 unikal basket).  Orta basket ölçüsü həftənin bütün günlərində sabitdir
  (2.57-2.61 aralığında) — gün basket ölçüsünə deyil, yalnız ümumi tranzaksiya sayına təsir edir.
- **Top 10 Müştəri Konsentrasiyası:** ümumi basket-lərin cəmi **0.70%**-ni təşkil edir. **Qeyd:**
  ilkin sənəddəki "top 10 müştəri 3.8%-ni təşkil edir" rəqəmi ilə **üst-üstə düşmür**, lakin əsas
  nəticə (**konsentrasiya azdır**) hətta daha güclü şəkildə təsdiqlənir.
- **Co-occurrence (Bonus):** ən çox birgə alınan cüt **"other vegetables" + "whole milk"**
  (222 basket), ardınca **"rolls/buns" + "whole milk"** (209) və **"soda" + "whole milk"** (174).

## 3. Business Insights (Mağaza Layout və İnventar Tövsiyələri)

1. **`whole milk` mərkəzi/anchor məhsuldur — mağaza layout-unda strateji yerləşdirilməlidir.**
   Ən çox alınan məhsul olmaqla yanaşı, demək olar bütün top co-occurrence cütlərində iştirak
   edir. Onu mağazanın arxasında/mərkəzində yerləşdirmək müştəriləri digər məhsullar arasından
   keçirər və cross-sell imkanını artırar.

2. **`other vegetables` + `whole milk` ən güclü kombinasiyadır — birgə promosyon nəzərdən
   keçirilməlidir.** Bu iki məhsul 222 basket-də birlikdə görünür, ən yüksək co-occurrence
   göstəricisidir — "bundle" endirimi və ya rəf yaxınlığı satışları artıra bilər.

3. **Cümə axşamı ən yoğun gündür — inventar və işçi qüvvəsi bu günə uyğun planlaşdırılmalıdır.**
   Tranzaksiya sayı digər günlərdən nəzərəçarpacaq dərəcədə yüksəkdir; digər tərəfdən orta basket
   ölçüsü həftə boyu sabit qalır — fərq alış sayında, alış həcmində deyil.

4. **Müştəri bazası geniş diversifikasiya olunub — sadiqlik proqramı bütün seqmentlərə
   hədəflənməlidir, tək bir VIP qrupuna deyil.** Top 10 müştəri ümumi basket-lərin cəmi 0.7%-ni
   təşkil edir və müştərilərin əksəriyyəti (72.3%) 2-5 dəfə ziyarət edib — sadiqlik proqramı
   yüksək dəyərli az sayda müştəriyə deyil, orta-tezlikli geniş bazaya yönəldilməlidir.
