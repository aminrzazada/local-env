# Market Share Shift Analysis Across Decades — Video Game Industry — Hesabat

## 1. Data Hazırlıq Addımları

- **Faylın Yüklənməsi:** `vgsales.csv` yükləndi. Faktiki ölçü: **16,598 sətir, 11 sütun**.
- **Filtrasiya:** `Year` NaN olan **271 sətir** silindi, sonra `Year <= 2016` filtri tətbiq
  olundu (2017–2020 arası az sayda və natamam sətirlər çıxarıldı) → final ölçü **16,323 sətir**.
- **Era Segmentasiyası:** 4 dekad-əsaslı qrup yaradıldı: **1980s** (1980–1989), **1990s**
  (1990–1999), **2000s** (2000–2009), **2010s** (2010–2016). **Qeyd:** ilkin sənəddəki "4 era:
  1980s, 1990s, 2000s, 2010s" iddiası ilə **tam üst-üstə düşür**.
- **Publisher Boşluqları:** `Publisher` sütununda **58 NaN** aşkarlandı — bu sətirlər Genre/
  Platform pivot-larında saxlanıldı, lakin publisher-dominance analizindən çıxarıldı (rank
  edilə bilməyən dəyər).

## 2. Əsas Statistik Göstəricilər

- **1980s-də Genre Liderliyi:** faktiki ən böyük pay **Platform (32.5%)**-dədir, Puzzle
  (16.7%) ikinci, Shooter (15.9%) üçüncü, Action isə **13.6% ilə cəmi 4-cü yerdədir**. **Qeyd:**
  ilkin sənəddəki "1980s-də Action dominantdır (29% pay)" iddiası ilə **üst-üstə düşmür** — nə
  rəqəm (13.6% vs 29%), nə də sıralama (4-cü yer vs #1) uyğun gəlmir. Bu dövrdə əslində Platform
  janrı (məs. Super Mario Bros. kimi oyunlar) bazara hakimdir.
- **2000s-də Shooter Trendi:** Shooter payı 1990s-də 5.4%-dən 2000s-də 9.4%-ə yüksəlib. **Qeyd:**
  ilkin sənəddəki "2000s-də yavaş-yavaş Shooter artır" iddiası **istiqamət üzrə təsdiqlənir**,
  lakin 2000s-də Shooter hələ top-3-ə girmir (Action 18.5%, Sports 17.3%, Misc 10.5% öndədir) —
  əsl sıçrayış yalnız 2010s-də baş verir (18.4%, #2 yer).
- **Platform Market Share — 2000s Lideri:** faktiki lider **PS2, 26.4%** pay ilə. **Qeyd:**
  ilkin sənəddəki "PS2 (2000s-in lideri, **11.8%**)" iddiasında liderlik istiqaməti düzgündür,
  lakin faiz rəqəmi **2.2 dəfə aşağı** göstərilib (11.8% vs faktiki 26.4%).
- **Platform Market Share — 2010s Lideri:** faktiki lider **PS3, 23.9%** pay ilə; X360 isə
  22.1% ilə **2-ci yerdədir**, lider deyil. **Qeyd:** ilkin sənəddəki "X360 (2010s-ə keçiddə
  öndə)" iddiası ilə **üst-üstə düşmür** — X360 güclü mövqedədir, lakin PS3-ü keçməyib.
- **Nintendo Publisher Payı:** 1980s-də **62.4%**, 2010s-də **11.9%**. **Qeyd:** ilkin
  sənəddəki "1980s-də 78%, 2010s-də 12%" iddiasında **2010s rəqəmi demək olar tam üst-üstə
  düşür** (11.9% vs 12%), lakin **1980s rəqəmi əhəmiyyətli dərəcədə fərqlidir** (62.4% vs 78% —
  ~16 xal aşağı). İstiqamət (kəskin düşüş) hər iki halda düzgün təsdiqlənir.
- **Action Janrının Sabitliyi:** faktiki datada Action yalnız **2000s (#1) və 2010s (#1)**-də
  top-3-dədir; 1980s-də 4-cü, 1990s-də isə **5-ci yerdədir** (10.9%, RPG/Racing/Sports-dan
  geridə). **Qeyd:** ilkin sənəddəki "Action janrı hər dekadda top 3-dədir – ən stabil janr"
  iddiası ilə **üst-üstə düşmür** — Action yalnız son iki erada dominant olub, ilk iki erada
  top-3-ə daxil deyil. Ən stabil top-3 janr əslində **Platform**-dur (1980s: #1, 1990s: #1,
  2000s: #7, 2010s: #7 — əslində Platform da 2000s-dən sonra kəskin düşüb, ona görə "ən stabil"
  tərifi faktiki datada heç bir janra tam uyğun gəlmir).
- **Puzzle Janrının Azalması:** 1990s-də **3.0%** → 2010s-də **0.8%**. **Qeyd:** ilkin
  sənəddəki "1990s: 7.2% → 2010s: 1.1%" iddiası ilə **istiqamət üzrə (kəskin azalma) təsdiqlənir**,
  lakin hər iki mütləq rəqəm fərqlidir (1990s: 3.0% vs 7.2%; 2010s: 0.8% vs 1.1%).
- **Bonus — CAGR (1980s→2010s):** ən sürətli böyüyən janrlar Simulation (+418%), Misc (+204%),
  Role-Playing (+172%); Puzzle (−30%) yeganə mənfi CAGR-lı əsas janrdır.
- **Bonus — Regional Dominance:** Role-Playing yeganə janrdır ki, JP/NA nisbəti 1-dən yuxarıdır
  (1.07) — Yaponiya-dominant. Puzzle, Platform və Fighting isə ən yüksək NA/EU nisbətinə malikdir
  — NA-dominant janrlar.

## 3. Business Insights

1. **İlkin fərziyyə ("Action 1980s-də dominant, 29%") faktiki datada təsdiqlənmir — 1980s-in
   həqiqi lideri Platform janrıdır (32.5%).** Bu, tarixi trend analizində erkən dövr məlumatının
   çox vaxt yaddaşdan/təxmindən deyil, ilkin datadan yenidən yoxlanmasının vacibliyini göstərir —
   xüsusilə kiçik seçim ölçüsünə malik dövrlərdə (1980s cəmi 205 sətir) tək bir populyar franşiza
   nəticələri asanlıqla dəyişə bilər.
2. **Platform liderliyi dövr üzrə davamlı sürüşür (NES → PS → PS2 → PS3), lakin ilkin sənəddəki
   dəqiq faiz və "kim öndədir" iddiaları (PS2 11.8% əvəzinə 26.4%; X360 əvəzinə PS3 lider) real
   rəqəmlərdən kənara çıxır.** Strateji planlaşdırma üçün hər zaman təzə pivot-based hesablama
   aparılmalı, əvvəlki hesabatlardakı rəqəmlərə etibar edilməməlidir.
3. **Nintendo-nun tənəzzülü istiqamət etibarilə təsdiqlənir (62% → 12%), lakin başlanğıc nöqtəsi
   ilkin gözləntidən (78%) aşağıdır.** Bu, hətta dominant bir "monopoliya" halında belə dəqiq
   faiz ölçülərinin qərar qəbulu üçün vacib olduğunu göstərir — 62% ilə 78% arasında fərq
   strateji reaksiyanın aqressivliyinə (məs. rəqabət qarşısı investisiya səviyyəsinə) təsir edə
   bilər.
4. **"Ən stabil janr" konsepti faktiki datada Action-a aid edilə bilməz — heç bir janr bütün 4
   erada top-3-də qalmır.** Bunun əvəzinə sənaye "dalğa-əsaslı" trend nümayiş etdirir: hər dekad
   fərqli janr kombinasiyası ilə hakimdir (Platform/Puzzle/Shooter → Platform/RPG/Racing →
   Action/Sports/Misc → Action/Shooter/Sports). Product/publisher strategiyası uzunmüddətli
   "təhlükəsiz" janr fərziyyəsindən çəkinməli, hər dekadın öz trend analizini aparmalıdır.

