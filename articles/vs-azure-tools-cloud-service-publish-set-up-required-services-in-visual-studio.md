<properties
   pageTitle="Felkészülés a közzététele vagy Visual Studio Azure alkalmazások telepítése |} Microsoft Azure"
   description="Ismerje meg a felhőben, tárterület fiók-szolgáltatások beállításához és konfigurálása az Azure eljárásokat."
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

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Felkészülés a közzététele vagy Visual Studio Azure alkalmazások telepítése

## <a name="overview"></a>– Áttekintés

Egy felhőalapú szolgáltatás projekt közzététele, előtt be kell állítania az alábbi szolgáltatásokat:

- Egy **felhőalapú szolgáltatás** az Azure környezetben fut, a szerepkörök

- A **tárterület-fiókot** , amelyet a Blob várólista és táblázat szolgáltatások hozzáférést biztosít.

Az alábbi eljárással állíthatja be az alábbi szolgáltatások, és állítsa be az alkalmazás


## <a name="create-a-cloud-service"></a>Hozzon létre egy felhőalapú szolgáltatásba

Azure közzététele egy felhőalapú szolgáltatásba, először létre kell hoznia egy felhőalapú szolgáltatásba, amely a szerepkörök futtatja az Azure környezetben. A témakör későbbi **létrehozása az Azure klasszikus portál használatával egy felhőalapú szolgáltatásba**, szakaszban leírt módon hozhat létre egy felhőalapú szolgáltatásba az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885). A közzétételi varázsló segítségével a Visual Studióban is létrehozhat egy felhőalapú szolgáltatásba.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Egy felhőalapú szolgáltatásba létrehozása a Visual Studio használatával

1. Nyissa meg a helyi menü az Azure projekt, és válassza a **Közzététel**lehetőséget.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Ha még nem jelentkezett be, a Microsoft-fiókkal vagy szervezeti fiókkal az Azure-előfizetéséhez tartozó, jelentkezzen be a felhasználónevét és jelszavát.

1. Válassza a **Tovább** gombra kattintva jelenítheti meg a **Beállítások** lap.

    ![Közzétételi varázsló közös beállításai](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. A **Cloud Services** listában válassza **Az új létrehozása**. Az **Azure szolgáltatás hozzon létre** párbeszédpanel jelenik meg.

1. Adja meg a felhőalapú szolgáltatás nevét. A nevet a szolgáltatás részét képezi, és ezért globálisan egyedinek kell lennie. Az nevem nem kis-és nagybetűk.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Egy felhőalapú szolgáltatásba létrehozása az Azure klasszikus portál használatával

1. Jelentkezzen be az [Azure klasszikus portált](http://go.microsoft.com/fwlink/?LinkId=253103) a Microsoft webhelyén.

1. (nem kötelező) Már létrehozott felhőalapú szolgáltatások listájának megjelenítéséhez kattintson a Cloud Services hivatkozásra a lap bal szélén.

1. Válassza a **+** ikonra a bal alsó sarkában, és válassza ki a **Felhőalapú szolgáltatás** a megjelenő menüben. Két lehetőség közül, **Gyors létrehozása** és **Egyéni létrehozása**, egy másik képernyő jelenik meg. Ha úgy dönt, hogy **Gyors létrehozása**, létrehozhat egy felhőalapú szolgáltatásba, csak az URL-CÍMÉT és megadásával a régió, ahol fizikailag tárolható. Ha úgy dönt, hogy **Egyéni létrehozása**, egy csomag (.cspkg-fájl), a (.cscfg) konfigurációs fájl és a tanúsítvány megadásával azonnal közzétehet egy felhőalapú szolgáltatásba. Egyéni létrehozása nem szükséges, ha azt szeretné, a felhőalapú szolgáltatás közzétenni az Azure projektben **Közzététel** parancsával. A **Közzététel** parancs a helyi menü Azure projekt érhető el.

1. Válassza a Visual Studio segítségével a felhőalapú szolgáltatás később közzéteendő **Gyors létrehozása** .

1. Adja meg a felhőben szolgáltatásbeli. A teljes URL-CÍMÉT a neve mellett jelenik meg.

1. A listában válassza a régió, ahol a felhasználók többsége találhatók.

1. Az ablak alján válassza a **Felhőalapú szolgáltatás hozzon létre** hivatkozást.

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

A tárterület-fiók a Blob várólista és táblázat szolgáltatások hozzáférést biztosít. A tároló fiók Visual Studio vagy az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkId=253103)használatával hozhat létre.

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Visual Studio segítségével tároló fiók létrehozása

1. Az **Intéző megoldás**nyissa meg a helyi menü a **tárhely** csomópont, és válassza a **Tárterület-fiók létrehozása**.

    ![Azure tároló új fiók létrehozása](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Jelölje ki, vagy írja be az alábbi adatokat az új tárterület-fiók a **Tárterület-fiók létrehozása** párbeszédpanel.
    - A Azure előfizetést, amelyhez a tárterület-fiók hozzáadásához.
    - Az új tároló fiók használni kívánt neve.
    - A terület vagy affinitás csoport (például a nyugati USA-beli vagy kelet-ázsiai).
    - A replikáció tárterület-fiók esetén például Geo felesleges használni kívánt típusát.

1. Amikor elkészült, kattintson a **Létrehozás**lehetőséget. Az új tárterület-fiók megjelenik a **Server Explorer** **tároló** listájában.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Tárterület fiók létrehozása az Azure klasszikus portál használatával

1. Jelentkezzen be az [Azure klasszikus portált](http://go.microsoft.com/fwlink/?LinkId=253103) a Microsoft webhelyén.

1. (Nem kötelező) A tárterület-fiókok megtekintéséhez válassza a **tárterület** -hivatkozás a panelen kattintson a lap bal szélén.

1. Az oldal bal alsó sarkában válassza a **+** ikonra.

1. A megjelenő menüben válassza a **tárhely**, és válassza a **Gyors létrehozásához**.

1. Nevezze el a tárterület-fiókot, amely egy egyedi URL-címe lesz.

1. Nevezze el a felhőalapú szolgáltatás. A teljes URL-CÍMÉT a neve mellett jelenik meg.

1. A régiók listában válassza a egy területet, ahol a felhasználók többsége találhatók.

1. Adja meg, hogy szeretné-e a replikáció geo engedélyezése. Ha engedélyezi a replikáció geo, az adatok több fizikai helyek csökkentse elvesztése lehetőségét program menti. Csökkentheti a költség geo helyfüggő eredményez, mivel később hozzáadása a funkció helyett a tárterület-fiók létrehozásakor, de ez a funkció tároló további drága lesz. További tudnivalókért lásd: [Geo-replikáció](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Az ablak alján válassza a **Tárterület-fiók létrehozása** hivatkozásra.

Miután létrehozta a tárterület-fiókját, látni fogja az URL-címeit, amely az egyes a Azure tároló szolgáltatást, valamint a fiókját az elsődleges és másodlagos hívóbetűk forrásokat is használhatja. Billentyűk segítségével szemben a tároló szolgáltatást kérések hitelesítést végezni.

>[AZURE.NOTE] A másodlagos hívóbetű az azonos hozzáférést biztosít a tárterület-fiókját az access elsődleges kulcsként és jön létre a biztonsági másolatot az elsődleges hívóbetű sérül. Ezenkívül javasolt, hogy a a hívóbetűk rendszeresen újragenerálása. Egy kapcsolat-karakterlánc beállítás használata a másodlagos billentyűt, miközben az elsődleges kulcs követező létrehozásakor módosíthatja is azt, ha szeretné használni, a másodlagos kulcsa újragenerálása regenerált elsődleges kulcsot, majd módosíthatja.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Az alkalmazás használatához a tárterület-fiók által biztosított szolgáltatások konfigurálása

Meg kell adnia a létrehozott Azure tároló szolgáltatások használatához tárolása hozzáférő bármely szerepkör. Ehhez a Azure projekt több szolgáltatás konfiguráció is használhatja. Alapértelmezés szerint két jönnek létre az Azure projekt. Több szolgáltatás konfiguráció használata esetén a kód használata ugyanazt a kapcsolati karakterláncot, de van-e egy másik értéket a kapcsolati karakterlánc minden szolgáltatás konfigurálása. Ha például egy szolgáltatás konfigurációja futtatásához és az alkalmazás helyileg használatával az Azure tároló irányító hibakeresése és egy másik szolgáltatáscsaládba konfigurációja közzétenni az Azure-alkalmazásokat is használhatja. Szolgáltatás konfigurációk kapcsolatos további tudnivalókért olvassa el a [Konfigurálása az Azure Project használatával több szolgáltatás konfigurációk](vs-azure-tools-multiple-services-project-configurations.md)című témakört.

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Az alkalmazás használatához a tárterület-fiók szolgáltatásainak konfigurálása

1. A Visual Studio alkalmazásban nyissa meg azt a Azure megoldást. A megoldás Intézőben nyissa meg a helyi menü egyes szerepkör a Azure projektben, a tároló szolgáltatást hozzáférő, és válassza a **Tulajdonságok parancsot**. A Visual Studio-szerkesztő megjelenik egy lapon a szerepkör a nevet. A mezőket a **beállítás** lapon láthatók.

1. A szerepkör tulajdonságlapok válassza a **Beállítások**elemre.

1. **Konfigurációjának** listájában válassza ki a használni kívánt szolgáltatás beállításai nevét. Ha azt szeretné, ha módosítani szeretné az összes a szerepkör a szolgáltatás beállításait, megadhatja, hogy **Az összes konfiguráció**.  Szolgáltatás beállítások módosításával kapcsolatos további tudnivalókért című **Kapcsolat karakterláncok kezelése a tárterület-fiókok** a következő témakörben [konfigurálása az Azure felhőalapú szolgáltatás a Visual Studio a szerepköröket](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Ha módosítani szeretné bármelyik csatlakozási karakterlánc beállításait, válassza a **…** a beállítás melletti gomb. Ekkor megjelenik a **Tárhely kapcsolati karakterlánc létrehozása** párbeszédpanel.

1. A **Csatlakozás használja**válassza **az előfizetés** lehetőséget.

1. Az **előfizetés** listában válassza az előfizetés. A választható előfizetések nem tartalmazza a használni kívánt, ha a **Publish Settings letöltési** hivatkozás kiválasztása

1. A **fiók neve** listában válassza a tárterület-fiókja nevére. Azure eszközök tároló fiók hitelesítő adatait automatikusan beolvassa a .publishsettings fájl használatával. Adja meg manuálisan a tárterület-fiókja hitelesítő adatait, válassza a **manuális megadott hitelesítő adatok** lehetőséget, és hajtsa ezt az eljárást. A tároló fiók nevét, és az elsődleges kulcs az [Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=213885)elérheti. Ha nem szeretne megadni a tárhely Fiókbeállítások manuálisan, válassza az **OK** gombra kattintva zárja be a párbeszédpanelt.

1. Kattintson az **Enter tároló fiók** hitelesítő adatait hivatkozására.

1. A **fiók neve** mezőbe írja be a tárterület-fiókja nevét.

    >[AZURE.NOTE] Jelentkezzen be az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885), és kattintson a **tárolás** gombra. A portál tároló partnerek listáját jeleníti meg. Ha úgy dönt, hogy a fiók, megnyílik, oldal. Ezen a lapon másolhatja a tárterület-fiók nevére. Ha egy korábbi verzióját használja a klasszikus portál tárterület-fiókja nevét a **Tárterület-fiókok** nézetben jelenik meg. Ez a név másolásához emelje ki a **Tulajdonságok** ablak a nézet, és válassza a Ctrl-C billentyűk. A Név beillesztése a Visual Studióban, válassza a **fiók neve** mezőbe, és válassza a Ctrl + V billentyűket.

1. A **fiókkulcs** mezőbe írja be az elsődleges kulcs másolása és illessze be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
    Másolása a következő kulcsot:

    1. Kattintson a megfelelő tárolási fióknak a lap alján a **Kulcsok kezelése** gombra.

    1. A **Hívóbetűk hozzáférés kezelése** lapon jelölje ki a szöveget, az access elsődleges kulcs, és válassza a Ctrl + C billentyűk.

    1. Az Azure-eszközök illessze be a **fiókkulcs** mezőbe a billentyűt.

    1. Választania kell megállapíthatja, hogy miként férhetnek hozzá a szolgáltatást a tárterület-fiókot az alábbi lehetőségek közül:
        - **HTTP használatát**. Ez a beállítás a szabványos. Ha például `http://<account name>.blob.core.windows.net`.
        - A biztonságos kapcsolat **Használata HTTPS** . Ha például `https://<accountname>.blob.core.windows.net`.
        - **Adja meg az egyéni végpontok** az egyes a három szolgáltatásokat. Kattintson az adott szolgáltatás mezőjébe írja be a végpontok.

        >[AZURE.NOTE] Az egyéni végpontok hoz létre, ha összetettebb kapcsolati karakterlánc hozhat létre. A karakterlánc-formátum használata esetén megadhatja a tárhely szolgáltatási végpontok, beleértve az egyéni tartománynevet regisztrálta a tárterület-fiók a Blob szolgáltatással. Is adhat meg csak blob-erőforrások egyetlen tárolóban átengedése aláírás keresztül férhet hozzá. Az egyéni végpontok létrehozásával kapcsolatos további tudnivalókért olvassa el a [Azure tárhely a Csatlakozási_karakterlánc konfigurálása](storage-configure-connection-string.md)című témakört.

1. Kapcsolati karakterlánc a módosítások mentéséhez kattintson az **OK** gombra, és válassza ki a az eszköztáron a **Mentés** gombra. Után a változtatások mentéséhez kaphat a kapcsolati karakterlánc értékét a kódban [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx)használatával. Az alkalmazás Azure közzétételekor válassza a szolgáltatás beállításai, amely tartalmazza az Azure tárterület-fiók a kapcsolati karakterlánc. Az alkalmazás közzététele után ellenőrizze, hogy az alkalmazás a várt módon működik az Azure tárolása ellen

## <a name="next-steps"></a>Következő lépések

További információk a Visual Studio Azure alkalmazások közzététele, olvassa el a [közzétételi egy felhőalapú szolgáltatásba, az Azure eszközeivel](vs-azure-tools-publishing-a-cloud-service.md)című témakört.
