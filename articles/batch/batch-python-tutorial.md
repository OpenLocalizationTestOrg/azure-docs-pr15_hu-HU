<properties
    pageTitle="Oktatóanyag – első lépések az Azure köteg Python ügyfél |} Microsoft Azure"
    description="További tudnivalók az Azure köteget, és a köteg szolgáltatást, és egy egyszerű forgatókönyv fejlesztésével alapfogalmak"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Az Azure köteg Python ügyfél – első lépések

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

[Azure] köteg alapjai[ azure_batch] és a [Köteg Python] [ py_azure_sdk] ügyfél, témákat ölelik fel egy kis köteg alkalmazás Python nyelven íródott. Megnézi az hogyan két minta parancsfájlok használatát a köteg szolgáltatás a felhőben, és azok együttműködését [Azure](./../storage/storage-introduction.md) adathordozós fájl előkészítése és lekérés Linux virtuális gépeken párhuzamos terhelési feldolgozása. Fogja közös köteg alkalmazás munkafolyamat információt és nyereség egy köteg fő összetevői például feladatok, feladatok, készletek alap megértését és csomópontok számítja ki.

![A köteg megoldás munkafolyamat (basic)][11]<br/>

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk azt feltételezi, hogy rendelkezik-e egy Python és Linux ismerős munka ismerete. Is feltételezi, hogy, hogy meg tudja megfelelnek a fiók létrehozását megadott alatti Azure és a köteg- és szolgáltatások.

### <a name="accounts"></a>Fiókok

- **Azure-fiók**: Ha még nem rendelkezik az Azure előfizetéssel, [ingyenes Azure-fiók létrehozása][azure_free_account].
- **Köteg fiók**: Miután Azure szóló előfizetés, [Hozzon létre egy köteg Azure-fiókot](batch-account-create-portal.md).
- **Tárterület-fiók**: [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) az [Azure kapcsolatos tároló fiókok](../storage/storage-create-storage-account.md)című témakör tartalmaz.

### <a name="code-sample"></a>Példa a kódot.

A Python oktatóanyag [kód minta] [ github_article_samples] a köteg sok mintakódok egyik megtalálható [a minták köteg azure] [ github_samples] GitHub tárházából. Gombra kattintva letöltheti az összes minta **Adatfeliratsor vagy letöltés > letöltése ZIP-** tárházba a Kezdőlap lapon, vagy az [azure-köteg-minták-master.zip] kattintva[ github_samples_zip] közvetlen letöltési hivatkozásra. Miután kicsomagolta, már a ZIP-fájl tartalmának, ebben az oktatóanyagban a két parancsfájlok találhatók a `article_samples` címtár:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python környezet

Futtassa a *python_tutorial_client.py* mintaparancsfájl a helyi számítógépen, szüksége van egy **Python értelmező** kompatibilis verziója **2.7** vagy **3,3 +**. A parancsprogram Linux és a Windows teszteléssel.

### <a name="cryptography-dependencies"></a>titkosítási függőségek

Telepíteni kell a függőségeket a [titkosítási] vonatkozó[ crypto] követel meg a könyvtár a `azure-batch` és `azure-storage` Python csomagok. Végezze el a platform megfelelő az alábbi műveletek egyikét, vagy olvassa el a [titkosítási telepítési] [ crypto_install] további információt a részletek:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* A Windows

    `pip install cryptography`

>[AZURE.NOTE] Ha Python 3.3 + Linux telepítésével, a python3 egyenértékű funkcióiról használható a Python függőségeket. Például a Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure csomagok

Ezután telepítse az **Azure köteg** és **Azure tároló** Python csomagok. Ezt megteheti a **mezőpont** és az itt található *requirements.txt* :

`/azure-batch-samples/Python/Batch/requirements.txt`

A probléma a következő **mezőpont** parancs, amellyel a köteget, és a tárhely csomagok telepítése:

`pip install -r requirements.txt`

Vagy telepítheti az [azure-köteg] [ pypi_batch] és [azure-tároló] [ pypi_storage] Python manuálisan csomagok:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] A parancsok az előtag szükséges `sudo` jogosultságok fiók használatakor. Ha például `sudo pip install -r requirements.txt`. Python csomagok telepítésével kapcsolatos további tudnivalókért lásd: a [Csomagok telepítése] [ pypi_install] a readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Köteg Python oktatóanyag kód minta

A köteg Python oktatóanyag kód minta két Python parancsfájl és néhány adatfájlok áll.

- **python_tutorial_client.PY**: hogyan kommunikáljon a köteget, és a tárhely szolgáltatások végrehajtani a párhuzamos terhelési a számítási csomópontok (virtuális gépeken futó). A *python_tutorial_client.py* parancsfájl a helyi számítógépen fut.

- **python_tutorial_task.PY**: A parancsfájlt, amelyet futtat a tényleges munka elvégzéséhez Azure található csomópontok számítja ki. A minta *python_tutorial_task.py* elemzi a szöveg egy (a bemeneti fájl) Azure-tárból letöltött fájlt. Kattintson a felső, amelyek akkor jelennek meg a bemeneti fájl három szavak listáját tartalmazó szövegfájl (a kimeneti fájl) elő. Után a kimeneti fájl létrehozott, *python_tutorial_task.py* Azure tárolóhoz feltölti a fájlt. Így letölthető az ügyfél parancsfájl, a számítógépen futó. A *python_tutorial_task.py* parancsfájl a köteg szolgáltatásban több számítási csomóponton párhuzamosan fut.

- **./data/taskdata\*.txt**: a szükséges a bemeneti következő három szövegfájlokból futtassa a számítási csomóponton feladatokat.

Az alábbi ábra mutatja be, az elsődleges, az ügyfél és a tevékenység parancsfájlok végrehajtott műveletek. Egyszerű munkafolyamathoz sok számítási megoldások köteg eszközzel létrehozott jellemző. Amíg meg nem bemutatják, minden szolgáltatás érhető el a köteget szolgáltatásban, szinte minden köteg forgatókönyv részeire ezt a munkafolyamatot.

![Példa a munkafolyamat köteg][8]<br/>

[**Lépés: 1.**](#step-1-create-storage-containers) **Tárolók** létrehozása az Azure Blob-tárolóhoz.<br/>
[**Lépés: 2.**](#step-2-upload-task-script-and-data-files) Tevékenység parancsfájl, és a bemeneti fájlok feltöltése a tárolók.<br/>
[**3 a lépést.**](#step-3-create-batch-pool) A köteg **készlet**létrehozása.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** A készlet **StartTask** letölti a tevékenység parancsfájl (python_tutorial_task.py) csomópontot, azok a csatlakozás a készlet.<br/>
[**Lépés: 4.**](#step-4-create-batch-job) Hozzon létre egy köteg **feladatot**.<br/>
[**5 a lépést.**](#step-5-add-tasks-to-job) **Tevékenységek** hozzáadása a projekthez.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** A feladatokat végrehajtani a csomópontok van ütemezve.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Minden tevékenység letölti a bemeneti adatok Azure-tárhelyről, majd a végrehajtás megkezdése.<br/>
[**6 a lépést.**](#step-6-monitor-tasks) Lync-feladatokat.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Feladatok elvégzettként azok Azure-tárolóhoz feltöltése kimeneti adataikat.<br/>
[**Lépés 7.**](#step-7-download-task-output) Töltse le a tevékenység kimeneti tároló.

Említett, nem minden köteg megoldás pontos lépések végrehajtása, illetve lehetnek a számos további, de ez példa bemutatja a köteg megoldást található általános folyamat.

## <a name="prepare-client-script"></a>Felkészülés az ügyféloldali parancsfájl

A minta, futtatása előtt *python_tutorial_client.py*adja hozzá a köteget, és a tárhely fiók hitelesítő adatait. Ha még nem tette meg, nyissa meg a fájlt a kedvenc szövegszerkesztőben, és frissítse a következő sort a hitelesítő adatait.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

A köteg- és fiók hitelesítő adatait belül az egyes szolgáltatásokhoz a fiók lap a az [Azure portálon]található[azure_portal]:

![A portál hitelesítő adatok köteg][9]
![tároló hitelesítő adatokat a portálon][10]<br/>

Az alábbi szakaszok azt elemzése a feldolgozása a terhelést a köteg szolgáltatásban a parancsfájlok által használt lépéseket. Javasoljuk, hogy a hivatkozott rendszeresen-szerkesztővel és parancsfájlok a cikk a többi alkalmazással munka közben.

Nyissa meg a következő sort a **python_tutorial_client.py** lépés 1 indítása:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Lépés: 1: A tároló létrehozása

![Tárolók létrehozása az Azure-tárolóhoz][1]
<br/>

Köteg Azure tároló személlyel beépített támogatja. Tárolók a tárterület-fiókjában nyújt a fájlokat, szükség szerint a köteg fiókjában futó feladatokat. A tárolók is biztosít, amelyek a feladatok eredményeznek kimeneti adatai tárolhatja. A legfontosabb dolog a *python_tutorial_client.py* parancsfájl működését a következő három tárolók létrehozása az [Azure Blob-tárolóhoz](../storage/storage-introduction.md#blob-storage):

- **alkalmazás**: Ez a tároló tárolja a Python parancsfájl, a feladatok, *python_tutorial_task.py*futtatása.
- **beviteli**: tevékenységek letölti a *bemeneti* tárolóból feldolgozása adatfájlok.
- **Kimenet**: tevékenységek beviteli feldolgozása elvégezni, ha azok feltöltődnek az eredmények a *kimeneti* tároló.

A tárterület-fiók használata és létrehozása a tárolók, az [azure-tárhely] használatának[ pypi_storage] csomag létrehozása egy [BlockBlobService] [ py_blockblobservice] objektum – "blob-ügyfél,." Azt majd hozzon létre három tárolók a blob ügyfélprogramban tárterület-fiókjában.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

A tárolók létrehozása után az alkalmazás letöltése feltöltheti a fájlokat, a feladatok által használt.

> [AZURE.TIP] [Azure blobtárolóhoz Python való használatáról](../storage/storage-python-how-to-use-blob-storage.md) a Azure tároló és BLOB áttekintése biztosít. Kell a olvasási lista tetején köteg együtt kezd dolgozni.

## <a name="step-2-upload-task-script-and-data-files"></a>Lépés: 2: Feladat parancsfájl és az adatok a fájlok feltöltése

![Tárolók tevékenység alkalmazás és a beviteli (adatok) fájlok feltöltése][2]
<br/>

A fájlművelet feltöltendő *python_tutorial_client.py* először határozza meg az **alkalmazás** és a **bemeneti** fájl elérési útja gyűjtemények léteznek a helyi számítógépen. Majd a tárolók az előző lépésben létrehozott feltölti meg ezeket a fájlokat.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Lista megértési, használja a `upload_file_to_container` függvény neve a webhelycsoportok és két [ResourceFile] fájlonként[ py_resource_file] gyűjtemények vannak töltve. A `upload_file_to_container` függvény alatt jelenik meg:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

Egy [ResourceFile] [ py_resource_file] tartalmaz a feladatok a köteg letölti-e egy számítási csomópontot, hogy a feladat futtatása előtt Azure-tárolóban lévő fájl URL-címét. A [ResourceFile][py_resource_file]. **blob_source** tulajdonság adja meg a fájlt a teljes URL-CÍMÉT, Azure-tárolóban lévő található. Az URL-címet is, ahol a fájl biztonságos hozzáférés átengedése aláírás (Társítások). A legtöbb tevékenységtípusok kötegben tartalmazzák *ResourceFiles* tulajdonságait, például:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

Ez a példa nem használja az JobPreparationTask vagy JobReleaseTask tevékenységtípusok, de olvashat róluk, a [Futtatás előkészítése és a Befejezés projektfeladatok Azure köteg csomópontok számítja ki](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Megosztott access aláírás (Társítások)

Megosztott access aláírások olyan karakterláncok, amelyek a tárolók és Azure-tárolóban lévő BLOB biztonságos hozzáférés nyújtása. A *python_tutorial_client.py* parancsfájl használja a két blob és tároló megosztott access aláírások, és bemutatja, hogyan ezek átengedése aláírás karakterláncok beszerzése a tárhely szolgáltatásból.

- **A megosztott access aláírások BLOB**: A készlet StartTask használ, amikor a tevékenység parancsfájl, és a bemeneti adatok fájlokat tárhelyről letölti blob-átengedése aláírások (lásd az alábbi [lépést #3](#step-3-create-batch-pool) ). A `upload_file_to_container` *python_tutorial_client.py* függvény, amely beolvassa az egyes blob átengedése aláírás kódot tartalmazza. Azt is így tesz, hívja fel a [BlockBlobService.make_blob_url] [ py_make_blob_url] a tárhely modulban.

- **A tároló átengedése aláírás**: egyes tevékenységek befejezte a munkát, a számítási csomópontra, ahogy azt feltölti a kimeneti fájl a *kimeneti* tároló Azure-tárolóban lévő. Ehhez a *python_tutorial_task.py* használja a tároló átengedése aláírás, amely a tároló írási hozzáférést biztosít. A `get_container_sas_token` *python_tutorial_client.py* függvény beolvassa a tároló átengedése aláíráshoz, amely ezután a feladatok parancssori argumentumként átadott. [Tevékenységek hozzáadása a projekthez](#step-5-add-tasks-to-job)#5, lépés a tároló Társítások a használatát ismerteti.

> [AZURE.TIP] Nézze meg a megosztott access aláírások, a két részből álló sorozat [rész 1: ismertetése a Társítások modell](../storage/storage-dotnet-shared-access-signature-part-1.md) és [rész 2: létrehozása és használata a Társítások a Blob szolgáltatással](../storage/storage-dotnet-shared-access-signature-part-2.md), ha többet szeretne tudni a tárterület-fiókjában adatok biztonságos hozzáférés biztosítása.

## <a name="step-3-create-batch-pool"></a>3 lépés: A köteg készlet létrehozása

![A köteg készlet létrehozása][3]
<br/>

A köteg **készlet** számítási csomópontok (virtuális gépeken futó), amelyen a köteg végrehajtja a egy projektfeladatok gyűjteménye.

Után a tevékenység parancsfájl és az adatok fájlokat a tárterület-fiókba feltöltések megjelenítése, *python_tutorial_client.py* indítja el a kapcsolati a köteg szolgáltatással a köteg Python modulról. Ehhez a [BatchServiceClient] [ py_batchserviceclient] jön létre:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Ezután a hívást, hogy a köteg fiók számítási csomópontok erőforráskészlethez tartozik jön létre `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Egy készlet létrehozásakor a [PoolAddParameter] definiálása[ py_pooladdparam] , amely Itt adhatja meg a készlet több tulajdonságának:

- **Azonosító** (*azonosító* – szükséges) készlet<p/>A legtöbb személyek a köteget, és az új készlet a köteg számla belüli egyedi Azonosítót kell rendelkeznie. A kód a készlet annak azonosítójával hivatkozik, és hogyan azonosítani a készletet az Azure- [portálon][azure_portal].

- **Számítási csomópontok számának** (*target_dedicated* - szükséges)<p/>Ez a tulajdonság adja meg, hány VMs telepítésének a készletben. Fontos, hogy ne feledje, hogy minden köteg fiók korlátozza a **magmintákat** (és ezért számítási csomópontok) egy köteg fiókban alapértelmezett **Kvóta** . Az alapértelmezett kvótáinak és útmutatást a [kvóta növelése](batch-quota-limit.md#increase-a-quota) (például a köteg fiókjában magmintákat maximális száma) a [kvótáinak és -korlátozások az Azure köteg szolgáltatás](batch-quota-limit.md)található. Ha már útvesztőjében kérő "Miért nem a készlet elérje a több mint csomópontok X?" a fő kvóta okozhatja.

- **Operációs rendszer** csomópontok (*virtual_machine_configuration* **vagy** *cloud_service_configuration* - szükséges)<p/>*Python_tutorial_client.py*, hogy egy [VirtualMachineConfiguration]használatával Linux csomópontok készlet létrehozása[py_vm_config]. A `select_latest_verified_vm_image_with_node_agent_sku` működik `common.helpers` egyszerűbbé teszi a [Microsoft Azure virtuális gépeken futó piactéren] használata[ vm_marketplace] képek. Lásd: [Azure köteg készletek található csomópontok kiszámítania rendelkezést Linux](batch-linux-nodes.md) piactér képek használatáról további információt.

- **Számítási csomópontok mérete** (*vm_size* - szükséges)<p/>Mivel Linux csomópontok megadjuk a saját [VirtualMachineConfiguration][py_vm_config], azt adja meg, hogy egy virtuális méretét (`STANDARD_A1` a mintában) [az Azure virtuális gépeken futó méretét](../virtual-machines/virtual-machines-linux-sizes.md)a. Ismét [rendelkezést Linux kiszámítania Azure köteg készletek található csomópontok](batch-linux-nodes.md) további információt talál.

- **Indítsa el a tevékenység** (*start_task* – nem kötelező)<p/>Együtt a fenti fizikai csomópont tulajdonságait, is megadhat egy [StartTask] [ py_starttask] a készlet (Ez még nem kötelező). A StartTask minden csomóponton végrehajtja a csomópontra a készlet csatlakozik, és minden alkalommal, amikor újraindítja csomópontot. A StartTask esetén különösen hasznos számítási csomópontok előkészítése feladatok, például a tevékenységek futó alkalmazások telepítése végrehajtását.<p/>A minta alkalmazásban a StartTask másolja a letöltés tárhelyről (amely a StartTask **resource_files** tulajdonság segítségével vannak megadva) *Munkakönyvtár* StartTask azt a *megosztott* könyvtárat a csomóponton futó feladatok elérhetik a fájlokat. Alapvetően, a program másolja `python_tutorial_task.py` a megosztott címtárhoz minden csomóponton, a csomópont csatlakozik a készlet, hogy a csomóponton futó egyetlen tevékenységet sem érhető el.

Észreveheti, hogy a hívást a `wrap_commands_in_shell` helper függvény. Ez a funkció külön parancsok gyűjteménye tart, és létrehoz egy tevékenység parancssori tulajdonság megfelelő parancs az egysoros.

A fenti kódtöredék lényeges is a két környezetben változók használata a StartTask tulajdonságlapján a **parancssor** : `AZ_BATCH_TASK_WORKING_DIR` és `AZ_BATCH_NODE_SHARED_DIR`. A köteg készletébe belül minden számítási csomópont automatikusan köteg jellemző környezet többváltozós van beállítva. Tevékenység végrehajtott folyamatának ezek a környezeti változók hozzáférése van.

> [AZURE.TIP] További tudnivalók a számítási csomópontok egy köteg készlet, valamint a tevékenység munka könyvtárak olvashat a rendelkezésre álló környezeti változók című szakaszban talál **környezeti beállítások tevékenységek** és a **fájlok és mappák** az [Azure köteg funkciók áttekintése](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Lépés: 4: Köteg feladat létrehozása

![Köteg feladat létrehozása][4]<br/>

A köteg **feladat** feladatok gyűjteménye, és a számítási csomópontok erőforráskészlethez tartozik egy társított. A kapcsolódó készlet számítási csomópontok hajtsa végre a projekt feladatait.

A feladat használható nem csak a rendszerezése és a kapcsolódó munkaterhelésekből, hanem olyan bizonyos korlátozások – például a maximális futtatókörnyezet a feladat (és kiterjesztés, feladatainak), a feladatok nyomon követése és más feladatok köteg fiók viszonyított prioritás feladat. Ebben a példában azonban a feladat társítva csak a készlet #3 lépésben létrehozott. Nincsenek további tulajdonság be van állítva.

Az összes köteg feladatok társítva adott erőforráskészlethez tartozik. A társítási azt jelzi, hogy mely csomópontok, hajtsa végre a projektfeladatokat. A [PoolInformation] használatával adja meg a készlet[ py_poolinfo] tulajdonság, az alábbi kódtöredéket látható módon.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Most, hogy a feladat már elkészült, a feladatok megjelennek a vonhatnak.

## <a name="step-5-add-tasks-to-job"></a>Lépés 5: Tevékenységek hozzáadása a projekthez

![Tevékenységek hozzáadása a projekthez][5]<br/>
*(1) feladatok bekerülnek a feladatot, (2) a tevékenységek mikorra van ütemezve csomóponton és (3) a feladatokat, töltse le az adatfájlok feldolgozása*

**Feladatok** köteg az egyes mennyiségű munka, hajtsa végre a számítási csomópontok. A feladatot a parancssor és futtatja a parancsfájlok vagy adja meg, ha a parancssorban végrehajtható.

Ténylegesen vonhatnak, feladatok hozzá kell adnia egy feladatot. Minden egyes [CloudTask] [ py_task] van beállítva egy parancssori tulajdonság és [ResourceFiles] [ py_resource_file] (mint az erőforráskészlet StartTask), hogy a tevékenység letölti a csomópontra a parancssor automatikusan végrehajtása előtt. A minta az egyes tevékenységek végrehajtja a csak egy fájlt. Így ResourceFiles levétel a egyetlen elemet tartalmaz.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Amikor környezeti változók hozzáférésük, például: `$AZ_BATCH_NODE_SHARED_DIR` vagy egy alkalmazás nem található a csomópont végrehajtása `PATH`, parancssorokat feladatot kell hívnia a rendszerhéj kifejezetten, mint például a `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Ez a követelmény általában szükségtelen, ha a feladatok végrehajtása egy alkalmazást a csomópontra a `PATH` és a környezeti változók nem hivatkozik.

Belül a `for` a fenti kódtöredék hurok, láthatja, hogy a parancssorból a tevékenységhez tartozó összeállítás *python_tutorial_task.py*átadott öt parancssori argumentumok:

1. **fájl elérési útja**: Ez az a helyi fájl elérési útját a csomóponton található. Ha a ResourceFile objektum `upload_file_to_container` jött létre a fenti lépésben 2, a fájl nevét a tulajdonság használt (a `file_path` ResourceFile konstruktorához paraméter). Ez azt jelzi, hogy megtalálható-e a fájlt az azonos könyvtárban *python_tutorial_task.py*a csomópontra.

2. **NUMWORDS**: A legfelső *N* szavakat a kimeneti fájl kell írni.

3. **storageaccount**: a tárterület-fiókot, amely a tároló, amelyhez a tevékenység kimenet fel kell tölteni a tulajdonosa nevét.

4. **storagecontainer**: a tárterület-tároló, amelyhez a kimeneti fájl fel kell tölteni a nevét.

5. **sastoken**: A megosztott access aláírás (Társítások), amely a **kimeneti** tároló Azure-tárolóban lévő írási hozzáférést biztosít. A *python_tutorial_task.py* parancsfájl használja a megosztott access aláírás amikor BlockBlobService hivatkozást hoz létre. Ez biztosít a tároló írási hozzáféréssel hívóbetű a tárterület-fiók nélkül.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Lépés a 6: Monitor feladatok

![Képernyő-feladatok][6]<br/>
*A parancsprogram (1) figyeli készenléti állapotát feladatot, és (2) a tevékenységek eredményül kapott adatokat feltöltheti Azure-tárolóhoz*

Tevékenységek felvétele után egy projektbe, ezeket automatikusan aszinkron és ütemeznie meg a készlet a feladathoz kapcsolódó számítási található csomópontok. A megadott beállítások alapján, köteg kezeli az összes tevékenység queuing, ütemezés, újra próbálkozik és más tevékenység felügyeleti feladatokat meg.

Vannak olyan sok megközelítés feladat-végrehajtás figyelése. A `wait_for_tasks_to_complete` *python_tutorial_client.py* függvényt tartalmaz egy egyszerű példa felügyeleti feladatok egy bizonyos állapot, ebben az esetben a [kész] [ py_taskstate] állapota.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>7 lépés: A tevékenység kimeneti letöltése

![Töltse le a tevékenység kimeneti tárhely][7]<br/>

Most, hogy a feladat befejezése után, a feladatok kimenetét letölthető Azure-tárhelyről. Ez a lépés hívást kezdeményez `download_blobs_from_container` a *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] A hívás `download_blobs_from_container` *python_tutorial_client.py* Megadja, hogy a fájlok kell letölt az otthoni címtár. Nyugodtan módosítása a kimenet helyét.

## <a name="step-8-delete-containers"></a>Lépés 8: Törlés tárolók

Az-előfizetést terhelő, amely az Azure-tárolóban lévő adatok, mert azt mindig jó ötlet bármely BLOB, amely már nincs szükség a köteg feladatokhoz eltávolításához. A *python_tutorial_client.py*, ez a lépés [BlockBlobService.delete_container]három hívásainak[py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Lépés a 9: A feladatot, és a készlet törlése

Az utolsó lépésben kéri a feladatot, és a *python_tutorial_client.py* parancsfájl által készített a készlet törléséhez. Bár nem az előfizetést terhelő feladatok és a *rendszer* saját maguk, feladatok számítási csomópontok az előfizetést. Emiatt azt javasoljuk, csomópontok kiosztani csak igény szerint. Használaton kívüli készletek törlése, lehet, hogy a karbantartás folyamat részeként.

A BatchServiceClient [JobOperations] [ py_job] és [PoolOperations] [ py_pool] egyaránt rendelkezik a megfelelő törlési módszerről nevezzük, ha a törlés megerősítéséhez:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Ne feledje, hogy az előfizetést terhelő számítási erőforrások – még nem használt készletek törlése összezárása költség. Is tartsa szem előtt, hogy egy készlet törlésekor az összes számítási található csomópontok készlethez, és, hogy a csomópontok adatokat fog nem lehet visszaállítani a készlet törlése után.

## <a name="run-the-sample-script"></a>Futtassa a mintaparancsfájl

Az oktatóprogram [kód minta] *python_tutorial_client.py* parancsfájl futtatásakor[github_article_samples], a konzol kimenet szövege a következőhöz hasonló. A szünet van `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` miközben a készlet számítási csomópontok jönnek létre elindul, és a készlet kezdő tevékenység parancsok végrehajtása. Az [Azure portál] használata[ azure_portal] a készlet követésére kiszámítania csomópontok, a feladatot, és a feladatok alatt és végrehajtása után. Az [Azure portál] használata[ azure_portal] vagy a [Microsoft Azure tároló Explorer] [ storage_explorer] a tárhely erőforrások (tárolók és BLOB) az alkalmazás által létrehozott megtekintéséhez.

>[AZURE.TIP] Futtassa a *python_tutorial_client.py* magából a `azure-batch-samples/Python/Batch/article_samples` címtár. Relatív elérési út a használja az `common.helpers` modul importálását, így előfordulhat, hogy `ImportError: No module named 'common'` Ha nem az a parancsfájl a címtáron belül.

Tipikus végrehajtás ideje **körülbelül 5-7 perc** az alapértelmezett beállítás a minta futtatásakor.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Következő lépések

Nyugodtan *python_tutorial_client.py* és *python_tutorial_task.py* kísérletezés a különböző számítási esetek. Ha például felveszi egy adatvégrehajtás késleltetés *python_tutorial_task.py* szimulálhatja hosszabb ideig futó feladatokat, és figyelje őket a portálon. Próbálja ki további tevékenységek felvétele és a számítási csomópontok számának módosítása. Adja hozzá a logika keresésének, és egy meglévő készlet sebesség végrehajtási idő használhatók.

Most, hogy a köteg megoldások alapvető munkafolyamat jártas, pedig alaposabban meg a további szolgáltatások a köteg szolgáltatás.

- Tekintse át az [Azure-áttekintés köteg szolgáltatások](batch-api-basics.md) cikk azt javasoljuk, hogy a szolgáltatás új.
- Indítsa el a további köteg fejlesztési cikkek **fejlesztési meg szeretné vizsgálni** a [Köteg tanulási javaslat]alatt a[batch_learning_path].
- Olvassa el a köteget, az a [TopNWords] "legnépszerűbb vagy legnagyobb N szavak" terhelését feldolgozása különböző végrehajtásának[ github_topnwords] minta.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Tárolók létrehozása az Azure-tárolóhoz"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Tárolók tevékenység alkalmazás és a beviteli (adatok) fájlok feltöltése"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Köteg készlet létrehozása"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Köteg feladat létrehozása"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Tevékenységek hozzáadása a projekthez"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Képernyő-feladatok"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Töltse le a tevékenység kimeneti tárhely"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "A köteg megoldás munkafolyamat (teljes diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "A portál a köteg hitelesítő adatok"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Tárterület hitelesítő adatok portálon"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "A köteg megoldás munkafolyamat (minimális diagram)"
