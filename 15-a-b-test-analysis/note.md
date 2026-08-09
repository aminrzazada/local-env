# A/B Test Results Analysis — Hesabat

## 1. Data Təmizləmə Addımları
- **Datasetin Yüklənməsi:** `ab_data.csv` (294,478 sətir, 5 sütun: `user_id`, `timestamp`,
  `group`, `landing_page`, `converted`) yükləndi.
- **Qrup Ölçüləri:** control ≈ 147,202, treatment ≈ 147,276 — qruplar demək olar tam
  balanslaşdırılıb (50/50).
- **Misassignment Aşkarlanması:** `group='treatment'` olub `landing_page='old_page'` görənlər
  və `group='control'` olub `landing_page='new_page'` görənlər tapıldı — cəmi **3,893 sətir**.
  Bu sətirlər eksperimentin təmizliyini pozduğu üçün silindi.
- **Təkrarlanan İstifadəçi:** təmizləmədən sonra 1 `user_id`-nin 2 dəfə göründüyü aşkarlandı
  (eyni qrup/səhifə, fərqli timestamp) — təkrar sətir silindi.
- **Son Təmiz Dataset:** **290,584 sətir** (control: 145,274, treatment: 145,310).

## 2. Əsas Statistik Göstəricilər

- **Control Conversion Rate:** **12.04%** (17,489 / 145,274)
- **Treatment Conversion Rate:** **11.88%** (17,264 / 145,310)
- **Mütləq Fərq:** treatment control-dan **-0.16 faiz bəndi (pp)** aşağıdır
- **Nisbi Fərq:** treatment control-a nisbətən **~-1.31%** aşağı performans göstərir
- **Gün üzrə Trend:** hər iki qrupun gündəlik conversion rate-i ~11-13% aralığında təsadüfi
  dalğalanır, sistematik trend və ya konkret günlərə bağlı meyl yoxdur — temporal bias yoxdur
- **Saat üzrə Trend (Bonus):** günün saatı üzrə də hər iki qrup ~10.7-13.1% aralığında təsadüfi
  dalğalanır, sabit sistematik nümunə yoxdur
- **1M İstifadəçi Miqyası (Bonus):** cari fərqə əsasən, bütün trafik yeni səhifəyə
  yönləndirilsəydi, hər 1 milyon istifadəçidə təxminən **1,578 conversion itkisi** baş verərdi

## 3. Business Insights 

1. **Yeni səhifə (treatment) heç bir ölçüdə köhnə səhifəni (control) üstələmir.** Həm mütləq
   (-0.16pp), həm nisbi (-1.31%) fərq mənfi istiqamətdədir — yeni dizayn daha az effektivdir.

2. **Fərq kiçik görünsə də, miqyasda əhəmiyyətlidir.** Yüksək trafikli sayt üçün 0.16pp-lik
   geriləmə milyonlarla istifadəçi səviyyəsində minlərlə itirilmiş conversion deməkdir (1M
   istifadəçidə ~1,578 itki).

3. **Nəticə vaxta bağlı təsadüfi amillə izah olunmur.** Gün və saat üzrə fərq sabit qalır —
   bu, müşahidə olunan geriləmənin müvəqqəti şərtlərdən deyil, səhifənin özündən qaynaqlandığını
   göstərir.

4. **Data keyfiyyəti yoxlanılıb və etibarlıdır.** Misassigned istifadəçilər silinib, qruplar
   ölçücə balanslaşdırılıb, təkrarlar təmizlənib — bu, nəticənin etibarlı əsasa söykəndiyini
   göstərir.

5. **Tövsiyə: yeni səhifə LAUNCH edilməməlidir.** Mövcud (köhnə) səhifə saxlanmalı, əvəzində
   yeni dizaynın ayrı-ayrı elementləri (tam səhifə deyil) ayrıca test edilə bilər ki, geriləmənin
   konkret səbəbi müəyyənləşdirilsin.

