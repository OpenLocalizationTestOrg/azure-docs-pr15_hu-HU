<properties
   pageTitle="Áttelepítése és a Visual Studio-Azure Felhőszolgáltatásba webalkalmazás közzététele |} Microsoft Azure"
   description="Megtudhatja, hogy miként áttelepítéséhez, és tegye közzé a webalkalmazás az Azure felhőszolgáltatásba Visual Studio segítségével."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Útmutató: áttelepítése és a Visual Studio-Azure Felhőszolgáltatásba webalkalmazás közzététele

Az üzemeltetési szolgáltatásokhoz előnyeit és Azure méretezhetősége érvénybe, érdemes lehet áttelepítéséhez, és tegye közzé a webalkalmazás az Azure felhőszolgáltatásba. Futtathatja a webalkalmazások Azure-ban minimális megváltozott a meglévő alkalmazáshoz.

>[AZURE.NOTE] Ez a témakör az üzembe helyezése a felhőbeli szolgáltatásokhoz, a webhelyek nem. Webhelyek telepítésével kapcsolatos további tudnivalókért lásd: a [Deploy Azure App szolgáltatásban webalkalmazást](./app-service-web/web-sites-deploy.md).

Speciális sablonokat is Visual C# és a Visual Basic támogatott listáját című **Projektsablonok támogatja** a témakör későbbi.

Először engedélyeznie kell a webalkalmazásban Azure Visual Studio alkalmazásból. Az alábbi ábra bemutatja a fő lépéseit, közzétenni a meglévő webes alkalmazás telepítéshez használni az Azure projekt hozzáadásával. Ez a folyamat ad a megoldás Azure projektben, a szükséges webes szerepkörhöz. Alapján, ha rendelkezik project web típusú, a projekt tulajdonságainak összeállítások is frissülnek, ha a szolgáltatás csomaghoz További összeállítások szükséges a telepítéshez.

![Microsoft Azure webalkalmazás közzététele](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] A **Konvertálás**, **Azure felhőalapú szolgáltatás Project konvertálása** parancs csak a megoldás a webes projekt megjelenik. A következő parancs például a megoldás a Silverlight projekt nem érhető el.
Szolgáltatás csomag létrehozása és közzététele az Azure-alkalmazásokat, figyelmeztetések vagy a hiba akkor fordulhat elő. Ezek a figyelmeztetések és hibák segíthet a problémák megoldása, mielőtt beállítaná Azure. Ha például egy hiányzó összeállítási kapcsolatos figyelmeztetés kaphat. További információ arról, hogy miként figyelmeztetéseket hibaként, [az Azure felhőalapú szolgáltatás Project Visual Studio konfigurálása](vs-azure-tools-configuring-an-azure-project.md)című témakört. Ha az alkalmazás összeállítása, indítsa el a számítási irányító helyileg használatával, vagy a projekt közzététele Azure, előfordulhat, hogy a **Hiba lista** ablakban az alábbi hiba: **A megadott path, a fájl nevét, vagy mindkettőt túl hosszúak**. Ez a hiba oka az, hogy teljesen minősített Azure projektnév hossza túl hosszú. A projekt nevét, a teljes elérési útját, beleértve a hosszát több, mint 146 karakter hosszúságú lehet. Például: a teljes projekt nevét, többek között a fájl elérési útja Silverlight alkalmazáshoz készült Azure projekt az: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Előfordulhat, hogy a megoldás áthelyezése egy másik mappába, amelynek a dolgozók rövidebb elérési útját csökkentse a hosszát a teljes projekt nevét.

Áttelepítése és a Visual Studio Azure webalkalmazás közzététele, kövesse az alábbi lépéseket.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Webalkalmazás engedélyezése az Azure történő telepítéséhez

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Ahhoz, hogy az Azure telepítéshez webalkalmazás

1. Engedélyezi a webes alkalmazás telepítéshez való Azure, nyissa meg a helyi menü egy webes projekthez a megoldás, és válassza az Azure környezetben projekt hozzáadása.

    Az alábbi műveletek történnek:

    - Az Azure projektet nevezik `<name of the web project>.Azure` bekerül az alkalmazás a megoldást.

    - A webhelyen a project web szerepkört bekerül az Azure projekthez.

    - A **Helyi példányt** tulajdonság értéke igaz, minden szükséges MVC 2, MVC 3 MVC szerelvények, 4 és a Silverlight üzleti alkalmazások. Ezek az összeállítások ad a telepítéshez használt szolgáltatás csomagot.

  >[AZURE.IMPORTANT] Ha más összeállítások vagy fájlok, ez a webalkalmazás szükséges, ezek a fájlok tulajdonságainak kézzel be kell állítania. A tulajdonságok beállítása olvashat című **Fájlok belefoglalása a szolgáltatás csomagban** a jelen cikk.

  >[AZURE.NOTE] Ha a megoldás **konvertálni**, Azure projektben már létezik egy adott webhelyen a project web szerepkört **Azure felhőalapú szolgáltatás Project átalakítása** nem jelenik meg a helyi menü ehhez a projekthez.

  Ha létre szeretne hozni az egyes webes projektek webes szerepkörök a webalkalmazásban több webes projektek van, az egyes webes projektekhez eljárás a lépéseket kell elvégeznie. Ezzel létrehozott minden egyes webes szerepkör külön Azure projektek. Minden egyes webes projekt külön-külön tehetők közzé. Azt is megteheti manuálisan vehet egy másik webes szerepkör Azure projekt a webalkalmazásban. Ehhez nyissa meg a helyi menü, a **szerepkörök** mappa a Azure projektben, és válassza a **Hozzáadás gombra**, majd a **Webes szerepkör projekt megoldást**, válassza a webes szerep hozzáadása a projekthez, és kattintson az **OK** gombra.

## <a name="use-an-azure-sql-database-for-your-application"></a>Az alkalmazás Azure SQL-adatbázis használata

Az SQL Server-adatbázishoz, amely a helyszíni használó webalkalmazáshoz Ha egy kapcsolati karakterláncot, módosítania kell a kapcsolati karakterlánc, egy példányának SQL-adatbázis használata Azure tároló helyette.

>[AZURE.IMPORTANT] Az előfizetés kell lehetővé teszi az SQL-adatbázis használata. Az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)szeretne elérni az előfizetést, eldöntheti, mely szolgáltatásokat nyújtja előfizetését. Az alábbi lépéseket a végleges [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)vonatkoznak. Ha az [Azure portálon](http://portal.microsoft.com)használja, a következő eljárással szakaszhoz.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>A webes szerepkörben használandó egy SQL-adatbázis-példány a kapcsolati karakterlánc

1. Egy példányának SQL-adatbázis létrehozása az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885), kövesse a következő cikkét: [Hozzon létre egy SQL-adatbázis kiszolgálót](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] A tűzfalszabályokat az SQL-adatbázis példányának beállításakor választania kell az **egyéb Azure szolgáltatás a kiszolgáló elérésére engedélyezése** jelölőnégyzetet.

1. A kapcsolati karakterlánc használt SQL-adatbázis egy példányának létrehozásához kövesse a lépéseket a következő szakaszban a következő cikkben: [az SQL-adatbázis létrehozása](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Másolja a vágólapra a kapcsolati karakterlánc használni a ADO.NET kapcsolati karakterláncot, az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)végezze el az alábbi lépéseket.  

  1. Kattintson az **adatbázis** gombra, és nyissa meg az előfizetést, az SQL-adatbázis példányának létrehozására használt csomópontját.

  1. A rendelkezésre álló példányok SQL-adatbázis megjelenítéséhez válassza az **SQL-adatbázisait** csomópontot.

  1. Az adatbázis tulajdonságainak megjelenítéséhez válassza ki az adatbázist. A **Tulajdonságok** nézet jelenik meg.

      >[AZURE.NOTE] Ha nem látszik az a **Tulajdonságok** megtekintése, szükség lehet megnyitni az elválasztó segítségével.

  1. A kapcsolati karakterláncot megjelenítéséhez kattintson a nézet mellett a három pontra (…) gombra.

    A **Kapcsolati karakterláncot** párbeszédpanel jelenik meg.

  1. Másolni szeretné a ADO.NET-kapcsolati karakterláncot, jelölje ki a szöveget, és válassza a a Ctrl + C billentyűket.

  1. A párbeszédpanel bezárásához kattintson a **Bezárás** gombra.

1. Le szeretné cserélni a kapcsolati karakterláncot az adott példányán SQL-adatbázis használata a fájlt, nyissa meg a fájlt, jelölje ki a meglévő kapcsolat-karakterlánc bejegyzést, és válassza a Ctrl + V billentyűket. Az SQL-adatbázis példányának ADO.NET kapcsolati karakterlánc lecseréli a meglévő kapcsolattal.

1. Is hozzá kell adnia a paraméter `MultipleActiveResultSets=True` kapcsolati karakterlánccal. A kapcsolattal kell rendelkeznie a következő formátumban:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Nem kötelező) Egy alternatív módszer a kapcsolati karakterláncot, közvetlenül a fájlt a megváltoztatása az új szakaszt szeretne felvenni a web.config átalakítása attól függően, hogy a build beállításokat, amelyek a szolgáltatás-csomag létrehozása fájlok egyikébe. Nyissa meg a Web.Debug.Config fájlt vagy a Web.Release.Config fájlt. Adja hozzá a következő szakasz be ezt a fájlt:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Mentse a fájlt, Ön által módosított, és az alkalmazás ismételt közzététele.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Az SQL-adatbázis egy példány használata az Azure klasszikus portál használatával

1. Az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)válassza az SQL-adatbázisait csomópontot.

  - Ha megjelenik a használni kívánt SQL-adatbázis példányát, válassza a megnyitáshoz.

  - Ha minden előfordulását eddig nem hozott létre, válassza ki a megfelelő hivatkozásra, és majd hozzon létre egy példány.

1. Nyissa meg, vagy hozzon létre egy adatbázis-példány követően a **Kapcsolati karakterláncot** hivatkozás kiválasztása

1. A lap alján válassza a tűzfal beállításainak és elfogadhatja az alapértelmezett értékeket, vagy állítsa be az értékeket, akkor a hivatkozást.

1. Másolja a vágólapra a ADO.NET csatlakozási karakterlánc, illessze be a web.config fájlt, a régi kapcsolati karakterláncot, a helyszíni adatbázis fölé, és vegye fel `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Azure webalkalmazás közzététele

### <a name="to-publish-a-web-application-to-azure"></a>Azure webalkalmazás közzététele

1. Az alkalmazás kipróbálása helyi kidolgozásában használó az Azure környezetben irányító számítja ki, nyissa meg a a helyi menü az Azure projekthez a webes szerep, és válassza a **indítási projektként beállítása**. Válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: **F5**).

    **Indítsa el a Azure hibakeresési környezetet** párbeszédpanel megnyílik, és az alkalmazás elindul a böngészőben. Indítsa el a különböző típusú webalkalmazás a számítási irányító az adott olvashat a című táblázatban ebben a szakaszban.

1. Az alkalmazás Azure közzététele a szolgáltatásokat beállítani, a Microsoft-fiók és az Azure előfizetéssel kell rendelkeznie. A következő témakörben ismertetett lépésekkel hozhatja létre a szolgáltatások beállítása: [előkészítése, közzététele és a Visual Studio Azure alkalmazások telepítése](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Azure webalkalmazás közzétenni, nyissa meg a helyi menü a webes projekt, és válassza a **Közzététel az Azure**.

    Az **Azure-alkalmazások közzététele** párbeszédpanel megnyílik, és a Visual Studio telepítési folyamatának. Közzétenni az alkalmazás használatáról további információt a **Visual Studio Azure alkalmazás közzététele** [egy felhőalapú szolgáltatásba, az Azure eszközeivel közzétételi](vs-azure-tools-publishing-a-cloud-service.md)című.

    >[AZURE.NOTE] A webalkalmazás az Azure projektből is közzéteheti. Ehhez nyissa meg a helyi menü az Azure projekt, és válassza a **Közzététel**lehetőséget.

1. A telepítés állapotának megtekintéséhez az **Azure tevékenységnapló** ablak tekinthet meg. A napló automatikusan megjelenik a telepítési folyamatot indításakor. Kibonthatja a tevékenység naplója információ megjelenítése a vonal elemre, az alábbi ábrán látható módon:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Nem kötelező) A telepítési folyamatot, nyissa meg a helyi menü, a vonal elem a tevékenység naplója, és válassza a **Mégse gombra, és távolítsa el**. Ezzel leállítja a telepítési folyamatot, és törli a telepítési környezet az Azure.

    >[AZURE.NOTE] A telepítési környezet eltávolításához után lett telepítve az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)kell használnia.

1. (Nem kötelező) A szerepkör-példányok kezdeni, miután Visual Studio automatikusan jeleníti meg a telepítési környezet a **Azure kiszámítania** csomópontot a **Felhőben Explorer** vagy a **Kiszolgáló Intézőben**. További lehetőségek megtekintheti az egyes szerepkör-példányok állapotát.

    Miközben továbbra is a inicializálás állapotban vannak, akkor az alábbi ábrán látható a szerepkör-példányok **Server Explorer** :

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Telepítés után az alkalmazás használatához válassza a üzembe melletti nyílra az **Azure tevékenységnapló** **Befejezett** állapotú megjelenésekor. Azure-ban a webes alkalmazás URL-címe megjelenik. Egy bizonyos típusú webes alkalmazás indítása az Azure olvashat az alábbi táblázatban látható.

    A következő táblázat felsorolja az adott webalkalmazások indítása az Azure vagy el futtatása helyileg használatával az Azure kiszámítania irányító webalkalmazás hibakeresési olvashat:

  	|Webes alkalmazás típusa|Helyi meghajtóra használata a számítási irányító Futtatás/hibakeresési|Az Azure fut.|
  	|---|---|---|
  	|ASP.NET-webalkalmazás|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Kattintson az URL-cím hiperhivatkozás jelenik meg a **telepítési** az **Azure tevékenységnapló** betöltése a böngészőben a Kezdőlap fülre.|
  	|ASP.NET MVC 2 webalkalmazáshoz|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Válassza a URL-cím hivatkozást, a **telepítési** az **Azure tevékenységnapló** betöltése a böngészőben a Kezdőlap lap jelenik meg.|
  	|ASP.NET MVC 3 webalkalmazáshoz|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Válassza a URL-cím hivatkozást, a **telepítési** az **Azure tevékenységnapló** betöltése a böngészőben a Kezdőlap lap jelenik meg.|
  	|ASP.NET MVC 4 webalkalmazáshoz|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Válassza a URL-cím hivatkozást, a **telepítési** az **Azure tevékenységnapló** betöltése a böngészőben a Kezdőlap lap jelenik meg.|
  	|ASP.NET üres webalkalmazáshoz|Az alkalmazásban beállított, a kezdőlap beállítása a webes projekthez hozzá kell adnia egy .aspx lapot. A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Ha van egy alapértelmezett .aspx lapot az alkalmazásban, válassza az URL-cím hiperhivatkozás az **Azure tevékenységnapló** jelenik meg a **telepítés** lap, és ezen az oldalon betöltése a böngészőben. Ha egy másik .aspx lapot, nyissa meg a következő formátumban az URL-e adott lapot szeretne:`<url for deployment>/<name of page>.aspx`|
  	|A Silverlight-alkalmazás|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Nyissa meg az adott lapot az alkalmazás a következő formátumban az URL-szükség:`<url for deployment>/<name of page>.aspx`|
  	|A Silverlight vállalati alkalmazás|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Nyissa meg az adott lapot az alkalmazás a következő formátumban az URL-szükség:`<url for deployment>/<name of page>.aspx`|
  	|A Silverlight navigációs alkalmazás|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Nyissa meg az adott lapot az alkalmazás a következő formátumban az URL-szükség:`<url for deployment>/<name of page>.aspx`|
  	|WCF-szolgáltatási alkalmazás|Meg kell a .svc fájlt, a kezdőlap beállítása a WCF-alapú szolgáltatás projekthez. A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Keresse meg a svc fájlt az alkalmazás az URL-címet a következő formátumban kell megadnia:`<url for deployment>/<name of service file>.svc`|
  	|WCF munkafolyamat-szolgáltatási alkalmazás|Meg kell a .svc fájlt, a kezdőlap beállítása a WCF-alapú szolgáltatás projekthez. A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Keresse meg a svc fájlt az alkalmazás az URL-címet a következő formátumban kell megadnia:`<url for deployment>/<name of service file>.svc`|
  	|ASP.NET dinamikus személyek|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Frissítenie kell a kapcsolati karakterlánc (lásd a következő szakaszt). Akkor kell nyissa meg az alkalmazás a következő formátumban az URL-az adott lapot:`<url for deployment>/<name of page>.aspx`|
  	|ASP.NET dinamikus adatok Linq SQL|A menüsávon, válassza a **hibakeresési**, **Indítsa el a hibakeresési** (billentyűzet: válassza ki az **F5** billentyűt.).|Meg kell kövesse a ezt az eljárást: SQL Azure-adatbázis használata az alkalmazás (lásd az ebben a témakörben korábbi szakaszt). Akkor kell nyissa meg az alkalmazás a következő formátumban az URL-az adott lapot:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Frissítés a kapcsolati karakterlánc ASP.NET dinamikus személyek

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Kapcsolati karakterlánc ASP.NET dinamikus szervezetek frissítése

1. ASP.NET dinamikus szervezetek webes alkalmazáshoz használt SQL Azure-adatbázis létrehozásához kövesse a témakör **az alkalmazás SQL Azure-adatbázis használata** eljárást.

1. Adja hozzá a táblák és mezők van szüksége az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)az adatbázist.

1. A kapcsolati karakterláncot, az alkalmazás a következő típusú formátuma a fájlt a következő:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Az alábbi képlettel történik a ADO.NET kapcsolattal az SQL Azure-adatbázis frissítése a *connectionString* értéket:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Mentse a fájlt a kapcsolati karakterláncot, a menüsávon végzett módosításokkal válassza a **fájl**, **web.config menteni**.

## <a name="supported-project-templates"></a>Támogatott projektsablonok

Azure webalkalmazás közzétételéhez az alkalmazást kell használnia a project-sablonok közül C# vagy Visual Basic, az alábbi táblázatban szereplő.

|A Project-sablon csoport|Project-sablon|
|---|---|
|Webes|ASP.NET-webalkalmazás|
|Webes|ASP.NET MVC 2 webalkalmazáshoz|
|Webes|ASP.NET MVC 3 webalkalmazáshoz|
|Webes|ASP.NET MVC4 webalkalmazáshoz|
|Webes|Üres ASP.NET webalkalmazás|
|Webes|ASP.NET MVC 2 üres webalkalmazáshoz|
|Webes|ASP.NET dinamikus adatok szervezetek webalkalmazás|
|Webes|ASP.NET dinamikus adatok Linq SQL webalkalmazáshoz|
|A Silverlight|A Silverlight-alkalmazás|
|A Silverlight|A Silverlight vállalati alkalmazás|
|A Silverlight|A Silverlight navigációs alkalmazás|
|WCF|WCF-szolgáltatási alkalmazás|
|WCF|WCF munkafolyamat-szolgáltatási alkalmazás|
|Munkafolyamat|WCF munkafolyamat-szolgáltatási alkalmazás|

## <a name="next-steps"></a>Következő lépések
További információt a közzétételi témakörben [Közzététel vagy Visual Studio Azure céljával Deploy](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). [A beállítás be nevű hitelesítő](vs-azure-tools-setting-up-named-authentication-credentials.md)is kivétele.
