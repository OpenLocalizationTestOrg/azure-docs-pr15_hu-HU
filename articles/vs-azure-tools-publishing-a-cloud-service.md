<properties
   pageTitle="Közzététel egy felhőalapú szolgáltatásba, az Azure eszközeivel |} Microsoft Azure"
   description="Tudnivalók arról, hogy miként teheti közzé a Azure felhőalapú szolgáltatás projektek Visual Studio segítségével."
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

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Közzététel egy felhőalapú szolgáltatásba, az Azure-eszközök használatával

Microsoft Visual Studio az Azure-eszközök használatával az Azure alkalmazást közvetlenül a Visual Studio közzéteheti. Visual Studio támogatja a fejlesztői vagy az éles üzemi környezetben, egy felhőalapú szolgáltatás integrált közzétételi.

Az Azure alkalmazások közzététele, előtt az Azure előfizetéssel kell rendelkeznie. Is be kell állítania egy felhőalapú szolgáltatás és a tárterület-fiókot az alkalmazás való használatra. Beállíthatja ezeket a az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Ha közzé, a a telepítési környezet jelölje ki a felhőalapú szolgáltatás. Is választania kell a tárterület-fiókot, amelyet a telepítéshez alkalmazáscsomag tárolására szolgál. A telepítés után az alkalmazáscsomag a tárterület-fiók törlődik.

Fejlesztése, és tesztelje az Azure alkalmazások vannak, használhatja a Web üzembe módosítások fokozatosan közzététele a weben szerepkörök. Egy telepítési környezet az alkalmazást a közzététel után webes üzembe lehetővé teszi a módosítások közvetlenül a virtuális gép a webes szerepkör futtató telepítéséhez. Nem rendelkezik csomag, és a teljes Azure alkalmazás minden alkalommal, amikor a webes szerepkör meg a módosítások teszteléséhez frissíteni kívánt közzétételéhez. Az eljárás is érhető el a webes szerepkör módosítások teszteléshez anélkül, hogy az alkalmazás, a telepítési környezet közzétett Várakozás a felhőben.

Az Azure alkalmazás közzététele és használatával szeretné frissíteni egy webes szerepkör webes üzembe, tegye a következőket:

- Közzététel vagy Visual Studio Azure alkalmazás csomagolása

- Frissítés részeként a fejlesztés és tesztelés ciklus webes szerepkör

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Közzététel vagy Visual Studio Azure alkalmazás csomag

Az Azure alkalmazás közzétételekor végezheti el a következő műveletek egyikét:

- Hozzon létre egy szolgáltatás csomagot: segítségével a csomagot, és a szolgáltatás konfigurációs fájl közzététele az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)egy központi telepítési környezet az alkalmazás.

- A Visual Studio Azure projekt közzététele: közzétenni közvetlenül az Azure az alkalmazás, a varázslóval közzététele. Tudnivalókért lásd: [Azure alkalmazás varázsló közzététele](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>A Visual Studio szolgáltatás csomag létrehozása

1. Amikor készen áll a közzététel az alkalmazás, nyissa meg a megoldást Intézőt, nyissa meg a helyi menü az Azure projekt, amely tartalmazza a szerepkörök, és válassza a közzététel lehetőséget.

1. Csak a szolgáltatási csomag létrehozásához tegye a következőket:  

  1. Válassza a helyi menü az Azure projekt **csomagot**.

  1. **Csomag Azure alkalmazás** párbeszédpanelen válassza a szolgáltatás beállításai, amelynek meg szeretné hozzon létre egy csomagot, és válassza a Szerkesztés konfiguráció.

  1. (nem kötelező) Kapcsolja be a távoli asztal az a felhőbeli szolgáltatástól közzététel után, jelölje be az **Összes szerepkörének távoli asztal engedélyezése** jelölőnégyzetet, és válassza a **Beállítások** konfigurálása a távoli asztal. Ha meg szeretné hibakeresése a felhőalapú szolgáltatás, közzététel után, kapcsolja be, a jelöljön ki **Minden szerepkörök távoli Debugger engedélyezése**a távoli hibakereséshez.

      További tudnivalókért lásd: [A távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).

  1. A csomag létrehozásához válassza ki a **csomag** hivatkozásra.

      Fájlkezelő szót jeleníti meg az újonnan létrehozott csomag fájl helyét. Ezt a helyet, hogy használni tudja az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)másolhatja.

  1. A csomag közzététele egy központi telepítési környezet, kell használnia ezen a helyen csomag helyeként hozzon létre egy felhőalapú szolgáltatásba, és a csomag telepítéséhez az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)környezetben.

1. (Nem kötelező) A telepítési folyamatot, a helyi menü, a vonal elem a tevékenység naplója, válassza a **Mégse gombra, és távolítsa el**. Ezzel leállítja a telepítési folyamatot, és törli a telepítési környezet az Azure.

    >[AZURE.NOTE] A telepítési környezet eltávolításához azt telepítését követően az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)kell használnia.

1. (Nem kötelező) Miután a szerepkör-példányok kezdeni, Visual Studio automatikusan jeleníti meg a telepítési környezet a **Cloud Services** csomópontot, a kiszolgáló Intézőben. Itt láthatja az egyes szerepkör-példányok állapotát. [A felhő Intézővel kezelése Azure forrásokban](vs-azure-tools-resources-managing-with-cloud-explorer.md)talál. Az alábbi ábrán látható a szerepkör-példányok, amíg ők is inicializálás állapotú:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Frissítés részeként a fejlesztés és tesztelés ciklus webes szerepkör

Ha az alkalmazás kódmentes infrastruktúra halmaza, de a webes szerepkörök ki kell gyakoribb, frissítése, használhatja a Web üzembe csak egy webes szerepkör frissítése a projektben. Ez a praktikus, ha nem szeretne újraépítéséhez és telepítsen újra a kódmentes dolgozói szerepkörök, vagy ha több webes szerepkörök van, és a webes szerepkörök egyikét a frissíteni kívánt.

### <a name="requirements"></a>Követelmények

Követelményei webes üzembe használata a webes szerepkör frissítése:

- **Fejlesztés és a tesztelés céljából csak:** A módosításai közvetlenül a virtuális gép a webes szerepkör futtató. Ha a virtuális számítógéphez is lehet a rendszer, a módosítások elvesznek, mert a szerepkör a virtuális gép újra az eredeti csomag közzétett használják. Tegye közzé újra az alkalmazást a legutóbbi változások beolvasása a webes szerep.

- **Csak a webes szerepkörök frissíthető:** Nem lehet frissíteni dolgozó szerepkörök. Ezeken kívül nem lehet frissíteni az a webhely role.cs RoleEntryPoint.

- **Csak támogatniuk kell egy példányát a webes szerepkörök:** A telepítési környezet nem használhat bármely webes szerepkör több példányát. Azonban több webes szerepkörök minden csak egy példányával támogatottak.

- **, Engedélyezze a távoli asztali kapcsolat:** Ebben az esetben, hogy a webhely üzembe is csatlakozhat a felhasználó és a jelszavát a virtuális gépen az Internet Information Services (IIS) futtató kiszolgáló szeretne telepíteni, a módosításokat. Ezenkívül előfordulhat, hogy kell csatlakoznia az IIS a virtuális gépen megbízható tanúsítvány hozzáadása a virtuális gépen. (Ez biztosítja, hogy a távkapcsolat az IIS webes üzembe által használt biztonságos.)

Az alábbi eljárás feltételezi, hogy az **Azure-alkalmazások közzététele** varázslót használja.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Ahhoz, hogy a webes telepítése az alkalmazás közzétételekor

1. Ha engedélyezni szeretné a **Webes üzembe engedélyezése** minden webes szerepkörök jelölőnégyzetet, meg kell adnia távoli asztali kapcsolat. Jelölje be a **Távoli asztal engedélyezése** az összes szerepkörének, és adja meg a hitelesítő adatokat, amellyel csatlakozni távolról a **Távoli asztali beállítása** mezőben megjelenő. [A távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md) talál további információt.

1. Engedélyezése webhelyen telepíteni az alkalmazást a webhely szerepkörök, jelölje be a **Webes üzembe engedélyezése minden webes szerepkörök**.

    Egy sárga figyelmeztetés háromszög jelenik meg. Webes Deploy a feltölthető bizalmas adatokat nem ajánlott alapértelmezés szerint egy nem megbízható, önaláírt tanúsítványt használja. Biztonságos ezt a folyamatot, bizalmas adatokhoz van szüksége, ha olyan webes üzembe kapcsolatok használandó SSL-tanúsítvány is hozzáadhat. Ezzel a tanúsítvánnyal kell lennie egy megbízható tanúsítványt. Információ arról, hogy miként ehhez című **Kattintva győződjön webes üzembe biztonságos** a témakör későbbi.

1. Válassza a **Tovább gombra** kattintva jelenítse meg az **Összegzés** képernyőn, és válassza a **Közzététel** a felhőbeli szolgáltatástól telepítéséhez.

    A felhőbeli szolgáltatástól közzé van téve. A virtuális gép létrehozott, így a Web üzembe használható nélkül szeretné frissíteni a webes szerepkörök közzétehető őket az IIS számára engedélyezett távkapcsolat tartalmaz.

    >[AZURE.NOTE] Ha be van állítva egy webes szerepkör egynél több példány, megjelenik egy figyelmeztető üzenetet, arról, hogy a webes szerepkörökhöz csak a csomagot, az alkalmazás közzététele létrehozott az egyik példány korlátozott mindig. Válassza **az OK** gombra. A követelmények szakaszban leírtak csak egy példánya minden szerepkör de egynél több webes szerepkör is.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Használatával szeretné frissíteni a webes szerepkör webes terjesztése

1. Webes üzembe használatához módosíthatja kódot a projekt bármely a Visual Studióban, amelyet közzétenni, majd kattintson a jobb gombbal a megoldás a projekt csomópontjának, és mutasson a **Közzététel**a webhelyen szerepkörök. A **Webhely közzététele** párbeszédpanel jelenik meg.

1. (Nem kötelező) Ha egy megbízható SSL-tanúsítványt az IIS távkapcsolat használható fel, törölheti a **nem megbízható tanúsítvány engedélyezése** jelölőnégyzetet. Olvashat, hogy webes üzembe biztonságos tanúsítvány hozzáadása című **Kattintva ellenőrizze webes üzembe biztonságos** a témakör későbbi.

1. Webes üzembe használatához a közzététel szerkezet van szüksége a felhasználónevet és jelszót, amelyet beállítása a távoli asztali kapcsolat a csomag először közzétételekor.

  1. A **felhasználónév**mezőbe írja be a felhasználónevet.

  1. A **jelszó**mezőbe írja be a jelszót.

  1. (Nem kötelező) Ha a jelszó mentése a profil szeretné, válassza a **jelszó mentése**.

1. A módosításainak közzététele a weben szerepkör, válassza a **Közzététel**lehetőséget.

    Az állapotsorban a **Közzététel lépések**jeleníti meg. A közzétételi befejezése **Közzététel sikeres** jelenik meg. A módosítások most lett telepítve az webes szerepkör a virtuális gépen. Most már elindíthatja az Azure-alkalmazásokat, a módosítások ellenőrzéséhez a Azure környezetben.

### <a name="to-make-web-deploy-secure"></a>Biztonságos webhely üzembe létesítéséhez

1. Webes Deploy a feltölthető bizalmas adatokat nem ajánlott alapértelmezés szerint egy nem megbízható, önaláírt tanúsítványt használja. Biztonságos ezt a folyamatot, bizalmas adatokhoz van szüksége, ha olyan webes üzembe kapcsolatok használandó SSL-tanúsítvány is hozzáadhat. Ezzel a tanúsítvánnyal kell lennie a megbízható tanúsítvány, és egy (hitelesítésszolgáltató) kap.

    Webes üzembe biztonság érdekében a virtuális gépeken az egyes a webes szerepkörök töltse fel a megbízható tanúsítvány, amely a webhelyhez használni kívánt telepítse az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885). Ezzel biztosíthatja, hogy a tanúsítvány bekerül a virtuális gép, amely a webes szerep jön létre, amikor az alkalmazás közzétételét.

1. Egy megbízható hitelesítésszolgáltató tanúsítványát az IIS távkapcsolat használandó hozzáadásához kövesse az alábbi lépéseket:

  1. A virtuális gép a webes szerepkör-kiszolgáló csatlakozik, jelölje ki a webes szerepkör-példányt a **Felhőben Explorer** vagy a **Kiszolgáló Intézőben**, és válassza a **Csatlakozás távoli asztali változatában** parancsot. A lépések részletes leírását a csatlakozás a virtuális géphez, akkor olvassa el [A távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md)olvashat.

      A böngésző kéri töltse le egy. RDP-fájlt.

  1. Az SSL-tanúsítvány felvétele, nyissa meg az alkalmazáskezelési szolgáltatás az IIS-kezelőben. Az IIS-kezelőben SSL engedélyezéséhez **kötések** megnyitása a **művelet** panelen. A **Webhely kötelező hozzáadása** párbeszédpanel jelenik meg. **A Hozzáadás**gombra, és válassza a **típus** legördülő lista HTTPS. Az **SSL-tanúsítvány** listában válassza az SSL-tanúsítvány volna jelentkezett be, a hitelesítésszolgáltató által és az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)feltöltése. További tudnivalókért olvassa el a [Csatlakozási beállítások megadása az alkalmazáskezelési szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=215824)című témakört.

      >[AZURE.NOTE] Ha felvesz egy megbízható hitelesítésszolgáltató tanúsítványát, a sárga figyelmeztetés háromszög eltűnik a **Közzététel varázsló**.

## <a name="include-files-in-the-service-package"></a>A szolgáltatás csomagban fájlok felvétele

Szükség lehet, hogy érhetők el a virtuális gépen szerepkör-készült egyes fájlokat felvenni a szolgáltatás csomagban. Érdemes lehet például az .exe vagy a szolgáltatás csomag indítási parancsfájl által használt .msi fájl. Vagy lehet, hogy szeretne adni egy webes szerepkör vagy dolgozói szerepkör projekt igénylő összeállítás. Ha meg szeretné jeleníteni őket hozzá kell adnia a megoldást az Azure alkalmazás fájlokat.

### <a name="to-include-files-in-the-service-package"></a>Ha meg szeretné jeleníteni a fájlok a szolgáltatás csomagban

1. Az összeállítás szolgáltatás csomagot hozzáadásához kövesse az alábbi lépéseket:

  1. Nyissa meg a projekt csomópontját, a projekt, amely nincs a hivatkozott összeállítás **Solution Explorer** .

  1. Az összeállítás hozzáadása a projekthez, nyissa meg a helyi menü, a **hivatkozások** mappa, és válassza a **Hivatkozás hozzáadása**. A hivatkozás hozzáadása párbeszédpanel jelenik meg.

  1. Válassza ki a hivatkozást, amelyet szeretne hozzáadni, majd válassza az **OK** gombra.

      A hivatkozás bekerül a **hivatkozások** mappában találja a listában.

  1. Nyissa meg a helyi menü a felvett altartomány összeállítás, és válassza a **Tulajdonságok parancsot**. A **Tulajdonságok** ablak.

      Ha meg szeretné jeleníteni Ez a szolgáltatás csomagban, a **helyi példányt lista** összeállítás válassza **Igaz**.

1. Nyissa meg a projekt csomópontját, a projekt, amely nincs a hivatkozott összeállítás **Solution Explorer** .

1. Az összeállítás hozzáadása a projekthez, nyissa meg a helyi menü, a **hivatkozások** mappa, és válassza a **Hivatkozás hozzáadása**. A **Hivatkozás hozzáadása** párbeszédpanel jelenik meg.

1. Válassza ki a hivatkozást, amelyet szeretne hozzáadni, majd válassza az **OK** gombra.

    A hivatkozás bekerül a **hivatkozások** mappában találja a listában.

1. Nyissa meg a helyi menü a felvett altartomány összeállítás, és válassza a **Tulajdonságok parancsot**. A Tulajdonságok ablak.

1. A összeállítás felvenni a szolgáltatás csomagban, a **Helyi példányt** listában válassza a **Igaz**.

1. A szolgáltatás csomag a webhely szerepkör projekthez hozzáadott fájlok használni, nyissa meg a helyi menü, a fájl, és válassza a **Tulajdonságok parancsot**. A **Tulajdonságok** párbeszédpanelen válassza a **Content** a **Művelet összeállítása** legördülő listából.

1. A szolgáltatás csomagban, a dolgozók szerepkör projekthez hozzáadott fájlok felvétele, nyissa meg a helyi menü, a fájl, és válassza a **Tulajdonságok parancsot**. Válassza a **Tulajdonságok** párbeszédpanelen **másolja a vágólapra, ha újabb** a **kimeneti könyvtár hova másolja** mezőben.

## <a name="next-steps"></a>Következő lépések

Ha többet szeretne tudni a közzététel az Azure a Visual Studióban, lásd: [Azure alkalmazás varázsló közzététele](vs-azure-tools-publish-azure-application-wizard.md).
