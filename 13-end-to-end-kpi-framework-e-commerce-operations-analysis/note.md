# End-to-End KPI Framework for E-Commerce Operations — Hesabat

## 1. Data Təmizləmə və Merge Addımları
- **5 Cədvəlin Merge Edilməsi:** `orders` → `order_items` (inner join, `order_id`) →
  `products` (`product_id`) → `category_translation` (`product_category_name`) →
  `order_reviews` (yalnız `review_score`, sol-join, `order_id`). **Qeyd:** ilkin sənəddəki "5
  cədvəl merge edildi; əsas açar: order_id və product_id"  tam üst-üstə düşür.
- **"Delivered" Statuslu Sifarişlərin Filtrasiyası:** yalnız `order_status == "delivered"`
  saxlanıldı. Item-səviyyəli **110,840 sətir**, unikal sifariş isə **96,478**. **Qeyd:** ilkin
  sənəddəki "96.5k sətir" rəqəmi faktiki nəticə ilə demək olar tam üst-üstə düşür.
- **KPI Hesablama Metodologiyası:** Revenue = item `price` cəmi; AOV = sifariş-səviyyəli
  gəlirin ortası (`groupby(order_id)` sonra `.mean()`, item-səviyyəli sadə orta deyil);
  Delivery Time = `order_delivered_customer_date - order_purchase_timestamp`; Review Score =
  orta `review_score`. `freight_value` (çatdırılma haqqı) KPI hesablamalarına daxil edilmədi —
  yalnız məhsul qiyməti (`price`) nəzərə alındı.
- **Etibarlılıq Filtri:** kateqoriya səviyyəli müqayisələrdə (top/bottom, korrelyasiya) yalnız
  ən azı **20 sifarişi** olan kateqoriyalar nəzərə alındı — kiçik ölçülü kateqoriyalarda
  ekstremal dəyərlər statistik cəhətdən etibarsızdır.

## 2. Əsas Statistik Göstəricilər

- **KPI 1 — Ümumi Gəlir:** **13.28M BRL**. **Qeyd:** ilkin sənəddəki "13.59M BRL" rəqəminə
  yaxındır (~2.3% fərq) — kiçik fərq ehtimal ki, `freight_value`-nin daxil edilib-edilməməsindən
  qaynaqlanır.
- **KPI 2 — AOV:** **137.65 BRL**. **Qeyd:** ilkin sənəddəki "140.7 BRL" rəqəminə yaxındır
  (~2.2% fərq) — eyni səbəbdən.
- **KPI 3 — Ortalama Çatdırılma Müddəti:** **12.01 gün**. **Qeyd:** ilkin sənəddəki "12.1 gün"
  ilə demək olar tam üst-üstə düşür — təsdiqləndi.
- **KPI 4 — Ortalama Review Score:** **4.08 / 5**. **Qeyd:** ilkin sənəddəki "4.09" ilə demək
  olar tam üst-üstə düşür — təsdiqləndi.
- **Ən Yüksək Gəlirli Kateqoriya:** **health_beauty (1.24M BRL)**. **Qeyd:** ilkin sənəddəki
  "bed_bath_table (1.71M BRL)" iddiası ilə **üst-üstə düşmür** — faktiki olaraq `bed_bath_table`
  1.04M BRL gəlirlə yalnız **3-cü yerdədir** (health_beauty və watches_gifts-dən sonra); fərq,
  ehtimal ki, `freight_value` daxil edilməsindən və ya fərqli merge strategiyasından qaynaqlanır.
- **Ən Pis/Yaxşı Review Score (kateqoriya):** `security_and_services` = **2.5** (2 sifariş) —
  ilkin sənəddəki "2.5" rəqəmi ilə **tam üst-üstə düşür** — təsdiqləndi. `fashion_childrens_clothes`
  = **5.0** (7 sifariş) — ilkin sənəddəki "4.5" rəqəmindən fərqlidir. Hər iki kateqoriya çox az
  sifarişə (2 və 7) malikdir, statistik cəhətdən etibarsızdır; min. 20 sifarişli filtrlə ən
  yaxşı kateqoriya **books_general_interest (4.51)**, ən pis isə **diapers_and_hygiene (3.38)**
  olur.
- **Çatdırılma — Review Korrelyasiyası (kateqoriya səviyyəsində, n≥20):** **r = -0.416**.
  **Qeyd:** ilkin sənəddəki "r = -0.43" rəqəminə çox yaxındır — istiqamət və güc tam təsdiqləndi.
- **Satıcı Performansı:** top 10 satıcı ümumi gəlirin **13.28%**-ni təşkil edir. **Qeyd:** ilkin
  sənəddəki "12.7%" rəqəminə çox yaxındır (~0.6 xal fərq) — praktiki olaraq təsdiqləndi.
- **Riskli Kateqoriyalar (Bonus):** çatdırılma ortalamadan yuxarı VƏ review ortalamadan aşağı
  olan **21 kateqoriya** (n≥20 filtrli 73 kateqoriyadan) tapıldı. Ən diqqətəlayiq:
  `office_furniture` — 20.4 gün orta çatdırılma, 3.52 review, 1,254 sifariş həcmi ilə ən böyük
  risk qrupudur.

## 3. Business Insights

1. **`office_furniture` kateqoriyası ən böyük əməliyyat riskini daşıyır — həm uzun çatdırılma
   (20.4 gün), həm aşağı review (3.52), həm də böyük həcm (1,254 sifariş).** Bu kateqoriya üçün
   logistika prioritetləşdirilməli — böyük həcm səbəbindən kiçik yaxşılaşdırma belə əhəmiyyətli
   ümumi effekt verər.

2. **Çatdırılma müddəti ilə review score arasında orta-güclü mənfi korrelyasiya (r=-0.42)
   kateqoriya səviyyəsində sistematikdir.** 21 kateqoriya həm ortalamadan yavaş çatdırılma, həm
   aşağı məmnuniyyətlə səciyyələnir — logistika investisiyaları (regional anbar şəbəkəsi) bu
   kateqoriyaların hamısına eyni zamanda təsir edər.

3. **Gəlir konsentrasiyası azdır (top 10 satıcı cəmi 13.3%), lakin satıcı gəlir paylanması
   güclü uzun-quyruqludur.** Kiçik satıcıların böyük əksəriyyəti minimal gəlir gətirir —
   onboarding və satıcı-tərəfli dəstək proqramları bu geniş "uzun quyruğu" böyütməyə yönəldilə
   bilər.

4. **`health_beauty` və `watches_gifts` ən böyük gəlir generatorlarıdır — inventar və
   marketinq büdcəsi bu kateqoriyalara mütənasib şəkildə ayrılmalıdır.** Bu iki kateqoriya
   birlikdə 2.4M+ BRL gəlir gətirir — stok-out riskinin qarşısını almaq üçün tədarük zəncirinin
   etibarlılığı xüsusi diqqət tələb edir.

5. **Aylıq gəlir trendi ümumilikdə artım göstərir, lakin 2017-11 (995K BRL, Black Friday
   effekti) kimi kəskin sıçrayışlar mövsümi hazırlıq tələb edir.** Bu pik ayda çatdırılma
   müddəti də yüksəlir (14.6 gün, illik ortadan yuxarı) — bayram öncəsi anbar/logistika
   gücləndirilməsi review score-un pik dövrlərdə düşməsinin qarşısını ala bilər.


