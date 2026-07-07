# Data Təmizləmə və İlkin Biznes Analizi Hesabatı

## 1. Data Təmizləmə Addımları 
- **Datanın Yüklənməsi:** İlkin satış datası uğurla yükləndi. Dataset tam olaraq **2823 sətir və 25 sütundan** ibarətdir.
- **Tarix Formatının Düzəldilməsi:** Sifariş tarixlərini ehtiva edən `ORDERDATE` sütunu mətn (object) tipindən standart Python `datetime` formatına uğurla çevrildi.
- **Boş Xanaların İdarə Olunması (Missing Values):** Data daxilindəki boşluqların real faiz nisbətləri müəyyən edildi: `ADDRESSLINE2` (~89.3%), `STATE` (~52.6%), `TERRITORY` (~38.0%) və `POSTALCODE` (~2.7%). Bu boşluqların heç biri silinməmiş, datanın struktur bütövlüyünü tam qorumaq üçün hamısı "Unknown"  dəyəri ilə əvəzlənmişdir. Hazırda datada **0 boş xana** mövcuddur.
- **Dublikatların Yoxlanılması:** Dataset daxilində tam təkrarlanan  sətirlər yoxlanıldı və heç bir dublikat qeyd qeydə alınmadı.

## 2. Əsas Statistik Göstəricilər 
- **Satış Həcmi ( SALES sütunu ):** - Minimum Satış: $482.13
  - Maksimum Satış: $14,082.80
  - Ortalama Satış (Mean): $3,553.05
  - Median Satış (50%): $3,184.80
- **Sifariş Statusları ( STATUS ):** Şirkətin logistika və sifariş icra prosesi olduqca sağlamdır; ümumi sifarişlərin **92.6%-i** uğurla "Shipped"  statusunu almışdır.

## 3. Biznes Anlayışları 
1. **Satışların Paylanması:** Satış dəyərlərinin böyük hissəsi  **$1,500 ilə $4,500** aralığında sıxlaşıb. $10,000-dan yuxarı satışlar nadir hallarda baş verir ki, bunlar da şirkətin topdan satış  və ya premium müştəriləridir.
2. **İllik Böyümə Trendi:** Şirkət 2003-cü ildən 2004-cü ilə keçiddə böyük bir sıçrayış edib və sifariş həcmi təxminən **39.4%** artıb. 2005-ci ildəki kəskin düşüş isə datanın tam olmamasından  qaynaqlanır.
3. **Coğrafi Bazar Üstünlüyü:** Şirkətin ən böyük və dominant bazarı açıq ara fərqlə **ABŞ-dır ** . Avropa regionunda isə İspaniya və Fransa satış sayına görə ön sıralardadır. Marketinq və reklam büdcəsini planlaşdırarkən bu coğrafi üstünlüklər nəzərə alınmalıdır.
4. **Sifariş Həcminin Qiymət Siyasətinə Təsiri:** Data daxilindəki məhsulların qiymət strategiyası ilə müştəri sifarişləri arasında tərs-mütənasib bir əlaqə zənciri görünür. Biznes gəlirlərinin kəskin şəkildə kiçik və orta ölçülü sövdələşmələrdən asılı olması, müştərilərin böyük büdcəli təkfaydalı alışlardansa, daha çevik və büdcəyə uyğun paketlərə üstünlük verdiyini sübüt edir. Bu səbəbdən bütöv logistika xərclərini azaltmaq üçün orta qiymət seqmentindəki məhsullara yönəlmiş çarpaz satış kampaniyaları genişləndirilməlidir.
5. **Əməliyyat və İnterfeys Məlumatlarının Optimallaşdırılması:** `ADDRESSLINE2` (~89.3%) və `STATE` (~52.6%) kimi coğrafi və ünvan sütunlarındakı yüksək boşluq dərəcəsi şirkətin beynəlxalq miqyasda qeydiyyat sisteminin qüsurlu olduğunu göstərir (məsələn, ABŞ-dan kənar bir çox Avropa ölkəsində "Ştat"  anlayışı yoxdur). Biznes baxımından , gələcəkdə müştəri profillərinin daha şəffaf şəkildə seqmentasiya edilməsi və çatdırılma xətalarının sıfıra endirilməsi üçün onlayn sifariş interfeysi hər bir ölkənin öz yerli ünvan standartına uyğun olaraq dinamik şəkildə optimallaşdırılmalıdır.