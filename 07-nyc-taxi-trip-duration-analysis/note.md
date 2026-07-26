# Hourly and Daily Demand Pattern Analysis for NYC Taxi Trips — Hesabat

## 1. Data Təmizləmə Addımları
- **Datanın Yüklənməsi:**  Dataset **1,458,644 sətir və 11 sütundan** ibarətdir, `id`-dən `trip_duration`-a qədər heç bir sütunda missing value aşkarlanmadı.
- **Zaman Aralığı:** Datasetin faktiki əhatə etdiyi dövr **2016-cı ilin 1 Yanvar – 30 İyun** tarixləridir (6 ay) — ilkin təhlil sənədində qeyd olunan "tam il" fərziyyəsi təsdiqlənmədi, buna uyğun olaraq mövsümi nəticələr yenidən qiymətləndirildi.
- **Duration Sütununun Çevrilməsi:** `trip_duration` saniyə ilə verilmişdir; `trip_duration_min = trip_duration / 60` düsturu ilə dəqiqəyə çevrildi.
- **Outlier Filtrasiyası:** Tapşırıqda tələb olunan qayda tətbiq edildi — **1 dəqiqədən qısa və 120 dəqiqədən uzun** səfərlər çıxarıldı. Bu, ilkin qeyd sənədindəki 24 saatlıq (~170 sətir) hədddən daha sərt filtrdir. Nəticədə **10,848 sətir (cəmi datasetin 0.74%-i)** çıxarıldı, təmizlənmiş dataset (`df_clean`) **1,447,796 sətirdən** ibarətdir. Filtrasiyadan əvvəl orta müddət 16.0 dəqiqə, maksimum isə 58,771 dəqiqəyə (GPS/log xətası) qədər çatırdı — filtrasiyadan sonra maksimum 120 dəqiqə ilə məhdudlaşdı və orta göstərici 14.0 dəqiqəyə düşdü, yəni ekstremal dəyərlər ortalamanı əhəmiyyətli dərəcədə yuxarı çəkirdi.
- **Zaman Komponentlərinin Çıxarılması:** `pickup_datetime`-dan `hour` (0-23), `weekday` (Bazar ertəsi=0), və `month` sütunları çıxarıldı. Tələb sayı üzrə aqreqasiyalar (saat/gün üzrə) tam dataset (`df`) üzərində aparıldı, çünki bu, müddət outlier-lərindən asılı deyil; müddət əsaslı aqreqasiyalar (saat/ay üzrə orta müddət) isə `df_clean` üzərində aparıldı.
- **Digər Sahələr:** `passenger_count`-da 60 sətirdə 0 sərnişin qeyd olunub (analitik məqsədlə çıxarılmayıb, əhəmiyyətsiz say); `store_and_fwd_flag` demək olar tamamilə "N" (1,450,599 sətir) təşkil edir, yəni serverlə əlaqə kəsilməsi hadisələri nadirdir.

## 2. Əsas Statistik Göstəricilər

- **Pik Tələbat Saatı:** Ən yüksək tələbat **18:00–19:00** saat aralığındadır (**90,600 səfər**); ən sakit saat isə **05:00–06:00**-dır (**15,002 səfər**) — pik saat off-peak saatdan təxminən **6 dəfə** çoxdur.
- **Həftənin Günləri üzrə Tələbat:** Ən çox səfər **Cümə** günü qeydə alınıb (**223,533 səfər**), ən az isə **Bazar ertəsi** günü (**187,418 səfər**). Maraqlıdır ki, iş günü (Bazar ertəsi–Cümə) üzrə orta gündəlik səfər sayı (**208,482**) həftəsonu (Şənbə–Bazar) ortalaması (**208,117**) ilə demək olar eynidir — sadə "iş günü vs həftəsonu" bölgüsü ümumi həcmi fərqləndirmir, fərq günlər arasında (Cümə vs Bazar ertəsi) daha kəskindir.
- **Saat üzrə Orta Müddət:** Ən uzun orta səfər müddəti **15:00** saatında (**16.2 dəqiqə**), ən qısası isə **06:00** saatında (**11.3 dəqiqə**) müşahidə olunur — müddət artımı tələbat pikindən (18:00) təxminən 3 saat əvvəl başlayır ki, bu, günorta-axşam tıxacının erkən formalaşdığını göstərir.
- **Müddət Paylanması (təmizlənmiş):** Orta müddət **14.0 dəqiqə**, median **11.1 dəqiqə**-dir (median ortalamadan aşağıdır, yəni paylanma sağa əyilmişdir — çoxlu sayda qısa səfər, az sayda nisbətən uzun səfər).
- **Aylıq Trend:** Orta səfər müddəti Yanvardan (13.2 dəqiqə) İyuna (14.9 dəqiqə) qədər ardıcıl artır. Bu, ilkin qeyd sənədində göstərilən "Aprel pikini" təsdiqləmir — dataset yalnız İyuna qədər olduğu üçün trend qış-dan yay əvvəlinə doğru **davamlı artım** kimi görünür, təkcə bir ayın (Aprel) izolə pik effekti kimi deyil. Səfər sayı baxımından isə ən çox səfər **Mart** ayında (256,189), ən az isə **Yanvar** ayında (229,707) qeydə alınıb.
- **Vendor Müqayisəsi :** Vendor 2 səfərlərin **53.5%-ni** (774,632 səfər), Vendor 1 isə **46.5%-ni** (673,164 səfər) təşkil edir. Orta müddət baxımından fərq minimaldır — Vendor 1: **13.93 dəqiqə**, Vendor 2: **14.08 dəqiqə** (median: 11.05 vs 11.12 dəqiqə) — praktiki əhəmiyyətli fərq yoxdur.
- **Heatmap :** Saat × həftənin günü heatmap-i göstərir ki, ən yüksək sıxlıq iş günləri axşam saatlarında (17:00–19:00) cəmlənib, həftəsonu isə pik daha gec saatlara (22:00–00:00) sürüşür — gecə əyləncə fəaliyyəti ilə üst-üstə düşür.

## 3. Business Insights

1. **Axşam pikinə (18:00–19:00) qədər əlavə sürücü ehtiyatı formalaşdırılmalıdır.** Tələbat off-peak saatdan (05:00) 6 dəfə yüksəkdir. Flotun günorta saatlarından (14:00–15:00) etibarən tədricən artırılması, real pikə çatana qədər tələbatı qarşılamağa kömək edər.

2. **Cümə ən yüksək, Bazar ertəsi ən aşağı tələbat günüdür — lakin ümumi iş günü/həftəsonu həcmi demək olar bərabərdir.** Bu, sadə "iş günü daha çox sürücü, həftəsonu daha az" qaydasının səhv olduğunu göstərir; planlaşdırma gün-gün əsasında aparılmalıdır, xüsusən Cümə üçün əlavə güc, Bazar ertəsi üçün isə azaldılmış heyət.

3. **Tıxac effekti tələbat pikindən əvvəl başlayır.** Orta müddət 15:00-da pik nöqtəyə çatır, halbuki səfər sayı yalnız 18:00-də piklənir. Bu, ETA (təxmini çatma vaxtı) hesablamalarının və sürücü növbə planlamasının günorta saatlarından etibarən genişləndirilməli olduğunu göstərir, təkcə axşam pikinə deyil.

4. **Orta müddət Yanvardan İyuna davamlı artır — mövsümi deyil, tədricən artan tıxac trendidir.** Dataset yalnız 6 ayı əhatə etdiyi üçün "Aprel yağış effekti" fərziyyəsi təsdiqlənmir. Bunun əvəzinə, il ərzində şəhər fəaliyyətinin artması ilə paralel tıxac artımı müşahidə olunur — yay aylarında dinamik qiymətləndirmə və ya daha geniş ETA buferi tətbiqi tövsiyə olunur.

5. **Vendor-lar arasında əhəmiyyətli xidmət fərqi yoxdur.** Vendor 2 bir qədər daha çox səfər həyata keçirsə də (53.5% vs 46.5%), orta müddət fərqi cəmi ~0.15 dəqiqədir. Bu, hər iki vendorun marşrut effektivliyi baxımından oxşar səviyyədə olduğunu göstərir — flot idarəetməsi qərarları vendor seçiminə əsaslanmamalıdır.

