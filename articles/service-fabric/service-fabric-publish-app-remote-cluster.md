<properties
    pageTitle="Az alkalmazás közzététele a Visual Studio távoli fürthöz |} Microsoft Azure"
    description="További információ a távoli szolgáltatás háló fürtre alkalmazás közzététele a Visual Studio segítségével."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Visual Studio segítségével a távoli fürtre alkalmazás közzététele

> [AZURE.SELECTOR]
- [A PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

A Visual Studio Azure Service háló kiterjesztése megoldást jelent egyszerűen megismételhető és parancsfájlok segítségével a szolgáltatás háló fürtre alkalmazás közzététele.

## <a name="the-artifacts-required-for-publishing"></a>A közzétételhez szükséges eltérések

### <a name="deploy-fabricapplicationps1"></a>Üzembe FabricApplication.ps1

Ez a egy PowerShell-parancsprogramot használó egy közzétételi profil paraméterként közzétételi szolgáltatás háló alkalmazásait. Mivel a parancsfájl az alkalmazás része, Ön Üdvözli az alkalmazás szükség szerint módosítsa a.

### <a name="publish-profiles"></a>Profilok közzététele

A szolgáltatás háló alkalmazás projekt **PublishProfiles** nevű mappa XML-fájlok egy alkalmazást, például: közzétételi alapvető információ tárolható tartalmazza:

- Szolgáltatás háló fürt kapcsolat paraméterei
- Alkalmazás paraméter fájl elérési útja
- Frissítési beállítások

Alapértelmezés szerint az alkalmazás automatikusan megtalálható lesz a két profilok közzététele: Local.xml és Cloud.xml. További profilok másolásával és beillesztésével közül az alapértelmezett fájlokat is hozzáadhat.

### <a name="application-parameter-files"></a>Alkalmazás paraméter fájlok

A szolgáltatás háló alkalmazás projekt **ApplicationParameters** nevű mappa a felhasználó által megadott alkalmazás nyilvánvalóan paraméterértékeket XML-fájlok tartalmazza. Alkalmazás jegyzékfájlok is paraméteres, így a különböző értékek telepítési beállítások-et is. Ha többet szeretne tudni az alkalmazás paraméterezése, olvassa el a [több környezetekben a szolgáltatás háló kezelése](service-fabric-manage-multiple-environment-app-configuration.md)című témakört.

>[AZURE.NOTE] Szereplő szolgáltatások a projekt először előtt kísérel meg szerkessze a fájlt egy szerkesztőben, illetve a közzététel párbeszédpanel keresztül kell létrehozni. Ennek az oka jegyzékfájlok része a szerkesztés során jön létre.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Alkalmazás közzététele a háló szolgáltatásalkalmazás közzététele párbeszédpanel használatával

Az alábbi lépéseket a **Háló szolgáltatásalkalmazás közzététele** párbeszédpanelen a Visual Studio szolgáltatás háló Tools által biztosított alkalmazás közzététele szemléltetik.

1. A helyi menü a szolgáltatásalkalmazás háló projekt válassza a **Közzététel...** a **Háló szolgáltatásalkalmazás közzététele** párbeszédpanel megjelenítéséhez.

    ![A ** közzététele szolgáltatás háló alkalmazás ** párbeszédpanel][0]

    A **Target profil** legördülő listában kiválasztott fájl beállítást, kivéve a **verziók cikkét**, minden mentési helyének. Egy meglévő profilhoz újrafelhasználása, vagy hozzon létre egy újat **< profilok kezelése... >** kiválasztása a **cél profil** legördülő listából. Ha úgy dönt, hogy a közzététel profilt, a párbeszédpanel megfelelő mezőibe tartalmának jelennek meg. Bármikor a módosítások mentéséhez, válassza a **Mentés profil** hivatkozásra.    

2. **Internetkapcsolat endpoint** szakaszban adja meg a helyi vagy távoli szolgáltatás háló fürt közzétételi végpontot. Hozzáadása vagy módosítása az internetkapcsolat endpoint, kattintson a **Kapcsolat végpontjának** legördülő listából. A lista mutatja az elérhető szolgáltatás háló fürt kapcsolat végpontokat is közzététele az Azure előfizetés(ek) alapján. Figyelje meg, hogy nem már jelentkezett a Visual Studióban, ha kéri erre.

    Választhat a rendelkezésre álló előfizetések és fürt a fürt kiválasztása párbeszédpanel használata

    ![A ** párbeszédpanelen válassza a szolgáltatás háló fürt **][1]

    >[AZURE.NOTE] Ha szeretné (például a fél fürtre) tetszőleges zárólap közzététele, használatáról a **közzétételi egy tetszőleges fürt végpontra** címszó alatt.

    Miután úgy dönt, hogy egy végpontot, a Visual Studio ellenőrzi a kapcsolatot a kijelölt szolgáltatás háló fürthöz. Ha a fürt nem biztonságos, Visual Studio csatlakozhat hozzá azonnal. Jó helyen jár Ha a fürt biztonságos, kell tanúsítványának telepítése a folytatás előtt a helyi számítógépre. Tájékozódhat [secure kapcsolatainak konfigurálása](service-fabric-visualstudio-configure-secure-connections.md) további információt. Amikor elkészült, válassza az **OK** gombra. A kijelölt fürt **Háló szolgáltatásalkalmazás közzététele** párbeszédpanel jelenik meg.

3. Az **Alkalmazás paraméter fájl** legördülő listában keresse meg az alkalmazás paraméter fájlt. Az alkalmazás paraméter fájl értékei a felhasználó által megadott paraméterekkel az alkalmazás nyilvánvalóan fájlban. Hozzáadása vagy módosítása a paraméter, válassza a **Szerkesztés** gombra. Adja meg vagy módosítsa a paraméter a **Paraméterek** rácson. Amikor elkészült, válassza a **Mentés** gombra.

    ![A ** szerkesztése paramétereinek ** párbeszédpanel][2]

4. Megadhatja, hogy ez tegye közzé a művelet **az alkalmazás frissítése** jelölőnégyzet használata frissítést. Frissítés közzététele műveletek normál különböznek műveletek közzététele. Lásd: a [Szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md) különbségek listáját. Frissítési beállítások megadásához válassza ki a **Frissítési beállítások konfigurálása** hivatkozásra. A frissítési paraméter szerkesztő jelenik meg. [Beállítás háló szolgáltatási kérelem a frissítés](service-fabric-visualstudio-configure-upgrade.md) , ha többet szeretne tudni a frissítési paraméterek című témakört.

5. Válassza a **cikkét verziók...** a **Verziók szerkesztése** párbeszédpanel megjelenítése gombra. Frissítenie kell alkalmazás és a szolgáltatás verziókkal helyébe frissítés. Lásd: a [szolgáltatás háló alkalmazás frissítése az oktatóprogram](service-fabric-application-upgrade-tutorial.md) megtudhatja, hogyan alkalmazás vagy szolgáltatás cikkét verziók hatása a frissítési folyamat.

    ![A ** módosítása verziója ** párbeszédpanel][3]

    Ha az alkalmazás és a szolgáltatás verziók például 1.0.0 vagy numerikus értékeket szemantikai verziószámozás 1.0.0.0 formátumban, válassza a a **automatikusan frissíti az alkalmazás és a szolgáltatás verziók** lehetőséget. Ha úgy dönt, hogy ezt a beállítást, a szolgáltatás és alkalmazás verziószámát automatikusan frissülnek, amikor egy kódot, config vagy adatok csomag verzió frissül. Ha inkább a verziók manuális szerkesztése, törölje a jelölőnégyzet, ez a szolgáltatás letiltásához.

    >[AZURE.NOTE] Projektben szereplő jelenjen meg az összes csomag Naplóbejegyzéshez tartozó hozhat létre a projekt elemeit az a szolgáltatás cikkét fájlokat szeretne.

6. Ha végzett az összes a szükséges beállításokat tartalmazó válassza az alkalmazás a kijelölt szolgáltatás háló fürthöz közzétenni a **Közzététel** gombra. A megadott beállításokat alkalmazza a program a közzétételi folyamat.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Tetszőleges fürt zárólap (beleértve a fél fürt) közzététele

A Visual Studio élmény közzététele egy, az Azure előfizetések egyikével társított távoli fürt közzététel van optimalizálva. Azonban is lehet (például szolgáltatás háló fél fürt) tetszőleges végpontok közzététel közvetlenül a közzététel profil XML szerkesztésével. Két közzététele fentebb ismertetett profilok alapértelmezett –**Local.xml** és **Cloud.xml**– által nyújtott, de Ön üdvözli a különböző környezetekben további profil létrehozása. Például érdemes lehet a külső fürt, talán nevű **Party.xml**közzétételi profil létrehozása.

Ha a nem biztonságos fürtre csatlakozik, szükséges mindössze fürt internetkapcsolat endpoint, például: `partycluster1.eastus.cloudapp.azure.com:19000`. Ebben az esetben a kapcsolat végpont közzététel profilban szeretne valami hasonló látható:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Ha a védett fürtre csatlakozik, is szüksége lesz az ügyfél a helyi tárolóba hitelesítéshez használt tanúsítvány részleteinek megadására. További részletekért olvassa el [a szolgáltatás háló fürtre konfigurálása a biztonságos kapcsolat](service-fabric-visualstudio-configure-secure-connections.md).

  Miután beállította a közzététel profil, hivatkozhat azt a közzététel párbeszédpanel alább látható módon.

  ![Új profil közzététele az közzététele párbeszédpanel][4]

  Figyelje meg, hogy ebben az esetben az új közzététel profil mutat az alapértelmezett alkalmazás paraméter fájlokat közül. Ez a megfelelő, ha azt szeretné, az azonos alkalmazás konfigurációjának közzététele egy számot a környezetek. Ezzel ellentétben, amelyhez ki a közzétenni kívánt környezetben különböző beállításokat kell esetben volna értelme alkalmazás megfelelő paraméter fájlt.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként automatizálhatja a közzétételi folyamat folyamatos integrációs környezetben, olvassa el a [szolgáltatás háló folyamatos integráció beállítása](service-fabric-set-up-continuous-integration.md)című témakört.


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
