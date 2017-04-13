<properties 
    pageTitle="Játék alkalmazás Azure Mobile tetszés szerint elmélyedhet végrehajtása"
    description="Játék alkalmazás forgatókönyv Azure Mobile tevékenységek végrehajtásához" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Játék alkalmazással mobil részvételét megvalósítása

## <a name="overview"></a>– Áttekintés

Egy játék indítási elindította az új horgászati témájú alapú role-play/stratégia játék alkalmazás. A játék 6 hónap volt lépéseket. Ez a játék nagyon nagy sikert, és letöltések milliónyi van pedig az adatmegőrzési nagyon magas és egyéb indítási játék alkalmazások összehasonlítása. A negyedéves Véleményezés értekezleten résztvevők elfogadja felhasználónként (ARPU) átlagos bevétel növelése van szükségük. Prémium játék csomagok állnak rendelkezésre, különleges ajánlatok. Ezek játék csomagok engedélyezése a felhasználóknak a Megjelenés és a teljesítmény, hogy horgászati témájú vonalak és lures vagy a játék tackles frissítése. Azonban csomag értékesítés olyan nagyon kicsi. Így azok döntse el, először elemzése a felhasználói felület-elemző eszközzel, és kattintson egy tetszés szerint elmélyedhet kidolgozása értékesítési segítségével növelje a program speciális szegmens.

Alapú [Azure Mobile tetszés szerint elmélyedhet - – első lépések a gyakorlati tanácsok](mobile-engagement-getting-started-best-practices.md) a egy tetszés szerint elmélyedhet stratégia összeállítása.

##<a name="objectives-and-kpis"></a>Célok és fő teljesítménymutatók

A mérkőzés szavakat a fő érdekeltekkel felel meg. Az összes azzal egyik legfontosabb cél - prémium csomag értékesítés növeléséhez 15 %-kal. Ez a cél készíthetnek üzleti fő teljesítménymutatók (KPI-k) mértéket és a meghajtó

* Milyen szinten a játék ezekről a csomagokról vásárolt?
* Mi az a bevétel felhasználónként, minden munkamenetben, heti és havi?
* Mik azok a kedvenc vásárlás típusú?

Rész 1 [– Első lépések](mobile-engagement-getting-started-best-practices.md) a KPI-k és célok meghatározása ismerteti. 

Az üzleti KPI-k most definiálva, és a Mobile termék kezelő tetszés szerint elmélyedhet a KPI-k határozza meg az új felhasználó trendek és az adatmegőrzési hoz létre.

* Adatmegőrzési figyelése és a következő intervallumok használata: napi, 2 naponta, hetente, havonta és három havonként
* Az aktív felhasználók száma
* Az alkalmazás értékelése az áruházban

Műszaki KPI-k felvették informatikai csapattag javaslatok alapján, az alábbi kérdésekre választ:

* Mi az a felhasználói elérési útját (melyik oldal megnyitják, hogy mennyi idő felhasználók töltött azt)
* Összeomlik vagy a hibák feltárása minden munkamenetben száma
* Mely operációs rendszer verziója felhasználóim fut?
* Mi az, hogy a felhasználók számára képernyője átlagos méretének?
* Milyen típusú internetkapcsolattal rendelkeznek a felhasználók számára?

Minden KPI a Mobile termék kezelő Itt adhatja meg az adatokat egyelőre, és hol található a saját playbook.

## <a name="engagement-program-and-integration"></a>Tetszés szerint elmélyedhet program és integrációja

Speciális tetszés szerint elmélyedhet programon összeállítását, mielőtt a Mobile projekt igazgató felelős a projekt mély működésének megismerése, hogy miként, és a felhasználók által felhasznált termékek kell rendelkeznie.

3 hónap után a Mobile projekt igazgató összegyűjtötte elég tartalmasabbá teszi a saját alkalmazás a leküldéses értesítést értékesítési adatok. Hogy megismeri:

* Az első vásárlás általában 14 szintjén történik. A vásárlás azokban az esetekben 90 %-át, olyan új legendary fegyverek $3 értéket.
* Ezekben az esetekben 80 %-át a felhasználók, akik rendelkeznek megvásárolta, folytassa a terméket, és ellenőrizze a további vásárol.
* A felhasználók, akik letelik a szint 20, hogy mennyit legfeljebb 10 USD/hét indítása
* Felhasználók általában 16, 24 és 32 szintjén prémium csomagok vásárlásához.

Köszönetnyilvánító az elemzés a Mobile projekt igazgató úgy dönt, hogy adott leküldéses értesítést sorozatok segítségével növelje a alkalmazás forgalmi létrehozása. Három, amely felhívja ő leküldéses sorozatok létrehoz: üdvözli a program, értékesítési Program és inaktív programot. További információt a [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) hivatkozik
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
