<properties
    pageTitle="Gyakori kérdések Azure DevTest Labs |} Microsoft Azure"
    description="A Azure DevTest Labs kapcsolatos gyakori kérdésekre adott válaszok"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs – gyakori kérdések

Ez a cikk néhány Azure DevTest Labs leggyakoribb kérdésekre ad választ.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Általános
- [Mi történik, ha szeretnék nem érkezett válasz Itt?](#what-if-my-question-isnt-answered-here)
- [Mire jók az Azure DevTest Labs?](#why-should-i-use-azure-devtest-labs) 
- [Mit jelent "megbízható nélkül, önkiszolgáló"?](#what-does-quotworry-free-self-servicequot-mean)
- [Hogyan használhatom az Azure DevTest Labs?](#how-can-i-use-azure-devtest-labs) 
- [Hogyan vagyok számlát kapni az Azure DevTest Labs?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Biztonsági 
- [Mik azok az Azure DevTest Labs különböző biztonsági szintjei?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Hogyan hozhatok létre engedélyezése a felhasználóknak egy meghatározott tevékenység elvégzéséhez szerepkörbe?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>KI/CD-integráció és automatizálási 
 
- [Azure DevTest Labs a ki és CD-re toolchain integrálása?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuális gépeken futó 
 
- [Miért nem látom az Azure virtuális gépeken futó lap Azure DevTest Labs belül jelenik meg az egyes VMs?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Egyéni képek és képletek közötti különbség](#what-is-the-difference-between-custom-images-and-formulas) 
- [Hogyan hozhatok létre több VMs ugyanazon sablonból egyszerre?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Hogyan a meglévő Azure VMs áthelyezése az Azure DevTest Labs labor?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Csatolható több lemezt a VMs?](#can-i-attach-multiple-disks-to-my-vms) 
- [Hogyan automatizálhatja a virtuális fájlfeltöltéskor hozhat létre egyéni képek folyamata?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Hogyan automatizálhatja a törlési folyamat az a labor az összes VMs?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Eltérések 
 
- [Mik azok az eltéréseket?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Labor konfigurálása 
 
- [Hogyan hozhatok létre laboratóriumi Azure erőforrás-kezelő sablonból?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Miért létrehozásakor a VMs a különböző erőforrás csoportok tetszőleges nevek? Átnevezése vagy módosítása az erőforrás csoportokhoz?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Hány labs hozhat létre a ugyanabban az előfizetésben?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Hány VMs hozhat létre egy labor?](#how-many-vms-can-i-create-per-lab)
- [Hogyan oszthatok meg közvetlen hivatkozást a labor?](#how-do-i-share-a-direct-link-to-my-lab)
- [Mi az Microsoft-fiókkal?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Hibaelhárítás 
 
- [Az eltérés virtuális létrehozása során nem sikerült. Hogyan háríthatók el azt?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Miért nem a virtuális hálózatát megfelelően menteni?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Mi történik, ha szeretnék nem érkezett válasz Itt?
Ha kérdésére nem találja meg itt, tudassa velünk, és segítünk talál választ.

- Tegyen közzé kérdést a [Disqus szál](#comments) Ez a cikk végén, és az Azure-gyorsítótár csoport és más közösségi tagok erről a cikkről vesznek.
- Egy nem elég széles közönség eléréséhez tegye közzé kérdését az [Azure DevTest Labs MSDN-fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), és az Azure DevTest Labs csapat- és más közösségi tagok foglalkozó.
- Ahhoz, hogy a szolgáltatás kérése, a kérések és az [Azure DevTest Labs felhasználói hangposta](https://feedback.azure.com/forums/320373-azure-devtest-labs)ötletek nyújt.

### <a name="why-should-i-use-azure-devtest-labs"></a>Mire jók az Azure DevTest Labs? 
Azure DevTest Labs mentheti a csapat időt és a pénzt. A fejlesztők létrehozása a saját környezetben használatával számos különböző alapjait, és eltérések segítségével gyorsan üzembe helyezéséhez és alkalmazásainak konfigurálása. Egyéni képek és a képletek használata esetén virtuális gépeken futó menthetők, a webhelysablonok és könnyen reprodukálható. Ezenkívül labs felajánl néhány konfigurálható házirendek labor rendszergazdák hulladék csökkentése és a csoport webhelytárában környezetekben kezelése. Ezek a házirendek olyan automatikus-leállítás költség küszöbérték, maximális VMs egy felhasználó, és a maximális virtuális méretét. Azure DevTest Labs részletesebb leírását olvassa el az [Áttekintés](devtest-lab-overview.md) , vagy olvassa el a [bevezető videóban](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Mit jelent "megbízható nélkül, önkiszolgáló"?
"Önkiszolgáló megbízható szabad," azt jelenti, hogy a fejlesztők és a tesztelést létrehozása a saját környezetben, szükség szerint, biztonsága arra, hogy Azure DevTest Labs csökkentheti kell a válaszra, és a költség szabályozhatja a rendszergazdáknak. A rendszergazdák megadhatja, melyik virtuális méretű engedélyezett maximális száma VMs, és ha a VMs elindítása és leállítása. Azure DevTest Labs is egyszerűen költségek figyelemmel, és hogyan használják a források tesztkörnyezetben zajló tájékoztató értesítések beállítása. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Hogyan használhatom az Azure DevTest Labs? 
Azure DevTest Labs akkor lehet hasznos, bármikor megkövetelése a fejlesztők, vagy a tesztelés környezetben, és szeretne gyorsan Reprodukálja őket, illetve kezelhetők a költség házirendek mentése. 

Az alábbiakban néhány olyan esetek, hogy ügyfeleink hogyan használják Azure DevTest Labs: 

- Kezelése – egy helyen fejlesztők és a vizsgálat környezetben, egyéni képek megosztása és a költség csökkentése érdekében a házirendek felhasználásával végig a csapat hoz létre.
- Az alkalmazás egyéni képek mentése a lemez állam egész a fejlesztés szakaszokra megtervezése.
- A költség viszonyított haladás nyomon. 
- Tömeges próba-verziót tartalmazó környezetek minőség-garancia teszteléshez létrehozása.
- Eltérések és képletek használatával egyszerűen konfigurálása és Reprodukálja a különböző környezetekben kérelmet. 
- Az hackathons (fejlesztők vagy próba munkán) VMs terjesztésével, és ezután egyszerűen vonja kiépítési őket az esemény befejeződésekor. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hogyan vagyok számlát kapni az Azure DevTest Labs? 
Azure DevTest Labs az ingyenes szolgáltatása, ami azt jelenti, hogy labs létrehozása és konfigurálása, a házirendek, sablont és eltérések ingyenes. Csak a használt belül a labs, például a virtuális gépeken futó, tároló fiókok és virtuális hálózatok Azure erőforrások fizet az előfizetésért. További tájékoztatást laboratóriumi erőforrásokat a költség további információ: [Azure DevTest Labs árak](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Mik azok az Azure DevTest Labs különböző biztonsági szintjei?  
Az access biztonsági [Azure Role-Based Access vezérlő (RBAC)](../active-directory/role-based-access-built-in-roles.md)határozza meg. Ha meg szeretné érteni, hogy hogyan működik a hozzáférést, könnyebben megértheti jogosultsági szerepkörbe és RBAC által meghatározott hatókör közötti különbségeket.

- **Engedély** - jogosultsági egy definiált érhető el egy adott művelet. Jogosultsági lehet például olvasható-elérése a virtuális gépeken futó. 
- **Szerepkör** - szerepkör az csoportosítható, a felhasználóhoz rendelt engedélyek csoportja. Ha például az "előfizetés tulajdonosa" előfizetés belül minden erőforrás hozzáférése van. 
- **Hatókör** - hatókör Azure erőforrás a hierarchiában egy szinten. A hatókör lehet például erőforráscsoport vagy egy egyetlen labor vagy a teljes előfizetést. 
 
Azure DevTest Labs hatálya alá szerepkörök felhasználói engedélyek meghatározása két típusa van: labor tulajdonos és labor felhasználói.

- **Labor tulajdonosa** – egy labor tulajdonosa a laboratóriumi belül erőforrások hozzáféréssel rendelkezik. Emiatt azokat is módosíthatja házirendeket, olvassa el és bármely VMs írni, a virtuális hálózat módosítása, és így tovább. 
- **Labor felhasználói** - labor felhasználó megtekintheti az összes labor erőforrások, például VMs, házirendek és virtuális hálózatok, de szabályok vagy bármely más felhasználók által létrehozott VMs nem módosíthatják. Akkor is egyéni szerepkörök létrehozása az Azure DevTest Labs, és azt is megtudhatja, hogy miként hajtsa végre a következő cikket [adott labor házirendek felhasználói engedélyek megadása](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)gombra. 
 
Mivel a tartományok hierarchikus, amikor a felhasználó rendelkezik engedélyekkel egy bizonyos hatókört a, automatikusan kapnak ezeket az engedélyeket minden alsóbb szintű hatókör vonatkozik. Például ha egy felhasználó előfizetés tulajdonosa szerepe van rendelve, majd hozzáféréssel rendelkeznek az összes erőforrás az előfizetést. Anyagi erőforrások közé tartoznak az összes virtuális gépeken futó, minden virtuális hálózatok és az összes labs. Egy előfizetés tulajdonosa így automatikusan labor tulajdonosa szerepe örökli. Jó helyen jár ellenkező nem igaz. Egy labor tulajdonosa labor, amely előfizetés szintnél alsó hatókör hozzáférése van. Emiatt a labor tulajdonosa nem látja a virtuális gépeken futó vagy a virtuális hálózatok vagy az erőforrások, amelyek nem tagjai az labor. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hogyan hozhatok létre engedélyezése a felhasználóknak egy meghatározott tevékenység elvégzéséhez szerepkörbe?
A teljes cikk arról, hogy miként létrehozása egyéni szerepkörök és engedélyek hozzárendelése szerepkörhöz található. Íme egy példa egy parancsprogramot, amely a "DevTest Labs speciális felhasználónak", amely elindítása és leállítása tesztkörnyezetben összes VMs jogosult szerepkör hoz létre:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Azure DevTest Labs a ki és CD-re toolchain integrálása? 
VSTS használja, ha van, amely lehetővé teszi a Megjelenés folyamat az Azure DevTest Labs automatizálása [Azure DevTest Labs feladatok bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) . A bővítmény felhasználása a következők:

- Létrehozása és üzembe helyezése automatikusan egy virtuális, és konfigurálja úgy az Azure-fájl másolása vagy PowerShell VSTS feladatok használata a legújabb verziójában. 
- Automatikusan rögzítése egy virtuális állapotának ellenőrzése meg megismételni a azonos virtuális további vizsgálatához a hiba után. 
- A Megjelenés folyamat végén a virtuális törlése, ha már nincs szükség. 

A következő blogbejegyzések nyújtanak útmutatást és információt a VSTS bővítmény használatáról:
 
- [Azure DevTest Labs – VSTS bővítmény](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Egy új virtuális egy meglévő AzureDevTestLab a VSTS az üzembe helyezése](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [VSTS megjelenés kezelés segítségével történő AzureDevTestLabs folyamatos telepítésekhez](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Az egyéb ki/CD-re toolchains lehet elérni az VSTS feladatok bővítmény korábban említett esetek hasonló módon érhető el az [Azure PowerShell-parancsmagok](../resource-group-template-deploy.md) és [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/) [Azure erőforrás-kezelő sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) telepítése. [A DevTest Labs REST API -khoz](http://aka.ms/dtlrestapis) integrálása a toolchain is használhatja.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Miért nem látom az Azure virtuális gépeken futó lap Azure DevTest Labs belül jelenik meg az egyes VMs?
Amikor egy virtuális Azure DevTest Labs jön létre, engedélyt kap, hogy virtuális eléréséhez. Képesek arra, hogy megtekintse az labs lap és a **virtuális gépeken futó** lap egyaránt. A DevTest Labs szerepkör a felhasználók láthatják használatával a labor **összes virtuális gépeken futó** lap tesztkörnyezetben létre virtuális gépeken futó. Azonban a DevTest Labs szerepkör-felhasználók nem automatikusan kapnak virtuális erőforrásokat, mások által létrehozott olvasási hozzáférést. Ezért ezeket VMs nem jelenik meg a **virtuális gépeken futó** lap. 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Egyéni képek és képletek közötti különbség 
Egyéni kép a egy virtuális (ezek olyan virtuális merevlemez), mivel a képlet, beállíthatja, hogy további beállításokat, hogy menti, és Reprodukálja a képet. Lehet, hogy egy egyéni kép helyett, ha szeretné gyorsan létrehozhat több környezetek azonos alapvető, megváltoztatható képhez. Lehet, hogy jobban, ha a legújabb bittel, egy virtuális hálózati alhálózat vagy betűméretének a virtuális konfigurációja Reprodukálja szeretne egy képletben. Részletesebb leírást ismertető, [Comparing egyéni képek és DevTest Labs képleteket](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hogyan hozhatok létre több VMs ugyanazon sablonból egyszerre? 
Használhatja a [VSTS feladatok bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) vagy [egy erőforrás-kezelő Azure-sablon készítése](devtest-lab-add-vm-with-artifacts.md#save-arm-template) létrehozása egy virtuális és [az erőforrás-kezelő Azure-sablon a Windows PowerShell telepítése](../resource-group-template-deploy.md)során. 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Hogyan a meglévő Azure VMs áthelyezése az Azure DevTest Labs labor? 
Lépjen közvetlenül VMs Azure DevTest Labs megoldást tervez azt, de jelenleg is másolhatja a meglévő VMs Azure DevTest Labs az alábbi képlettel történik: 

1. A virtuális másolhatja a meglévő virtuális gép a [Windows PowerShell-parancsprogramot](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) használata 
1. [A saját kép létrehozása](devtest-lab-create-template.md) az Azure DevTest Labs labor belül. 
1. Hozzon létre egy virtuális a saját kép tesztkörnyezetben 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Csatolható több lemezt a VMs? 
Több lemezre csatolása VMs használata támogatott.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hogyan automatizálhatja a virtuális fájlfeltöltéskor hozhat létre egyéni képek folyamata? 
Kétféleképpen lehet:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) használható másolásához vagy virtuális fájlok feltöltése a labor társított tárterület-fiókjába.
- Alkalmazás a különálló, futtatja a Windows, OSX és Linux a [Microsoft Azure tároló Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) .   
 
A cél tárterület-fiók a labor társított megkereséséhez kövesse az alábbi lépéseket:

1. Jelentkezzen be az [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Válassza ki **Az erőforrás csoportok** a bal oldali ablaktáblában. 
1. Keresse meg és jelölje ki a társított a labor erőforrás-csoportot. 
1. Az **Áttekintés** lap válassza a tárterület-fiókok egyikét. 
1. Jelölje ki a **BLOB**.
1. Keresse meg a feltöltést a listában. Ha még nem létezik ilyen lépés: 4 # vissza, és próbálja meg egy másik tárterület-fiókot.
1. Az **URL-címet** használja a cél a AzCopy parancs.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hogyan automatizálhatja a törlési folyamat az a labor az összes VMs?

Kívül VMs törlése a labor az Azure-portálon, törölheti az összes a VMs a labor egy PowerShell-parancsprogramot használatával. A következő példában egyszerűen módosítsa az **értékek módosítása** Megjegyzés alatt paraméter értékét. Meghallgathatja a `subscriptionId`, `labResourceGroup`, és `labName` a labor lap az Azure-portálon értékeit. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Mik azok az eltéréseket? 
Eltérések elemei testre szabható bevezetését tervezi a legújabb bit vagy egy virtuális alakzatot a fejlesztők eszköz használható. A virtuális csatolt néhány egyszerű kattintással létrehozása során, és a virtuális már kiépítve, az eltérések telepítése és konfigurálása a virtuális. Saját [nyilvános Github tárházba](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)vannak a különböző már meglévő eltéréseket, de könnyen is [saját eltérések Szerző](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hogyan hozhatok létre laboratóriumi Azure erőforrás-kezelő sablonból? 
Egy [labor Azure erőforrás-kezelő sablonok Github tárházából](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) telepítheti a következőképpen nyújtott- vagy módosítása az egyéni sablonokat a labs létrehozásához. Ezek a sablonok mindegyikének egy hivatkozást, amelyre rákattintva telepítése, mint laboratóriumi-szerint a saját Azure előfizetés vagy testre szabhatja a sablon és a [PowerShell alrendszerrel vagy Azure CLI telepítése](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Miért létrehozásakor a VMs a különböző erőforrás csoportok tetszőleges nevek? Átnevezése vagy módosítása az erőforrás csoportokhoz? 
Erőforráscsoport ily módon készült Azure DevTest Labs kezelheti a felhasználói engedélyek és hozzáférés virtuális gépeken futó sorrendjének. Miközben a virtuális a kívánt nevet a áthelyezheti egy másik erőforráscsoport, ezzel nem ajánlott. Dolgozunk, a nagyobb rugalmasság érdekében engedélyezése a élmény javítására.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hány labs hozhat létre a ugyanabban az előfizetésben? 
Nem meghatározott a vonatkozó korlát előfizetésenként létrehozott labs nem. Azonban használt erőforrások korlátozódik előfizetésenként. Kapcsolatos [korlátozások és kvóták kirótt Azure előfizetések](../azure-subscription-service-limits.md) és [Ezek a korlátok növeléséről](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)erről. 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Hány VMs hozhat létre egy labor? 
Nem meghatározott a vonatkozó korlát per labor létrehozott VMs nem. Azonban jelenleg laboratóriumi támogatja az operációs rendszert futtató szabványos tárolóban lévő egyszerre csak körülbelül 40 VMs és prémium tárolóban lévő párhuzamosan fut 25 VMs. Jelenleg dolgozunk, ezek a korlátok növelésével kapcsolatban. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hogyan oszthatok meg közvetlen hivatkozást a labor?

Közvetlen hivatkozás a labor felhasználók megosztása végezheti el az alábbi eljárást.

1. Tallózással keresse meg a labor az Azure-portálon.
2. Nyissa meg a böngészőjében labor URL másolása másokkal, és ossza meg a labor felhasználókkal. 

>[AZURE.NOTE] Ha a labor felhasználók [MSA fiókkal](#what-is-a-microsoft-account) rendelkező külső felhasználókat, és a vállalat az Active directory nem tartoznak, jelenhet hiba esetén az alábbi hivatkozást lépés. Ha az azok hiba jelenik meg, kérje meg őket, kattintson a nevére, az Azure portál jobb felső sarkában, és jelölje be a csak a menü **címtár** szakaszáról laboratóriumi tartalmazó könyvtár.

### <a name="what-is-a-microsoft-account"></a>Mi az Microsoft-fiókkal?

Microsoft-fiókkal, használja az szinte minden, a Microsoft eszközök és szolgáltatások érheti el. Az e-mail címét és a Skype, a Outlook.com, a onedrive-on, a Windows Phone és a Xbox LIVE – való bejelentkezéshez használt jelszót, és azt jelenti, hogy a fájlok, fényképek, partnerek és a beállítások, amelyeket követve, bármilyen eszközről. 

>[AZURE.NOTE] Microsoft-fiók használatával hívható "Windows Live ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Az eltérés virtuális létrehozása során nem sikerült. Hogyan háríthatók el azt? 
Olvassa el a blogbejegyzés - írt egy saját MVP - [hibák elhárítása az eltéréseket AzureDevTestLabs adatkapcsolat](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) megtudhatja, hogy miként juthat a sikertelen eltérés vonatkozó naplókat. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Miért nem a virtuális hálózatát megfelelően menteni?  
Egy lehetősége, hogy a virtuális hálózat nevét tartalmazza-e az időszakok. Ha igen, próbálja meg az időszakok eltávolítása vagy lecserélése kötőjeleket, és ezután mentse újra a a virtuális hálózat.
