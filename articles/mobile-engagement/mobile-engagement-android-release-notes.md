<properties
    pageTitle="Azure mobil tetszés szerint elmélyedhet Android SDK integrációja"
    description="Legújabb frissítésekkel és Azure Mobile tetszés szerint elmélyedhet Android SDK eljárások"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Kibocsátási megjegyzések

## <a name="423-08102016"></a>4.2.3 (08/10/2016)

- Nincsenek további WIFI zárolása.
- A javítás hivatkozásra egy kölcsönös kizárás init (programhibák 4.2.0 bevezetett) előtt getDeviceId hívásakor.

## <a name="422-05172016"></a>4.2.2 (05/17 vagy 2016)

- Teljesítménybeli javításokat.

## <a name="421-05102016"></a>4.2.1 (2016/05/10)

- Biztonság: webes nézet helyi fájl hozzáférés letiltása lehetőséget.
- Biztonság: eltávolítása `EngagementPreferenceActivity` feleslegessé vált, és nem biztonságos nyúló osztály `PreferenceActivity` osztály.
- Biztonsági: tevékenységek most dokumentált használandó vannak `exported="false"`, a jelző SDK korábbi verziójában is használható.

## <a name="420-03112016"></a>4.2.0 (03/11-es vagy 2016)

- MIT most licencbe a SDK csomagjában talál.
- Lehetővé teszi, hogy egy egyedi eszközazonosítót megadása SDK inicializálni időben.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)

- Teljesítménybeli javításokat.

## <a name="414-01262016"></a>4.1.4 (01/26 és 2016-ban)

- Teljesítménybeli javításokat.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)

- Teljesítménybeli javításokat.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)

- Teljesítménybeli javításokat.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Teljesítménybeli javításokat.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)

- Új jogosultsági modell kezelni az Android M.
- Most konfigurálhatja hely szolgáltatások használata helyett futásidőben `AndroidManifest.xml`.
- A jogosultsági hiba kijavítása: Ha `ACCESS_FINE_LOCATION`, majd `ACCESS_COARSE_LOCATION` már nincs szükség.
- Teljesítménybeli javításokat.

## <a name="400-07062015"></a>4.0.0 (2015/07/06)

-   Belső protokollt változtatásokat, hogy a analytics és leküldéses megbízhatóbb.
-   Natív leküldéses (GCM/OPA) most is használható az alkalmazás, meg kell adnia a leküldéses marketingkampány bármilyen natív leküldéses hitelesítő adatait.
-   Áttekintés értesítés javítása: után tolni éppen megjelenített csak 10s voltak.
-   A hiba megoldása a webes nézet: a hivatkozásra kattintva is végrehajtása az alapértelmezett művelet URL-CÍMÉT.
-   A helyi tároló kezelésével kapcsolatos ritka összeomlik a javítás hivatkozásra.
-   Javítása dinamikus konfigurációs karakterlánc kezelése.
-   EULA frissíteni.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)

-   Azure mobil tetszés szerint elmélyedhet kezdeti kiadását
-   a kapcsolati karakterlánc konfiguráció appId konfigurációs váltja fel.
-   Üzenetek küldése és fogadása tetszőleges XMPP tetszőleges XMPP személyektől származó API eltávolítja.
-   Az eszközök közötti üzenetek küldése és fogadása API eltávolítja.
-   Biztonsági javításokat.
-   Eltávolítja a Google Play és SmartAd nyomon követést.
