# Outlier Detection and Market Benchmarking — Google Play Store — Hesabat

## 1. Data Təmizləmə və Hazırlıq Addımları

- **Faylın Yüklənməsi:** `googleplaystore.csv` yükləndi. Faktiki ölçü: **10,841 sətir, 13
  sütun**.
- **Korrupt Sətir:** `index 10472`-də sütun sürüşməsi (column-shift) aşkarlandı — `Category`
  sahəsində "1.9" (əslində Rating dəyəri) qeyd olunub və bütün sonrakı sütunlar bir mövqe sola
  sürüşüb. Yalnız 1 sətir təsirləndiyi üçün sətir tam silindi (realignment aparılmadı). 
- **Dtype Düzəlişləri:** `Installs` ("+", "," silindi → int), `Price` ("$" silindi → float),
  `Reviews` (→ int), `Size` ("M" → MB, "k" → KB/1000, "Varies with device" → NaN) düzəldildi.
- **Rating Boşluqları:** Rating sütununda **1,474 NaN** aşkarlandı → kateqoriya medianı ilə
  dolduruldu (kateqoriyada median yoxdursa, qlobal median istifadə olundu). 
- **Size Boşluqları:** Size sütununda **1,695 NaN** ("Varies with device" daxil) aşkarlandı →
  eyni məntiqlə (kateqoriya medianı) dolduruldu. 
- **Dublikatların Silinməsi:** **1,181 dublikat** `App` sətri tapıldı → hər app üçün ən yüksək
  `Reviews`-a malik versiya saxlanıldı. **Qeyd:** ilkin sənəddəki "1181 dublikat" iddiası ilə
  **tam üst-üstə düşür**.
- **Son Ölçü:** Təmizləmədən sonra dataset **9,659 sətirə** düşdü (korrupt sətir − 1, dublikat
  − 1,181, `Type`/`Content Rating` boşluqları görə − bir neçə sətir).

## 2. Əsas Statistik Göstəricilər

- **IQR Outlier — Reviews:** normal aralığın üst sərhədi **73,596 review**, outlier sayı
  **1,659** (bütün app-ların 17.2%-i). **Qeyd:** ilkin sənəddəki "847 outlier (>3.7M reviews)"
  iddiası ilə **üst-üstə düşmür** — həm say (1659 vs 847), həm də hədd (73.6K vs 3.7M) əhəmiyyətli
  dərəcədə fərqlidir. Fərq çox güman ki, ilkin qiymətləndirmənin daha sərt/fərqli bir hədd
  (məs. 3×IQR və ya fərqli kvartil hesablama metodu) istifadə etməsindən qaynaqlanır; standart
  1.5×IQR qaydası ilə hədd xeyli aşağı çıxır.
- **IQR Outlier — Installs:** normal aralığın üst sərhədi **2,498,500 install**, outlier sayı
  **1,981** (20.5%). İlkin sənəddə bu göstərici üçün ayrıca rəqəm verilməyib.
- **Outlier-lərin Kateqoriya üzrə Paylanması:** GAME (**368**), FAMILY (258), TOOLS (135),
  PHOTOGRAPHY (101), COMMUNICATION (84) — ən çox outlier-li ilk beş kateqoriya. **Qeyd:** ilkin
  sənəddəki "GAME kateqoriyasında outlier-lar xüsusilə çoxdur" iddiası **təsdiqlənir** —
  istiqamət düzgündür, sadəcə mütləq rəqəmlər fərqlidir (yuxarıdakı bənd).
- **Free vs Paid — Ortalama Rating:** Free = **4.18**, Paid = **4.27**. **Qeyd:** ilkin
  sənəddəki "Free 4.18 / Paid 4.26" iddiası ilə **praktik olaraq üst-üstə düşür** (fərq 0.01
  xaldan azdır).
- **Free vs Paid — Median Rating:** hər ikisi **4.3** — mean göstəricisindən fərqli olaraq
  median üzrə fərq demək olar yoxdur, yəni mean-dəki kiçik fərq bir neçə aşağı-reytinqli free
  app-ın təsirindən yarana bilər.
- **Free vs Paid — Median Installs:** Free = **100,000**, Paid = **1,000** — 100 qat fərq;
  rating oxşar olsa da, auditoriya miqyası tamam fərqlidir.
- **Ən Yüksək Median Install-a Malik Kateqoriyalar:** EDUCATION, SHOPPING, ENTERTAINMENT, GAME,
  PHOTOGRAPHY, VIDEO_PLAYERS, WEATHER — hamısı **1,000,000** median install ilə bərabər liderlik
  edir. **Qeyd:** ilkin sənəddəki "Communication (50M+)" iddiası ilə **üst-üstə düşmür** —
  faktiki datada COMMUNICATION kateqoriyasının median install-ı cəmi **100,000**-dir (315 app
  arasında) və top-10-a belə düşmür. 50M+ rəqəmi çox güman ki, mean və ya tək bir mega-app-ın
  (WhatsApp, Messenger və s.) təsiri ilə hesablanıb — median bu cür outlier-lərə qarşı davamlı
  olduğu üçün fərqli (və daha real) mənzərə verir.
- **Rating ↔ Reviews Korrelyasiyası:** r = **0.05**. **Qeyd:** ilkin sənəddəki "r = 0.06"
  iddiası ilə **praktik olaraq üst-üstə düşür**.
- **Size ↔ Rating Korrelyasiyası:** r = **0.04**. **Qeyd:** ilkin sənəddəki "r = 0.01" iddiası
  ilə **istiqamət üzrə üst-üstə düşür** (hər ikisi demək olar sıfıra yaxın, əhəmiyyətsiz
  korrelyasiya), lakin mütləq rəqəm bir qədər fərqlidir.
- **Top-5 Over-Performing Kateqoriya** (rating + installs birgə rank üzrə): EDUCATION,
  HEALTH_AND_FITNESS, GAME, SHOPPING, PHOTOGRAPHY / WEATHER (bərabər).
- **Bonus — Paid Free-dən Əhəmiyyətli Üstün Olduğu Kateqoriyalar** (rating fərqi >0.15 xal):
  NEWS_AND_MAGAZINES (+0.6), EDUCATION (+0.4), ENTERTAINMENT (+0.4), ART_AND_DESIGN (+0.3),
  SHOPPING (+0.2), WEATHER (+0.2).

## 3. Business Insights

1. **Reviews/Installs outlier hədləri ilkin gözləntidən xeyli aşağıdır (73.6K vs güman edilən
   3.7M).** Bu o deməkdir ki, "outlier" statusu əslində orta ölçülü bir app üçün belə əldə
   edilə bilər — 73.6K review-dan yuxarı hər app statistik mənada "kənar dəyər" sayılır. Product
   team üçün bu, benchmark hədlərinin (məsələn "uğurlu app" tərifinin) real datadan çıxarılmalı
   olduğunu göstərir, sırf intuisiya ilə yox.

2. **GAME və FAMILY kateqoriyaları outlier baxımından digərlərindən qat-qat önə çıxır (368 və
   258 outlier).** Bu kateqoriyalarda uğur qeyri-bərabər paylanıb — bir neçə mega-hit bazarın
   böyük payını çəkib aparır. Bu kateqoriyalarda planlaşdırma və proqnozlaşdırma üçün mean
   əvəzinə median göstəricilər istifadə edilməlidir.

3. **"Communication (50M+)" fərziyyəsi median əsaslı analizdə təsdiqlənmir — real median
   yalnız 100,000-dir.** Bu, ilkin qiymətləndirmənin çox güman ki, bir-iki nəhəng app-ın (məs.
   WhatsApp) təsiri altında formalaşdığını göstərir. Kateqoriya səviyyəsində strateji qərarlar
   (məs. hansı bazara giriş) mütləq median/outlier-robust göstəricilərə əsaslanmalıdır, əks
   halda bazarın real ölçüsü şişirdilmiş görünə bilər.

4. **Paid app-ların reytinqi Free-dən aşağı deyil (median hər ikisində 4.3), lakin auditoriya
   100 qat kiçikdir.** Bu, monetizasiya seçimində "paid = aşağı keyfiyyət qavrayışı" riskinin
   real olmadığını, sadəcə miqyas trade-off-unun mövcud olduğunu göstərir — paid strategiya
   böyümə əvəzinə dərinlik (ARPU) hədəfləyən komandalar üçün məntiqlidir.

5. **NEWS_AND_MAGAZINES, EDUCATION və ENTERTAINMENT kimi kateqoriyalarda paid versiyalar
   free-dən nəzərəçarpacaq dərəcədə yüksək reytinqlidir (+0.3–0.6 xal).** Bu niş bazarlarda
   istifadəçilər reklamsız/premium təcrübəyə görə ödəniş etməyə daha meylli görünür — bu
   kateqoriyalarda "freemium-dan paid-ə keçid" təcrübələri sınaqdan keçirilə bilər.


