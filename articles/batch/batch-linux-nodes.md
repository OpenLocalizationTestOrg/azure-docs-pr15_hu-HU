<properties
    pageTitle="Azure köteg készletek Linux csomópontok |} Microsoft Azure"
    description="További információ a párhuzamos számítási munkaterhelésekből Linux virtuális gépeken futó Azure kötegben forrásanyagok a folyamat."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Azure köteg készletek Linux számítási csomópontok kiépítése

A párhuzamos számítási munkaterhelésekből futtatásához Linux és a Windows virtuális gépeken Azure köteg is használhatja. Ez a cikk részletes Linux számítási csomópontok készletek létrehozása a köteg szolgáltatásban mindkét a [Köteg Python] használatával[ py_batch_package] és a [Köteg .NET] [ api_net] ügyfél tárak.

> [AZURE.NOTE] [Alkalmazáscsomagok](batch-application-packages.md) által jelenleg nem támogatott Linux számítási csomóponton.

## <a name="virtual-machine-configuration"></a>Virtuális gép konfigurálása

A köteg számítási csomópontok erőforráskészlethez tartozik létrehozásakor, két lehetőség áll, amelyhez a jelölje ki a méret csomópontot, és az operációs rendszer: Cloud Services beállításait, illetve virtuális gép beállításait.

**Cloud Services konfigurálása** nyújt a Windows csomópontok *csak*számítja ki. Rendelkezésre álló számítási csomópont méretű [Cloud Services méretben](../cloud-services/cloud-services-sizes-specs.md)jelennek meg, és elérhető operációs rendszerek jelennek meg az [Azure Vendég OS kiadásokban és SDK kompatibilitási mátrix](../cloud-services/cloud-services-guestos-update-matrix.md). Egy készlet Azure Cloud Services csomópontok tartalmazó létrehozásakor kell adja meg csak a csomópont méretének és a "OS család", amely a korábban már említettük cikkekben talál. A Windows készletek csomópontok számítja ki, a Cloud Services leggyakrabban használatos.

**Virtuális gép konfigurációs** Linux és a Windows képek nyújt a számítási csomópontot. Rendelkezésre álló számítási csomópont méretű [az Azure virtuális gépeken futó méretben](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) és [az Azure virtuális gépeken futó méretben](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) jelennek meg. Virtuális gép konfigurációs csomópontok tartalmazó készletet hoz létre, ha meg kell adnia a csomópontok, a virtuális gép Képhivatkozás és a köteg csomópont ügynök telepítve legyen azon a csomópontok Termékváltozat méretének.

### <a name="virtual-machine-image-reference"></a>Virtuális gép Képhivatkozás

A köteg szolgáltatás [virtuális gép skála állítja be](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) biztosítására használja Linux számítási csomópontot. A [Microsoft Azure piactéren]által biztosított e virtuális gépeken futó operációs rendszer lemezképei[vm_marketplace]. Egy virtuális gép Képhivatkozás konfigurálásakor adja meg a tulajdonságok egy piactér virtuális gép kép. A következő tulajdonságokat szükség, ha virtuális gép kép hivatkozás létrehozása:

| **Képtulajdonságok hivatkozás** | **Példa** |
| ----------------- | ------------------------ |
| A Publisher         | Kanonikus                |
| Ajánlat             | UbuntuServer             |
| RAKTÁRI SZÁM               | 14.04.4-LTS              |
| Verzió           | legújabb                   |

> [AZURE.TIP] Akkor talál további információt a tulajdonságok és a piactér képek [választó Linux virtuális gép képek CLI vagy PowerShell Azure-ban és a Navigálás](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md)listában. Ne feledje, hogy nem az összes piactér képek jelenleg kompatibilis köteget. További tudnivalókért lásd: a [csomópont ügynök Termékváltozat](#node-agent-sku).

### <a name="node-agent-sku"></a>Csomópont ügynök Termékváltozat

A köteg csomópont agent futtat a minden egyes készletben csomópontot, és a csomóponton és a köteg szolgáltatás között a parancs és a vezérlő felhasználói felületet nyújt program. Vannak olyan a csomópont-ügynök, különböző operációs rendszereken termékváltozatban, vagyis különböző példányait. Lényegében a virtuális gép konfiguráció létrehozásakor első adja meg a virtuális gép képhivatkozás, és adja a csomópont agent telepítése a képet. Minden egyes csomópont ügynök Termékváltozat általában kompatibilis több virtuális gép képekkel. Íme néhány példa csomópont ügynök termékváltozatok:

* Batch.node.ubuntu 14.04
* Batch.node.centos 7
* Batch.node.Windows amd64

> [AZURE.IMPORTANT] Nem minden virtuális gép képeket a piactér rendelkezésre álló kompatibilisek a jelenleg elérhető köteg csomópont ügynökök. A köteg SDK kell használnia a rendelkezésre álló csomópont agent termékváltozatok és a virtuális gép képek, amellyel azok kompatibilisek listáját. Lásd a jelen cikk további információt a [listában a virtuális gép képek](#list-of-virtual-machine-images) .

## <a name="create-a-linux-pool-batch-python"></a>Linux készlet létrehozása: köteg Python

A következő kódrészletet látható, hogyan használható a [Microsoft Azure köteg ügyfél tár Python] [ py_batch_package] Ubuntu kiszolgáló-készlet létrehozása a számítási csomópontot. A köteg Python modul dokumentációját a következő helyen található [azure.batch csomag] [py_batch_docs] a dokumentumok olvasható.

Ez a kódtöredék létrehoz egy [ImageReference] [ py_imagereference] explicit módon, és adja meg az egyes annak tulajdonságait (publisher ajánlatot, Termékváltozat, verziója). A gyártási kódot, azonban azt javasoljuk, hogy használja-e a [list_node_agent_skus] [ py_list_skus] határozza meg, és válassza a rendelkezésre álló kép és csomópont ügynök Termékváltozat kombinációval futásidőben módszert.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

A korábban említett, akkor javasoljuk, hogy a [ImageReference] helyett[ py_imagereference] kifejezetten, használja a [list_node_agent_skus] [ py_list_skus] módszer dinamikusan kijelölése a kép kombinációval jelenleg támogatott csomópont ügynök/piactérről. A következő Python kódtöredékének jeleníti meg ezt a módszert használatát.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Linux készlet létrehozása: köteg .NET

A következő kódrészletet szemlélteti, hogyan használhatja a [Köteg .NET] [ nuget_batch_net] ügyfél tár Ubuntu kiszolgáló-készlet létrehozása a számítási csomópontot. A [Köteg .NET hivatkozás dokumentációjában] találhat[ api_net] MSDN webhelyen.

A következő kódrészletet használja a [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] metódus jelölje ki a listából a jelenleg támogatott piactér képpel és csomópont ügynök Termékváltozat kombinációk. Ez a módszer akkor célszerű, mert támogatott kombinációk listája időről-időre változhat. Támogatott kombinációk legtöbbször kerülnek.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Bár az előző kódtöredékének használ a [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] módszert dinamikusan listában, és válassza a kép és csomópont ügynök Termékváltozat kombinációk (ajánlott) támogatott, beállíthatja úgy is egy [ImageReference] [ net_imagereference] explicit módon:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Virtuális gép képek listája

Az alábbi táblázat a piactér virtuális gép képeket is a rendelkezésre álló köteg csomópont ügynökök kompatibilis Ez a cikk utolsó frissítésekor. Fontos, hogy ne feledje, hogy a lista nem végleges, mivel a képek és a csomópont ügynökök előfordulhat, hogy, illetve el bármikor. Azt javasoljuk, hogy a köteg-alkalmazások és szolgáltatások mindig használata a [list_node_agent_skus] [ py_list_skus] (Python) és [ListNodeAgentSkus] [ net_list_skus] megállapítja, és válassza ki a jelenleg elérhető termékváltozatok (köteg .NET).

> [AZURE.WARNING] Az alábbi lista bármikor változhat. Mindig a módszerekkel **lista csomópont ügynök Termékváltozat** a köteg API-hoz elérhető listára, és válassza ki a kompatibilis virtuális gép és csomópont ügynök termékváltozatok a köteg feladatok futtatásakor.

| **A Publisher** | **Ajánlat** | **Kép Termékváltozat** | **Verzió** | **Csomópont ügynök Termékváltozat azonosító** |
| ------- | ------- | ------- | ------- | ------- |
| Kanonikus | UbuntuServer | 14.04.0-LTS | legújabb | Batch.node.ubuntu 14.04 |
| Kanonikus | UbuntuServer | 14.04.1-LTS | legújabb | Batch.node.ubuntu 14.04 |
| Kanonikus | UbuntuServer | 14.04.2-LTS | legújabb | Batch.node.ubuntu 14.04 |
| Kanonikus | UbuntuServer | 14.04.3-LTS | legújabb | Batch.node.ubuntu 14.04 |
| Kanonikus | UbuntuServer | 14.04.4-LTS | legújabb | Batch.node.ubuntu 14.04 |
| Kanonikus | UbuntuServer | 14.04.5-LTS | legújabb | Batch.node.ubuntu 14.04 |
| Kanonikus | UbuntuServer | 16.04.0-LTS | legújabb | Batch.node.ubuntu 16.04 |
| Credativ | Debian | 8 | legújabb | 8 Batch.node.debian |
| OpenLogic | CentOS | 7.0-s | legújabb | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.1. | legújabb | Batch.node.centos 7 |
| OpenLogic | A HPC centOS | 7.1. | legújabb | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.2. | legújabb | Batch.node.centos 7 |
| Az Oracle | Az Oracle-Linux | 7.0-s | legújabb | Batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | legújabb | Batch.node.opensuse 13.2 |
| SUSE | openSUSE-termékek | 42.1 | legújabb | Batch.node.opensuse 42.1 |
| SUSE | A HPC SLES | 12 | legújabb | Batch.node.opensuse 42.1 |
| SUSE | SLES | 12-SP1 | legújabb | Batch.node.opensuse 42.1 |
| a Microsoft-reklámok | normál-adatok-tudományos-virtuális | normál-adatok-tudományos-virtuális | legújabb | Batch.node.Windows amd64 |
| a Microsoft-hirdetések | Linux-adatok-tudományos-virtuális | linuxdsvm | legújabb | Batch.node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 CSOMAG | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-adatközponthoz | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-adatközponthoz | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows-kiszolgáló-technikai előzetes | legújabb | Batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Linux csomópontok összekapcsolása

Fejlesztés alatt vagy hibaelhárítás során, akkor azt tapasztalhatja azt szükségesek, hogy jelentkezzen be a készlet csomópontját. Windows számítási csomópontok eltérően Linux csomópontok összekapcsolása nem használható távoli asztali Protocol (RDP). Ehelyett a köteg szolgáltatás lehetővé teszi, hogy minden csomóponton távkapcsolat SSH hozzáférést.

A következő Python kódrészletet felhasználót hoz létre a minden egyes készletben, amelyre szükség van a távkapcsolat csomópontra. Kattintson az egyes csomópontok biztonságos rendszerhéj (SSH) kapcsolati adatait nyomtatja ki.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Az alábbiakban a korábbi kód, amely tartalmazza a négy Linux csomópontok készlet minta kimenet:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Figyelje meg, hogy helyett egy jelszót, megadhatja egy SSH nyilvános kulcs csomópont felhasználó létrehozásakor. A Python SDK ezt úgy teheti a **ssh_public_key** paraméter használata a [ComputeNodeUser][py_computenodeuser]. A .NET, ebben az esetben a [ComputeNodeUser]használatával[net_computenodeuser]. [SshPublicKey] [net_ssh_key] tulajdonság.

## <a name="pricing"></a>Árak

Azure köteg Azure Cloud Services és Azure virtuális gépeken futó technológia épül. A köteg szolgáltatás magát kínálják áttérhet, ami azt jelenti, hogy az előfizetést terhelő csak a számítási erőforrásokat, hogy a köteg megoldások felhasználni. Ha úgy dönt, hogy a **Cloud Services konfigurálása**, akkor megterheljük [Cloud Services árak] alapján[ cloud_services_pricing] struktúra. Ha úgy dönt, hogy **Virtuális gép konfigurációs**, akkor megterheljük [virtuális gépeken futó árak] alapján[ vm_pricing] struktúra.

## <a name="next-steps"></a>Következő lépések

### <a name="batch-python-tutorial"></a>Köteg Python oktatóprogram

Egy részletesebb oktatóprogramért kezelése köteg Python használatával tanulmányozza [az Azure köteg Python ügyfél – első lépések](batch-python-tutorial.md). A kereső [kód minta] [ github_samples_pyclient] egy segítő függvényt tartalmaz `get_vm_config_for_distro`, egy másik módszerrel nemcsak a konfigurációt virtuális gép szerezni, amely jeleníti meg.

### <a name="batch-python-code-samples"></a>Köteg Python mintakódok

Nézze meg a többi [Python kód minták] [ github_samples_py] [a minták köteg azure] [ github_samples] GitHub, amely megmutatja, hogyan hajtanak végre közös köteg például készlet, a feladat és a tevékenység létrehozása több parancsfájlok tárházából. A [fontos] [ github_py_readme] , amely olvashatja a Python minták rendelkezik a szükséges csomagok telepítésével kapcsolatos részleteket.

### <a name="batch-forum"></a>Köteg-fórum

Az [Azure köteg fórum] [ forum] MSDN webhelyen található kiváló hely az megvitatása a köteget, és a szolgáltatással kapcsolatos kérdéseket. Hasznos "stickied" bejegyzéseket, és kérdések feltevése a kérdések, amíg a köteg megoldások generál merülnek fel.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
