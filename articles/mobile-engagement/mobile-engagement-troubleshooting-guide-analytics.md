<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet hibaelhárítási útmutatójának - elemzés" 
   description="Analytics, figyelés, szegmens és irányítópult problémáinak megoldása az Azure Mobile tetszés szerint elmélyedhet" 
   services="mobile-engagement" 
   documentationCenter="" 
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

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Útmutató Analytics, figyelés, szegmens és irányítópult-problémák elhárítása

Az alábbi táblázat a lehetséges problémákat, hogyan Azure Mobile tetszés szerint elmélyedhet a alkalmazást, az eszközök és a felhasználók adatokat gyűjt a előfordulhatnak.

## <a name="missingdelayed-information"></a>Hiányzó vagy késleltetett információk

### <a name="issue"></a>A probléma
- Információ a Analytics, szegmens vagy irányítópult megjelenő késik.
- Hiányoznak adatok figyelése.
- Hiányzik az analitikai, szegmens vagy irányítópult információ.
- Szerezze meg a szegmens korlátozza.

### <a name="causes"></a>Okok

- Használhatja a Analytics API, a Monitor API, és ellenőrizheti, hogy minden adat, található meg a felhasználói felület szegmensek API keresztül az API-k.
- Ha az Azure Mobile tetszés szerint elmélyedhet SDK nem megfelelően integrált a alkalmazásba majd akkor nem láthatja a Analytics, szegmens, figyelés vagy irányítópultok adatokat.
- Szegmensek nem lehet a létrehozásuk után megváltozik, szakaszokat csak lehet "Klónozott" (a másolt) vagy "semmisíteni" (a törölt). Szakaszokat csak 10 feltétel tartalmazhat.
- Hiányzó adatok felügyelet alól tesztelése legegyszerűbben a próba-eszköz beállítása, távolítsa el, illetve telepítse újra az alkalmazást a vizsgált eszközök.
- Adatok frissítése 24 óránként Analytics, szegmens vagy irányítópultok.
- Új szakaszokat információk addig, amíg a 24 óra, még akkor is, ha a szakasz előző információk alapján létrehozása után nem jeleníthető meg.
- A felhasználói felület analytics-adatok szűrése az ilyen típusú, az alkalmazás (például "lefagy a" név szerint szűrt jelennek meg az 1-es verzió és az alkalmazás 2-es verziójú) verzióját függetlenül minden példák jelennek meg.
- Az időtartamot elemzéséhez alapul a felhasználói beállításokat, a dátumot, egy felhasználóhoz, akinek a telefonhoz helytelenül megadott dátumot sikerült jelenjenek meg a megfelelő időszakon belül.
- Nincs adatok be van jelentkezve a gomb "tesztelése" használatakor kiszolgálóoldali verembe küldi, az adatok csak be van jelentkezve valós leküldéses kampányok.

## <a name="cant-locate-items-in-ui"></a>A felhasználói felületen nem találja az elemek

### <a name="issue"></a>A probléma
- Nem hozható létre a szegmensek egyes beépített alapján, vagy egyéni alkalmazás információ nyomon követése a feltétel.
- Nem találja meg, bizonyos beépített, vagy egyéni alkalmazás információ nyomon követése a Analytics, figyelés és irányítópultok feltétel.
- Nem tudja értelmezni Analytics figyelés, szegmens és irányítópultok adatainak.

### <a name="causes"></a>Okok

- Néhány beépített elemek és az alkalmazás információ címkék csak akkor érhetők leküldéses feltételként, de nem lehet hozzáadott szegmensre vagy látható Analytics figyelés és irányítópult. 
- Beépített elemek és az alkalmazás információ címkék, amelyek nem adhatók hozzá egy szakasz meg kell az egyes marketingkampány feltételeknek kiválasztásával listáját, a telepítő ugyanazt a funkciót, mint egy szakasz alapján.
- Lásd: az analitikai, figyelés, szegmens és irányítópultok szakaszok Azure Mobile tetszés szerint elmélyedhet a felhasználói felület kapcsolatban további segítséget a helyi menük és információt.

## <a name="crash-troubleshooting"></a>Hibaelhárítás összeomlik

### <a name="issue"></a>A probléma
- Alkalmazás összeomlik a Analytics figyelés és irányítópult tetején látható.

### <a name="causes"></a>Okok

- Hibák elhárítása alkalmazás összeomlik a Analytics figyelés és irányítópult látható ellenőrizze, hogy a kibocsátási megjegyzések az ismert problémák a SDK csomagjában talál a korábbi verzióival.
- További kapcsolatos hibák elhárítása alkalmazás összeomlik a próba az alkalmazás van telepítve, és a Eszközazonosítót az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület "Monitor – események" szakaszában keresse meg a esemény végrehajtása. Hajtsa végre az eseményt, amely az alkalmazás összeomlik, és további információt a "Monitor – összeomlik" szakasz Azure Mobile tetszés szerint elmélyedhet a felhasználói felület kikeresése okoz. 

 
