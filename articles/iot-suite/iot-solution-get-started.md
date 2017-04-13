<properties
    pageTitle="Példa MyDriving Azure IoT: rövid útmutató az első |} Microsoft Azure"
    description="Első lépések az alkalmazás, amely egy teljes szeretné, hogy miként tervezővel egy IoT rendszer Microsoft Azure Értékáram-elemzés, gépi tanulási és esemény hubok használatával."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT rendszer: első lépések

MyDriving rendszer mutatja be, a tervezés és telemetriai gyűjt eszközökről, a felhőben adatokat dolgoz fel és gépi tanulási adaptív választ megadására vonatkozik szokásos [Internetes a dolog](iot-suite-overview.md) (IoT) megoldás végrehajtásához. A bemutató adatait a autós utakat, naplózza a telefonra és a adaptereken által gyűjtött adatokat a autós ellenőrző rendszerből származó adatokkal. Visszajelzés küldése a vezetői listastílus összehasonlítva más felhasználók használja ezeket az adatokat.

A valós MyDriving célja az első lépések az egyéni IoT megoldás létrehozása. De előtt, hogy pedig hozzuk működésbe a MyDriving alkalmazás magát – teszt felhasználói csoportunk tagként a. Ezzel kap egy élményét az alkalmazás és a rendszer egy a fogyasztói mögötte előtt, architektúráját elmélyedhet. Azt is bemutatja a HockeyApp, az alfa és béta elosztása az alkalmazások kezelése izgalmas módszer annak ellenőrzésére felhasználók.

## <a name="use-the-mobile-experience"></a>A mobil felület használata

A MyDriving alkalmazást tudja használni, ha egy Android, az iOS és a Windows 10-es eszközön.

### <a name="android-and-windows-10-mobile-installation"></a>Android és Windows 10 Mobile telepítése

Az eszközön:

1.  Fejlesztési alkalmazás letiltása:

    -   Android: A **Beállítások** > **biztonsági**alkalmazás letiltása **Ismeretlen**forrásokból.

    -   Windows 10 esetén: A **Beállítások** > **frissítések** > **Fejlesztők számára**, a **fejlesztői üzemmód**beállításáról.

2.  Bekapcsolódás béta próba csoportunk bejelentkezve- vagy [HockeyApp](https://rink.hockeyapp.net)való bejelentkezéshez. HockeyApp megkönnyíti a alkalmazásban tesztelje a felhasználók a korai kiadásai terjesztése.

    Ha használja a Windows 10, használja a Edge böngészővel.

    Ha egy összeállítás 2016 résztvevő volt, jelentkezzen be az azonos Microsoft fiókjához használt e-mail címével regisztrált a a konferencia a Microsoft gombok segítségével. Már regisztrált a HockeyApp.

    ![HockeyApp bejelentkezési képernyője](./media/iot-solution-get-started/image1.png)

3.  Töltse le és telepítse az alkalmazást a további lehetőségek:

    -   [Android-alapú](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10-ben](http://rink.io/spMyDrivingUWP)

    Két elemek találhatók. A tanúsítvány **Megbízható személyek**telepítése. Ezután tudja telepíteni az alkalmazást.

*Kapcsolatos problémák megoldásához, az alkalmazás elindítása a Windows 10 Mobile?* Lehet, hogy a telefonon frissítés, vagy két mögött. Győződjön meg arról, hogy a legújabb frissítések róla, hogy, illetve telepítheti:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS-telepítés

Ha, felügyelt Build 2016, töltse le az alkalmazást HockeyApp próba csoportunk tagjaként:

1.  IOS-eszközén jelentkezzen be az [HockeyApp](https://rink.hockeyapp.net).
    Használja a Microsoft bejelentkezési gomb, és jelentkezzen be az azonos Microsoft fiók e-mailt, a konferencia regisztrált egyikét. (Nem használható az e-mail címmel és jelszóval mezők.)

    ![HockeyApp bejelentkezési képernyője](./media/iot-solution-get-started/image1.png)

2.  A HockeyApp irányítópulton jelölje ki a MyDriving, és töltse le.

3.  Engedélyezze a HockeyApp bétaverzió:

    egy. Nyissa meg a **Beállítások** > **Általános** > **profilok és a mobileszköz-kezelés.**

    b. A **Bit Stadium GmbH** tanúsítvány megbízható.

Ha Build 2016 nem vehet részt, összeállítása, és az alkalmazások terjesztése, saját maga:

1.   Töltse le a kódot [a GitHub].

2.   Építse fel, és telepítse a [Xamarin]használatával.

A [MyDriving útmutató](http://aka.ms/mydrivingdocs)részletesebb tájékoztatás található.

## <a name="get-an-obd-adapter-optional"></a>Get-OBD kártya (nem kötelező)

Része, amely miatt ez egy valós internetes a dolog, amit rendszer! Nélkül, az alkalmazással, de még egy kis játék a valódi, és ezek nem drága.

Fedélzeti diagnosztika (OBD), a funkció a autó a garázs használó finomhangolása a autós felfelé, és a páratlan zajokat és a figyelmeztetés lámpa diagnosztizálása. Csak a autós remek régiségek, található szoftvercsatornát valahol a kézi általában mögött férni, az irányítópulton csoportban a fedőlapot. A megfelelő összekötő beszerzése a motor teljesítmény mértékek és bizonyos módosításokat. Egy OBD összekötő olcsón vásárolhatja a szokásos helyről. A telefonon alkalmazás Bluetooth vagy Wi-Fi segítségével köti össze.

Ebben az esetben megyünk a autós csatlakoztatása a felhőhöz. A közvetlen kapcsolatot a OBD a mobiltelefonjára, de az alkalmazás működik, mint a továbbítási. A autós telemetriai küldi egyenesen az MyDriving IoT-központban hol feldolgozott jelentkezzen be a közúti utakat és mérje fel, hogy a vezetői stílust.

Csatlakozás egy OBD eszközön:

1.  Ellenőrizze, hogy a autós van-e egy OBD szoftvercsatorna.

2.  Szerezze be a OBD adaptereken:

    -   Ha egy Android és Windows phone használata esetén szüksége egy Bluetooth-kompatibilis OBD II kártya. [BAFX termékek 34t5 Bluetooth OBDII ellenőrzési eszköz]használja azt.

    -   Ha egy IOS rendszerű telefon használata esetén szüksége egy Wi-Fi-kompatibilis OBD kártya. Ez [ScanTool OBDLink MX Wi-Fi: OBD kártya és diagnosztikai képolvasó].

3.  A telefonon szeretne csatlakozni a OBD kártya mellékelt utasításokat követve. Tartsa szem előtt a következőket:

    -   A telefonon a **Beállítások** lapon a Bluetooth-adaptert kell megfeleltetni.

    -   A Wi-Fi-adaptert rendelkeznie kell a tartomány 192.168.xxx.xxx címre.

4.  Ha több autók, egy külön kártya elérheti az egyes (legfeljebb három).

Ha nem egy OBD adaptert, az alkalmazás továbbra is küld adatok helyét és sebességét a telefon GPS címzett a a háttér és megkérdezi, hogy szeretné-e egy OBD hasonlóan.

Talál arról, hogy az alkalmazás hogyan használja a OBD kártya adatait, és hozhat létre saját OBD eszköz szakasz 2.1-es, "IoT eszközök," a [MyDriving útmutató](http://aka.ms/mydrivingdocs)lehetőségekről többet.

## <a name="use-the-app"></a>Az alkalmazás használata

Indítsa el az alkalmazást. Van egy kezdeti quickstart útmutató ismerteti, hogy hogyan működik a.

### <a name="track-your-trips"></a>A utakat nyomon követése

Koppintson a Felvétel gomb (nagy piros kör a képernyő alján) gombra az utazás, és koppintson ismét a végén.

![Ábra: a Felvétel gomb utazása nyomon követése](./media/iot-solution-get-started/image2.png)

Utazás, minden indításakor nincs OBD eszköz esetén, meg kell adnia a simulatort használja használni kívánt.

Utazás végén koppintson a Leállítás gombra, és összefoglaló információk.

![Összefoglaló utazás példája](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Tekintse át a utakat

![Példa egy múltbeli út](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>A profil áttekintése

![Példa a vezetői stílusú profil](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>A próba visszajelzést küldhet nekünk

Mivel MyDriving jumpstart segítségével létrehozott saját IoT rendszerek, bizonyára szeretnénk Örömmel vesszük észrevételeit arról, hogyan jól működik. Tudathatja velünk, ha:

- Felmerülő problémák vagy kihívásokkal kapcsolatban.

- Van egy bővítmény pontra, amely alkalmasabb az Ön esetében tenné.

- Megtalálta elvégezheti az egyes igényeinek hatékonyabb lehetőséget.

- Ha bármely más javaslatok MyDriving vagy a dokumentáció javítására.

Az alkalmazásból MyDriving magát, használhatja a beépített HockeyApp visszajelzés mechanizmusa: iOS és az Android csak ad a telefonon a rázzuk, vagy használja a menü **Visszajelzés** parancsa. Képernyőkép, ez fog automatikusan csatolni, hogy azt tudni fogja, hogy mi beszélgetésben van kapcsolatban. És ha vannak olyan unfortunate összeomlik, HockeyApp gyűjt-e az összeomlást naplók közli velünk a róluk. A visszajelzés a [HockeyApp portálon]keresztül.

Is fájl [GitHub a probléma], vagy hagyja a lenti megjegyzést (hu-hu edition).

Megnézi előre a hallásra Öntől!

## <a name="next-steps"></a>Következő lépések

-   Ismerkedjen meg a [MyDriving útmutató](http://aka.ms/mydrivingdocs) megtudhatja, hogy hogyan azt által tervezett és a teljes MyDriving rendszer beépített.

-   [Létrehozása és helyezhetnek üzembe a rendszer saját](iot-solution-build-system.md) az erőforrás-kezelő Azure parancsfájlok segítségével. A [MyDriving útmutató](http://aka.ms/mydrivingdocs) is végigvezeti Önt területeket, ahol gondoskodnia fog a legtöbb testre szabott funkciók.

  [a GitHub]: https://github.com/Azure-Samples/MyDriving
  [Xamarin használata]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX termékek 34t5 Bluetooth OBDII ellenőrzési eszköz]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX Wi-Fi: OBD kártya és diagnosztikai képolvasó]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp portál]: https://rink.hockeyapp.org
  [a GitHub hiba]: https://github.com/Azure-Samples/MyDriving/issues
