# Engagement Analytics and Correlation Deep-Dive — YouTube Trending Data — Hesabat

## 1. Data Təmizləmə Addımları
- **Datasetin Yüklənməsi:** `USvideos.csv` (40,949 sətir, 16 sütun) UTF-8 encoding ilə yükləndi.
  **Qeyd:** ilkin sənəddəki "40949 sətir, 16 sütun" rəqəmi tam üst-üstə düşür.
- **Kateqoriya Parse Edilməsi:** `US_category_id.json` (YouTube API formatı) parse edilib,
  `category_id → category_name` lüğəti yaradıldı və `.map()` ilə tətbiq edildi — bütün 40,949
  sətir uğurla map edildi (0 boş dəyər), 16 unikal kateqoriya.
- **Tarix Çevrilməsi:** `trending_date` (`YY.DD.MM` formatı) və `publish_time` (ISO formatı)
  datetime-a çevrildi.
- **Metrik Mühəndisliyi:** `engagement_rate = (likes+dislikes+comment_count)/views×100`,
  `like_rate = likes/views×100`, `days_to_trend = trending_date - publish_time`.
- **Dublikatların Silinməsi:** hər video üçün yalnız **ilk trending görünüşü** saxlanıldı
  (`video_id` üzrə, `trending_date`-ə görə sıralanıb). 40,949 sətir → **6,351 unikal video**.

## 2. Əsas Statistik Göstəricilər

- **Views ↔ Likes Korrelyasiyası:** r = **0.761**. **Qeyd:** ilkin sənəddəki "r = 0.84" rəqəminə
  yaxındır (fərq ~0.08), istiqamət (güclü müsbət) təsdiqləndi.
- **Comment_count ↔ Dislikes Korrelyasiyası:** r = **0.68**. **Qeyd:** ilkin sənəddəki "r = 0.71"
  rəqəminə çox yaxındır (fərq ~0.03) — praktiki olaraq təsdiqləndi: mübahisəli videolar
  həqiqətən daha çox həm dislike, həm şərh alır.
- **Ən Yüksək Orta Engagement Rate:** **Music (8.23%)**. **Qeyd:** ilkin sənəddəki "ən yüksək
  engagement rate: News & Politics (6.8%)" iddiası ilə **üst-üstə düşmür** — faktiki olaraq
  `News & Politics` engagement rate baxımından **ən aşağı kateqoriyadır** (2.64%). Bu, ən böyük
  uyğunsuzluqlardan biridir.
- **Ən Yüksək Orta Views:** **Music (1.39M)**. **Qeyd:** ilkin sənəddəki "Music (12.4M), lakin
  engagement rate ortalamanın altında" iddiasının yalnız istiqaməti (Music #1 views) düzgündür —
  konkret rəqəm (12.4M) faktiki nəticədən (1.39M) ~9x yüksəkdir və "aşağı engagement" iddiası
  əksinədir (Music əslində ən yüksək engagement-ə malikdir). Fərq ehtimal ki, ilkin sənədin
  dedup edilməmiş və ya kumulyativ view sayı üzərindən hesablamasından qaynaqlanır.
- **Publish Həftəsi Günü — Views:** Cümə (894K) və Bazar (859K) ən yüksək orta views göstərir,
  lakin fərq həftənin digər günlərindən böyük deyil (~30% aralığında) — zəif, lakin
  nəzərəçarpan effekt.
- **Trending Həftəsi Günü — Engagement (Bonus doğrulama):** **Bazar ertəsi (Monday) ən aşağı
  orta engagement rate-ə malikdir (3.98%)**. **Qeyd:** ilkin sənəddəki "Bazar ertəsi trending
  videolar ən az engagement göstərir" iddiası ilə **tam üst-üstə düşür** — təsdiqləndi.
- **Tags Sayı ↔ Views Korrelyasiyası:** r = **0.042**. **Qeyd:** ilkin sənəddəki "r = 0.18"
  rəqəmindən **daha da zəifdir** — hər iki nəticə eyni istiqamətə (zəif müsbət) işarə etsə də,
  faktiki əlaqə demək olar sıfıra yaxındır.
- **Top 10 Trending Video (views üzrə):** 7/10 **Music** və **Entertainment** kateqoriyalarına
  aiddir; `days_to_trend` demək olar bütün top videolarda **1-7 gün** aralığındadır — viral
  məzmun çox sürətli trending-ə çatır.
- **Bonus — Passiv İzləyici / Niş-Sadiq Auditoriya:** passiv izləyici seqmentində (yüksək views,
  aşağı engagement) **Entertainment** və **Sports** üstünlük təşkil edir; niş-sadiq seqmentdə isə
  **Howto & Style** və **People & Blogs** üstünlük təşkil edir.

## 3. Business Insights (Content Creator / Media Team üçün)

1. **Music kateqoriyası həm ən yüksək views, həm ən yüksək engagement rate-i eyni anda təmin
   edir — bu, ən "verimli" məzmun sahəsidir.** Digər yüksək-views kateqoriyalardan fərqli olaraq,
   Music izləyicini həm cəlb edir, həm də aktiv reaksiyaya sövq edir — musiqi məzmununa
   investisiya ROI baxımından prioritet olmalıdır.

2. **News & Politics ən aşağı engagement rate-ə malikdir — bu kateqoriyada "keçici baxış"
   davranışı üstünlük təşkil edir.** İzləyicilər xəbər videolarını izləyir, lakin nadir hallarda
   like/comment ilə reaksiya verir — açıq sual/müzakirə formatları engagement-i artıra bilər.

3. **Views ilə likes arasında güclü əlaqə (r=0.76) var, lakin views ilə engagement_rate
   arasında demək olar heç bir əlaqə yoxdur (r=0.057).** Böyük auditoriya avtomatik yüksək
   faizli reaksiya təmin etmir — engagement rate artırmaq üçün ayrıca strategiya (CTA-lar,
   community-yönümlü məzmun) lazımdır, sadəcə reach kifayət deyil.

4. **Comment_count ilə dislikes arasında orta-güclü əlaqə (r=0.68) mübahisəli məzmunun həm
   daha çox müzakirəyə, həm də daha çox mənfi reaksiyaya səbəb olduğunu göstərir.** Media
   komandaları mövzu seçimində bu tarazlığı nəzərə almalıdır.

5. **Tags sayının views-a praktiki təsiri yoxdur (r=0.042) — SEO-yönümlü tag-doldurma
   strategiyası effektiv deyil.** Resurslar tag optimizasiyasından çox, thumbnail/title
   keyfiyyətinə və nəşr vaxtının seçilməsinə (Cümə/Bazar günlərinə yaxın) yönəldilməlidir.


