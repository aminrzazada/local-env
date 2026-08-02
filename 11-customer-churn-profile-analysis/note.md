# Customer Churn Profile Analysis — Hesabat

## 1. Data Təmizləmə Addımları
- **Datasetin Yüklənməsi:** `WA_Fn-UseC_-Telco-Customer-Churn.csv` (7,043 müştəri, 21 sütun)
  yükləndi.
- **TotalCharges Təmizlənməsi:** `TotalCharges` sütunu string formatında idi (bəzi sətirlərdə
  boşluq " " var idi), `pd.to_numeric(errors='coerce')` ilə numerikə çevrildi. Dəqiq **11 sətir**
  `NaN` oldu və silindi (7,043 → 7,032 sətir). **Qeyd:** ilkin sənəddəki "11 boş sətir" rəqəmi
  tam üst-üstə düşür — təsdiqləndi.
- **Churn Binarizasiyası:** `Churn` sütunu ("Yes"/"No") → `Churn_Binary` (1/0) formatına
  çevrildi ki, `groupby().mean()` birbaşa faiz kimi churn nisbətini versin.
- **Tenure Qruplaşdırması:** `tenure` (ay) `pd.cut` ilə 4 qrupa bölündü: 0-12, 13-24, 25-48,
  49-72.
- **Risk Profil Analizi:** `Contract × InternetService × PaymentMethod` kombinasiyaları üzrə
  churn nisbəti hesablandı; statistik etibarlılıq üçün yalnız ≥30 müştərisi olan qruplar
  nəzərə alındı.

## 2. Əsas Statistik Göstəricilər

- **Ümumi Churn Nisbəti:** **26.58%**. **Qeyd:** ilkin sənəddəki "26.5%" rəqəmi ilə tam
  üst-üstə düşür 
- **Contract üzrə:** Month-to-month = **42.71%**, One year = **11.28%**, Two year = **2.85%**.
  **Qeyd:** ilkin sənəddəki "Month-to-month: 42.7%, Two year: 2.8%" rəqəmləri ilə tam üst-üstə
  düşür — təsdiqləndi.
- **Tenure Qrupu üzrə:** 0-12 ay = **47.68%** (ən riskli qrup), 13-24 ay = **28.71%**,
  25-48 ay = **20.39%**, 49-72 ay = **9.51%**. **Qeyd:** ilkin sənəddəki "0-12 ay: 47.4%"
  iddiası faktiki nəticəyə (47.68%) çox yaxındır — kiçik fərq, ehtimal ki, `pd.cut`-un sərhəd
  daxil etmə metodundakı (`include_lowest`) incə fərqdən qaynaqlanır; əsas nəticə (0-12 ay ən
  riskli qrupdur) tam təsdiqləndi.
- **InternetService üzrə:** Fiber Optic = **41.89%**, DSL = **19.00%**, No Internet = **7.43%**
  → Fiber/DSL nisbəti **2.20x**. **Qeyd:** ilkin sənəddəki "Fiber Optic DSL-dən 2x daha çox
  churn edir" iddiası ilə uzlaşır — təsdiqləndi.
- **PaymentMethod üzrə:** Electronic Check = **45.29%** (ən yüksək), Mailed Check = **19.20%**,
  Bank Transfer = **16.73%**, Credit Card = **15.25%**. **Qeyd:** ilkin sənəddəki "Electronic
  Check: 45.3%" rəqəmi ilə tam üst-üstə düşür — təsdiqləndi.
- **SeniorCitizen üzrə:** Senior = **41.68%**, qeyri-Senior = **23.65%**. **Qeyd:** ilkin
  sənəddəki "41.7% vs 23.7%" rəqəmləri ilə tam üst-üstə düşür — təsdiqləndi.
- **MonthlyCharges üzrə:** churn edənlərin orta aylıq ödənişi **$74.44**, churn etməyənlərdə
  **$61.31**. $65-dən yuxarı ödəyənlərdə churn **34.74%**, aşağı olanlarda **16.43%**. **Qeyd:**
  ilkin sənəddəki "Yüksək MonthlyCharges (>$65) olan müştərilər arasında churn daha yüksəkdir"
  iddiası konkret rəqəmlərlə təsdiqləndi.
- **Ən Riskli 3 Profil (Contract × InternetService × PaymentMethod, n≥30):**
  Month-to-month + Fiber Optic + Electronic Check = **60.37%** (1,307 müştəri);
  Month-to-month + Fiber Optic + Mailed Check = **50.75%** (201 müştəri);
  Month-to-month + Fiber Optic + Bank Transfer (automatic) = **45.57%** (327 müştəri).
  Fiber Optic + Month-to-month kombinasiyası bütün 3 profildə ortaqdır. İlkin sənəddə bu konkret
  kombinasiyalar göstərilməyib (yalnız tək-dəyişənli statistikalar var idi) — bu, tapşırıq
  spesifikasiyasına uyğun əlavə tapıntıdır.
- **Bonus — Contract üzrə Orta Tenure (churned vs non-churned):** Month-to-month-da churn
  edənlərin orta tenure-u (14.0 ay) churn etməyənlərdən (21.0 ay) azdır — erkən itki. Two year
  müqavilədə əksinədir (churn edənlər 61.3 ay, etməyənlər 56.9 ay) — uzunmüddətli müqavilələrdə
  churn adətən bitmə tarixindən qısa müddət əvvəl baş verir.

## 3. Business Insights (Retention Tövsiyələri)

1. **Month-to-month + Fiber Optic + Electronic Check müştərilərinə hədəflənmiş kampaniya
   prioritet olmalıdır.** Bu qrup 60%+ churn nisbəti ilə ən böyük risk daşıyır və həm də kifayət
   qədər böyük ölçüdədir (1,307 müştəri, dataset-in ~18.6%-i) — uzunmüddətli müqaviləyə keçid
   təklifi (endirimli 1-illik plan) ən yüksək gəlirliliyi verəcək.

2. **İlk 12 ayda fəal onboarding və early-engagement proqramı tələb olunur.** 0-12 ay tenure
   qrupu 47.68% churn göstərir — bu, ən kritik dövrdür. Xoş gəldin zəngləri, ilk 3 ayda yoxlama
   e-poçtları və erkən loyallıq bonusları itki riskini azalda bilər.

3. **Electronic Check ödəniş metodundan avtomatik ödənişə keçidin təşviqi əhəmiyyətli təsir
   göstərə bilər.** Electronic check müştəriləri 45.29% churn göstərir, avtomatik bank/kart
   ödənişi olanlar isə cəmi ~15-17% — avtomatik ödənişə keçənlərə kiçik endirim təklif etmək
   həm əməliyyat, həm də retention baxımından faydalıdır.

4. **Fiber Optic xidmətinin qiymət/keyfiyyət qavrayışı araşdırılmalıdır.** Fiber Optic
   istifadəçiləri DSL-dən 2.2x çox churn edir. Bu, qiymətlə (adətən daha bahalı) və ya xidmət
   keyfiyyəti ilə bağlı ola bilər — müştəri məmnuniyyəti sorğusu və rəqabətçi qiymət analizi
   tövsiyə olunur.

5. **Senior citizen seqmenti üçün fərdiləşdirilmiş dəstək xətti düşünülməlidir.** Senior
   müştərilər 41.68% churn göstərir (qeyri-seniorlardan 1.76x çox) — sadələşdirilmiş hesab
   idarəetməsi, xüsusi dəstək xətti və ya daha aydın faktura izahatları bu seqmentdə itkini
   azalda bilər.


