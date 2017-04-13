<properties
    pageTitle="Állítsa be az Azure-App szolgáltatásban webalkalmazások környezetekben átmeneti tárolására"
    description="Megtudhatja, hogy miként szakaszos közzétételi Azure App szolgáltatásban web Apps alkalmazások használata."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Átmeneti tárolására környezetekben web Apps alkalmazások Azure alkalmazás szolgáltatás beállítása
<a name="Overview"></a>

Amikor rendszerbe állítják a web app [Alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714), telepítheti egy külön telepítési tárolóhely az alapértelmezett gyártási tárolóhely helyett a **normál** vagy a **prémium** alkalmazás szolgáltatás terv módban fut. Telepítési helyek ténylegesen élő web Apps alkalmazások, a saját állomásnevekké. Web app tartalom és konfigurációk elemek között két telepítési helyek, beleértve a termelési tárolóhely is kell cserélni. Az alkalmazást a telepítés tárolóhely üzembe helyezése a következő előnyöket nyújtja:

- Ellenőrizheti egy átmeneti tárolásra szolgáló telepítési tárolóhely web app változásai lecserélése, és a gyártási tárolóhely előtt.

- Először webalkalmazást bevezetéshez egy tárolóhely és éles lecserélése azt ellenőrzi, hogy a a tárolóhely összes előfordulását előtt éles cserélni folyamatban van bemelegíteni. Ezzel a legrövidebb leállás elkerülhető, amikor rendszerbe állítják a web App alkalmazásban. A forgalom átirányítása gördülékeny, és nincs kérések szövegdobozokban eredményeként felcserélése műveletek nem jelennek. A teljes munkafolyamat automatizálható konfigurálása [Automatikus felcserélése](#configure-auto-swap-for-your-web-app) előtti felcserélése érvényesítése nem szükségessége esetén.

- A korábban szakaszos webalkalmazást tárolóhely után egy felcserélése, ekkor megjelenik az előző gyártási web app. Ha nem a várt módon a módosításokat a gyártási tárolóhely be cserélni, végezheti el az azonos felcserélése közvetlenül a "utolsó ismert jó webhely" megszerezni vissza.

Minden alkalmazás szolgáltatás terv üzemmódban különféle telepítési helyek támogatja. Megtudhatja, hogy hány résidők a web app támogatja a mód, lásd: az [Alkalmazás szolgáltatás árak](/pricing/details/app-service/).

- Ha több helyek a web App alkalmazásban, a mód nem módosítható.

- Méretezés nem áll rendelkezésre tesztkörnyezetben helyek.

- Csatolt az erőforrás-kezelés tesztkörnyezetben helyek nem támogatott. Az [Azure-portálon](http://go.microsoft.com/fwlink/?LinkId=529715) csak elkerülhető, ez egy gyártási tárolóhely potenciális hatása a tesztkörnyezetben tárolóhely ideiglenes áthelyezése egy másik alkalmazás szolgáltatás terv módot. Figyelje meg, hogy a tesztkörnyezetben tárolóhely kell még egyszer megosztása ugyanazt a módot a termelési tárolóhely válthat a két helyek előtt.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>A telepítési tárolóhely felvétele a web App alkalmazásba ##

A **normál** vagy a **támogatási** módban, mielőtt több telepítési helyek engedélyezése a web app futnia kell.

1. Az [Azure-portálon](https://portal.azure.com/)nyissa meg a webalkalmazás lap.
2. Kattintson a **Beállítások**gombra, és kattintson a **telepítés helyek**gombra. A **telepítési helyek** lap, válassza **A tárolóhely hozzáadása**.

    ![Egy új telepítési tárolóhely hozzáadása][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > A web app még nincs megnyitva a **normál** vagy **prémium** módja az, ha kap egy a támogatott módok engedélyezve van a közzététel szakaszos jelző üzenet. Ezen a ponton, azt a vezérlőt, válassza a **frissítés** , és keresse meg a továbblépés előtt a web App a **mérete** fülre.

2. Kattintson a **Hozzáadás a tárolóhely** lap nevezze el a tárolóhely, és válassza ki, hogy egy másik már létező telepítési tárolóhely web app konfiguráció klónozhatja. Kattintson a továbbra is.

    ![Konfigurációs forrás][ConfigurationSource1]

    Az első alkalommal felvesz egy tárolóhely, csak akkor fog két lehetőség közül választhat: adatfeliratsor konfiguráció az alapértelmezett tárolóhely gyártási vagy egyáltalán nem.

    Több helyek létrehozása után lesz egy tárolóhely kívül a gyártási konfiguráció klónozhatja:

    ![Konfigurációs források][MultipleConfigurationSources]

5. A **telepítési helyek** lap kattintson a telepítés tárolóhely mértékek és konfigurációs, mint bármely más webalkalmazás a tárolóhely, egy lap megnyitásához. **Your-Web-App-Name-Deployment-slot-Name** jelenik meg, amelyben emlékezteti arra, a telepítési tárolóhely megjelenítő a lap tetején.

    ![Telepítési tárolóhely cím][StagingTitle]

5. Kattintson az alkalmazás a tárolóhely lap URL-t. Figyelje meg a telepítési tárolóhely saját hostname (állomásnév) van, és egy élő alkalmazást is. A telepítési tárolóhely elérésének korlátozására lásd: [Alkalmazás szolgáltatás Web App – webes hozzáférésének letiltása nem éles üzemi helyek](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Telepítési tárolóhely létrehozását követően nem nem. A tárolóhely egy másik tárházba ág vagy egy teljesen mást tárházba telepítheti. A tárolóhely konfigurációs is módosíthatja. Közzététel profilt vagy telepítési hitelesítő adataival a tartalom frissítések telepítési tárolóhely használja.  Ha például azt is megteheti, [Ez az mely számjegy tárolóhely közzététele](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>A telepítési helyek konfigurálása ##
Ha egy másik telepítés tárolóhely konfiguráció, klónozhatja, a klónozott konfigurációja szerkeszthető. Ezenkívül konfigurációs elemeket követi a tartalmat egy felcserélése (nem tárolóhely adott) keresztül közben egyéb konfigurációs elemek maradnak mindaddig a azonos tárolóhely után egy felcserélése (tárolóhely adott). Az alábbi listákat a beállításokat, amelyek változni fog a helyek cserél megjelenítése.

**Beállítások vannak cserélni, amely**:

- Általános beállítások – például keretrendszer verziója, 32 vagy 64 bites, webes sockets
- Alkalmazás beállításainak (beállítható úgy, hogy egy tárolóhely szigorúan)
- Kapcsolati karakterlánc (beállítható úgy, hogy egy tárolóhely szigorúan)
- Eseménykezelő-megfeleltetések
- Ellenőrzés és diagnosztikai beállítások
- WebJobs tartalom

**A beállításokat, amelyek nem cserélhető**:

- Közzétételi végpontok
- Egyéni tartomány neve
- Az SSL-tanúsítványok és kötések
- Méretarány-beállításai
- WebJobs bejegyzéstípusait

Egy alkalmazás beállítás és a kapcsolati karakterláncot egy (nem cserélni) tárolóhely szigorúan konfigurálásához az egy adott tárolóhely az **Alkalmazás beállításai** lap eléréséhez, majd jelölje be a konfigurációs elemeket, amelyek a tárolóhely kell szigorúan **Tárolóhely beállítás** gombra. Figyelje meg, hogy megjelölése egy konfigurációs elem tárolóhely adott létrehozásáról szóló az elem nem cserélhető végig a web app társított telepítési helyek hatása van.

![Tárolóhely beállításai][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Telepítési helyek felcserélése ##

>[AZURE.IMPORTANT] Az egy telepítési tárolóhely webalkalmazást éles cserél, mielőtt győződjön meg arról, hogy az összes nem tárolóhely adott beállítások vannak pontosan úgy, ahogyan szeretné használni, a csere célhely.

1. Telepítési helyek megjelenítését, kattintson a **Csere** gombra a web App alkalmazásban parancs sávján vagy egy telepítési tárolóhely a parancssávon. Győződjön meg arról, hogy a felcserélése forrás és a csere-céltartományban helyesen vannak beállítva. A csere cél általában, akkor a gyártási tárolóhely.  

    ![Csere gomb][SwapButtonBar]

3. Kattintson az **OK gombra** a művelet végrehajtásához. Amikor a művelet befejeződött, a telepítési helyek van már cserélni.

## <a name="configure-auto-swap-for-your-web-app"></a>Állítsa be az automatikus felcserélése a webalkalmazások számára ##

Automatikus felcserélése egyszerűsítik DevOps forgatókönyvek, amelyhez folyamatosan bevezetését tervezi a web App alkalmazásban, a nulla hideg kezdő és a nulla legrövidebb leállás vége a web app felhasználói számára. Amikor egy telepítési tárolóhely be van állítva az automatikus felcserélése éles, minden alkalommal, amikor az adott tárolóhely leküldéses, a kód frissítés, alkalmazás szolgáltatás fog automatikusan felcserélése a web app éles után meg van már bemelegíteni a tárolóhely a.

>[AZURE.IMPORTANT] Ha engedélyezi az automatikus felcserélése az egy tárolóhely, győződjön meg róla a tárolóhely konfigurációját pontosan a tervezett a cél tárolóhely (Ez általában a termelési tárolóhely) konfigurációját.

Automatikus felcserélése beállítása egy tárolóhely használata egyszerű. Kövesse az alábbi lépéseket:

1. A **Telepítési helyek** lap egy tesztkörnyezetben tárolóhely jelölje ki, a tárolóhely a lap **Összes beállításai** gombra kattintva.  

    ![][Autoswap1]

2. Kattintson az **alkalmazás beállításait**. Válassza ki **a** **Automatikus felcserélése**, jelölje ki a kívánt cél tárolóhely **Automatikus felcserélése tárolóhely**, és kattintson a **Mentés** gombra a parancssávon. Ellenőrizze, hogy a tárolóhely konfigurációs pontosan a cél tárolóhely szánt konfigurációját.

    Az **értesítések** fülre fog flash zöld **sikeres** , a művelet befejezése után.

    ![][Autoswap2]

    >[AZURE.NOTE] Teszteléséhez automatikus felcserélése a webalkalmazásban is először ki kell választania egy tesztkörnyezetben cél tárolóhely **Automatikus felcserélése tárolóhely** , hogy megismerje a szolgáltatás.  

3. Hajtsa végre az adott telepítési tárolóhely leküldéses kódot. Automatikus felcserélése rövid idő múlva történik, és a frissítés jelennek meg a cél tárolóhely URL-címen.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Többfázisú felcserélése a webes alkalmazás használata ##

Többfázisú felcserélése célja, hogy egy tárolóhely, például a csatlakozási_karakterlánc szigorúan konfigurációs elemek környezetében érvényesítése egyszerűsítése érdekében érhető el. Ezekben az esetekben lehet hasznos lehet ahhoz, hogy a felcserélése céllista ilyen konfigurációs elemek alkalmazása a felcserélése forrás, és az érvényesítés előtt felcserélése ténylegesen lép érvénybe. Miután felcserélése cél konfigurációs elemek alkalmazza a program a felcserélése forrás elérhető műveletek végrehajtásával a csere vagy visszaállítása eredeti konfiguráció felcserélése forrást, amely a felcserélése megszakítása hatása van is. Többfázisú felcserélése érhető el az Azure PowerShell-parancsmagok minták szerepelnek az Azure PowerShell-parancsmagok telepítési helyek csoportban.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Visszavonás egy gyártási alkalmazás után felcserélése ##

Ha után egy tárolóhely felcserélése gyártási azonosítja a hibák, visszaállíthatja a helyek előtti felcserélése állapotának által a két azonos helyek azonnal lecserélése.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Egyéni bemelegítési előtt felcserélése ##

Egyes alkalmazások megkövetelheti egyéni bemelegítési műveletek. A applicationInitialization konfigurációs elem Web.config előtt kérelem érkezik végrehajtandó egyéni inicializálni műveletek megadását teszi lehetővé. A csere művelet elvégzéséhez a egyéni bemelegítési megvárja. Íme egy példa web.config fragment.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>A telepítési tárolóhely törlése##

Az egy telepítési tárolóhely lap kattintson az **törlése** a parancssávon.  

![A telepítési tárolóhely törlése][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Telepítési helyek Azure PowerShell-parancsmagok

Azure PowerShell modul, amely tartalmaz kezelése a Windows PowerShell-támogatással web app telepítési helyek Azure alkalmazás szolgáltatás kezelésére szolgáló keresztül Azure-parancsmagok.

- Információ való telepítéséről és konfigurálásáról Azure Powershellt, és Azure PowerShell hitelesítése az Azure-előfizetéséhez megtudhatja, [hogy miként telepítése és konfigurálása a Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Webalkalmazás létrehozása

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>A telepítési tárolóhely egy webalkalmazás létrehozása

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Többfázisú felcserélése kezdeményezése és forrás tárolóhely cél tárolóhely konfigurációs alkalmazása

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Többfázisú felcserélése első szakaszának visszavált és a forrás tárolóhely beállításainak visszaállítása

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Telepítési helyek felcserélése

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Telepítési tárolóhely törlése

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Telepítési helyek Azure parancssori kezelőfelületről Azure-parancsok

Az Azure CLI platformok parancsok az Azure-támogatással webalkalmazás telepítési helyek kezelésére szolgáló ismertetése

- Való telepítéséről és konfigurálásáról az Azure CLI, például az Azure CLI csatlakozhat az Azure-előfizetése, hogy miként utasításokat [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md)című cikkben találja.

-  Az elérhető parancsok listája az Azure CLI az Azure-alkalmazás szolgáltatás, hívja a `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure listában
Az aktuális előfizetés webalkalmazások tudni hívja fel az **azure listában**, az alábbi példának megfelelően.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Azure-webhely létrehozása
Hozzon létre egy telepítési tárolóhely, hívja az **azure-webhely létrehozása** , és adja meg a meglévő webes alkalmazás nevét és a tárolóhely szeretne létrehozni, a következő példának megfelelően a nevét.

`azure site create webappslotstest --slot staging`

Ha engedélyezni szeretné a új tárolóhely adatforrás-vezérlő, a **mely számjegy –** a beállítást választja, a következő példának megfelelően használja.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Azure webhely felcserélése
Szeretné, hogy a frissített példányban bővítőhely a termelési alkalmazás, a paranccsal **azure webhely felcserélése** végrehajtása felcserélése, ahogy a következő példában. A gyártási alkalmazás nem léphetnek minden alkalommal lefelé, és nem fog alá hideg indítás.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure webhely törlése
Egy telepítési tárolóhely, amely már nincs szükség törléséhez használja a **azure webhely törlése** parancs a következő példának megfelelően.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="next-steps"></a>Következő lépések ##
[Azure alkalmazás szolgáltatás Web App – nem éles üzemi helyek webes hozzáférésének letiltása](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure ingyenes próbaverziója](/pricing/free-trial/)

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
