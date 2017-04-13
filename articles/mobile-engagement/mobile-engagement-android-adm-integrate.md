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
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>Tetszés szerint elmélyedhet ADM integrálása

> [AZURE.IMPORTANT] Követniük kell a integrációs leírtakat tetszés szerint elmélyedhet integrálni szeretné a hogyan Android dokumentum Ez az útmutató következő előtt.
>
> A dokumentum akkor lehet hasznos, csak akkor, ha már integrált a gyermekektől modul és a leküldéses Amazon eszköz tervének. Ha integrálni szeretné vannak kampányok az alkalmazásban, olvassa el először integráció tetszés szerint elmélyedhet elérhetőségét Android-eszközön.

##<a name="introduction"></a>– Bevezetés

ADM integrálása lehetővé teszi, hogy az alkalmazás kerül a célba juttatása Amazon Android-eszközön.

Mindig a SDK tolódik ADM tartalmak számára tartalmaz a `azme` kulcs a adatobjektumok. Így ADM más célra az alkalmazást használ, ha veremmutatót billentyűre alapján szűrhető.

> [AZURE.IMPORTANT] Csak Amazon Kindle rendszert futtató telefonokhoz Android 4.0.3 vagy a fenti használhatók Amazon eszköz üzenetküldés; integrálhatja azonban ez a kód biztonságos más eszközökön.

##<a name="sign-up-to-adm"></a>Iratkozzon fel ADM

Ha nem tette meg, engedélyeznie kell ADM a Amazon-fiókban.

Az eljárás a részletes: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Az eljárás befejeztével beolvasása:

-   OAuth hitelesítő adatait (ügyfél-azonosító és egy ügyfél titkos) engedélyezni szeretné a eszközök leküldéses részvételét.
-   Szorosan együtt kell működniük a alkalmazásba API kulcs.

##<a name="sdk-integration"></a>SDK-integráció

### <a name="managing-device-registrations"></a>Eszköz regisztrációk kezelése

Az egyes kell küldenie a regisztrációs parancs a ADM kiszolgálók, egyéb esetben ezek nem érhető el.

Ha már használja a [ADM ügyfél tárban], és már telepítve van az [integrált ADM] közvetlenül látogathat android-sdk-adm-fogadásához.

Ha meg van nem integrált ADM még, tetszés szerint elmélyedhet engedélyezze azt az alkalmazást a egyszerűbb foglalja magában:

Szerkesztés a `AndroidManifest.xml` fájl:

-   Adja hozzá az Amazon névtér, a fájl kell kezdődnie jelennek meg:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Belül a `<application/>` címkézéséhez, ez a szakasz hozzáadása:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Miután megadta az amazon címkét, ha a projekt összeállítása cél alatti Android 2.1 előfordulhat build hiba. Kell használnia, egy **Android 2.1 +** build target (ne aggódjon, továbbra is a `minSdkVersion` értéke 4).
-   Integrálhatja a ADM API-ja kulcs eszközként következő [ezt az eljárást].

Ezután kövesse az utasításokat a következő szakaszok.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Kommunikáció a tetszés szerint elmélyedhet leküldéses szolgáltatás regisztrációs azonosítót és értesítéseket

Kommunikáció a regisztrációs azonosítót az eszköz a tetszés szerint elmélyedhet leküldéses szolgáltatás és az értesítéseket, az alábbi módon hozzáadása a `AndroidManifest.xml` fájl, benne a `<application/>` címke (még ha a használt ADM tetszés szerint elmélyedhet nélkül):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Győződjön meg róla, alábbi engedélyekkel rendelkezik a `AndroidManifest.xml` (előtt a `</application>` címke).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Engedélyek biztosítása tetszés szerint elmélyedhet OAuth hitelesítő adatok

A OAuth hitelesítő adatait (ügyfél-azonosító és ügyfél titkos) tetszés szerint elmélyedhet portálon nyújt.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM ügyfél-tárban]:https://developer.amazon.com/sdk/adm/setup.html
[integrált ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Ez az eljárás]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
