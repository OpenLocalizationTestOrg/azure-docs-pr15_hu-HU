<properties
    pageTitle="A Szolgáltatáskezelés API (Python) – útmutató szolgáltatás használata"
    description="Megtudhatja, hogy miként programozás útján elvégezhető leggyakoribb szolgáltatás felügyeleti Python."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>A Python Szolgáltatáskezelés használata

> [AZURE.NOTE] Szolgáltatás felügyeleti API a API-val új erőforrás kezelése, jelenleg elérhető az előnézeti kiadásokban cserél.  További információt [az erőforrás-kezelés Azure dokumentáció](http://azure-sdk-for-python.readthedocs.org/) használatáról olvashat bővebben az új erőforrás Management API a Python.

Ez az útmutató megtudhatja, hogy hogyan programozás útján elvégezhető leggyakoribb szolgáltatás felügyeleti Python. Az [Azure SDK Python](https://github.com/Azure/azure-sdk-for-python) **ServiceManagementService** osztály támogatja a nagy része a szolgáltatás kezelésével kapcsolatos funkciók az [Azure klasszikus portál] elérhető való hozzáférés[ management-portal] (például **létrehozása, módosítása, és törlése a felhőszolgáltatások, a telepítések, a kezelés adatszolgáltatások, és a virtuális gépeken futó**). Ez a funkció akkor lehet hasznos, az alkalmazások Szolgáltatáskezelés programozott hozzáféréssel kell rendelkeznie.

## <a name="WhatIs"> </a>Szolgáltatás szolgáltatás
A szolgáltatás felügyeleti API nagy, az [Azure klasszikus portál]elérhető Szolgáltatáskezelés funkciónak programozott hozzáférést biztosít[management-portal]. Az Azure SDK Python lehetővé teszi a felhőszolgáltatások és a tárterület-fiók kezelése.

A szolgáltatás felügyeleti API használatához [az Azure-fiók létrehozásához](https://azure.microsoft.com/pricing/free-trial/)szükséges.

## <a name="Concepts"> </a>Fogalmak
Az Azure SDK Python fussa az [Azure szolgáltatás felügyeleti API][svc-mgmt-rest-api], amely olyan, a REST API-t. Az összes API-műveletek SSL végrehajtásának sorrendjét, és egymást kölcsönösen hitelesíteni a X.509 v3 tanúsítványok használatával. Az alkalmazáskezelési szolgáltatás Azure-ban, vagy közvetlenül az interneten futó HTTPS kérelmet küldhet és fogadhat a HTTPS választ alkalmazástól szolgáltatás belül is érhetők el.

## <a name="Installation"> </a>Telepítése

Az összes, a jelen cikkben ismertetett funkciók érhetők el a a `azure-servicemanagement-legacy` csomag, amelyek mezőpont telepíthetők. A (például ha még kezdő az Python), a telepítéssel kapcsolatos további információra kíváncsi, olvassa el ez a cikk: [Python telepítése és az Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Hogyan: Szolgáltatáskezelés csatlakoztatása
Csatlakozás az Szolgáltatáskezelés végpontot, szüksége van az Azure előfizetés azonosítója és érvényes kezelési tanúsítvány. Az előfizetés azonosítója keresztül az [Azure klasszikus portál]szerezhet[management-portal].

> [AZURE.NOTE] Azure SDK Python v0.8.0, óta ajánlatos most OpenSSL készült, ha a Windows rendszeren futó tanúsítványok.  Python 2.7.4 igényel vagy újabb verziójában. Azt javasoljuk, hogy a felhasználók számára OpenSSL helyett .pfx, tanúsítványok valószínűleg eltávolítja a jövőben .pfx támogatása óta.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Tanúsítványok kezelése a Windows és Mac vagy Linux (OpenSSL)
A kezelés tanúsítvány létrehozása [OpenSSL](http://www.openssl.org/) is használhatja.  Biztosan szükséges a kiszolgáló egy két tanúsítvány létrehozása (egy `.cer` fájl), és egy, az ügyfél (egy `.pem` fájl). Hozhat létre a `.pem` fájlt, hajtsa végre:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Hozhat létre a `.cer` tanúsítvány, hajtsa végre:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Azure tanúsítványokkal kapcsolatos további tudnivalókért lásd: [Azure Cloud Services áttekintés Certificates](./cloud-services-certs-create.md). OpenSSL paraméterek teljes körű leírását az dokumentációjában [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)címen.

Létrehozott ezeket a fájlokat, miután szeretne feltölteni a `.cer` a "Feltöltés" művelet "Beállítások" lapján az [Azure klasszikus portálon]keresztül Azure fájl[management-portal], és jegyezze fel a mentési kell a `.pem` fájl.

Miután beszerzett az előfizetés azonosítója, a tanúsítvány létre, és a feltöltés a `.cer` fájl az Azure-csatlakozhat az Azure management végpontot megkerülhetők, az előfizetés azonosítója és elérési útját a `.pem` **ServiceManagementService**fájlt:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Az előző példában `sms` **ServiceManagementService** objektum. A **ServiceManagementService** osztály eredetű az elsődleges Azure szolgáltatások kezelésére használható.

### <a name="management-certificates-on-windows-makecert"></a>(MakeCert) a Windows Management tanúsítványok

A számítógép használatával hozhat létre felügyeleti önaláírt tanúsítvány `makecert.exe`.  Nyissa meg a **Visual Studio parancssor** , hogy a **rendszergazda** , és a *AzureCertificate* cseréli a tanúsítvány nevét, akinek szeretne használni, a következő parancsot használja.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

A parancs hoz létre a `.cer` fájlt, és a **személyes** tanúsítvány-tárolóban a telepítést. További információ a [Tanúsítványok Azure Cloud Services szolgáltatás áttekintése](./cloud-services-certs-create.md)című cikkben.

Miután létrehozta a tanúsítvány, szeretne feltölteni a `.cer` a "Feltöltés" művelet "Beállítások" lapján az [Azure klasszikus portálon]keresztül Azure fájl[management-portal].

Miután beszerzett az előfizetés azonosítója, a tanúsítvány létre, és a feltöltés a `.cer` fájl az Azure-csatlakozhat az Azure management végpontot megkerülhetők, az előfizetés azonosítóját, illetve a tanúsítvány helyét a **személyes** tanúsítvány-tárolóban való **ServiceManagementService** (ismét cseréli *AzureCertificate* a tanúsítvány neve):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Az előző példában `sms` **ServiceManagementService** objektum. A **ServiceManagementService** osztály eredetű az elsődleges Azure szolgáltatások kezelésére használható.

## <a name="ListAvailableLocations"> </a>Hogyan: elérhető helyek lista

A szolgáltatások elhelyezésére elérhető helyek lista, használja a **lista\_helyek** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Amikor létrehoz egy felhőalapú szolgáltatásba vagy tárhelyszolgáltatáshoz meg kell adnia egy érvényes helyet. A **lista\_helyek** módszer mindig naprakész a jelenleg elérhető helyek listáját adja eredményül. Az írás, kezdve az elérhető helyek a következők:

- Nyugati Európa
- Észak-Európa
- Délkelet-ázsiai
- Kelet-ázsiai
- A központi Amerikai Egyesült Államok
- A központi Észak-amerikai
- A központi Dél-Amerikai Egyesült Államok
- Nyugati Amerikai Egyesült Államok
- Kelet-Amerikai Egyesült Államok
- Japán kelet
- Japán nyugati
- Brazília Dél
- Ausztrália keleti
- Ausztrália Könyvesbolt is.

## <a name="CreateCloudService"> </a>Hogyan: hozzon létre egy felhőalapú szolgáltatásba

Hozzon létre egy alkalmazást, és indítsa el az Azure-ban, ha a kód és a konfigurációs közös úgynevezett az Azure [felhőszolgáltatásba][] (más néven egy *szolgáltatást is* a korábbi Azure verziókban). A **létrehozása\_üzemeltetett\_szolgáltatás** módszer lehetővé teszi, hogy hozzon létre egy új szolgáltatott azáltal, hogy a szolgáltatott szolgáltatás nevét (Ez az Azure egyedinek kell lennie), címke (base64 automatikusan kódolt), leírását és helyét.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Megtekintheti az előfizetéshez tartozó szolgáltatott szolgáltatások listáját a **lista\_üzemeltetett\_szolgáltatások** módszer:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Ha szeretne egy adott szolgáltatott szolgáltatás adatainak, ehhez megkerülhetők, hogy a szolgáltatott szolgáltatás nevét a **első\_üzemeltetett\_szolgáltatás\_tulajdonságok** módszer:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Miután létrehozott egy felhőalapú szolgáltatásba, a kód telepítheti a szolgáltatással rendelkező a **létrehozása\_telepítési** módot.

## <a name="DeleteCloudService"> </a>Hogyan: törlése egy felhőalapú szolgáltatásba

Törölhet egy felhőalapú szolgáltatásba is megkerülhetők, a szolgáltatás neve, hogy a **törlése\_üzemeltetett\_szolgáltatás** módszer:

    sms.delete_hosted_service('myhostedservice')

Törölhet is egy szolgáltatást, mielőtt a szolgáltatást az összes telepítés először törölnie kell. (Lásd: [Útmutató: a telepítés törlése](#DeleteDeployment) további információt.)

## <a name="DeleteDeployment"> </a>Útmutató: a telepítés törlése

A telepítés törléséhez használja a **törlése\_telepítési** módot. A következő példa bemutatja a telepítés nevű törlése `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Hogyan: hozzon létre egy tárhelyszolgáltatáshoz

[Tárhelyszolgáltatáshoz](../storage/storage-create-storage-account.md) férhet hozzá Azure [BLOB](../storage/storage-python-how-to-use-blob-storage.md), [táblázatok](../storage/storage-python-how-to-use-table-storage.md)és [sorok](../storage/storage-python-how-to-use-queue-storage.md). Hozzon létre egy tárhelyszolgáltatáshoz, szüksége nevét a szolgáltatás (3 és 24 kisbetűk között és Azure belül egyedinek), leírás, címke (legfeljebb 100 karakter Base64 automatikusan kódolt) és helyét. A következő példa bemutatja, hogyan hozhat létre egy tárhelyszolgáltatáshoz hely megadásával.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Megjegyzés: az előző példában, amely állapotát a **létrehozása\_tárolás\_fiók** művelet tudja visszaszerezni által visszaadott eredménye megkerülhetők **létrehozása\_tárolás\_fiók** való a **első\_művelet\_állapot** módszert.  

Jeleníthet meg a tárterület-fiókok és tulajdonságaik együtt a **lista\_tároló\_fiókok** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Hogyan: egy tárhelyszolgáltatáshoz törlése

A tároló szolgáltatás nevét megkerülhetők egy tárhelyszolgáltatáshoz törölheti a **törlése\_tároló\_fiók** módszer. Egy tárhelyszolgáltatáshoz törlésekor a szolgáltatás (BLOB, táblázatok és sorok) tárolható összes adatra.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Útmutató: a lista elérhető operációs rendszerek

A lista az elérhető szolgáltatások elhelyezésére operációs rendszerek, használja a **lista\_működő\_rendszerek** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Másik lehetőségként használhatja a **lista\_működő\_rendszer\_családok** módszer, csoportok, az operációs rendszerek család:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Útmutató: az operációs rendszer kép létrehozása

Operációs rendszer képet a kép tárházba hozhatja létre a **hozzáadása\_os\_kép** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

A rendelkezésre álló operációs rendszer képek listában használja a **lista\_os\_képek** módot. Platform-képek és a felhasználó képek tartalmazza:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Útmutató: az operációs rendszer kép törlése

Egy felhasználó kép törléséhez használja a **törlése\_os\_kép** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Hogyan: virtuális gép létrehozása

A virtuális gép létrehozásához először kell egy [felhőalapú szolgáltatás](#CreateCloudService)hozzon létre.  Használatáról virtuális gép telepítési létrehoznia a **létrehozása\_virtuális\_gépi\_telepítési** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Útmutató: a virtuális gép törlése

Virtuális gép törléséhez, először törölje a telepítési használata a **törlése\_telepítési** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

A felhőalapú szolgáltatást használ majd törölhetők a **törlése\_üzemeltetett\_szolgáltatás** módszer:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Útmutató: A rögzített virtuális gép képen lévő virtuális gép létrehozása

A virtuális kép rögzítéséhez, először felhívja a **rögzítése\_virtuális\_kép** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Ezután győződjön meg arról, hogy sikeresen rögzítse a képet, használja a **lista\_virtuális\_képek** API-t, és győződjön meg arról, hogy a kép megjelenik az eredményeket:

    images = sms.list_vm_images()

Végül hozhat létre a virtuális gépet használ a rögzített képet az **létrehozása\_virtuális\_gépi\_telepítési** előtt, de ezúttal fázisban a vm_image_name inkább módszer

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

További arról, hogy miként Linux virtuális gép rögzítése, [hogyan rögzítéséhez Linux virtuális géphez.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Rögzítése a Windows virtuális gép kapcsolatos további tudnivalókért lásd: [hogyan rögzítése a Windows virtuális gépet.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Következő lépések

Most, hogy megtanulta Szolgáltatáskezelés alapjait, a [teljes API hivatkozás dokumentációjában találhat az Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) , és végezze el az összetett feladatokat egyszerűen a python alkalmazás kezeléséhez.

További tudnivalókért lásd: a [Python Developer Center](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[felhőalapú szolgáltatás]:/services/cloud-services/

