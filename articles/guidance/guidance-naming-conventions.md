<properties
   pageTitle="Azure erőforrások elnevezési szabályai ajánlott |} Útmutató |} Microsoft Azure"
   description="Azure erőforrások elnevezési szabályai ajánlott. Hogyan virtuális gépeken futó, tároló fiókok, hálózatok, virtuális hálózatok, alhálózat és egyéb Azure személyek neve"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Azure erőforrások ajánlott elnevezési szabályai

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

A választási lehetőségek, a Microsoft Azure-ban bármely erőforrás nevét, fontos, mert:

- Érdemes nehezen később neveként.
- Nevek kell az adott erőforrás típusa követelményeinek.

Következetes elnevezési szabályokat könnyebben erőforrások keresse meg azt. Is jelezheti, hogy a szerepkör a megoldás egy erőforrás.

Ez a témakör az elnevezési szabályokat és korlátozásokat az Azure erőforrások összefoglaló és javaslatok elnevezési szabályai eredeti halmazából.  Is használhatja ezeket a javaslatok kiindulási pontként saját bizonyos szabályok az igényeinek megfelelően.  

A fájlelnevezési szabályokat sikerének kulcsa létrehozásáról és az alkalmazások és a szervezetek keresztül követni őket. 

## <a name="naming-subscriptions"></a>Az előfizetések elnevezése

Azure előfizetések megadásakor a részletes nevek ellenőrizze a környezet és az egyes előfizetések törlése célját ismertetése.  Ha sok előfizetést tartalmazó környezetben dolgozik, megosztott elnevezni követő javíthatja áttekinthetővé.

Elnevezési előfizetések ajánlott mintájának van:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Vállalati általában lenne ugyanaz az egyes előfizetések. Egyes vállalatok azonban a gyermek vállalataihoz a szervezeti struktúra lehetnek. Ezek a vállalatok előfordulhat, hogy egy központi informatikai csoport kezeli. Ebben az esetben azok is lehet megkülönböztetni úgy, hogy a szülő vállalatnév (*Contoso*) és a gyermek vállalatnév (*Északi szél*).

- Részleg egy nevet a szervezeten belül, amikor személyek csoportja működik. Ezt az elemet a választható névtér belül.

- Termék sorban egy adott termék vagy függvény, amely a részleg belül megtörténik a nevének.
Ez a általában nem kötelező, a belső elérésű szolgáltatásokhoz és alkalmazásokhoz. Azonban ajánlott egyszerűen elválasztás és azonosítás (például mint számlázási rekordok törlése szétválasztására) igénylő nyilvános szolgáltatások használatához.

- Környezete a szolgáltatások, például a fejlesztők, a kérdések és válaszok – vagy a gyártási és az alkalmazások telepítési életciklusa leíró nevét.

| Vállalati | Szervezeti egység | Vonal termék vagy szolgáltatás | Környezet | Teljes név  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| A contoso | SocialGaming | AwesomeService | Gyártási | A Contoso SocialGaming AwesomeService előállítása |
| A contoso | SocialGaming | AwesomeService | A fejlesztői | A Contoso SocialGaming AwesomeService fejlesztők |
| A contoso | AZT | InternalApps | Gyártási | A Contoso informatikai InternalApps előállítása |
| A contoso | AZT | InternalApps | A fejlesztői | A Contoso informatikai InternalApps fejlesztők |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Elő-/ utótagok segítségével védekezhet az ellentmondás

Azure-ban erőforrások megadásakor ajánlott használata a közös prefixumokban vagy utótag típusa és az erőforrás környezetében.  Metaadat-, a típus kapcsolatos összes információt közben környezetben, elérhető programozás útján, gyakori elő-/ utótagok alkalmazása egyszerűbbé teszi a vizuális azonosítása.  Ha elő-/ utótagok beépítése az elnevezésük is egységes, fontos pontosan meghatározhatja, hogy a utótag (előtag) nevére az elején vagy végén (utótag).  

Íme például egy olyan számítási motor szolgáltatója szolgáltatás két lehetőség nevet:

- SvcCalculationEngine (előtag)
- CalculationEngineSvc (utótag)

Különböző tényezők, amelyek bemutatják, az adott erőforrás elő-/ utótagok is hivatkozhat. Az alábbi táblázatban néhány példát mutat jellemző felhasználási módjuk látható.

| Méretarány | Példa | Jegyzetek |
| ------ | ------- | ----- |
| Környezet | a fejlesztői termékkatalógus, a kérdések és válaszok | A környezet, az erőforrás-azonosító |
| Hely | UW (US nyugati), ue (US kelet) | Azonosítja a régió, amelybe az erőforrás telepítve van |
| Példány | 01, 02 | Az erőforrások rendelkező egynél több elnevezett példány (webkiszolgálón stb.). |
| Termék vagy szolgáltatás | szolgáltatás | Azonosítja a terméket, alkalmazás vagy szolgáltatás, amely támogatja az erőforrás |
| Szerepkör | SQL-ben webes üzenetküldés | Azonosítja azt a szerepkört, a hozzárendelt erőforrás |

Egy adott használni kívánt névformátumot a vállalata vagy a projektek fejlesztésekor tapasztalhassa fontosabb elő-/ utótagok és elfoglalt helyük (utótag vagy előtag) közös készletének kiválasztása.

## <a name="naming-rules-and-restrictions"></a>Elnevezési szabályokat és korlátozásokat

Azure-ban erőforrás-szolgáltatás típusonként kényszeríti elnevezési korlátozásokat, illetve a hatókör; csoportja bármely elnevezési konvenció vagy minta kapcsolódó elnevezési szabályokat és hatókör kell igazodjon.  Például közben egy virtuális nevét a DNS-név megfelelteti (és így szükséges az összes Azure belül egyedinek kell), a VNET neve van hatóköre az erőforráscsoport létrehozott belül azt.

Az általános zsugorítása a speciális karaktereket (`-` vagy `_`), az első vagy utolsó karaktere egy tetszőleges nevet. Ezeket a karaktereket okoz a legtöbb érvényességi szabályok sikertelen lesz.

| Kategória | Szolgáltatás vagy entitás | Hatókör | Hossza | Géphez | Érvényes karakterek: | Javasolt mintával | Példa |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Erőforráscsoport | Erőforráscsoport | Globális | 1 – 64-es | Kis-és nagybetűk | Betű vagy, aláhúzás és kötőjel | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Erőforráscsoport | Elérhetőség beállítása | Erőforráscsoport | 1-80 | Kis-és nagybetűk | Betű vagy, aláhúzás és kötőjel | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Általános | Címke | Társított entitás | 512 (név), a 256 (érték) | Kis-és nagybetűk | Alfanumerikus | `"key" : "value"` | `"department" : "Central IT"` |
| Számítási | Virtuális gépen | Erőforráscsoport | 1 – 15 | Kis-és nagybetűk | Betű vagy, aláhúzás és kötőjel | `<name>-<role>-<instance>` | `profx-sql-001` |
| Tárhely | Tárterület fióknév (adatok) | Globális | 3 – 24 | Kisbetű | Alfanumerikus | `<service short name><type><number>` | `profxdata001` |
| Tárhely | Tárterület fióknév (lemez) | Globális | 3 – 24 | Kisbetű | Alfanumerikus | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Tárhely | Tároló neve | Tárterület-fiók | 3 – 63 |   Kisbetű | Betű vagy és kötőjel | `<context>` | `logs` |
| Tárhely | BLOB neve | A tároló | 1 – 1024 | Kis-és nagybetűk | Bármely URL-cím karakter | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Tárhely | Várólista neve | Tárterület-fiók | 3 – 63 | Kisbetű | Betű vagy és kötőjel | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Tárhely | Táblázat neve | Tárterület-fiók | 3 – 63 |Kis-és nagybetűk | Alfanumerikus | `<service short name>-<context>` | `awesomeservice-logs` |
| Tárhely | Fájlnév | Tárterület-fiók | 3 – 63 | Kisbetű | Alfanumerikus | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Hálózati | Virtuális hálózati (VNet) | Erőforráscsoport | 2 – 64-es | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `<service short name>-[section]-vnet` | `profx-vnet` |
| Hálózati | Alhálózat | Szülő-VNet | 2 – 80 | Nagybetűk | Betű vagy, aláhúzás, szaggatott és időszak | `<role>-subnet` | `gateway-subnet` |
| Hálózati | Hálózati kapcsolat | Erőforráscsoport | 1-80 | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Hálózati | Hálózati biztonsági csoport | Erőforráscsoport | 1-80 | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Hálózati | Hálózati biztonsági csoport szabály | Erőforráscsoport | 1-80 | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `<descriptive context>` | `sql-allow` |
| Hálózati | Nyilvános IP-cím | Erőforráscsoport | 1-80 | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `<vm or service name>-pip` | `profx-sql1-pip` |
| Hálózati | Terheléselosztó | Erőforráscsoport | 1-80 | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `<service or role>-lb` | `profx-lb` |
| Hálózati | Töltse be a megfelelő szabályaival Config | Terheléselosztó | 1-80 | Nagybetűk | Betű vagy szaggatott, aláhúzás és időszak | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Erőforrások címkékkel rendszerezése

Az Azure erőforrás-kezelő tetszőleges szöveges karakterláncok azonosítani tudja a környezetben, és egyszerűsítheti az automatizálási a címkézési szervezetek támogatja.  Ha például a címke `"sqlVersion: "sql2014ee"` VMs tudta azonosítani az SQL Server 2014-es vállalati Edition rendszerű az automatizált parancsfájlok futtatásának velük szemben környezetben.  Címkék használandó kiegészítheti, és javíthatja minőségét a választott elnevezési szabályokat oldalán környezetben.

> [AZURE.TIP]Egy másik címkék előnye, hogy a címkék erőforrás-csoportokat, lehetővé téve a szervezetek összehangolására elemezve telepítések keresztül, és hogyan időtartamát.

Minden erőforrás-erőforráscsoport legfeljebb **15** címkék állhat. A címke nevét a 512 karakternél korlátozódik, és a címke értéke legfeljebb 256 karakterből állhatnak.

Erőforrás-címkézés kapcsolatos további tudnivalókért tekintse át [az Azure erőforrások rendszerezéséhez használata címkék](../resource-group-using-tags.md).

A közös címkézési használati esetek a következők:

- A **Számlázás**; Erőforrások csoportosítása és a társítás számlázási vagy költség biztonsági kódot.
- **Szolgáltatási környezetben azonosító**; Csoportok az erőforrások azonosítása erőforrás csoportok közötti gyakori műveletek és csoportosítás
- A **hozzáférés-vezérlés és biztonsági környezet**; Rendszergazdai szerepkör azonosító alapján portfólió, rendszer, szolgáltatás, alkalmazás, példány stb.

> [AZURE.TIP]Címke korai – gyakran címke.  A legjobb helyen séma címkézés alapterv van, és fölé, hogy utólag alakítanunk helyett idő módosítása.  

Példa néhány gyakori címkézési módszerekkel:

| Címkenév | Kulcs | Példa | Megjegyzés |
| -------- | --- | ------- | ------- |
| Számla / belső visszaterhelési azonosító | billTo  | `IT-Chargeback-1234` | Egy belső I/O vagy a számlázási kódot. |
| Operátor vagy közvetlenül felelős személy (DRI) | managedBy | `joe@contoso.com`  | Alias vagy e-mail címe |
| Projekt neve | Projekt-neve | `myproject`  | A projekt vagy a termékkulcs sor név |
| Project verzió | Project-verzió | `3.4`  | A projekt vagy a termékkulcs sor verziója |
| Környezet | környezet | `<Production, Staging, QA >` | Környezetvédelmi azonosító | 
| Réteg | réteg | `Front End, Back End, Data` | Réteg vagy szerepkör/környezetre azonosítása |
| Adatok profil | dataProfile | `Public, Confidential, Restricted, Internal` | Védelmi szintjének az erőforrás tárolt adatok |
 
## <a name="tips-and-tricks"></a>Tippek és trükkök

Bizonyos típusú erőforrás megkövetelheti további gondot a elnevezéséről és a szabályokat.

### <a name="virtual-machines"></a>Virtuális gépeken futó

Nagyobb topológiák, különösen gondosan elnevezési virtuális gépeken futó leegyszerűsíti a szerepkört, és az egyes gépi célját azonosító, és segítségével, így több kiszámítható a parancsfájlok.

> [AZURE.WARNING]Minden Azure virtuális gép az Azure erőforrás nevét és az operációs rendszer állomásnév tartalmaz.  
> Ha az erőforrás neve és a host name eltérőek, a VMs kezelése nehéz lehet és kerülendő.
> Ha van már egy virtuális gép létrehozott egy .vhd például a beállított operációs rendszert tartalmazó a hostname (állomásnév) tartalmazza.

- [A Windows Server VMs elnevezési szabályai](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Tárterület-fiókok és a tárhely szervezetek

Nincsenek két fő használati eset tároló fiókok - biztonsági másolatot a lemezt a VMs és BLOB, a sorok és a táblázatok adatainak tárolása.  Virtuális lemez használt tárterület-fiókokat kell hajtsa végre az elnevezésük is egységes társítása őket a szülő virtuális nevét a (és a potenciális kell több tárterületet fiókot az csúcsteljesítményű virtuális termékváltozatok, is alkalmazhat szám utótag).

> [AZURE.TIP]Tárterület-fiókok – az adatok vagy a lemezt - kell követniük elnevezni, amely lehetővé teszi, hogy több tárterületet fiókjához kell kapcsolatos károkozásra (azaz mindig használata egy numerikus utótag).

Akkor lehetséges eléréséhez blob Azure tárterület-fiókját az egyéni tartománynevet.
Az alapértelmezett végpont a Blob-szolgáltatás `https://mystorage.blob.core.windows.net`.

A blob-végpont egy egyéni tartományt (például www.contoso.com) megfeleltetése a tárterület-fiókjához, ha is elérheti, de azt a tartományt a blob-adatok a tárterület-fiókjában. Ha például az egyéni tartománynevet `http://mystorage.blob.core.windows.net/mycontainer/myblob` mint érhető el `http://www.contoso.com/mycontainer/myblob`.

Ez a funkció beállításával kapcsolatos további tudnivalókért olvassa el az [egyéni tartománynevet a Blob-tároló végpont beállítása](../storage/storage-custom-domain-name.md).

További információt a BLOB, a tárolók és a táblázatok elnevezési:

- [Elnevezéséről és hivatkozó tárolók BLOB és metaadatok](https://msdn.microsoft.com/library/dd135715.aspx)
- [Sorok és a metaadatokat elnevezése](https://msdn.microsoft.com/library/dd179349.aspx)
- [Táblázatok elnevezése](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Blob nevét karakterek tetszőleges kombinációját is tartalmazhat, de foglalt URL-cím karakterek megfelelően kell megjelölni. Elkerülheti, végződhet ponttal (.), perjel (/), blob-nevek vagy sorrend vagy a kettő kombinációját. Kiállítás, amelyet a perjellel **virtuális** Directoryban elválasztó karaktert. Ne használjon perjellel (\) blob nevét. Az ügyfél API-khoz előfordulhat, hogy engedélyezze azt, de majd nem megfelelően kivonat, és az aláírások nem egyezik meg fog.

Még nem lehetséges a tárterület-fiókja vagy a tároló nevének módosításához, miután lett létrehozva.
Ha szeretne egy új nevet, törölje azt, majd hozzon létre egy újat.

> [AZURE.TIP] Azt javasoljuk, hogy, elnevezési konvenció létrehozása az összes tároló fiókok és típusú új szolgáltatás vagy alkalmazás fejlesztésének megélénkült előtt.

## <a name="example---deploying-an-n-tier-service"></a>Példa - a-n szintű szolgáltatás üzembe helyezése

Ebben a példában azt határozza meg n szintű konfigurációjának álló előtér IIS-kiszolgálókat (a Windows Server VMs is), (a két Windows Server VMs is), az SQL Server-ElasticSearch fürt (6 Linux VMs található), és a társított tárterület-fiók esetén virtuális hálózatok erőforrás csoport, és terheléselosztó.

Lássuk először a helyi szabályokat az ehhez az alkalmazáshoz megadásával:

| Személy | Kiállítás | Leírás  |
| ------ | ---------- | ------------ |  
| Szolgáltatás neve | `profx` | Az alkalmazás vagy szolgáltatás üzembe helyezéséhez rövid neve |
| Környezet | `prod` | Ez a az éles üzemi (nem pedig a kérdések és válaszok, próba stb.) |

Az, hogy az eredeti azt tudja majd feltérképezni a szabályok az erőforrás különböző:

| Erőforrás típusa | Kiállítás alap | Példa |
| ------------- | --------------- | ------- |
| Előfizetés | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Erőforráscsoport | `servicename-rg` | `profx-rg` |
| Virtuális hálózati | `servicename-vnet` | `profx-vnet` |
| Alhálózat | `role-subnet` | `sql-vnet` |
| Terheléselosztó | `servicename-lb` | `profx-lb` |
| Virtuális gépen | `servicename-role[number]` | `profx-sql0` |
| Tárterület-fiók | `<vmnamenodashes>st<num>` | `profxsql0st0` |

A diagram következő látható:

![alkalmazás topológia diagram] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Minta Keresőalkalmazás topológiája")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Minta – üzembe helyezése a fenti minta Azure CLI parancsfájl

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
