# Regional Sales Breakdown and Genre Trends in Video Game Market — Hesabat

## 1. Data Təmizləmə Addımları
- **Datanın Yüklənməsi:** İlkin video oyun satış datası  uğurla yükləndi. Dataset tam olaraq **16,598 oyun və 11 sütundan** ibarətdir.
- **Missing Value İdarəsi:**
  - `Year` sütununda boşluq olan **271 sətir drop edildi** .
  - `Publisher` sütunundakı boşluqlar (58 sətir) **"Unknown"** ilə dolduruldu — sətirlər saxlanıldı, çünki Sales məlumatı tamdır və nəşriyyatçı olmadan da region/janr analizinə töhfə verir.
- **Dublikat Yoxlanışı:** `df.duplicated()` ilə tam təkrarlanan sətirlər yoxlanıldı, heç bir dublikat aşkar edilmədi.
- **Təmizlənmiş Dataset:** Son dataset **16,327 oyun** ilə bütün sonrakı aqreqasiya və trend analizlərinin əsasını təşkil etdi.

## 2. Əsas Statistik Göstəricilər

- **Janr Liderləri (Qlobal Satış):** `Action` ($1,722.9M) sənayenin ən çox satan janrıdır, ardınca `Sports` ($1,309.2M) və `Shooter` ($1,026.2M) gəlir. Bu üç janr birlikdə qlobal satışın böyük hissəsini təşkil edir.
- **Platform Liderləri:** `PS2` tarixin ən çox satan platformasıdır (**$1,233.5M**), ardınca `X360` ($969.6M) və `PS3` ($949.4M) gəlir — uzun məhsul həyat dövrü (2000-2013) PS2-nin liderliyinin əsas səbəbidir.
- **Nəşriyyatçı Liderləri:** `Nintendo` ən güclü publisher-dir — **$1,784.4M** qlobal satışla ikinci yerdəki `Electronic Arts`-dan ($1,093.4M) əhəmiyyətli dərəcədə öndədir. Bu üstünlük əsasən Wii/NES/DS platformalarındakı ekskluziv first-party oyunlardan qaynaqlanır.
- **Regional Bazar Payı:** Qlobal satışın **49.1%-i North America**, **27.3%-i Europe**, **14.6%-i Japan**-a, qalan hissəsi isə digər regionlara aiddir.
- **Regional Janr Üstünlüyü:**
  - **NA:** `Action` liderdir (861.8M$, NA payı ~50%).
  - **EU:** `Action` liderdir (516.5M$, EU payı ~30%).
  - **JP:** `Role-Playing` liderdir (350.3M$) — bu, digər iki bazardan **fərqli** nümunədir. Role-Playing janrının qlobal satışının **37.9%-i** JP bazarından gəlir (müqayisə üçün: Action-da bu rəqəm cəmi 9.2%, Shooter-da 3.7%).
- **İllik Trend:** **2008-ci il** sənayenin pik ili olub (**678.9M** ədəd qlobal satış), bundan sonra tədricən azalma müşahidə olunur (dataset yalnız fiziki/pərakəndə satışları əhatə etdiyi üçün bu, rəqəmsal distribusiyaya keçidin ilkin əlaməti ola bilər).
- **Ən Çox Satan Oyun:** `Wii Sports` (Wii, 2006, Sports) **82.7M** satışla siyahının başındadır — ikinci yerdəki `Super Mario Bros.`-dan (40.2M) iki dəfədən çox öndədir.
- **Onilliy üzrə Orta Satış (Bonus):** **80-ci illər** oyun başına ən yüksək orta satışa malikdir (**1.84M/oyun**) — bu dövrdə az sayda, lakin çox güclü hit oyunların (məs. Super Mario Bros.) olması ilə izah olunur. 90-cı illər (0.72M), 2000-ci illər (0.50M) və 2010-cu illər (0.49M) tədricən azalan orta satış göstərir — bazarın genişlənməsi ilə oyun sayının artması orta göstəricini aşağı çəkib.

## 3. Regional Bazar Insight-ları

1. **Yaponiya bazarı fərqli janr strategiyası tələb edir.** NA və EU bazarlarında Action/Shooter janrları dominant olduğu halda, Japan bazarında Role-Playing janrı həm mütləq, həm də nisbi göstəricilərdə (37.9% JP payı) əsas yeri tutur. Qlobal nəşriyyatçılar JP bazarına giriş üçün lokallaşdırılmış RPG strategiyası nəzərdən keçirməlidir, Action/Shooter janrlı title-lar isə JP-də nisbətən zəif performans göstərəcək.

2. **Nintendo-nun bazar liderliyi first-party ekosistem strategiyasının nəticəsidir.** Top 10 ən çox satan oyunun əksəriyyəti Nintendo mülkiyyətindədir və öz platformalarında (Wii, NES, DS) satılıb. Bu, exclusive content-in həm platform, həm də nəşriyyatçı səviyyəsində bazar payına birbaşa təsirini göstərir — rəqiblər üçün strong IP portfeli əsas rəqabət üstünlüyüdür.

3. **2008-ci ildən sonrakı azalma tənəzzül kimi deyil, ölçmə sərhədi kimi şərh edilməlidir.** Dataset fiziki pərakəndə satışları əhatə edir; rəqəmsal distribusiyanın (Steam, PSN, Xbox Live) 2008-dən sonra sürətlə böyüməsi real bazar ölçüsünün bu datasetdə tam əks olunmamasına səbəb ola bilər. Management qərarları üçün rəqəmsal satış datası ilə tamamlanmalıdır.

4. **PS2-nin uzunmüddətli liderliyi ilə yeni nəsil platformaların (PS3, X360) daha qısa müddətdə oxşar nəticələr əldə etməsi arasındakı fərq diqqətəlayiqdir.** Bu, yeni konsol nəsillərinin bazara girişində ilk illərdə güclü exclusive title portfeli ilə sürətli mövqe əldə etmə zərurətini vurğulayır — məhsul həyat dövrü get-gedə qısalır, ona görə də erkən mərhələdə bazar payı əldə etmək strateji cəhətdən daha vacib olur.


