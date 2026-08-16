# E-Commerce Funnel Analysis — View to Purchase — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları
- **Faylların Yüklənməsi və Birləşdirilməsi:** `2020-Jan.csv` və `2020-Feb.csv` yükləndi və
  `pd.concat` ilə birləşdirildi. Faktiki ölçü: Yanvar **4,264,752 sətir**, Fevral **4,156,682
  sətir** (cəmi 8,421,434 sətir). **Qeyd:** ilkin sənəddəki "~2.6M / ~1.7M sətir" təxmini faktiki
  ölçüdən əhəmiyyətli dərəcədə fərqlidir (1.6-2.4x daha çoxdur) — bu, ilkin qiymətləndirmənin
  dataset-in fərqli/kiçik bir snapshot-una əsaslanmasından qaynaqlana bilər. Yüklənmiş fayllar
  tam və korrupsiyasız olaraq yoxlanıldı (əvvəl/son sətirlər və ortadan nümunə əl ilə təsdiqləndi).
- **Event Type Araşdırılması:** faktiki datada **4 event tipi** aşkarlandı: `view` (47.4%),
  `cart` (27.3%), `remove_from_cart` (19.3%), `purchase` (6.0%). İlkin sənəddə yalnız 3 tip
  (view 85.1%/cart 8.7%/purchase 6.2%) qeyd olunub — həm say, həm faiz baxımından fərqlidir.
  `remove_from_cart` tapşırıq spesifikasiyasına uyğun olaraq funnel qurulmasında istifadə
  edilmədi.
- **Funnel Konstruksiyası:** hər mərhələdə **unikal `user_id`** sayıldı (event sayı deyil):
  View (ən azı 1 baxış), Cart (ən azı 1 səbətə əlavə), Purchase (ən azı 1 alış).
- **Kateqoriya Data Keyfiyyəti:** `category_code` sütununun **98.2%**-i boşdur — kateqoriya
  breakdown yalnız dolu dəyərlər üzərində aparıldı.
- **Qiymət Data Keyfiyyəti:** purchase event-lərində **71 sətirdə mənfi qiymət** aşkarlandı
  (min: -$79.37) — çox güman ki, geri qaytarma (refund) əməliyyatlarını əks etdirir.

## 2. Əsas Statistik Göstəricilər

- **Funnel Ölçüləri:** View = **716,987** unikal istifadəçi, Cart = **162,035**, Purchase =
  **49,473**.
- **Conversion Rate-lər:** View→Cart = **19.94%**, Cart→Purchase = **30.21%**, overall
  View→Purchase = **6.54%**. **Qeyd:** ilkin sənəddəki "View→Cart 10.2%, Cart→Purchase 71.4%,
  overall 7.3%" rəqəmləri ilə **üst-üstə düşmür** — ən böyük fərq Cart→Purchase-dədir (71.4%
  vs 30.21%), yalnız overall rəqəm (7.3% vs 6.54%) nisbətən yaxındır.
- **Ən Yüksək Conversion-lu Kateqoriya:** **`apparel.glove` (18.43%)**. **Qeyd:** ilkin
  sənəddəki "accessories (12.1%)" iddiası ilə **üst-üstə düşmür** — faktiki datada
  `accessories.bag` kateqoriyasının conversion dərəcəsi cəmi **1.21%**-dir və top-8 arasında ən
  aşağı performans göstərən kateqoriyalardan biridir.
- **Yanvar vs Fevral:** overall conversion Yanvarda **6.65%**, Fevralda **6.38%** —
  **-0.27pp dəyişiklik**. **Qeyd:** ilkin sənəddəki "Fevralda 0.8pp artım" iddiası ilə
  **əksinədir** — faktiki nəticə kiçik geriləmə göstərir, artım deyil.
- **Top 10 Brand:** ümumi alışların **32.63%**-ni təşkil edir. **Qeyd:** ilkin sənəddə "top 5
  brand 34%" göstərilib — fərqli meyar (top 5 vs top 10) üzərindən müqayisə edildiyi üçün
  birbaşa doğrulama mümkün deyil, lakin rəqəmlər yaxın diapazondadır.
- **Qiymət/Səbət Dəyəri:** orta məhsul qiyməti **$5.00**, sessiya-səviyyəli orta səbət dəyəri
  **$41.03**, maksimum səbət dəyəri **$1,566.81**. **Qeyd:** ilkin sənəddəki "orta səbət $26.8,
  maksimum $2140" rəqəmləri ilə **üst-üstə düşmür** — ölçü tərifi (tək məhsul qiyməti vs səbət
  cəmi) fərqli ola bilər, ona görə birbaşa müqayisə çətindir.
- **IQR Outlier Analizi:** normal aralıq [-$4.37, $11.52], outlier sayı **34,947** (bütün
  alışların 6.91%-i).
- **Bonus — View→Purchase Müddəti:** orta **131.81 saat (5.49 gün)**, median isə cəmi **4.53
  saat** — paylanma güclü sağa-əyilmiş, əksər istifadəçi eyni gün qərar verir.
- **Bonus — Yüksək-View/Aşağı-Conversion Məhsullar:** **627 məhsul** (min. 100 baxışlı
  məhsulların dörddə birindən çoxu) yüksək maraq görür, lakin conversion dərəcəsi 5.26%-dən
  aşağıdır (bəzilərində 0.13%-ə qədər).

## 3. Business Insights

1. **Cart→Purchase mərhələsi (30.21%) əsas drop-off nöqtəsidir, View→Cart (19.94%) deyil.**
   İstifadəçilərin təxminən 70%-i səbətə məhsul əlavə etdikdən sonra alışı tamamlamır — checkout
   prosesinin sadələşdirilməsi, ödəniş metodlarının genişləndirilməsi və "abandoned cart"
   e-poçt kampaniyaları prioritetləşdirilməlidir.

2. **`apparel.glove` kateqoriyası ən yüksək conversion-a (18.43%) malikdir, `accessories.bag`
   isə ən aşağılardan biridir (1.21%).** Bu böyük fərq kateqoriyalar arası çarpaz-satış
   strategiyasını yenidən nəzərdən keçirməyi tələb edir — yüksək-konvertasiya kateqoriyalarının
   vitrin yerləşməsi genişləndirilə bilər.

3. **Fevral ayında overall conversion rate Yanvara nisbətən yaxşılaşmayıb, əksinə kiçik
   geriləmə (-0.27pp) var.** Hər hansı fevral kampaniyasının effektivliyi sual altındadır —
   kampaniya keçirilibsə, onun konkret təsiri araşdırılmalıdır.

4. **627 məhsul yüksək maraq görür, lakin demək olar heç alınmır (conversion < 5.26%).** Bu
   məhsullar üçün qiymət, məhsul səhifəsi keyfiyyəti (şəkillər, təsvir) və stok statusu ayrıca
   audit edilməli — bu, aşağı-asılmış-meyvə (low-hanging fruit) optimallaşdırma imkanıdır.

5. **Convert olan istifadəçilərin median qərar müddəti cəmi 4.5 saatdır, lakin orta göstərici
   5.5 gündür — qütbləşmiş davranış mövcuddur.** Əksər alıcılar eyni gün qərar verir (impulsiv
   alış), azsaylı bir qrup isə həftələrlə müqayisə edir. Sürətli qərar verənlər üçün "indi al"
   təcililiyi, uzun-düşünənlər üçün isə retarget/xatırlatma e-poçtları fərqli yanaşma tələb edir.




