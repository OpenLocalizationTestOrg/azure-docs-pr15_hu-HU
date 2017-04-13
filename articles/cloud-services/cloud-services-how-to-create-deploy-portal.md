<properties
    pageTitle="Hogyan hozhat létre, és egy felhőalapú szolgáltatás üzembe |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre és üzembe egy felhőalapú szolgáltatásba, az Azure portálon."
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Hogyan hozhat létre és üzembe egy felhőalapú szolgáltatásba

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-create-deploy-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-create-deploy.md)

Az Azure portál kétféle módon hozhat létre és üzembe egy felhőalapú szolgáltatásba: *Gyors létrehozása* és *Egyéni létrehozása*.

Ez a cikk ismerteti a módszernek az alkalmazását a gyors létrehozásához hozzon létre egy új felhőszolgáltatásba és **feltöltése** tölthet fel, és telepíteni egy felhőalapú szolgáltatás csomagot Azure-ban. Ezt a módszert, ha az Azure portal segítségével pillanatok alatt elérhető kényelmes hivatkozások befejezéséhez az összes követelmények menet közben. Ha készen áll a felhőalapú szolgáltatás üzembe, létrehozásakor, műveleteket hajthat végre mindkét használatával egyéni hozzon létre egy időben.

> [AZURE.NOTE] Ha közzé szeretné tenni a felhőalapú szolgáltatás a Visual Studio csapat szolgáltatások (VSTS), használja a gyors létrehozása, és majd VSTS történő közzététel beállítása az Azure quickstart útmutató vagy az irányítópulton. További tudnivalókért lásd: [Azure használatával Visual Studio csapat szolgáltatásai folyamatos kézbesítési][TFSTutorialForCloudService], vagy olvassa el az **Első lépések** lap súgóját.

## <a name="concepts"></a>Fogalmak
Három összetevőkre van szükség szerint egy felhőalapú szolgáltatásba, az Azure alkalmazások telepítése:

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

- Ha egy felhőalapú szolgáltatásba telepíteni szeretné, hogy Secure Sockets Layer (SSL) a titkosítást, [az alkalmazás beállítása](cloud-services-configure-ssl-certificate-portal.md#modify) az SSL használja.

- Ha azt szeretné, szerepkör-példányok, [állítsa be a szerepkörök](cloud-services-role-enable-remote-desktop.md) távoli asztali távoli asztali kapcsolat beállítása. Ez csak végezheti el a klasszikus portálon.

- Ha be szeretné állítani részletes figyelése a felhőalapú szolgáltatás, Azure diagnosztika engedélyezése a felhőalapú szolgáltatást. *Minimális figyelése* (alapértelmezett szintű figyelő) a host operációs rendszerek szerepkör-példányok (virtuális gépeken futó) gyűjtött teljesítmény számláló használja. További mértékek alapján a teljesítményadatok belül a szerepkör-példányok ahhoz, hogy közelebb elemzésének alkalmazás feldolgozása során előforduló problémák *részletes figyelése* gyűjti össze. Hogyan engedélyezhető az Azure diagnosztika, olvassa el [az Azure engedélyezése diagnosztika](cloud-services-dotnet-diagnostics.md).

Szeretne készíteni egy felhőalapú szolgáltatásba telepített webes szerepkörök vagy dolgozó szerepkörök, [létre](cloud-services-model-and-package.md#servicepackagecspkg)kell a service csomag.

## <a name="before-you-begin"></a>Első lépések

- Ha még nem telepítette az Azure SDK, kattintson a **Telepítse az Azure SDK** megnyitni az [Azure letöltési lapját](https://azure.microsoft.com/downloads/), és töltse le a nyelvhez, amelyben a programkódjának elkészítése inkább a SDK csomagjában talál. (Is később ehhez lehetőséget.)

- Ha bármelyik szerepkör-példányok kér egy tanúsítványt, a tanúsítványok létrehozása Felhőbeli szolgáltatásokhoz .pfx fájl egy titkos kulccsal. A közben [is feltölthet a tanúsítványok Azure]() létrehozását, és a felhőbeli szolgáltatástól telepítését.

- Ha a felhőbeli szolgáltatástól affinitás alkalmazás telepítéshez használni, a affinitás csoport létrehozása A felhőalapú szolgáltatásba, és egyéb Azure szolgáltatás üzembe ugyanazon a helyen egy tartomány affinitás alkalmazás is használhatja. A affinitás csoport **Affinitás csoportok** lapon az Azure klasszikus portál **hálózatok** területén hozhat létre.


## <a name="create-and-deploy"></a>Hozzon létre és telepítsen

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **Új > virtuális gépeken futó**, és görgessen lefelé, és kattintson a **Felhőbeli szolgáltatástól**.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Az adatok lapon megjelenő alján kattintson a **Létrehozás**gombra. 
4. Az új **Felhőszolgáltatásba** lap írjon be egy értéket a **DNS-nevét**.
5. Hozzon létre egy új **Erőforráscsoport** , vagy jelöljön ki egy meglévőt.
6. Válasszon egy **helyet**.
7. Kattintson a **csomag**parancsra. Ekkor megnyílik a **csomag feltöltése** lap. Töltse ki a szükséges mezőket.  

     Ha be a szerepköröket tartalmaz egy példányát, győződjön meg arról, **még akkor is, ha egy vagy több szerepköröket tartalmaz egy példányát üzembe** be van jelölve.

8. Győződjön meg arról, hogy **Kezdő telepítési** van-e jelölve.
9. Bezárul a **csomag feltöltése** lap **az OK** gombra.
10. Ha nem rendelkezik bármely tanúsítványok hozzáadni, kattintson a **Létrehozás**gombra.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Töltse fel a tanúsítvány

Ha a telepítőcsomag volt, [használja a tanúsítványok](cloud-services-configure-ssl-certificate-portal.md#modify), most is feltölthet a tanúsítvány.

1. Jelölje be a **tanúsítványok**, és válassza a **Hozzáadás a tanúsítványok** lap az SSL-tanúsítvány .pfx fájl és a tanúsítvány, majd adja meg a **jelszót**
2. Kattintson a **Csatolás tanúsítvány**, és kattintson a **Hozzáadás a tanúsítványok** lap kattintson az **OK gombra** .
3. Kattintson a **Létrehozás** gombra a **Felhőbeli szolgáltatástól** lap. A telepítési **kész** állapotát elérésekor is folytassa a következő lépésekkel.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>A sikeres befejezését üzembe ellenőrzése

1. Kattintson a felhőalapú szolgáltatás példánya.

    Az állapot jelenítsen meg, hogy a szolgáltatás nem **fut**.

2. A **Essentials**kattintson a **Webhely URL-címe** egy webböngészőben nyissa meg a felhőalapú szolgáltatás.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Következő lépések

* [A felhőalapú szolgáltatás általános konfigurálása](cloud-services-how-to-configure-portal.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name-portal.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md)beállítása.