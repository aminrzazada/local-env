# Istanbul Alış-veriş Mərkəzləri Datası üzrə Çoxölçülü Analiz Hesabatı

## 1. Data Təmizləmə Addımları
- **Datanın Yüklənməsi:** İlkin Istanbul shopping mall satış datası uğurla yükləndi. Dataset tam olaraq **99,457 sətir və 10 sütundan** ibarətdir.
- **Tarix Formatının Düzəldilməsi:** Sifariş tarixlərini ehtiva edən `invoice_date` sütunu mətn (object) tipindən standart Python `datetime` formatına uğurla çevrildi. İllik və mövsümi trendləri analiz etmək üçün bu sütundan əlavə olaraq `year` və `month` parametrləri çıxarıldı.
- **Boş Xanaların İdarə Olunması (Missing Values):** Dataset daxilindəki bütün sütunlar üzrə boşluq (missing value) yoxlanışı aparıldı. Hesabat nəticələrinə əsasən datada heç bir buraxılmış və ya boş xana aşkar edilməmişdir. Hazırda datada **0 boş xana** mövcuddur və data tam təmizdir.
- **Dublikatların Yoxlanılması:** Dataset daxilində tam təkrarlanan sətirlər yoxlanıldı (`df.duplicated()`) və heç bir dublikat qeyd qeydə alınmadı.
- **Gəlir Sütununun Yaradılması:** Hər əməliyyat üçün `revenue = quantity * price` düsturu ilə gəlir sütunu hesablanaraq bütün sonrakı analizlərin əsasını təşkil etdi.

## 2. Əsas Statistik Göstəricilər
- **AVM Bölgüsü:** Dataset 10 fərqli AVM-i (Kanyon, Mall of Istanbul, Istinye Park və s.) əhatə edir. Gəlir baxımından **Mall of Istanbul** ($50.87M) və **Kanyon** ($50.55M) aparıcı iki mərkəzdir, digər AVM-lərlə fərq 2–4 dəfəyə çatır.
- **Kateqoriya üzrə Liderlər:** Ümumi gəlir bazasında ən yüksək satış həcmi **$113.99M** ilə `Clothing` (34,487 əməliyyat, ümumi əməliyyatların ~34.7%-i) kateqoriyasına məxsusdur, dərhal ardınca **$66.55M** ilə `Shoes` və **$57.86M** ilə `Technology` kateqoriyaları gəlir.
- **Ödəniş Metodu:** Müştərilərin **44.7%-i Cash**, 35.1%-i Credit Card, 20.2%-i isə Debit Card ilə ödəniş edir — bu nisbət demək olar ki, bütün kateqoriyalarda eynidir.
- **Gender Bölgüsü:** Qadın müştərilər əməliyyatların **59.8%-ni** ($150.21M gəlir), kişilər isə **40.2%-ni** ($101.30M gəlir) təşkil edir.
- **Orta Əməliyyat Məbləği:** AVM-lər arasında ən yüksək orta çek (`avg_order_value`) **Emaar Square Mall**-a məxsusdur ($2,578.7/əməliyyat), Mall of Istanbul ($2,550.9) və Kanyon (~$2,550.3) ona çox yaxındır.
- **İllik Gəlir:** 2022-ci il ($115.44M) 2021-ci ilə ($114.56M) nisbətən cüzi öndə olaraq ümumi gəlir baxımından ən yüksək il olub; 2023-cü ilin rəqəmi (~$21.5M) natamam data dövrünə görə əhəmiyyətli dərəcədə aşağı görünür.

## 3. Biznes Anlayışları
1. **Kateqoriya Strategiyası və İnventar Prioritetləşdirilməsi:** `Clothing`, `Shoes` və `Technology` şirkətin əsas gəlir lokomotivləridir və birlikdə ümumi gəlirin böyük əksəriyyətini təşkil edir. Bu üç kateqoriyaya raf sahəsi, vitrin yerləşdirilməsi və endirim kampaniyaları baxımından prioritet verilməli, aşağı gəlir gətirən `Souvenir` və `Books` kimi kateqoriyalarla çarpaz satış strategiyaları nəzərdən keçirilməlidir.
2. **Rəqəmsal Ödənişə Keçidin Təşviqi:** Cash ödənişinin bütün kateqoriyalarda demək olar ki, eyni nisbətdə (44–46%) dominant olması göstərir ki, bu, kateqoriyadan asılı olmayan ümumi müştəri davranışıdır. Kart/mobil ödəniş üçün sadiqlik bonusları və ya cashback təşviqləri tətbiq etməklə əməliyyat xərcləri azaldıla, məlumat izlənməsi isə yaxşılaşdırıla bilər.
3. **Qadın Seqmentinə Fokuslu Marketinq, Kişi Seqmentində Böyümə Potensialı:** Qadın müştərilər gəlirin əsas hərəkətverici qüvvəsidir (59.8%). Marketinq büdcəsinin qadın-yönümlü kateqoriyalara (Clothing, Cosmetics) ağırlıq verməsi məntiqlidir, lakin kişi seqmentinin xərcləmə potensialını artırmaq üçün Technology və Shoes kimi kateqoriyalarda hədəflənmiş kampaniyalar əlavə gəlir mənbəyi ola bilər.
4. **Premium Müştəri Profili olan AVM-lərin Fərqli İdarə Edilməsi:** Emaar Square Mall ümumi gəlir həcminə görə kiçik olsa da, ən yüksək orta əməliyyat məbləğinə malikdir — bu, az sayda, lakin daha çox xərcləyən müştəri bazasının olduğunu göstərir. Bu tip AVM-lər üçün ümumi trafik artırma əvəzinə premium məhsul çeşidi və upsell strategiyaları daha effektiv ola bilər, halbuki Mall of Istanbul və Kanyon kimi yüksək-həcmli mərkəzlərdə diqqət trafik və əməliyyat sayının artırılmasına yönəldilməlidir.
5. **İllik Trend Şərhində Ehtiyatlılıq:** 2022-ci il ən yüksək gəlirli il olsa da, 2021 ilə fərq çox cüzidir və praktik olaraq sabit bir trend göstərir. 2023-cü ilin kəskin aşağı rəqəmi isə real satış tənəzzülü kimi deyil, dataset-in bu ili yalnız qismən əhatə etməsi kimi şərh edilməlidir — management qərar qəbul etməzdən əvvəl 2023 datasının hansı tarixə qədər əhatə etdiyini təsdiqləməlidir.

