<properties
   pageTitle="A Visual Studióban az alkalmazások kezelése |} Microsoft Azure"
   description="Visual Studio segítségével létrehozása, kialakítása, csomag, üzembe helyezéséhez és a szolgáltatás háló-alkalmazások és szolgáltatások hibakeresési."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studio segítségével egyszerűsítése írása és a szolgáltatás háló alkalmazások kezelése

Az Azure Service háló alkalmazások és szolgáltatások Visual Studio segítségével kezelheti. Miután beállította [a fejlesztői környezet beállítása](service-fabric-get-started.md), a Visual Studio szolgáltatás háló alkalmazások létrehozását, adja hozzá a services vagy a csomagot, külső.FÜGGV és alkalmazások telepítéséhez a helyi fejlesztési fürt is használhatja.

## <a name="deploy-your-service-fabric-application"></a>A szolgáltatás háló alkalmazások telepítése

Alapértelmezés szerint az alkalmazás telepítése egyesíti egy egyszerű működésbe az alábbi lépéseket:

1. Alkalmazáscsomag létrehozása
2. Alkalmazáscsomag feltöltése a kép áruházból
3. Regisztrálás az alkalmazás típusa
4. Bármelyik eltávolítása szolgáltatásalkalmazás-példányok futtatása
5. Új alkalmazás példány létrehozása

A Visual Studióban nyomja le az **F5 billentyűt** fogja is telepíteni az alkalmazás és a debugger csatolása szolgáltatásalkalmazás-példányok. Alkalmazások telepítése hibakeresés nélkül **Ctrl + F5 billentyűkombinációt** is használhatja, vagy közzéteheti a helyi vagy távoli fürtre a közzététel profil használatával. További tudnivalókért lásd: [Közzététel egy alkalmazást a távoli fürtre Visual Studio segítségével](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Az alkalmazás hibakeresési mód

Alapértelmezés szerint a Visual Studio az alkalmazás típusú meglévő példányok eltávolítja, amikor leállítja a hibakereséshez vagy (Ha telepítette az alkalmazást a debugger csatolása nélkül), amikor telepítsen újra az alkalmazást. Ebben az esetben az alkalmazás az összes adat törlődik. Helyileg hibakeresése során, miközben előfordulhat, hogy szeretné megőrizni, amely már létrehozott egy új verzió, az alkalmazás tesztelésekor, ahhoz, hogy az alkalmazás futását, vagy azt szeretné, hogy az alkalmazás frissítése későbbi hibakeresési munkamenetek. Visual Studio szolgáltatás háló Tools adja meg **Az alkalmazás hibakeresési mód**, mely vezérlők az **F5** el kell távolítania az alkalmazás, hogy tartani az alkalmazást egy hibakeresési munkamenet befejeződik, vagy az alkalmazás későbbi hibakeresési esetében, a frissített, hanem eltávolítását és üzlethez engedélyezése után nevű tulajdonságot.

#### <a name="to-set-the-application-debug-mode-property"></a>Az alkalmazás hibakeresési mód tulajdonság beállítása

1. A helyi menüben a project alkalmazást kattintson a **Tulajdonságok parancsra** (vagy nyomja le az **F4** billentyűt).
2. A **Tulajdonságok** ablak a **Alkalmazás hibakeresési mód** tulajdonsága.

    ![Alkalmazás hibakeresési mód tulajdonság beállítása][debugmodeproperty]

Ezek a rendelkezésre álló beállítások a **Alkalmazás hibakeresési mód** .

1. **Automatikus frissítése**: az alkalmazás továbbra is fennáll, amikor befejeződik a hibakeresési munkamenet futtatásához. A következő **F5** kezeli a telepítési frissítésként végrehajtással figyelt automatikus gyorsan frissíteni az alkalmazás újabb kiadására fűzött dátum karakterlánccal. A frissítési folyamat megőrzi az előző hibakeresési munkamenetben beírt adatokat.

2. **Tartsa alkalmazás**: az alkalmazás a fürt továbbra is fut, amikor befejeződik a hibakeresési munkamenetet. A következő az **F5 billentyűparancs hatására** az alkalmazás törlődik, és a fürt telepítik az újonnan beépített alkalmazást.

3. **Alkalmazás eltávolítása** hatására az alkalmazás eltávolítja a hibakeresési munkamenet befejeződésekor.

**Automatikus** frissítési adatok megőrződik a szolgáltatás háló alkalmazás frissítési funkcióinak alkalmazásával, de azt van beállítva, hogy optimalizálás a biztonsági, hanem teljesítményét. Alkalmazások, és hogyan akkor lehet, hogy frissíthet valós környezetben frissítésével kapcsolatos további tudnivalókért olvassa el a [szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md)című témakört.

![Példa: dátum hozzáfűzi az új alkalmazás verziója][preservedata]

>[AZURE.NOTE] Ez a tulajdonság nem létezik a szolgáltatás háló eszközök 1.1-es verziójával előtt for Visual Studio. 1.1 használja az **Indításkor adatok megtartása** tulajdonság ugyanazt a viselkedést eléréséhez. A "Megtartása alkalmazás" lehetőség a szolgáltatás háló eszközök 1.2-es verziójában jelent meg for Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Szolgáltatás hozzáadása az szolgáltatás háló alkalmazás

Az alkalmazás a bővíthetők adjon hozzá az új szolgáltatásokat.  Győződjön meg arról, hogy a szolgáltatás szerepel-e a alkalmazáscsomag, adja hozzá az **Új háló szolgáltatás...** menüpont a szolgáltatáshoz.

![Az alkalmazás új háló szolgáltatás hozzáadása][newservice]

Jelölje be az alkalmazás hozzáadása háló Service projekttípus, és adjon meg egy nevet a szolgáltatás.  Lásd: [kiválaszthatja a szolgáltatásbeli keretet](service-fabric-choose-framework.md) segít eldönteni, hogy melyik szolgáltatás típusa használni.

![Jelölje be az alkalmazás hozzáadása háló Service projekttípus][addserviceproject]

Az új szolgáltatás a megoldást, és a meglévő alkalmazáscsomag hozzáadva. A szolgáltatás hivatkozásokat és a szolgáltatás alapértelmezett példány hozzáadódik az alkalmazás jegyzék. A szolgáltatás létrejön és elindítása a következő alkalommal, amikor az alkalmazás telepítéséhez.

![Az új szolgáltatás a alkalmazás jegyzék hozzáadódik][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>A szolgáltatás háló alkalmazás csomag

Az alkalmazás és a szolgáltatásai fürt telepítéséhez szüksége-alkalmazáscsomag létrehozása.  A csomag rendezi, az alkalmazás jegyzék, szolgáltatás manifest(s) és más szükséges adott elrendezésű fájlokat.  Visual Studio állítja be, és a csomagot, a project alkalmazás mappában, a "pkg" címtárban kezeli.  **Csomag** kattintás az **alkalmazás** helyi menüből hoz létre, vagy az alkalmazáscsomag frissíti.  Érdemes a teendő, ha az alkalmazás egyéni PowerShell-parancsfájlokat használatával rendszerbe.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Távolítsa el az alkalmazások és a felhő Intézővel alkalmazás típusai

Egyszerű fürt adatkezelési műveletek Intézővel felhő, amely indíthatja el a **Nézet** menüben a Visual Studio végezheti el. Például alkalmazások törlése és alkalmazás különböző típusú helyi vagy távoli fürt leépítése.

![Alkalmazás eltávolítása](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Sokoldalúbb fürt kezelési funkciók [a fürt háló Intézővel megjelenítése](service-fabric-visualizing-your-cluster.md)című témakör tartalmaz.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

- [Szolgáltatás háló alkalmazásmodell](service-fabric-application-model.md)
- [Szolgáltatás háló alkalmazás telepítési](service-fabric-deploy-remove-applications.md)
- [Több környezetekben alkalmazás paramétereinek kezelése](service-fabric-manage-multiple-environment-app-configuration.md)
- [A szolgáltatás háló alkalmazásban](service-fabric-debugging-your-application.md)
- [A fürt megjelenítése szolgáltatás háló Explorerrel](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
