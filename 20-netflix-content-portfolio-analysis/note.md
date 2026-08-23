# Netflix Content Portfolio Analysis and Strategy Insights — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları
- **Datasetin Yüklənməsi:** `netflix_titles.csv` — 8,807 title, 12 sütun.
- **Boş Dəyərlərin Doldurulması:** `director` (29.9% boş), `cast` (9.4% boş), `country` (9.4%
  boş) sütunları `"Unknown"` ilə dolduruldu.
- **Tarix Parse Edilməsi:** `date_added` (bəzi sətirlərdə əvvəlində boşluq olan string) `.strip()`
  ilə təmizlənib datetime-a çevrildi; `year_added` və `month_added` çıxarıldı.
- **Janr Exploziyası:** `listed_in` sütunu vergüllə bölünüb (`.split(', ')`) və `.explode()` ilə
  hər title-ın hər janrı ayrıca sətirə çevrildi ki, həqiqi janr tezliyi hesablansın.
- **TV Show Mövsüm Sayı Çıxarılması:** `duration` sütunundan (`"3 Seasons"` formatı) regex ilə
  rəqəm hissəsi çıxarıldı.

## 2. Əsas Statistik Göstəricilər

- **Type Paylanması:** 6,131 Movie (69.6%), 2,676 TV Show (30.4%).
- **İllik Pik:** ən çox məzmun 2019-cu ildə əlavə edilib (2,016 title).
- **Strategiya Dəyişikliyi (year_added × type, %):** 2014-cü ildə Movie payı ~79%, TV Show payı
  2016-cı ildə öz pikinə çatır (41.0%), sonra 2017-2020 aralığında ~25-31% arasında dalğalanır,
  2021-də isə 33.7%-ə yüksəlir — trend ümumilikdə TV Show-un payının artması istiqamətindədir,
  lakin xətti deyil.
- **Top 3 Janr:** International Movies (2,752), Dramas (2,427), Comedies (1,674).
- **Top 3 Ölkə:** United States (2,818), India (972), United Kingdom (419).
- **Ölkə üzrə Type Bölgüsü:** India məzmununun 92%-i (893/972) Movie-dir; Yaponiya isə əksinə
  TV Show-a meyillidir (169 TV Show vs 76 Movie).
- **TV Show Mövsüm Paylanması:** 1 mövsümlü TV Show payı 67.0%-dir (1,793/2,676) — ən çox
  görülən qrup.
- **Content Rating:** TV-MA ən çox istifadə olunan reytinq kateqoriyasıdır (3,207 title, həm
  Movie həm TV Show arasında #1).
- **Ay üzrə Əlavələr:** ən çox məzmun İyul (827) və Dekabr (813) aylarında əlavə edilib —
  bunlar ilin ən yüksək iki ayıdır.
- **Bonus — Ölkə üzrə Orta Release-Gap:** Egypt (10.49 il) və India (6.76 il) ən böyük gap-ə
  malikdir (əsasən arxiv məzmunu); Spain (0.72 il) və South Korea (1.70 il) ən kiçik gap-ə
  malikdir (əsasən təzə məzmun).
- **Bonus — Top 5 Rejissor:** Rajiv Chilaka (19 title, əsasən Children & Family Movies), Raúl
  Campos/Jan Suter (18 title, demək olar tamamilə Stand-Up Comedy), Suhas Kadav (16 title),
  Marcus Raboy (16 title, Stand-Up Comedy), Jay Karas (14 title, Stand-Up Comedy).

## 3. Business Insights

1. **TV Show payı zaman keçdikcə ümumilikdə artıb (2014-də ~79% Movie-dən 2021-də ~34% TV Show-a
   qədər), lakin trend xətti deyil — 2016-cı ildə pik (41%) var, sonra qismən geriləmə.** Bu,
   Netflix-in TV Show strategiyasının sabit xətti artım deyil, sınaq-və-tənzimləmə prosesi
   olduğunu göstərir.

2. **TV Show-ların 67%-i cəmi 1 mövsümlüdür — bu, "limited series" və ya erkən ləğv edilmə
   nümunəsini göstərir.** Yeni TV Show sifariş edərkən, uzunmüddətli sadiqlik yaratmaq üçün 2-ci
   mövsümə davam etmə strategiyası (yalnız pilot uğuruna əsaslanmadan) nəzərdən keçirilə bilər.

3. **India məzmununun 92%-i Movie-dir, halbuki Yaponiya TV Show-a daha meyillidir.** Bu,
   regional istehlak vərdişlərinə uyğun məzmun növü strategiyasının artıq mövcud olduğunu
   göstərir — hər bazar üçün "bir ölçü hamıya uyğun deyil" yanaşması effektivdir.

4. **Egypt və India üçün Netflix əsasən arxiv məzmunu (orta 10.5 və 6.8 il köhnə) əlavə edir,
   Spain və South Korea üçün isə demək olar yeni buraxılan məzmun (orta <2 il).** Bu fərq,
   Netflix-in bəzi bazarlarda güclü orijinal istehsal/sürətli lisenziyalaşdırma investisiyası
   etdiyini, digərlərində isə mövcud kataloqu doldurmaqla kifayətləndiyini göstərir.

5. **TV-MA reytinqi ən çox istifadə olunan kateqoriyadır (3,207 title, ~36% bütün kataloqdan) —
   kataloq əsasən yetkin auditoriyaya yönəlib.** Ailə-yönümlü məzmun (TV-Y, TV-G) nisbətən azdır
   — əgər Netflix daha gənc/ailə seqmentini genişləndirmək istəyirsə, bu, mövcud boşluğu göstərən
   bir fürsətdir.


