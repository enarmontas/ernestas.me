---
layout: post
title: "Kaip veikia slaptažodžiai ir įspėjimas CityBee klientams"
date: 2021-02-16
tags: citybee, security, privacy
comments: true
source:
  name: LinkedIn
  url: https://www.linkedin.com/pulse/kaip-veikia-slapta%C5%BEod%C5%BEiai-ir-%C4%AFsp%C4%97jimas-citybee-ernestas-narmontas-/
---

![CityBee](/images/2021/citybee-openssl.png)

Vakar paaiškėjo, kad buvo nutekinti CityBee klientų duomenys.
Juose yra vardai, pavardės, asmens kodai ir el. pašto adresai. Šalia asmens duomenų yra ir slaptažodžių *hash'ai*.

Straipsnyje paaiškinsiu kas tai yra, kaip veikia slaptažodžiai ir kodėl laikas juos pasikeisti.

Maiša (angl. *hash*) yra slaptažodžio reprezentacija po apdorojimo vienakrypte kriptografine funkcija.
Tarkim `slaptazodis1` apdorojus vienakrypte funkcija naudojant SHA1 algoritmą tampa *hash'u* `b60eb503798153ea0d43c701d7a9e0c9ead739af`.
Paėmus šitą *hash'ą* su juo nelabai ką galime padaryti, nėra algoritmo galinčio pasakyti koks slaptažodis slepiasi po juo, nes jis buvo sukurtas vienakrypte funkcija.
Nėra būdo atgaline kryptimi jo iššifruoti.

Kai jungdamiesi prie sistemos naudojate slaptažodį, jis yra apdorojamas vienakrypte funkcija ir validavimui naudojamas *hash'as*.
Jeigu jis sutampa, jūs prisijungiate prie sistemos. Svarbu suprasti, kad visą laiką tas pats žodis turi tą patį *hash'ą*.

Blogoji žinia ta, kad įprasti žodžiai ir jų kombinacijos jau yra viešose duomenų bazėse. Galima atlikti paiešką ir surasti koks žodis slepiasi po *hash'u*.

Tarkim jei [hashtoolkit.com](hashtoolkit.com) įvesiu [`b60eb503798153ea0d43c701d7a9e0c9ead739af`](https://hashtoolkit.com/decrypt-hash/?hash=b60eb503798153ea0d43c701d7a9e0c9ead739af) man parodys jo reikšmę `slaptazodis1`. Taip yra ne todėl, kad būtų kažkoks algoritmas, galintis iššifruoti, bet tiesiog kažkas jau įkėlė tą žodį ir jo *hash* reprezentaciją.

Šiai problemai spręsti inžinieriai naudoja *salting* (sūdymas?). *Salt'as* yra tam tikras simbolių derinys pridedamas prie jūsų slaptažodžio.
Tarkim, jei jūsų slaptažodis yra `grazusoras`, o sistemos pridedamas *salt'as* yra `j5IcQ3kiQd`, galutinis slaptažodis gaunasi `grazusorasj5IcQ3kiQd`.
Tokio derinio SHA1 *hash* reprezentacija yra `ade4817c9fd16f3a3761dac54c6793e283c50181`.

Kai jungiatės prie sistemos, jūs žinote slaptažodį, sistema žino *salt'ą*.
Jų kombinacija paverčiama *hash'u* ir taip prisijungiate prie sistemos.
Privalumas yra tas, kad net nutekinus duomenis su jais nieko nebūtų galima padaryti, nes nežinoma antra dalis (jūsų slaptažodis).
Viešos duomenų bazės taip pat nieko nerastų, nebent didžiulio atsitiktinumo dėka.
Pabandymui, galima paieškoti
[`ade4817c9fd16f3a3761dac54c6793e283c50181`](https://hashtoolkit.com/decrypt-hash/?hash=ade4817c9fd16f3a3761dac54c6793e283c50181) - jokių rezultatų nėra.
Nors *salt'as* nėra galutinė slaptažodžių saugumo priemonė, tai tik visiškas minimumas apsunkinantis slaptažodžių "iššifravimą".

Blogiausia dalis.

CityBee sistemos nenaudojo *salt'ų*. Visi *hash'ai* esantys nutekintoje duomenų bazėje (pasak forumo narių kuriame įkelta) yra tiesiog vartotojų slaptažodžių reprezentacija.
Jei slaptažodžio *hash* jau yra viešoje duomenų bazėje, labai lengva rasti jo reikšmę.

Mano patarimas?

Bėkit keisti slaptažodžių jeigu naudojote tokius kaip
`labadiena` (SHA1: [`1a33b6316d7b17c32ea65c0495f467e32e6e1423`](https://hashtoolkit.com/decrypt-hash/?hash=1a33b6316d7b17c32ea65c0495f467e32e6e1423)),
`jonas1990` (SHA1: [`26f95bd2f5ff00d1edef8b023cbd94a854ec0a9c`](https://hashtoolkit.com/decrypt-hash/?hash=26f95bd2f5ff00d1edef8b023cbd94a854ec0a9c)) ir pan.

Keiskit ne tik CityBee, bet ir Facebook ar LinkedIn. Visur kur naudojate tą patį el. paštą ir slaptažodį. O tikriausiai naudojate.

Šitas CityBee atvejis parodo kaip svarbu yra turėti unikalų ir iš skirtingų raidžių, skaičių bei simbolių sudarytą slaptažodį.
