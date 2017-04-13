<properties
    pageTitle="Jelentkezzen be a több geographies modulok"
    description="A jelentés, amely jelzi, ha két jelentkezzen modulok felhasználók tűnt különböző régiók és a modulokat lehetetlenné megtett ezeket a területek között a felhasználó bejelentkezési között eltelt idő származik."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-from-multiple-geographies"></a>A több geographies bejelentkezések

Ez a jelentés sikeres bejelentkezési bővítmények olyan felhasználótól, ahol két bejelentkezési bővítmények tűnt különböző területei származnak, és a bejelentkezési bővítmények között eltelt idő nem teszi lehetővé a felhasználónak az adott területek között van utazott tartalmaz. Lehetséges okai lehetnek:

- Felhasználó jelszavának megosztás másokkal

- Felhasználó használja a távoli asztali bejelentkezési webböngészőben elindítására

- Egy támadó van fiókba jelentkezett be a felhasználó másik ország

- Felhasználó használja a virtuális Magánhálózati vagy a proxykiszolgáló

- Felhasználó van bejelentkezve különféle eszközökön egyszerre, például az asztali és a mobiltelefonon, és az IP-cím, a mobiltelefonszáma szokatlan.

Ez a jelentés származó találatok jelenjenek meg kell követnie az sikeres bejelentkezési eseményeket, és a bejelentkezési bővítmények, a régiók, ahol a bejelentkezési bővítmények tűnt származik, és a becsült utazási időt között eltelt idő ezeket a területek között. A utazási látható csak egy becsült érték és a tényleges utazási időt, a helyek között eltérhetnek.


![Jelentkezzen be a több geographies modulok](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)
