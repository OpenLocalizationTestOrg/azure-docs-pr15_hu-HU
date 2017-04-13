<properties
    pageTitle="Azure mobil tetszés szerint elmélyedhet Android SDK integrációja"
    description="Legújabb frissítésekkel és Azure Mobile tetszés szerint elmélyedhet Android SDK eljárások"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Mobil tetszés szerint elmélyedhet GCM integrálása

> [AZURE.IMPORTANT] Követniük kell a integrációs leírtakat tetszés szerint elmélyedhet integrálni szeretné a hogyan Android dokumentum Ez az útmutató következő előtt.
>
> A dokumentum akkor lehet hasznos, csak akkor, ha már integrált a vannak modul és a Google Play eszközök leküldéses terv. Ha integrálni szeretné vannak kampányok az alkalmazásban, olvassa el először integráció tetszés szerint elmélyedhet elérhetőségét Android-eszközön.

##<a name="introduction"></a>– Bevezetés

GCM integrálása lehetővé teszi, hogy az alkalmazás kell eltolni.

Mindig a SDK tolódik GCM tartalmak számára tartalmaz a `azme` kulcs a adatobjektumok. Így GCM más célra az alkalmazást használ, ha veremmutatót billentyűre alapján szűrhető.

> [AZURE.IMPORTANT] Csak az Android 2.2 rendszerű eszközökön vagy a fent, a Google Play telepítve, és a Google problémákat problémákat háttér kapcsolatra engedélyezve is kell eltolni GCM; használatával Ez a kód biztonságos eszközön nem támogatott (csak a módok használ) azonban integrálhatja.

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>A Google Cloud üzenetküldés projekt létrehozása API használatával

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK-integráció

### <a name="managing-device-registrations"></a>Eszköz regisztrációk kezelése

Az egyes kell küldenie a regisztrációs parancs a Google-kiszolgálók, egyéb esetben ezek nem érhető el.

Egy eszközt is is unregister parancsot a GCM értesítések (az eszköz nem automatikusan regisztrált, ha az alkalmazás eltávolítása).

Ha Ön nem használja a [Google Play SDK] , vagy nem már küld a regisztrációs leképezést saját magát, tetszés szerint elmélyedhet regisztrál az eszköz automatikusan azt teheti meg.

Engedélyezéséhez adja hozzá az alábbiak szerint a `AndroidManifest.xml` fájl, benne a `<application/>` címke:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>A tevékenységek leküldéses szolgáltatás regisztrációs azonosítót kommunikációhoz és értesítéseket

Kommunikáció a regisztrációs azonosítót az eszköz a tetszés szerint elmélyedhet leküldéses szolgáltatás és az értesítéseket, az alábbi módon hozzáadása a `AndroidManifest.xml` fájl, benne a `<application/>` (még ha rekordjait önállóan kezeli eszköz regisztrációk) nyomon követése:

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Győződjön meg róla, alábbi engedélyekkel rendelkezik a `AndroidManifest.xml` (után a `</application>` címke).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Mobil tetszés szerint elmélyedhet hozzáférést a GCM API használatával

Hajtsa végre az [Ez az útmutató](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile tetszés szerint elmélyedhet hozzáférést biztosítani a GCM API használatával.

[A Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
