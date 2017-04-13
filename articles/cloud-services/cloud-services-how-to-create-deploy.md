<properties
    pageTitle="Hogyan hozhat létre, és egy felhőalapú szolgáltatás üzembe |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre és üzembe egy felhőalapú szolgáltatásba, a gyors létrehozása módszerrel Azure-ban."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Hogyan hozhat létre és üzembe egy felhőalapú szolgáltatásba

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-create-deploy-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-create-deploy.md)

Az Azure klasszikus portál kétféle módon hozhat létre és üzembe egy felhőalapú szolgáltatásba: **Gyors létrehozása** és **Egyéni létrehozása**.

Ebből a témakörből megtudhatja, hogyan gyors létrehozása módszer segítségével hozzon létre egy új, felhőalapú szolgáltatásba, majd **feltöltése** tölthet fel, és telepíteni egy felhőalapú szolgáltatás csomagot Azure-ban. Ezt a módszert, ha az Azure klasszikus portal segítségével pillanatok alatt elérhető kényelmes hivatkozások befejezéséhez az összes követelmények menet közben. Ha készen áll a felhőalapú szolgáltatás üzembe, létrehozásakor, műveleteket hajthat végre mindkét használatával **Egyéni hozzon létre**egy időben.

> [AZURE.NOTE] Ha közzé szeretné tenni a felhőalapú szolgáltatás a Visual Studio csapat szolgáltatások (VSTS), használja a gyors létrehozása, és majd VSTS történő közzététel beállítása az **Első lépések** vagy az irányítópulton. További tudnivalókért lásd: [Azure használatával Visual Studio csapat szolgáltatásai folyamatos kézbesítési][TFSTutorialForCloudService], vagy olvassa el az **Első lépések** lap súgóját.

## <a name="concepts"></a>Fogalmak
Három összetevők szükségesek, annak érdekében, hogy az Azure-ban egy felhőalapú szolgáltatásba, alkalmazások telepítése:

- **Definiált szolgáltatás**  
  A felhőben definíciós fájl (.csdef) határozza meg, hogy a szolgáltatás modell, beleértve a szerepkörök számát.

- **Szolgáltatás konfigurálása**  
  A felhő konfigurációs fájl (.cscfg) biztosít a felhőben beállításait szolgáltatás és az egyes szerepköreiről, például a szerepkör példányainak száma.

- **A Service csomag**  
  A szolgáltatás csomag (.cspkg) tartalmaz, az alkalmazás kódja és beállításokat és a szolgáltatás-definíciós fájl.
  
Ezek, hogy miként hozhat létre egy csomag kapcsolatban többet is megtudhat [Itt](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Az alkalmazás előkészítése
Mielőtt terjeszthet egy felhőalapú szolgáltatásba, létre kell hoznia a felhőalapú szolgáltatás csomag (.cspkg) az alkalmazás kódját, és egy felhőalapú szolgáltatás konfigurációs fájl (.cscfg). Az Azure SDK nyújt eszközöket előkészítéséhez szükséges telepítési ezeket a fájlokat. Az [Azure letöltési](https://azure.microsoft.com/downloads/) lapjáról, azon a nyelven, amelyben az alkalmazás programkódjának elkészítése inkább a SDK is telepítheti.

Három felhőalapú szolgáltatás szolgáltatásokhoz speciális konfiguráció szolgáltatás csomag exportálása előtt:

- Ha egy felhőalapú szolgáltatásba telepíteni szeretné, hogy Secure Sockets Layer (SSL) a titkosítást, [az alkalmazás beállítása](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) az SSL használja.

- Ha azt szeretné, szerepkör-példányok, [állítsa be a szerepkörök](cloud-services-role-enable-remote-desktop.md) távoli asztali távoli asztali kapcsolat beállítása.

- Ha be szeretné állítani részletes figyelése a felhőalapú szolgáltatás, Azure diagnosztika engedélyezése a felhőalapú szolgáltatást. *Minimális figyelése* (alapértelmezett szintű figyelő) a host operációs rendszerek szerepkör-példányok (virtuális gépeken futó) gyűjtött teljesítmény számláló használja. "Részletes figyelése * gyűjti össze a szerepkör-példányok ahhoz, hogy alkalmazás feldolgozása során előforduló problémák közelebb elemzésének belül teljesítmény adatokon alapuló további mértékek. Hogyan engedélyezhető az Azure diagnosztika, olvassa el [Diagnosztika segítségével, így az Azure-ban](cloud-services-dotnet-diagnostics.md).

Szeretne készíteni egy felhőalapú szolgáltatásba telepített webes szerepkörök vagy dolgozó szerepkörök, [létre](cloud-services-model-and-package.md#servicepackagecspkg)kell a service csomag.

## <a name="before-you-begin"></a>Első lépések

- Ha még nem telepítette az Azure SDK, kattintson a **Telepítse az Azure SDK** megnyitni az [Azure letöltési lapját](https://azure.microsoft.com/downloads/), és töltse le a nyelvhez, amelyben a programkódjának elkészítése inkább a SDK csomagjában talál. (Is később ehhez lehetőséget.)

- Ha bármelyik szerepkör-példányok kér egy tanúsítványt, a tanúsítványok létrehozása Felhőbeli szolgáltatásokhoz .pfx fájl egy titkos kulccsal. Azt is megteheti [Töltse fel a tanúsítványok Azure,](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) létrehozása és telepítése a felhőalapú szolgáltatást.

- Ha a felhőbeli szolgáltatástól affinitás alkalmazás telepítéshez használni, a affinitás csoport létrehozása A felhőalapú szolgáltatásba, és egyéb Azure szolgáltatás üzembe ugyanazon a helyen egy tartomány affinitás alkalmazás is használhatja. A affinitás csoport **Affinitás csoportok** lapon az Azure klasszikus portál **hálózatok** területén hozhat létre.


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Útmutató: hozzon létre egy felhőalapú szolgáltatásba, használja a gyors létrehozása

1. Az [Azure klasszikus portált](http://manage.windowsazure.com/), kattintson az **Új**>**kiszámítania**>**Felhőszolgáltatásba**>**Gyors létrehozásához**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. **URL-CÍMÉT**írja be a nyilvános URL-címét a gyártási környezetekben a felhőalapú szolgáltatás eléréséhez használni altartomány nevét. Az URL-cím gyártási telepítések esetén: http://*myURL*. cloudapp.net.

3. **Régió vagy affinitás csoportot**válassza ki a földrajzi régióban vagy affinitás csoport a felhőbeli szolgáltatástól való telepítéséhez. Ha a felhőalapú szolgáltatás telepítése egyéb Azure szolgáltatás egy régión belüli ugyanazon a helyen szeretné egy affinitás csoport kijelölése

4. Kattintson a **felhőalapú szolgáltatás hozzon létre**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Figyelheti az üzenetmezőbe, az ablak alján a folyamat állapotát.

    A **Cloud Services** terület megjelenik, amelyen az új felhőszolgáltatásba jelenik meg. Ha létrehozott az állapotra változik, felhőalapú szolgáltatás létrehozása sikeresen befejeződött.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Útmutató: feltöltése egy tanúsítványt egy felhőalapú szolgáltatásba

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)kattintson a **Cloud Services**, kattintással jelölje ki azt a felhőalapú szolgáltatást, és kattintson a **tanúsítványok**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. **Töltse fel a tanúsítvány** vagy **feltöltése**gombra.

3. **Fájl**használatával **Tallózás** jelölje ki a tanúsítványt (.pfx fájl).

4. A **jelszó**mezőbe írja be a titkos kulcs a tanúsítvány.

5. Kattintson az **OK gombra** (be van jelölve).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Nézze meg a feltöltés az üzenetterület alább látható módon elért haladás. A feltöltés befejeződött, a tanúsítvány nem adja hozzá a táblához. Az üzenet területen kattintson az OK gombra kattintva zárja be az üzenetet.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Útmutató: egy felhőalapú szolgáltatásba terjesztése

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)kattintson a **Cloud Services**, kattintással jelölje ki azt a felhőalapú szolgáltatást, és válassza az **Irányítópult**.

2. **Töltse fel egy új éles üzemi** vagy **feltöltése**gombra.

3. **Telepítési felirat**írja be az új telepítés – például MyCloudServicev4 nevét.

3. A **csomagot**használatával **Tallózás** jelölje be a szolgáltatás csomag (.cspkg) használni kívánt fájlt.

4. **Konfigurációs**segítségével **Tallózás** jelölje be a szolgáltatás beállítása (.cscfg) használni kívánt fájlt.

5. Ha a felhőbeli szolgáltatástól bármely szerepkörök tartalmazó csak egy példánya, jelölje be a a **üzembe, még akkor is, ha egy vagy több szerepköröket tartalmaz egy példányát** jelölőnégyzetet, ahhoz, hogy a telepítés folytatásához.

    Azure is csak 99.95 készültségi való hozzáférés biztosítása a felhőbeli szolgáltatástól karbantartási és frissítései során Ha minden szerepkör legalább két példánya van. Ha szükséges, után is hozzáadhat további szerepkör-példányok a **méret** lapon a felhőbeli szolgáltatástól rendszerbe. További tudnivalókért olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.

6. Kattintson **az OK gombra** (jelölő) kezdje el a felhőalapú szolgáltatás telepítése.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Az üzenet területen a telepítés állapotának figyelheti meg. Kattintson az OK gombra, ha el szeretné rejteni az üzenetet.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>A sikeres befejezését üzembe ellenőrzése

1. Az **Irányítópult**elemre.

    Az állapot jelenítsen meg, hogy a szolgáltatás nem **fut**.

2. A **fontos**kattintson a webhely URL-CÍMÉT a webböngészőben nyissa meg a felhőalapú szolgáltatás.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Következő lépések

* [A felhőalapú szolgáltatás általános konfigurálása](cloud-services-how-to-configure.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md)beállítása.