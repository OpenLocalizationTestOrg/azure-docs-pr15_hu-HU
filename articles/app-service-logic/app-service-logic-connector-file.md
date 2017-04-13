<properties
    pageTitle="A fájl connector használatakor az összefüggés-alkalmazások |} Microsoft Azure alkalmazás szolgáltatás"
    description="Hogyan hozhat létre és a fájl összekötő vagy API-alkalmazás beállítása és vele az Azure alkalmazás szolgáltatás összefüggés-alkalmazásban"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Első lépések a fájl összekötő, és adja hozzá az összefüggés-alkalmazás
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

Csatlakozás egy fájlrendszer tölthet fel, töltse le és más elemek beszúrásával a fájlok a számítógépről. Logika alkalmazások válthat a különféle adatforrásokat és ajánlat összekötők gyűjtéséhez és feldolgozása az adatok alapján. A fájl összekötő is adhat az üzleti munkafolyamatok és a folyamat adatok ezzel a munkafolyamattal összefüggés-alkalmazásból részeként. 

A fájl összekötő a hibrid kapcsolatkezelő a host fájlrendszer hibrid kapcsolatot használja.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Fájl összekötő az összefüggés-alkalmazás létrehozása ##
A fájl összekötő használatához először az összekötő API alkalmazás fájl egy példányának létrehozásához szükséges. Ezt megteheti az alábbi képlettel történik:

1.  Nyissa meg a Microsoft Azure piactéren a + új használatával az Azure-portálra a bal oldalon lehetőséget.
2.  Keressen "a fájl összekötő".
3.  A keresési eredmények közül válassza a **Fájl összekötő (előzetes verzió)** .
4.  Kattintson a **Létrehozás** gombra
5.  Állítsa be a fájl összekötőt a következőképpen:  
![][1]

    - **Név** - adjon egy nevet a fájl-összekötő
    - **Csomagbeállítási**
        - **Gyökérmappa** – adja meg a legfelső szintű mappa elérési útja az állomásgép. Pl.. D:\FileConnectorTest
        - A **szolgáltatás Bus kapcsolati karakterlánc** - adja meg a szolgáltatás Bus kapcsolati karakterlánc. Ellenőrizze, hogy van-e a szolgáltatás bus névtér típusú szabványos, és nem egyszerű szolgáltatás Bus jelfogók felhasználása működéséhez.  Csatlakozás a hibrid kapcsolatkezelő szolgáltatás Bus továbbítási szolgál.
    - **Alkalmazás szolgáltatáscsomagja** - jelölje ki vagy alkalmazás szolgáltatás terv létrehozása
    - A **réteg árak** – az összekötő egy árak szint kiválasztása
    - **Erőforráscsoport** – jelölje be, vagy az összekötő tároló kell erőforrás csoport létrehozása
    - **Előfizetés** - jelöljön ki egy előfizetést, ez az összekötő létrehozni kívánt
    - **Hely** - válassza ki a földrajzi hely, ahol meg szeretné az összekötő telepítése

4. A kattintson a Létrehozás gombra. Létrejön egy új fájlt összekötő

## <a name="configure-hybrid-connection-manager"></a>Hibrid kapcsolatkezelő konfigurálása ##
Miután az API-alkalmazás példány jön létre, tallózással keresse meg annak az irányítópultra.  A Tallózás gombra kattintva erre > API-alkalmazások > jelölje ki azt a fájlt összekötőt API-alkalmazást.  További lehetőségek a hibrid kapcsolatkezelő kell beállítania.
[A hibrid kezelővel]témakörben talál további információt a konfigurálása és a hibaelhárítás a hibrid kapcsolatkezelő.

## <a name="using-the-file-connector-in-your-logic-app"></a>A fájl connector használatakor az logika alkalmazásában ##
Az API-alkalmazás létrehozása után most már használhatja a fájl összekötő műveletet az logika számára. Ehhez kell:

1.  Logika új alkalmazás létrehozása, és válassza a fájl csatlakozóval azonos erőforráscsoport. [Logika új alkalmazás]létrehozása kövesse az utasításokat.

2.  Nyissa meg a "Eseményindítók és a műveletek" logika alkalmazások Tervező megnyitásához, és a forgalom konfigurálása az létrehozott logika alkalmazásból.

3.  A fájl összekötő úgy tűnik, a gyűjteményben kattintson a jobb oldali "API az erőforráscsoport lévő" szakaszában.

4.  A fájl összekötő API-alkalmazást be a szerkesztő gombra kattintva az "fájl összekötő" legördülő. fájl összekötő közzététele egy eseményindító és 4 műveletek:  
![][5]

6.  Az alábbiak mindegyik egyes tulajdonságok közzététele. Az alábbi képen az eseményindító tulajdonságait sorolja fel, és a fájl művelet el:  
![][6]

7. Miután ezek van konfigurálva, a kiváltó ok mező és a művelet használható a folyamat. Hasonlóképpen egyéb műveleteket is beállítható, valamint.

> [AZURE.NOTE] A fájl eseményindító törli a fájlt, miután sikeresen beolvasni a mappát.

## <a name="file-connector-rest-apis"></a>A fájl összekötő REST API-hoz ##
Az összekötő nem egy logikai alkalmazás használatához a REST API-k, az összekötő által elérhetővé tett valószínűleg kihasználják. Akkor is-e API-definíciók Tallózás használatával-Api alkalmazás > Nézet > fájl összekötő. Most kattintson a definíció API lens az összefoglalás területen megtekintése a az összekötő által elérhetővé tett API-k:  
![][7]

Részletek az API-k [API-definíciós fájl összekötő]találhatók.

## <a name="do-more-with-your-connector"></a>További műveletek az összekötő
Most, hogy az összekötő hoz létre, felveheti egy üzleti munkafolyamattal: Ehhez használja egy logikai alkalmazás. Lásd: [logika alkalmazások melyek?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók Ismerkedés az Azure összefüggés-alkalmazásokhoz, [próbálja logika alkalmazás](https://tryappservice.azure.com/?appservice=logic), ahol azonnal létrehozhat egy rövid életű starter logika alkalmazást az alkalmazás szolgáltatás megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

[Összekötők és az API-alkalmazások hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=529766)megtekintése a Swagger REST API hivatkozást.

Tekintse át a teljesítmény statisztika és a vezérlő biztonsági az összekötő is. Lásd: [kezelése és figyelheti a beépített API-alkalmazások és -összekötő](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Logika új alkalmazás létrehozása]: app-service-logic-create-a-logic-app.md
[Összekötő API-definíciós fájl]: https://msdn.microsoft.com/library/dn936296.aspx
[A hibrid kezelővel]: app-service-logic-hybrid-connection-manager.md
