<properties 
   pageTitle="A felhő Intézővel Azure erőforrások kezelésére |} Microsoft Azure"
   description="Megtudhatja, hogy miként tallózással keresse meg és Azure erőforrások Visual Studio felhő Intézővel."
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

# <a name="managing-azure-resources-with-cloud-explorer"></a>A felhő Intézővel Azure erőforrások kezelése

##<a name="overview"></a>– Áttekintés

Felhőalapú Explorer lehetővé teszi, hogy könnyebben és gyorsabban tallózással keresse meg és kezelheti az Azure erőforrásokat a Visual Studio IDE belül lett tervezve. Akkor is, például annak segítségével az [Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy a böngészőben nyissa meg a megfelelő Web App alkalmazásban vagy egy csatolása, vagy egy blob-tárolóhoz tulajdonságainak megtekintése, és nyissa meg a Blob-tárolóhoz szerkesztőben.

Felhőalapú Explorer az Azure erőforrás manager Papírhalom, hasonlóan az [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040)épül. Erőforrások, például az Azure erőforráscsoport és Azure szolgáltatások, például a összefüggés-alkalmazások és az API-alkalmazások értelmezésére képes, és [szerepköralapú hozzáférés-vezérlés](./active-directory/role-based-access-control-configure.md) (RBAC) támogat. Azure erőforrások hozzáadása vagy a módosított megtekintéséhez válassza az a felhő Explorer eszköztáron a **frissítés** gombra.

Felhőalapú Explorer telepítve van a Visual Studio Tools részeként Azure SDK 2.7. 

## <a name="prerequisites"></a>Előfeltételek

- Visual Studio 2015 RTM.

- A Visual Studio Azure SDK eszközeit. 
- Rendelkeznie kell az Azure-fiók, és jelentkezzen be az Azure erőforrások megtekinthetik a felhőben Intézőben. Ha nincs telepítve egyik, létrehozhat egy fiókot, mindössze néhány perc az. Ha az MSDN előfizetéssel rendelkezik, olvassa el a [Azure kedvezménye MSDN előfizetők számára](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). [Ingyenes próbaverzió fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/), megtekintése

- Ha felhő Explorer nem látható, megtekintheti **Nézet**és az **Egyéb Windows,** **Felhőalapú Explorer** menüsávján kiválasztásával.

## <a name="manage-azure-accounts-and-subscriptions"></a>Azure fiókok és előfizetések kezelése

Az a felhő Intézőben Azure erőforrások megtekintéséhez kell egy vagy több aktív előfizetések Azure fiókkal bejelentkezni. Ha egynél több Azure-fiók, felveheti a felhőben Intézőben, és válassza az előfizetések foglalandó a felhőben Explorer erőforrás nézetben.

Ha még nem használta az Azure előtt, vagy még nem adott hozzá a szükséges számlákat Visual Studio, kérni fogja erre.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Azure fiók az felhő Explorer hozzáadása

1. Válassza a beállítások ikont a felhőben Explorer eszköztáron.

1. Válassza a **fiók hozzáadása** hivatkozásra. Jelentkezzen be az Azure-fiók amelynek erőforrásait, amelyben tallózni szeretne. A fiók kiválasztása legördülő lista választják ki az imént hozzáadott fiókba. A fiókhoz tartozó jelennek meg a fiók bejegyzés alatt.

    ![Azure előfizetések hozzáadása](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Azure előfizetések kiválasztása](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Jelölje be a jelölőnégyzetet a fiók előfizetésekhez a Tallózás gombra, és válassza az **Alkalmaz** gombra.

    A kijelölt előfizetéseket az Azure erőforrások felhő Explorer jelennek meg.

## <a name="to-remove-an-azure-account"></a>Ha el szeretné távolítani az Azure-fiók

1. Válassza a **fájl**, a **Fiókbeállítások** a menüsávon.

1. A **Fiókbeállítások** párbeszédpanel **Minden fiók** csoportjában válassza ki a **eltávolítása** parancs a fióknév mellett el szeretné távolítani. Jegyzet, hogy ez a parancs csak a fiók eltávolítása a Visual Studio – informatikai az Azure-fiók magát nincs hatással.

## <a name="view-resource-types-or-groups"></a>Erőforrás-típusok megtekintése vagy csoportok

Ha szeretné megtekinteni az Azure erőforrások, megadhatja vagy **Erőforrástípus** , vagy **Az erőforrás csoportok** megtekintése.

![Az erőforrás nézet legördülő menü](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **Erőforrástípus** nézet, amely szintén a közös nézetben az [Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040)használja, az Azure erőforrások, például web Apps alkalmazások, a tárterület-fiókok és a virtuális gépeken futó típusuk szerint kategorizált jeleníti meg. Ez a hasonló hogyan Azure erőforrásokra mutató jelennek meg a kiszolgáló Intézőben.

- Csoportok Erőforrásnézet kategorizálja az Azure erőforrásokat az Azure esetén társított.

 
    Erőforráscsoport egy Azure, általában egy adott alkalmazás által használt erőforrások az első lépésekhez. Azure erőforráscsoport olvashat az [Azure erőforrás-kezelő áttekintése](./resource-group-overview.md)című témakörben találhat.

## <a name="view-and-navigate-resources"></a>Megtekintéséhez és erőforrások

Nyissa meg az Azure-erőforrás és annak adatait a felhőben Intézőben megtekintéséhez bontsa ki az elem típusa vagy tartozó erőforráscsoport, és válassza ki a az erőforrás. Ha úgy dönt, hogy egy erőforrás, információ a két füle felhő Explorer alján jelenik meg.

![Az erőforrás nézet kiválasztása](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- A **Műveletek** lap jeleníti meg a műveletek felhő Explorer a kijelölt erőforráshoz tartozó. Kattintson a helyi menü, az erőforrás is megjelenik a rendelkezésre álló műveletek.

- A **Tulajdonságok** lapon látható az erőforrás, például a típus, a területi és az erőforrás van társítva csoport tulajdonságait.

Minden erőforrásnak megvan- **portálon nyissa meg**a műveletet. Ha úgy dönt, hogy ez a művelet, felhőalapú Explorer a kijelölt erőforrás az [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040)jeleníti meg. Ez a funkció különösen hasznos mélyen beágyazott erőforrások között.

További műveletek és tulajdonságértékeket is megjelenhetnek az Azure erőforrás alapján. Például web apps és az alkalmazások logika is **Megnyitás böngészőben** és a **csatolása debugger** kívül **portálon nyissa meg**a műveletek. Nyissa meg a szerkesztők műveletek jelenik meg, ha úgy dönt, hogy a tárhely blob-fiókot, várólista vagy táblázat. Azure alkalmazások **URL-CÍMEK** és **állapot** tulajdonság tartozik, míg a tároló erőforrásokat billentyű és a kapcsolati karakterlánc tulajdonságok.

## <a name="search-resources"></a>Keresés erőforrások

Keresse meg az Azure-fiók előfizetések egy konkrét névvel ellátott erőforrások, írja be a nevét a keresőmezőbe a felhőben Intézőben.

![Erőforrások keresése az a felhő Intézőben](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

A Keresés mezőbe beírt karaktereket, mint az erőforrás fa csak az adott karaktereket egyező erőforrások jelennek meg.

