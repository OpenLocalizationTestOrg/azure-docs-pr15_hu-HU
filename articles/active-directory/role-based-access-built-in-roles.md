<properties
    pageTitle="RBAC: Beépített szerepkörök |} Microsoft Azure"
    description="Szerepköralapú hozzáférés-szerepalapú szerepkörök Ez a témakör ismerteti a beépített."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Beépített szerepkörök

Azure szerepköralapú hozzáférés vezérlő (RBAC) megtalálható a következő beépített szerepkörök, hogy a felhasználók, csoportok és szolgáltatások rendelhetők. A beépített szerepkörök definíciója nem módosíthatók. [Egyéni szerepkörök Azure RBAC](role-based-access-control-custom-roles.md) igazítása a szervezet igényeinek is létrehozhat.

## <a name="roles-in-azure"></a>Szerepkörök Azure-ban

Az alábbi táblázat rövid ismertetését beépített szerepkörök. Kattintson a szerepkör nevére, **Műveletek** és **notactions** a szerep részletes listájának megjelenítéséhez. A **Műveletek** tulajdonság megadja az engedélyezett műveletek Azure erőforrásait. Művelet karakterláncok helyettesítő karaktereket is használhat. A **notactions** tulajdonság adja meg, hogy a műveleteket, amelyeket az engedélyezett műveletek tartoznak.

>[AZURE.NOTE] Az Azure szerepkör-definíciók is folyamatosan bővül. Ez a cikk az naprakészek a lehető, de a Azure PowerShellben mindig megtalálhatja a legfrissebb szerepkörök definíciók. A parancsmagok használata `(get-azurermroledefinition "<role name>").actions` vagy `(get-azurermroledefinition "<role name>").notactions` alkalmazhatók.

| Szerepkör neve | Leírás |
| --------- | ----------- |
| [API Management szolgáltatás közös munka](#api-management-service-contributor) | Kezelheti az API-kezelés szolgáltatások |
| [Alkalmazás Hírcsatornájában összetevő közös munka](#application-insights-component-contributor) | Kezelheti az alkalmazás az összefüggéseket összetevők |
| [Automatizálási operátor](#automation-operator) | Használhatja, leállítása, felfüggesztheti, folytathatja a feladatok |
| [BizTalk közös munka](#biztalk-contributor) | Kezelheti a BizTalk szolgáltatások |
| [Közreműködői ClearDB MySQL-adatbázis](#cleardb-mysql-db-contributor) | Kezelheti a ClearDB MySQL-adatbázis |
| [Közös munka](#contributor) | Hozzáférés kivételével minden kezelheti. |
| [Adatok gyári közös munka](#data-factory-contributor) | Létrehozhat és adatok gyárak és a bennük a gyermek erőforrások kezelése. |
| [DevTest Labs felhasználói](#devtest-labs-user) | Minden tekintheti meg és csatlakozás a Kezdés, indítsa újra és leállítás virtuális gépeken futó |
| [A DNS-zóna közös munka](#dns-zone-contributor) | DNS-zónák és a rekordok kezeléséhez |
| [DocumentDB-fiók közös munka](#documentdb-account-contributor) | Kezelheti a DocumentDB fiókok |
| [Intelligens rendszerek fiók közös munka](#intelligent-systems-account-contributor) | Intelligens rendszerek fiókok is kezelése |
| [Hálózati közös munka](#network-contributor) | Hogyan kezelhetik az összes hálózati erőforrások |
| [Új Relic APM fiók közös munka](#new-relic-apm-account-contributor) | Új Relic teljesítményét Alkalmazáskezelés fiókok és az alkalmazások kezelhetik |
| [Tulajdonos](#owner) | Kezelheti a szolgáltatás, beleértve a hozzáférést |
| [A képernyőolvasók](#reader) | Megtekintheti a mindent, de nem lehet módosítani |
| [Vgx.dll gyorsítótár közös munka](#redis-cache-contributor) | Kezelheti a vgx.dll gyorsítótárát |
| [A Feladatütemező feladat gyűjtemények közös munka](#scheduler-job-collections-contributor) | Kezelheti a ütemezőt feladat gyűjtemények |
| [Keresési szolgáltatás közös munka](#search-service-contributor) | Kezelheti a keresési szolgáltatás |
| [Biztonsági kezelője](#security-manager) | Biztonsági összetevők, a biztonsági házirendek és a virtuális gépeken futó is kezelése |
| [SQL-adatbázis közös munka](#sql-db-contributor) | Hogyan kezelhetik az SQL-adatbázisok, de nem saját biztonsági házirendek |
| [SQL-biztonsági Manager](#sql-security-manager) | Hogyan kezelhetik az SQL-kiszolgálók és adatbázisok biztonsággal kapcsolatos házirendek |
| [Az SQL Server közös munka](#sql-server-contributor) | Hogyan kezelhetik az SQL-kiszolgálók és adatbázisok, de nem saját biztonsági házirendek |
| [Klasszikus tárterület-fiók közös munka](#classic-storage-account-contributor) | Kezelheti a klasszikus tárterület-fiókok |
| [Tárterület-fiók közös munka](#storage-account-contributor) | Kezelheti a tárterület-fiókok |
| [Felhasználói hozzáférés-rendszergazda](#user-access-administrator) | Azure erőforrások felhasználó hozzáférését kezelheti |
| [Klasszikus virtuális gép közös munka](#classic-virtual-machine-contributor) | Kezelheti a klasszikus virtuális gépeken futó, de nem a virtuális hálózati vagy tároló fiókot, amelyhez csatlakozott |
| [Virtuális gép közös munka](#virtual-machine-contributor) | Kezelheti a virtuális gépeken futó, de nem a virtuális hálózati vagy tároló fiókot, amelyhez csatlakozott |
| [Klasszikus hálózati közös munka](#classic-network-contributor) | Kezelhetik a klasszikus virtuális hálózatok és fenntartott IP-címei |
| [Webes csomag közös munka](#web-plan-contributor) | Kezelheti a tervek webhely |
| [Webhely közös munka](#website-contributor) | Kezelhetik a webhelyek, de nem a webes tervek, amelyhez csatlakozott |

## <a name="role-permissions"></a>Szerepkör-engedélyek
Az alábbi táblázat ismerteti az egyes szerepkört fordítani engedélyeket. A **Műveletek**, amelyek a megadásával engedélyezhetik, és **NotActions**, őket korlátozó is tartalmazhat.

### <a name="api-management-service-contributor"></a>API Management szolgáltatás közös munka
Kezelheti az API-kezelés szolgáltatások

| **Műveletek** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Hozzon létre és API-kezelés szolgáltatások kezelése |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvasási szerepkörök és szerepkör-hozzárendelés |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="application-insights-component-contributor"></a>Alkalmazás Hírcsatornájában összetevő közös munka
Kezelheti az alkalmazás az összefüggéseket összetevők

| **Műveletek** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és szerepkör-hozzárendelés |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.Insights/components/* | Létrehozhatja és kezelheti az összefüggéseket összetevők |
| Microsoft.Insights/webtests/* | Létrehozhatja és kezelheti a webhely vizsgálatok |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="automation-operator"></a>Automatizálási operátor
Használhatja, leállítása, felfüggesztheti, folytathatja a feladatok

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és szerepkör-hozzárendelés |
| Microsoft.Automation/automationAccounts/jobs/read | Olvassa el az automatizálási fiók feladatok |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Az automatizálási fiók feladat folytatása |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Az automatizálási fiók feladat leállítása |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Automatizálási fiók feladat adatfolyamok olvasása |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Az automatizálási fiók feladat felfüggesztése |
| Microsoft.Automation/automationAccounts/jobs/write | Írja be az automatizálási fiók feladatok |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Olvassa el az automatizálási fiók ütemezett feladat |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Olvassa el az automatizálási fiók ütemezett feladat |
| Microsoft.Automation/automationAccounts/read | Olvassa el az automatizálási fiókok |
| Microsoft.Automation/automationAccounts/runbooks/read | Automatizálási runbooks olvasása |
| Microsoft.Automation/automationAccounts/schedules/read | Olvassa el az automatizálási fiók ütemtervek |
| Microsoft.Automation/automationAccounts/schedules/write | Írja be az automatizálási fiók ütemtervek |
| Microsoft.Insights/components/* | Létrehozhatja és kezelheti az összefüggéseket összetevők |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="biztalk-contributor"></a>BizTalk közös munka
Kezelheti a BizTalk szolgáltatások

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és szerepkör-hozzárendelés |
| Microsoft.BizTalkServices/BizTalk/* | Hozzon létre és BizTalk szolgáltatások kezelése |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="cleardb-mysql-db-contributor"></a>Közreműködői ClearDB MySQL-adatbázis
Kezelheti a ClearDB MySQL-adatbázis

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és szerepkör-hozzárendelés |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |
| successbricks.cleardb/Databases/* | Hozzon létre és ClearDB MySQL-adatbázis kezelése |

### <a name="contributor"></a>Közös munka
Hozzáférés kivételével minden kezelhetik

| **Műveletek** ||
| ------- | ------ |
| * | Hozzon létre és típusú erőforrások kezelése |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Szerepkörök és szerepkör-hozzárendelés nem törölhetők |  
| Microsoft.Authorization/*/Write | Nem hozható létre szerepkörök és szerepkör-hozzárendelés |

### <a name="data-factory-contributor"></a>Adatok gyári közös munka
Létrehozhatja és kezelheti az adatok gyárak, és a gyermek erőforrások határértékeket.

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.DataFactory/dataFactories/* | Létrehozhatja és kezelheti az adatok gyárak, és a gyermek erőforrások határértékeket. |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="devtest-labs-user"></a>DevTest Labs felhasználói
Minden tekintheti meg és csatlakozás a Kezdés, indítsa újra és leállítás virtuális gépeken futó

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Compute/availabilitySets/read | Olvassa el a elérhetősége kifejezéskészletek tulajdonságai |
| Microsoft.Compute/virtualMachines/*/read | Olvassa el a tulajdonságok egy virtuális gép (virtuális méretű, futtatókörnyezet állapot, virtuális bővítmények stb.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Virtuális gépeken futó felszabadítása |
| Microsoft.Compute/virtualMachines/read | A tulajdonságok egy virtuális gép olvasása |
| Microsoft.Compute/virtualMachines/restart/action | Indítsa újra a virtuális gépeken futó |
| Microsoft.Compute/virtualMachines/start/action | Indítsa el a virtuális gépeken futó |
| Microsoft.DevTestLab/*/read | Laboratóriumi tulajdonságainak olvasása |
| Microsoft.DevTestLab/labs/createEnvironment/action | Hozzon létre egy tesztkörnyezetben |
| Microsoft.DevTestLab/labs/formulas/delete | Képlet törlése |
| Microsoft.DevTestLab/labs/formulas/read | Olvassa el a képletek |
| Microsoft.DevTestLab/labs/formulas/write | Hozzáadása vagy módosítása a képletek |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Labor házirendek felmérése |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bekapcsolódás a betöltés terheléselosztó kódmentes cím készletben |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bekapcsolódás egy terheléselosztó bejövő hálózati Címfordítást szabály |
| Microsoft.Network/networkInterfaces/*/read | Olvassa el a tulajdonságok egy hálózati kapcsolat (például az összes a terheléselosztókkal, amely a hálózati kapcsolaton egy része) |
| Microsoft.Network/networkInterfaces/join/action | A virtuális gép csatlakozzon a hálózati kapcsolat |
| Microsoft.Network/networkInterfaces/read | Olvassa el a hálózati kapcsolatok |
| Microsoft.Network/networkInterfaces/write | Hálózati kapcsolatok írása |
| Microsoft.Network/publicIPAddresses/*/read | Olvassa el a tulajdonságok egy nyilvános IP-cím |
| Microsoft.Network/publicIPAddresses/join/action | Bekapcsolódás egy nyilvános IP-cím |
| Microsoft.Network/publicIPAddresses/read | Olvassa el a hálózati nyilvános IP-címek |
| Microsoft.Network/virtualNetworks/subnets/join/action | Csatlakozás egy virtuális hálózathoz |
| Microsoft.Resources/deployments/operations/read | Olvassa el a telepítési műveletek |
| Microsoft.Resources/deployments/read | Olvassa el a telepítések |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Storage/storageAccounts/listKeys/action | Listát tároló fiók kulcsok |

### <a name="dns-zone-contributor"></a>A DNS-zóna közös munka
Kezelheti a DNS-zónák és a rekordok.

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/\*/olvasása | Olvasási szerepkörök és szerepkör-hozzárendelés |
| Microsoft.Insights/alertRules/\* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.Network/dnsZones/\* | DNS-zónák és a rekordok létrehozása és kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/\* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/\* | Hozzon létre és támogatási jegyek kezelése |


### <a name="documentdb-account-contributor"></a>DocumentDB-fiók közös munka
Kezelheti a DocumentDB fiókok

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.DocumentDb/databaseAccounts/* | DocumentDB-fiókok létrehozása és kezelése |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="intelligent-systems-account-contributor"></a>Intelligens rendszerek fiók közös munka
Intelligens rendszerek fiókok is kezelése

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.IntelligentSystems/accounts/* | Intelligens rendszerek-fiókok létrehozása és kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="network-contributor"></a>Hálózati közös munka
Hogyan kezelhetik az összes hálózati erőforrások

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.Network/* | Létrehozhatja és kezelheti a hálózatok |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="new-relic-apm-account-contributor"></a>Új Relic APM fiók közös munka
Új Relic teljesítményét Alkalmazáskezelés fiókok és az alkalmazások kezelhetik

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |
| NewRelic.APM/accounts/* | Új Relic alkalmazás teljesítményének management fiókok létrehozása és kezelése |

### <a name="owner"></a>Tulajdonos
Kezelheti a szolgáltatás, beleértve a hozzáférést

| **Műveletek** ||
| ------- | ------ |
| * | Hozzon létre és típusú erőforrások kezelése |

### <a name="reader"></a>A képernyőolvasók
Megtekintheti a mindent, de nem lehet módosítani

| **Műveletek** ||
| ------- | ------ |
| * / olvasása | Olvassa el a titkos kulcsok kivételével az összes típusú erőforrásokat. |

### <a name="redis-cache-contributor"></a>Vgx.dll gyorsítótár közös munka
Kezelheti a vgx.dll gyorsítótárát

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Cache/redis/* | Létrehozhatja és kezelheti a vgx.dll gyorsítótárát |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="scheduler-job-collections-contributor"></a>A Feladatütemező feladat gyűjtemények közös munka
Kezelheti a Feladatütemező feladat gyűjtemények

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Scheduler/jobcollections/* | Létrehozhatja és kezelheti a feladat gyűjtemények |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése  |

### <a name="search-service-contributor"></a>Keresési szolgáltatás közös munka
Kezelheti a keresési szolgáltatás

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Search/searchServices/* | Létrehozhatja és kezelheti a keresési szolgáltatás |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése  |

### <a name="security-manager"></a>Biztonsági kezelője
Biztonsági összetevők, a biztonsági házirendek és a virtuális gépeken futó is kezelése

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.ClassicCompute/*/read | Olvassa el a konfigurációs adatok klasszikus virtuális gépeken futó számítja ki. |
| Microsoft.ClassicCompute/virtualMachines/*/write | A virtuális gépeken futó konfiguráció írása |
| Microsoft.ClassicNetwork/*/read | Klasszikus hálózati konfigurációja olvashat  |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Security/* | Biztonsági összetevők és a házirendek létrehozása és kezelése |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése  |

### <a name="sql-db-contributor"></a>SQL-adatbázis közös munka
Hogyan kezelhetik az SQL-adatbázisok, de nem saját biztonsági házirendek

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvasási szerepkörök és a hozzárendelések szerepkör |
| Microsoft.Insights/alertRules/* | Hozzon létre és figyelmeztetési szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Sql/servers/databases/* | Hozzon létre és SQL-adatbázisok kezelése |
| Microsoft.Sql/servers/read | Olvassa el az SQL-kiszolgálók |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Naplózási házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/auditingSettings/* | A naplózási beállítások nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Naplóbejegyzések nem tudja olvasni. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Kapcsolat házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Nem szerkeszthetők a szabályok maszkolás adatok |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Biztonsági értesítés házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Biztonsági mértékek nem szerkeszthetők. |

### <a name="sql-security-manager"></a>SQL-biztonsági Manager
Hogyan kezelhetik az SQL-kiszolgálók és adatbázisok biztonsággal kapcsolatos házirendek

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a Microsoft-hitelesítés |
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Sql/servers/auditingPolicies/* | Hozzon létre, és az SQL server naplózási házirendek kezelése |
| Microsoft.Sql/servers/auditingSettings/* | Létrehozhatja és kezelheti az SQL server naplózási beállítás |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Hozzon létre, és az SQL server-adatbázis naplózási házirendek kezelése |
| Microsoft.Sql/servers/databases/auditingSettings/* | Hozzon létre, és az SQL server-adatbázis naplózási beállításainak kezelése |
| Microsoft.Sql/servers/databases/auditRecords/read | Naplózási rekordjainak olvasása |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Hozzon létre, és az SQL server-adatbázis kapcsolat házirendek kezelése |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Hozzon létre, és az SQL server-adatbázis adatainak maszkolás házirendek kezelése |
| Microsoft.Sql/servers/databases/read | Olvasás SQL-adatbázisait |
| Microsoft.Sql/servers/databases/schemas/read | Olvasható az SQL server-adatbázis sémák |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Olvasható az SQL server-adatbázis táblaoszlopok |
| Microsoft.Sql/servers/databases/schemas/tables/read | Olvasható az SQL server-adatbázistáblák |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Hozzon létre, és az SQL server adatbázis biztonsági értesítés házirendek kezelése |
| Microsoft.Sql/servers/databases/securityMetrics/* | Létrehozhatja és kezelheti az SQL server-adatbázis biztonsági mérőszámok |
| Microsoft.Sql/servers/read | Olvassa el az SQL-kiszolgálók |
| Microsoft.Sql/servers/securityAlertPolicies/* | Hozzon létre, és az SQL server biztonsági értesítés házirendek kezelése |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="sql-server-contributor"></a>Az SQL Server közös munka
Hogyan kezelhetik az SQL-kiszolgálók és adatbázisok, de nem saját biztonsági házirendek

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés|
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Sql/servers/* | Hozzon létre és SQL-kiszolgálók kezelése |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Az SQL server naplózási házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/auditingSettings/* | Az SQL server naplózási beállítások nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Az SQL server-adatbázis naplózási házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL server-adatbázis naplózási beállításai nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Naplóbejegyzések nem tudja olvasni. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Az SQL server adatbázis kapcsolat házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-adatbázis adatainak maszkolás házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Az SQL server adatbázis biztonsági értesítés házirendek nem szerkeszthetők. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Az SQL server-adatbázis biztonsági mértékek nem szerkeszthetők. |
| Microsoft.Sql/servers/securityAlertPolicies/* | Az SQL server biztonsági értesítés házirendek nem szerkeszthetők. |

### <a name="classic-storage-account-contributor"></a>Klasszikus tárterület-fiók közös munka
Kezelheti a klasszikus tárterület-fiókok

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.ClassicStorage/storageAccounts/* | Tárterület-fiókok létrehozása és kezelése |
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="storage-account-contributor"></a>Tárterület-fiók közös munka
Képes tárolás fiókokat, de nem érhető el őket.

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el az összes engedély |
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.Insights/diagnosticSettings/* | Diagnosztikai beállítások kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Storage/storageAccounts/* | Tárterület-fiókok létrehozása és kezelése |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="user-access-administrator"></a>Felhasználói hozzáférés-rendszergazda
Azure erőforrások felhasználó hozzáférését kezelheti

| **Műveletek** ||
| ------- | ------ |
| * / olvasása | Olvassa el a titkos kulcsok kivételével az összes típusú erőforrásokat. |
| Microsoft.Authorization/* | Engedély kezelése |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="classic-virtual-machine-contributor"></a>Klasszikus virtuális gép közös munka
Kezelheti a klasszikus virtuális gépeken futó, de nem a virtuális hálózati vagy tároló fiókot, amelyhez csatlakozott

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.ClassicCompute/domainNames/* | Létrehozhatja és kezelheti a tartományneveket klasszikus számítási |
| Microsoft.ClassicCompute/virtualMachines/* | Létrehozhatja és kezelheti a virtuális gépeken futó |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Bekapcsolódás a hálózati biztonsági csoportok |
| Microsoft.ClassicNetwork/reservedIps/link/action | Hivatkozás: fenntartott IP-címei |
| Microsoft.ClassicNetwork/reservedIps/read | Olvasás fenntartott IP-címek |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Virtuális hálózatához csatlakozzanak |
| Microsoft.ClassicNetwork/virtualNetworks/read | Olvassa el a virtuális hálózatok |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Olvassa el a tárhely fiók lemez |
| Microsoft.ClassicStorage/storageAccounts/images/read | További tárterület fiók képek |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listát tároló fiók kulcsok |
| Microsoft.ClassicStorage/storageAccounts/read | Olvassa el a klasszikus tárterület-fiókok |
| Microsoft.Insights/alertRules/* | Hozzon létre és Hírcsatornájában értesítés szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="virtual-machine-contributor"></a>Virtuális gép közös munka
Kezelheti a virtuális gépeken futó, de nem a virtuális hálózati vagy tárolás fiókot, amelyhez csatlakozott

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.Compute/availabilitySets/* | Létrehozni és kezelni a számítási elérhetőségének beállítása |
| Microsoft.Compute/locations/* | Hozzon létre, és a számítási helyek kezelése |
| Microsoft.Compute/virtualMachines/* | Létrehozhatja és kezelheti a virtuális gépeken futó |
| Microsoft.Compute/virtualMachineScaleSets/* | Létrehozhatja és kezelheti a virtuális gép skála beállítása |
| Microsoft.Insights/alertRules/* | Hozzon létre és Hírcsatornájában értesítés szabályok kezelése |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bekapcsolódás a hálózati webalkalmazás átjáró kódmentes cím-készletek |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bekapcsolódás a betöltés terheléselosztó kódmentes cím készletek |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Csatlakozás terheléselosztó hálózati Címfordítást készletek bejövő |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Csatlakozás terheléselosztó bejövő hálózati Címfordítást szabályok |
| Microsoft.Network/loadBalancers/read | Olvasás terheléselosztókkal |
| Microsoft.Network/locations/* | Létrehozhatja és kezelheti a hálózati helyeken |
| Microsoft.Network/networkInterfaces/* | Létrehozhatja és kezelheti a hálózati kapcsolatok |
| Microsoft.Network/networkSecurityGroups/join/action | Bekapcsolódás a hálózati biztonsági csoportok |
| Microsoft.Network/networkSecurityGroups/read | Olvassa el a hálózati biztonsági csoportok |
| Microsoft.Network/publicIPAddresses/join/action | Bekapcsolódás a hálózati nyilvános IP-címek |
| Microsoft.Network/publicIPAddresses/read | Olvassa el a hálózati nyilvános IP-címek |
| Microsoft.Network/virtualNetworks/read | Olvassa el a virtuális hálózatok |
| Microsoft.Network/virtualNetworks/subnets/join/action | Bekapcsolódás a virtuális hálózati alhálózat |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Storage/storageAccounts/listKeys/action | Listát tároló fiók kulcsok |
| Microsoft.Storage/storageAccounts/read | További tárterület-fiókok |
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="classic-network-contributor"></a>Klasszikus hálózati közös munka
Kezelhetik a klasszikus virtuális hálózatok és fenntartott IP-címei

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.ClassicNetwork/* | Létrehozhatja és kezelheti a klasszikus hálózatok |
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |

### <a name="web-plan-contributor"></a>Webes csomag közös munka
Kezelheti a tervek webhely

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |
| Microsoft.Web/serverFarms/* | Létrehozhatja és kezelheti a kiszolgálófarm |

### <a name="website-contributor"></a>Webhely közös munka
Kezelhetik a webhelyek, de nem a webes tervek, amelyhez csatlakozott

| **Műveletek** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Olvassa el a hitelesítés |
| Microsoft.Insights/alertRules/* | Hozzon létre, és a hírcsatornájában értesítés szabályok kezelése |
| Microsoft.Insights/components/* | Létrehozhatja és kezelheti az összefüggéseket összetevők |
| Microsoft.ResourceHealth/availabilityStatuses/read | Az erőforrások állapotának olvasása |
| Microsoft.Resources/deployments/* | Hozzon létre és erőforrás-csoport telepítések kezelése |
| Microsoft.Resources/subscriptions/resourceGroups/read | Olvassa el az erőforrás-csoportok |  
| Microsoft.Support/* | Hozzon létre és támogatási jegyek kezelése |
| Microsoft.Web/certificates/* | Létrehozása és kezelése a webhelyhez tanúsítványok |
| Microsoft.Web/listSitesAssignedToHostName/read | Állomásnév rendelt webhelyeket olvasása |
| Microsoft.Web/serverFarms/join/action | Csatlakozás kiszolgálófarm |
| Microsoft.Web/serverFarms/read | Olvassa el a kiszolgálófarm |
| Microsoft.Web/sites/* | Hozzon létre, és a webhelyek kezelése |

## <a name="see-also"></a>Lásd még:
- [Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): Ismerkedés a RBAC az Azure-portálon.
- [Azure RBAC egyéni szerepkörök](role-based-access-control-custom-roles.md): megtudhatja, hogy miként hozhat létre egyéni szerepkörök access igényeinek.
- [Access létrehozása előzmények jelentés módosítása](role-based-access-control-access-change-history-report.md): nyomon követheti a RBAC módosuló szerepkör-hozzárendelés.
- [Szerepköralapú hozzáférés-vezérlés – hibaelhárítás](role-based-access-control-troubleshooting.md): Ismerkedés a gyakori problémák elhárítására vonatkozó javaslatok.
