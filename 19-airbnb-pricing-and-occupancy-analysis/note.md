# Airbnb Pricing and Occupancy Analytics — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları
- **`listings.csv` Yüklənməsi:** 3,818 elan, 92 sütun.
- **`price` Təmizlənməsi:** `"$85.00"` formatındakı string `$`/`,` simvolları silinərək float-a
  çevrildi.
- **`calendar.csv` Yüklənməsi:** 1,393,570 sətir.
- **`date`/`available` Çevrilməsi:** `date` datetime-a, `available` (`"t"`/`"f"`) binar (`1`/`0`)
  formata çevrildi.
- **`price` (calendar) Boş Dəyər Doldurulması:** `available="f"` günlərdə `price` təbii olaraq
  boşdur (bütün sətirlərin 32.9%-i) — hər elanın öz mövcud qiymətlərinin medianı ilə dolduruldu.
- **Occupancy Rate Hesablanması:** hər `listing_id` üçün `available=False` günlərin faizi
  hesablanıb `listings`-ə `id` üzrə merge edildi.

## 2. Əsas Statistik Göstəricilər

- **Ən Bahalı Məhəllə:** Briarcliff (median $173.5/gecə, 14 elan)
- **Ən Ucuz Məhəllə:** Olympic Hills (median $55.5/gecə, 16 elan)
- **room_type Paylanması:** Entire home/apt 66.55%, Private room 30.38%, Shared room 3.06%
- **Ən Yüksək Occupancy-li Məhəllə:** South Lake Union (orta 54.67%, 27 elan)
- **Capitol Hill (neighbourhood_group) Occupancy-i:** orta 35.90%
- **Mövsümi Qiymət Fərqi:** yay (İyun-Avqust) orta qiyməti qışdan (Dek-Yan-Fev) ~10.06% yüksəkdir
  ($113.00 vs $102.67)
- **minimum_nights ↔ price Korrelyasiyası:** r = 0.017 (praktiki olaraq sıfıra yaxın)
- **number_of_reviews ↔ occupancy_rate Korrelyasiyası:** r = -0.094 (zəif, mənfi istiqamətdə)
- **Instant Bookable Müqayisəsi:** qiymət fərqi -1.00%, occupancy fərqi -1.09% (hər ikisi
  praktiki olaraq əhəmiyyətsiz)
- **Bonus — Gəlir Potensialı üzrə Ən Yaxşı Məhəllə:** Montlake (~$37,740/il orta gəlir
  potensialı) — bu, ən yüksək qiymətli məhəllə (Briarcliff) ilə eyni deyil; qiymət və
  occupancy-nin kombinasiyası fərqli nəticə verir

## 3. Business Insights

1. **Ən yüksək gəlir potensiallı məhəllə (Montlake) ən bahalı məhəllə (Briarcliff) ilə eyni
   deyil — qiymətə görə deyil, gəlir potensialına görə investisiya qərarı verilməlidir.**
   Montlake orta qiymət və orta-yüksək occupancy-ni birləşdirərək ən yüksək illik gəlir
   potensialını verir — sırf "ən bahalı məhəllədə mülk al" strategiyası səhv nəticələrə apara bilər.

2. **Review sayı occupancy ilə praktiki olaraq əlaqəli deyil (r=-0.09).** Bu, "çox review = çox
   rezervasiya" fərziyyəsini dəstəkləmir — host-lar review toplamağa deyil, birbaşa qiymət
   strategiyası, təqvim idarəetməsi və elan keyfiyyətinə fokuslanmalıdır.

3. **Instant bookable statusu nə qiymətə, nə occupancy-ə əhəmiyyətli təsir göstərmir (hər ikisi
   ~-1%).** Host-lar instant booking-i "aşağı qiymət, yüksək doluluq" mübadiləsi kimi
   düşünməməlidir — bu funksiya əsasən rahatlıq təmin edir, gəlirlilik strategiyası deyil.

4. **Mövsümi qiymət dəyişkənliyi mövcuddur, lakin mötədildir (yay qışdan ~10% baha).**
   Seattle-də mövsümi qiymətləndirmə strategiyası real, lakin ölçülü effektə malikdir — çox
   kəskin mövsümi qiymət artımı real bazar davranışını əks etdirməyə bilər və rezervasiyaları
   itirə bilər.

5. **South Lake Union ən yüksək orta occupancy-ə (54.7%) malikdir, Capitol Hill isə orta sırada
   (35.9%) yerləşir.** Sabit, davamlı rezervasiya axını istəyən investorlar üçün South Lake
   Union kimi yüksək-occupancy məhəllələr Capitol Hill kimi məşhur, lakin nisbətən aşağı-occupancy
   məhəllələrdən daha etibarlı seçim ola bilər.


