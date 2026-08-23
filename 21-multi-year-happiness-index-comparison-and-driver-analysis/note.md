# Multi-Year Happiness Index Comparison and Driver Analysis — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları
- **Datasetin Yüklənməsi:** 5 ayrı fayl — `2015.csv` (158 ölkə), `2016.csv` (157), `2017.csv`
  (155), `2018.csv` (156), `2019.csv` (156) — World Happiness Report məlumatları.
- **Sütun Adlarının Standartlaşdırılması:** hər il fərqli sütun adlandırma konvensiyası
  istifadə edib (məs. 2015/2016-da `Economy (GDP per Capita)`, 2017-də `Economy..GDP.per.Capita.`
  , 2018/2019-da sadəcə `GDP per capita`). Bütün sütunlar açar-söz axtarışı
  (`gdp`, `family`/`social support`, `health`, `freedom`, `generosity`, `trust`/`corruption`)
  ilə vahid sxemə uyğunlaşdırıldı: `Country, Region, Rank, HappinessScore, GDP, SocialSupport,
  Health, Freedom, Generosity, Corruption, Year`.
- **Year Sütununun Əlavəsi:** hər fayl birləşdirilməzdən əvvəl özünə uyğun `Year` (2015–2019)
  dəyəri ilə etiketləndi, sonra `pd.concat()` ilə vahid DataFrame-ə (782 sətir) birləşdirildi.
- **Region Boşluqlarının Doldurulması:** 2017–2019 fayllarında `Region` sütunu ümumiyyətlə
  yoxdur. 2015/2016-dan çıxarılan Country → Region xəritəsi ilə geriyə doldurma edildi; adı
  fərqli yazılan bir neçə ölkə (`Trinidad & Tobago`, `Northern Cyprus` və s.) əvvəlcə
  normallaşdırıldı. Yalnız 2 ölkə (North Macedonia, Gambia) üçün region müəyyən edilə bilmədi.
- **Boş Dəyərlərin Doldurulması:** `Corruption` sütununda 1 boş dəyər ümumi ortalama ilə
  dolduruldu.

## 2. Əsas Statistik Göstəricilər

- **2019-da ən xoşbəxt ölkə:** Finland (7.769); **ən aşağı:** South Sudan (2.853).
- **Top 10 Ölkə (2015–2019 ortalama Happiness Score):** Denmark (7.546), Norway (7.541),
  Finland (7.538), Switzerland (7.511), Iceland (7.511), Netherlands (7.405), Canada (7.351),
  Sweden (7.319), New Zealand (7.313), Australia (7.276).
- **Bottom 10 Ölkə (ortalama Happiness Score):** Burundi (3.079), Central African Republic
  (3.134), Syria (3.292), South Sudan (3.383), Rwanda (3.439), Tanzania (3.466), Afghanistan
  (3.513), Togo (3.544), Yemen (3.626), Madagascar (3.745).
- **Korrelyasiya Reytinqi (Happiness Score ilə):** GDP per Capita ən güclü əlaqəni göstərir
  (r = 0.79), ardınca Health/Life Expectancy (r = 0.74), Social Support (r = 0.65), Freedom
  (r = 0.55), Corruption (r = 0.40) və ən zəif — Generosity (r = 0.14).
- **2015→2019 arası dəyişiklik (5 ildə davamlı olan 145 ölkə üzrə):**
  - **Top 5 Yüksələn:** Benin (+1.543), Ivory Coast (+1.289), Togo (+1.246), Honduras (+1.072),
    Burkina Faso (+1.000).
  - **Top 5 Düşən:** Venezuela (−2.103), Zambia (−1.022), Zimbabwe (−0.947), Haiti (−0.921),
    Malawi (−0.882).
- **Regional Ortalama Happiness Score:** Australia and New Zealand (7.29) və North America
  (7.17) ən yüksək; Sub-Saharan Africa (4.19) və Southern Asia (4.58) ən aşağı — təxminən 3
  bal fərq (0–10 şkalasında).
- **Bonus — Yüksək Generosity, Aşağı Happiness (outlier):** 41 sətir bu kateqoriyaya düşür;
  Myanmar (2015–2019 arası hər ildə), Syria, Haiti və Sri Lanka təkrarlanan nümunələrdir —
  yüksək coməiyyətçilik xoşbəxtlik reytinqini yüksəltmir.

## 3. Business/Policy Insights

1. **GDP per capita xoşbəxtliyin ən güclü proqnozlaşdırıcısıdır (r = 0.79), Health (r = 0.74)
   və Social Support (r = 0.65) onu izləyir.** İqtisadi rifah və sağlamlıq göstəriciləri
   "yumşaq" amillərdən (Freedom, Corruption, Generosity) daha çox çəkiyə malikdir — milli
   xoşbəxtlik siyasəti formalaşdırarkən əsas diqqət iqtisadi böyümə və sağlamlıq
   infrastrukturuna yönəldilməlidir.

2. **Generosity ilə Happiness Score arasında demək olar heç bir xətti əlaqə yoxdur (r = 0.14),
   halbuki Myanmar, Syria, Haiti kimi ölkələr yüksək comərdlik göstərsə də xoşbəxtlik
   reytinqində aşağı yerlərdədir.** Bu, comərdliyin mədəni/dini amillərdən qaynaqlandığını,
   lakin milli rifahın göstəricisi olmadığını təsdiqləyir — comərdlik təkbaşına siyasət aləti
   kimi istifadə edilməməlidir.

3. **2015→2019 arası ən böyük dəyişikliklər tədricən deyil, kəskin sosial-iqtisadi
   şoklardan qaynaqlanır.** Venezuela-nın çöküşü (−2.10) digər bütün düşənlərdən qat-qat
   böyükdür və davam edən humanitar/iqtisadi böhranı əks etdirir; Benin (+1.54) və Ivory
   Coast (+1.29) kimi ölkələrdə isə sürətli sabitləşmə/bərpa müşahidə olunur. Bu, xoşbəxtlik
   indeksinin qısamüddətli siyasi/iqtisadi hadisələrə həssas olduğunu göstərir.

4. **Regional fərqlər çox böyükdür və sabitdir (Australia/NZ 7.29 vs Sub-Saharan Africa
   4.19) — təxminən 3 bal fərq beş il ərzində əsasən dəyişməz qalıb.** Bu, qlobal xoşbəxtlik
   bərabərsizliyinin struktur xarakterli olduğunu və qısamüddətli müdaxilələrlə asanlıqla
   aradan qaldırıla bilməyəcəyini göstərir.

5. **India və oxşar inkişaf etməkdə olan bazarlarda olduğu kimi, TV Show/Movie strategiyası
   analogiyasında — burada da "bir ölçü hamıya uyğun deyil" prinsipi keçərlidir:** hər regionun
   öz unikal GDP-Happiness münasibəti var (bax: bonus scatter plot), ona görə də qlobal siyasət
   tövsiyələri regional kontekstə uyğunlaşdırılmalıdır.
