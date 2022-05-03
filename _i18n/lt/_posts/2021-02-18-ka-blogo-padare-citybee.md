---
layout: post
title: "Ką blogo padarė CityBee?"
date: 2021-02-18
tags: citybee, security, privacy
comments: true
twitter: narmontas
source:
  name: LinkedIn
  url: https://www.linkedin.com/pulse/k%C4%85-blogo-padar%C4%97-citybee-ernestas-narmontas--1c/
---

![CityBee](/images/2021/citybee-automobilis.jpg)
*CityBee nuotrauka*

Pastebėjau nemažai nuomonių ginančių CityBee, net lyginančių žmonių pasipiktinimą
ir kolektyvinį ieškinį su [raganų medžiokle](https://www.linkedin.com/pulse/kolektyvinis-ie%C5%A1kinys-kuriam-nepritariu-paulius-avizinis)
ar [linčo teismu](https://www.linkedin.com/pulse/citybee-lin%25C4%258Do-teismo-vertinimas-gvidas-kontautas).

Niekas nieko nelinčiuoja, norima tik vieno dalyko: atsakomybės.
Nesuvaidintos, bet atviros ir nuoširdžios atsakomybes, su kuria ateina pripažinimas, atsiprašymas ir pasiruošimas atlyginti padarytą žalą.
Iki šiol girdime tik kažkokius išsisukinėjimus.
Apie komunikaciją per daug neišsiplėsiu, nes tai jau plačiai
[buvo aptarta](https://www.linkedin.com/pulse/pam%25C4%2585stymai-apie-citybee-duomen%25C5%25B3-nutek%25C4%2597jim%25C4%2585-marius-vitkevi%25C4%258Dius).

Iš techninės pusės, tikrai buvo padaryta grubių klaidų.
Nežinau ar tai [NFQ](https://www.nfq.lt/musu-darbai-industriju-patirtis/citybee),
[Squalio](https://squalio.com/stories/citybee/) ar pačių CityBee klaidos, nes visi labai [nusimetinėja](https://squalio.com/stories/citybee/) atsakomybę.

### Viešai prieinamos duomenų bazės kopijos
Duomenų bazės atsarginės kopijos laikomos viešoje
[Azure Blob saugykloje](https://www.linkedin.com/feed/update/urn:li:activity:6767537488685748225).
Atsarginė kopija nebuvo šifruota.
[Šiame straipsnyje](https://www.linkedin.com/pulse/citybee-duomen%C5%B3-nutek%C4%97jimas-ir-azure-rimvydas-velykis) paaiškinama,
kad Microsoft Azure šiuo aspektu yra saugi sistema, tad norint palikti duomenis viešai prieinamus reikia papildomų pastangų.
Kaip ir nebūtų didelės tragedijos, jei bent slaptažodžiai ir asmeniniai duomenys būtų tinkamai šifruojami. Bet buvo kitaip.

### Nebuvo slaptažodžių validavimo
Vartotojai vietoj slaptažodžio naudojo tiesiog savo vardą ar tokius slaptažodžius kaip `citybee`, `nesvarbu`, `nesakysiu` ar `labas123`.
Žinoma, klientai patys kalti šioje vietoje, tačiau sistemoje svarbu turėti gerą slaptažodžio validavimą.
Daugumoje servisų neužsiregistruosite jei vietoj slaptažodžio naudosite savo asmeninius duomenis ar kompanijos pavadinimą.

CityBee validavimo neturėjo bent jau iki 2018 vasario mėn., kai buvo padaryta pavogtų duomenų kopija.
Greičiausiai validavimo nebuvo net iki 2020 balandžio mėn., kai pašalinis žmogus
[naudojosi svetimomis paskyromis](https://www.lrytas.lt/auto/radaras/2020/04/24/news/sulaikytas-asmuo-galimai-jungesis-prie-svetimu-citybee-paskyru-14632643/).
Tada CityBee reagavo geriau: deaktyvavo visų paskyrų slaptažodžius ir paprašė juos pasikeisti.

### Slaptažodžiai šifruojami prastu algoritmu
SHA1 algoritmas yra [laikomas nesaugiu](https://www.computerworld.com/article/3173616/the-sha1-hash-function-is-now-completely-unsafe.html).
Negana to, kartu su šiuo algoritmu nebuvo naudojama "druska" (angl. *salt*) slaptažodžių šifravime.
[Šiame straipsnyje](/lt/kaip-veikia-slaptazodziai-ir-ispejimas-citybee-klientams) plačiau aprašiau kaip tai veikia.
Trumpai: daugumą slaptažodžių galima pasižiūrėti naudojant viešas duomenų bazes.
Viešoje erdvėje buvo pavyzdžių, kad net Seimo narių slaptažodžiai buvo "atkoduoti" ir paaiškėjo, kad jie naudojo savo vardą.
Jei būtų naudojamas patikimesnis slaptažodžių šifravimo algoritmas, šita problema būtų žymiai mažesnė.

### Asmens kodai nešifruojami duomenų bazės eilutėse
Jautrūs asmens duomenys kaip asmens kodas tiesiog laikomi duomenų bazės eilutėse, be jokio šifravimo.
Tokius duomenis reiktų šifruoti atskiru algoritmu ir iššifruoti tik esant reikalui: tarkim vairuotojo pažymėjimo validavimui.
Na gerai, gal absoliuti dauguma įmonių šito nedaro, bet bent jau nelaiko atsarginių kopijų viešai prieinamuose puslapiuose.

### Tai kas čia blogo?
Kai tu valdai tokius duomenis kaip asmens kodai ar vairuotojų pažymėjimai - negali daryti tokių klaidų.
Suprasčiau, jei būtų padaryta tik viena kažkuri klaida. Tai būtų tiesiog žmogiška klaida.

Kol kas sunku kažką užtikrintai teigti, nes galimai yra ir daugiau nutekintų duomenų bei
[krūva neatsakytų klausimų](https://www.linkedin.com/pulse/citybeeleaks-lik%25C4%2599-klausimai-artur-or%25C5%25A1evski).
Iš to ką turime jau dabar, matosi, kad CityBee neapdairiai tvarkė vartotojų duomenims.

Dėl to CityBee turi atsakyti už padarytą žalą. Jie nėra labdaros organizacija kurios reiktų gailėti.
Tai yra verslas remiamas [rizikos kapitalo](https://www.crunchbase.com/funding_round/citybee-719c-series-unknown--ecbc5139).
Judėti greitai su tam tikromis rizikomis yra jų pasirinkimas, tačiau reikia susitaikyti ir su galimomis pasekmėmis bei nuostoliais.

Niekas nesako, kad dabar reiktų CityBee pasmerkti visiems laikams ir nulinčiuoti.
Priešingai, labai tikiuosi jie iš to pasimokys ir teiks paslaugas dar geriau.
Tikiuosi ir žmonės išmoks labiau apsisaugoti internete, nebenaudos savo vardo vietoj normalaus slaptažodžio.

Nieko tam CityBee neatsitiks, gal net sustiprės, kaip rašo
[šio straipsnio](https://www.linkedin.com/pulse/%25C4%25AFvyko-priva%25C4%258Di%25C5%25B3-asmen%25C5%25B3-duomen%25C5%25B3-vagyst%25C4%2597-labai-gerai-liaudanskas) autorius.
