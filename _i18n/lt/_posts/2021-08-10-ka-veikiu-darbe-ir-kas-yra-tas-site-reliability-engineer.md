---
layout: post
title: "Ką veikiu darbe ir kas yra tas Site Reliability Engineer?"
date: 2021-08-10
tags: sre, work, vinted
---

![Tinklo srautas į Consul serverius](/images/2021/consul-traffic.jpg)

Kai draugai ir šeima manes klausia ką veikiu [Vinted](https://www.vinted.com) ir kas yra tas Site Reliability Engineer, būna labai sunku paaiškinti.
Neseniai užbaigiau projektą, kurį naudosiu kaip pavyzdį. Paprastais žodžiais ir beveik be techninių nesąmonių.

Vinted turime tūkstančius serverių, kuriuose veikia skirtingi servisai, atliekantys tam tikrą funkciją.
Krūva serverių gali būti duomenų bazės, kita krūva nuotraukos, dar kažkiek paieškos sistema, dar kitur mokėjimo sistema ir daug kitų servisų.
Kai jų yra tūkstančiai ir jų vieta ar statusas nuolat keičiasi, rankiniu būdų to suvaldyti tampa neįmanoma.
Tam reikia turėti didelį inventorių, kuris padeda tai padaryti automatiškai.

Šią problemą sprendžiame naudodami [HashiCorp Consul](https://www.consul.io/) sistemą, kurios pagalba galima lengvai atrasti norimo serviso adresus.
Užtenka užregistruoti visus adresus ir nukreipti užklausas į Consul. Tada kreipiantis į `vinted.service.consul` adresą, sistema atiduoda serverių sąrašą, kurie yra pasiruošę aptarnauti užklausą.

Problema atsiranda tada, kai produkto veikimas (šiuo atveju Vinted) priklauso nuo kitos sistemos veikimo (šiuo atveju Consul).
Jei sistema yra nepasiekiama, produktas taip pat neveikia.
Užklausa iš vartotojo įrenginio ateina iki Vinted, tada svetainė arba telefono programėlė bando kreiptis į `vinted.service.consul` ir jei Consul neveikia - negalim užbaigti užklausos.

Per praėjusius metus Vinted turėjome 2 incidentus dėl Consul problemų. Kiekvieną incidentą Vinted neveikė arba veikė labai prastai apie valandą laiko.
Taip nutiko todėl, kad mes turėjome seną Consul versiją, kurioje nebuvo įjungtas adresų kešavimas ir daug kitų svarbių nustatymų, neleidžiančių sistemai sugriūti padidėjus srautui.

Buvo aiški problema, kuri tiesiogiai liečia mūsų vartotojus. Komanda man patikėjo spręsti problemą.

Atlikau daug testų, parašiau nemažai kodo kuris konfigūruoja Consul, pakeičiau serverius kuriuose sukasi Consul.

Rezultatas: Consul veikia stabiliai ir nekelia galvos skausmo įmonėje. Bent jau kol kas. Grafike matosi kaip sumažėjo tinklo srautas į Consul serverius.

Tai ką aš veikiu Vinted? Gerinu sistemų veikimą ir taip didinu svetainės (*site*) patikimumą (*reliability*).
