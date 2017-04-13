<properties
    pageTitle="Azure CLI parancsok Szolgáltatáskezelés módban |} Microsoft Azure"
    description="Azure parancs sor kezelőfelületről parancsok Szolgáltatáskezelés módban a klasszikus telepítési modell telepítések kezelése"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure CLI parancsok az Azure Szolgáltatáskezelés (asm) mód

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Műveleteket is elvégezheti, [További információ: az erőforrás-kezelő modell parancsokat](virtual-machines/azure-cli-arm-commands.md), és használja a CLI [erőforrások](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) áttelepítése az erőforrás-kezelő modellhez klasszikus.

Ebben a cikkben képletszintaxisát és Azure CLI parancsok kell létrehozni vagy kezelni a klasszikus telepítési modell Azure erőforrások használata általában lehetőségeit. Ezek a parancsok a CLI futtatásával Azure Szolgáltatáskezelés (asm) módban érhetők el. Ez nem teljes hivatkozást, és CLI verziójától némileg eltérő parancsok és a paraméterek jelenhet meg. 

Az első lépések, [telepítse az Azure CLI](xplat-cli-install.md) első, és [csatlakozzon az Azure-előfizetése](xplat-cli-connect.md).

Az aktuális parancs szintaxisának és a parancssorból beállítások, írja be a `azure help` vagy, ha meg kívánja jeleníteni egy adott parancs `azure help [command]`. Is megtalálhatók CLI példák dokumentációjában létrehozásához és kezeléséhez adott Azure szolgáltatások.

Szögletes zárójelek között látható választható paraméterek (például `[parameter]`). Szükség az összes paramétert.

Kívül parancs-specifikus választható paraméterek itt ismertetett vannak a három választható paraméterek, például kérelem beállításai és az állapot kódok részletes kimeneti megjelenítendő használható. A `-v` paraméter nyújt részletes kimenet, és a `-vv` paraméter részletesebb még részletes kimenet. A `--json` lehetőséget a kimenet az eredmény nyers json formátumban.

## <a name="setting-asm-mode"></a>Beállítás asm mód

A következő paranccsal engedélyezése Azure CLI Szolgáltatáskezelés mód parancsai.

    azure config mode asm

>[AZURE.NOTE] A CLI Azure erőforrás-kezelő és Azure Szolgáltatáskezelés mód, egymást kölcsönösen kizáró. Ez azt jelenti, hogy egy módban létrehozott erőforrások nem lehet felügyelni a módból.

## <a name="manage-your-account-information-and-publish-settings"></a>A fiókadatok kezelése és a közzétételi beállításokat
Egy a CLI csatlakozhat fiókjához módja az Azure előfizetési adatokat használatával. (Lásd: [Csatlakozás az Azure CLI az Azure-előfizetésbe](xplat-cli-connect.md) további lehetőségekért.) Az Azure klasszikus portált a közzététel beállításfájl itt leírt módon szerezheti be ezt az információt. A közzétételi beállításokat tartalmazó fájl importálhatja a beállítás, amely a CLI állandó helyi konfiguráció használ, további műveletek. Csak akkor van szüksége az importálandó a egyszer közzétételi beállításokat.

**fiók letöltési [kapcsolók]**

Ez a parancs elindítja a .publishsettings fájl letöltése az Azure klasszikus portálról böngészőben.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**fiók importálása [kapcsolók] &lt;fájl >**


Ez a parancs publishsettings fájl vagy tanúsítvány importálása, hogy szolgál az eszköz jövőbeli munkamenetek.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] A publishsettings fájlt is tartalmazhatnak egynél több előfizetés (Ez azt jelenti, hogy az előfizetés nevét és azonosító) adatait. A publishsettings fájl importálásakor az első előfizetés lesz az alapértelmezett leírását. Másik-előfizetést használ, futtassa a következő parancsot:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**fiók törlése a [kapcsolók]**

Ez a parancs az importált tárolt publishsettings eltávolítja. Ez a parancs használata, ha befejezte a eszközzel ezen a számítógépen, és szeretné biztosítani, hogy az eszköz nem használható a fiókjához jövőbeli munkamenetek.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**Ügyféllista [kapcsolók]**

Az importált előfizetések lista

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**fiók beállítása [kapcsolók] &lt;előfizetés&gt;**

Az aktuális előfizetés beállítása

###<a name="commands-to-manage-your-affinity-groups"></a>A affinitás csoportok kezelésére szolgáló parancsok

**számla affinitás csoport lista [kapcsolók]**

Ez a parancs az Azure affinitás csoportok listája.

Affinitás csoportok is be kell állítani egy virtuális gépeken futó csoportja több fizikai gépek hasábokat. A affinitás csoport megadhatja, hogy a tényleges gépek legközelebb egymással a lehető csökkentheti a hálózati késés.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**fiók affinitás-csoport létrehozása [kapcsolók] &lt;neve&gt;**

Ez a parancs létrehoz egy affinitás csoport

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**fiók affinitás csoport megjelenítése [kapcsolók] &lt;neve&gt;**

Ez a parancs megjeleníti a részletek, a affinitás csoport

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**a fiók csoport affinitás törlése [kapcsolók] &lt;neve&gt;**

Ez a parancs a megadott affinitás csoport törlése

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>A fiók környezet kezelésére szolgáló parancsok

**Ügyféllista boríték [kapcsolók]**

A fiók környezetben listája

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**fiók boríték megjelenítése [kapcsolók], [környezet]**

Fiók környezet részletek megjelenítése

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**fiók boríték hozzáad [kapcsolók], [környezet]**

Ez a parancs felvétele a fiók-környezet

**fiók boríték beállítása [kapcsolók], [környezet]**

Ez a parancs a fiók környezet beállítása

**a fiók boríték [kapcsolók], [környezet] törlése**

Ez a parancs a megadott környezet törli a fiókhoz

## <a name="commands-to-manage-your-classic-virtual-machines"></a>A klasszikus virtuális gépeken futó kezelésére szolgáló parancsok
Az alábbi ábra mutatja, milyen klasszikus Azure virtuális gépeken futó az Azure felhőszolgáltatásba éles üzembe környezetben futtatott.

![Azure technikai Diagram](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Hozzon létre és új** hoz létre a meghajtó (Ez azt jelenti, hogy a diagram e:\); blob-tárolóhoz egy korábban létrehozott, de leválasztott lemez **csatolása** csatol egy virtuális számítógépre.

**virtuális létrehozása [kapcsolók] &lt;dns-neve > &lt;kép > &lt;felhasználónév > [jelszó]**

Ez a parancs az Azure virtuális gép hoz létre. Alapértelmezés szerint a saját felhőszolgáltatásában virtuális gépeken (virtuális) hoz létre. Megadhatja, hogy egy virtuális gép hozzá kell adni egy meglévő felhőalapú szolgáltatást a - c lehetőséggel keresztül itt leírtak szerint.

A virtuális létrehozása parancs, például az Azure klasszikus portálon, csak hoz létre virtuális gépeken futó az éles üzemi környezetben környezetben. Nem hozhat létre virtuális gép egy felhőalapú szolgáltatásba átmeneti tárolásra szolgáló telepítési környezetében nincs lehetőség. Ha előfizetése nem rendelkezik egy meglévő Azure tárterület-fiókot, a parancs hoz létre egyet.

Megadhatja, hogy egy helyen – hely paraméterrel, vagy megadhat egy affinitás csoport – affinitás csoport paraméterrel. Ha sem van megadva, a program kéri a adja meg egy érvényes helyek közül.

A megadott jelszót 8-123 karakter hosszú legyen, és a virtuális gép esetén az operációs rendszerének összetettsége jelszókövetelmények felel meg.

Ha azt tervezi, hogy SSH használatával kezelheti a telepített Linux virtuális gép (mint általában a helyzet), engedélyeznie kell a SSH keresztül a -e lehetőség a virtuális gép létrehozásakor. Még nem lehet SSH engedélyezni, a virtuális gép létrehozását követően.

A Windows virtuális gépeken futó később engedélyezheti RDP-végpontjának port 3389 hozzáadásával.

Ez a parancs a következő opcionális kapcsolókat támogatott:

**- c, – csatlakozás** hozzon létre egy korábban létrehozott telepítési belül a virtuális gép egy szolgáltatójánál. Ha - vmname nem használja ezt a beállítást választja, automatikusan létrejön az új virtuális gép nevét.<br />
**-n, – virtuális neve** Adja meg a virtuális gép nevét. Ez a paraméter üzemeltetési szolgáltatás neve alapértelmezés szerint vesz igénybe. Ha - vmname nincs megadva, a nevet az új virtuális gép jelenik meg &lt;szolgáltatás neve >&lt;azonosító >, ahol &lt;azonosító > meglévő virtuális gépeken futó a szolgáltatást, valamint 1 száma. Például ez a parancs segítségével virtuális gép hozzáadása szolgáltatójánál, amely tartalmaz egy meglévő virtuális gép MyService, ha az új virtuális gép nevű MyService2.<br />
**-u, – blob-URL-címe** Adja meg a cél blob tároló URL-CÍMÉT, amelyben létre szeretné hozni a virtuális gép lemez. <br />
**-z, – virtuális-méret** Adja meg a virtuális gép méretét. Érvényes értékek a következők: "ExtraSmall", "– kicsi", "Közepes", "Nagy", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Az alapértelmezett értéke "Kicsi". <br />
**-r** RDP kapcsolat hozzáadása a Windows virtuális gép. <br />
**-e, – ssh** SSH kapcsolat hozzáadása a Windows virtuális gép. <br />
**-t, – ssh-tanúsítvány** Megadja a SSH tanúsítványt. <br />
**-s** Az előfizetés <br />
**-o – közösségi** A megadott kép egy közösségi képe <br />
**-w** A virtuális hálózat neve <br/>
**-l, – hely** elérési útja (például "Észak központi US"). <br />
**-a, – affinitás-csoport** Itt adhatja meg a affinitás csoport.<br />
**-w, – ezek olyan virtuális network name** Adja meg, amelyhez hozzá kívánja adni az új virtuális gép virtuális hálózaton. Virtuális hálózatok állíthat be, és az Azure klasszikus portálról kezeli.<br />
**-b, – alhálózat nevek** A virtuális gép hozzárendelése alhálózat nevét adja meg.

Ebben a példában a MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB a platform által biztosított képet. Operációs rendszer képek kapcsolatos további tudnivalókért olvassa el a virtuális képlista című témakört.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**virtuális létrehozása a &lt;dns-neve > &lt;szerepkör-fájl >**

Ez a parancs az Azure virtuális gép JSON szerepkör fájlból hoz létre.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**virtuális lista [kapcsolók]**

Ez a parancs megjeleníti az Azure virtuális gépeken futó. A json – lehetőséget Itt adhatja meg, hogy a a visszaadott eredmény nyers JSON formátumban.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**virtuális helylistájában [kapcsolók]**

Ez a parancs megjeleníti az összes rendelkezésre álló Azure-fiók hely.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**virtuális megjelenítése [kapcsolók] &lt;neve >**

Ez a parancs megjeleníti az Azure virtuális gép olvashat. A json – lehetőséget Itt adhatja meg, hogy a a visszaadott eredmény nyers JSON formátumban.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**virtuális törlése [kapcsolók] &lt;neve >**

Ez a parancs az Azure virtuális gép törli. Alapértelmezés szerint ez a parancs nem törli az Azure blob, amely az operációs rendszer és az adatok lemezzel készült. Ha törölni szeretné a blob és a virtuális gépen, amelyen alapul, adja meg, a -b lehetőséget.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**virtuális indítása [kapcsolók] &lt;neve >**

Ez a parancs elindítja az Azure virtuális gépen.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**virtuális indítsa újra a [kapcsolók] &lt;neve >**

Ez a parancs az Azure virtuális gép újraindul.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**virtuális leállítás [kapcsolók] &lt;neve >**

Ez a parancs leállítja az Azure virtuális gépen. Megadhatja, hogy a számítási erőforrás nem kiadott a leállítás a -p lehetőséget is használhatja.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**virtuális rögzítési &lt;virtuális neve > &lt;cél-kép-neve >**

Ez a parancs rögzíti Azure virtuális gép képet.

Egy virtuális gép képe csak rögzíthetők, ha a virtuális gép állapot **Leállítva**. Állítsa le a virtuális gép a továbblépés előtt.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**virtuális exportálás [kapcsolók] &lt;virtuális neve > &lt;az útvonal >**

Ez a parancs fájlba exportálja az Azure virtuális gép képe

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Az Azure virtuális gép végpontok kezelésére szolgáló parancsok
A következő ábrán egy tipikus telepítési klasszikus virtuális gép több példány architektúráját. Ebben a példában port 3389 meg nyitva, a virtuális gépeken (az RDP access). A virtuális gépeken a terheléselosztó útvonal forgalmat a virtuális géphez által használt is van egy belső IP-címét (például 168.55.11.1). A belső IP-címet is használható virtuális gépeken futó közötti kommunikációt.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Virtuális gépeken futó külső kérések lépjen egy terheléselosztó keresztül. Emiatt kérések nem lehet megadni egy adott virtuális készülék a telepítések több virtuális gépeken futó szemben. A több virtuális gépeken futó telepítésekhez port hozzárendelése kell beállítania a virtuális gépeken futó (virtuális nyílású) és a terheléselosztó (lb nyílású) között.

**virtuális végpont létrehozása &lt;virtuális neve > &lt;lb nyílású > [virtuális nyílású]**

Ez a parancs létrehoz egy virtuális gép végpontot. Megadhatja, hogy ahhoz, hogy közvetlen kiszolgáló megtérülése a végpont alapértelmezés szerint engedélyezve u – vagy – engedélyezése közvetlen-kiszolgáló-vissza is használhatja.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**virtuális végpont létrehozása-több [kapcsolók] &lt;virtuális neve > &lt;lb nyílású > [:&lt;virtuális nyílású > [:&lt;Protocol (protokoll) > [:&lt;enable-közvetlen-kiszolgáló-return > [:&lt;lb-set-neve > [:&lt;vizsgálati-protocol > [:&lt;vizsgálati nyílású > [:&lt;vizsgálati-path > [:&lt;belső lb neve >]]] {1 –*}**

Hozzon létre több virtuális végponttal.

**virtuális végpont törlése [kapcsolók] &lt;virtuális neve > &lt;végpontjának neve >**

Ez a parancs töröl egy virtuális gép végpontot.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**virtuális végpont lista &lt;virtuális neve >**

Ez a parancs megjeleníti az összes virtuális gép végpontok. A json – lehetőséget Itt adhatja meg, hogy a a visszaadott eredmény nyers JSON formátumban.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**virtuális végpont frissítés [kapcsolók] &lt;virtuális neve > &lt;végpontjának neve >**

Ez a parancs frissíti egy virtuális végpontot új értékek használja ezeket a beállításokat.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**virtuális végpont megjelenítése [kapcsolók] &lt;virtuális neve >**

Ez a parancs látható egy virtuális a részletek, a végpontok

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Azure virtuális gép képeit kezelésére szolgáló parancsok

Virtuális gép képek találhatók a rögzített, már konfigurált virtuális gépeken futó replikálhatók, szükség szerint.

**virtuális képlista [kapcsolók]**

Ez a parancs megkapja a virtuális gép képek listáját. A képek három típusba sorolhatók: Microsoft által létrehozott képek, az "MSFT" elé, mely a szállító nevű elé, és a képeket, harmadik fél által létrehozott képek létrehozása. Képek létrehozásához is rögzíthet egy meglévő virtuális gép vagy a kép készítése egy egyéni .vhd blob-tárolóhoz feltöltött. Egy egyéni .vhd használatával kapcsolatos további tudnivalókért olvassa el a virtuális kép létrehozása című témakört.
A json – lehetőséget Itt adhatja meg, hogy a a visszaadott eredmény nyers JSON formátumban.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**virtuális kép megjelenítése [kapcsolók] &lt;neve >**

Ez a parancs megjeleníti a virtuális gép kép részleteit.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**virtuális kép törlésének [kapcsolók] &lt;neve >**

Ez a parancs a virtuális gép kép törlése.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**virtuális kép létrehozása &lt;neve > [forrás elérési útja]**

Ez a parancs létrehoz egy virtuális gép képe. Egyéni .vhd fájlok blob-tároló feltöltött vannak, és akkor jön létre a virtuális gép képe a onnan. Ezt követően az ezen a képen virtuális gép hozhat létre virtuális géphez. A hely és OS paraméterek szükség.

>[AZURE.NOTE]Ez a parancs jelenleg csak feltöltése rögzített .vhd fájlok támogatja. Dinamikus .vhd fájl feltöltése, használja az [Azure virtuális segédprogramok lépjen](https://github.com/Microsoft/azure-vhd-utils-for-go).

Egyes rendszerek folyamat fájl leíró korlátai ír elő. A korlát túllépésekor egy fájl leíró korlát hibát jelenít meg. Futtatását is lehetővé teszi az paranccsal újra -p &lt;szám > paraméter csökkentheti a párhuzamos feltöltések maximális számát. A párhuzamos feltöltések alapértelmezés szerinti maximális száma 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Azure virtuális gép adatok lemezt kezelésére szolgáló parancsok

Adatok lemezt a virtuális gépen használható blob-tárolóhoz .vhd fájlok közül. További információt az adatok lemezt blob-tároló, akkor olvassa el a korábbi látható Azure technikai diagram van telepítve.

Adatok lemezt csatolása szolgáló parancsok (azure virtuális lemez csatolása és azure virtuális lemez csatolása és új) logikai egység szám (logikai) hozzárendelése a csatlakoztatott adatok lemezt, igény szerint SCSI protokollt. Az első virtuális géphez csatlakoztatott adatok lemez logikai 0 van-e hozzárendelve, a következő hozzárendelt logikai 1, és így tovább.

Ha leválasztja az azure virtuális lemezzel adatok lemezen leválasztása parancs, használja a &lt;logikai&gt; paraméter, hogy melyik lemezen, hogy le.

>[AZURE.NOTE] Meg kell mindig leválasztása adatok lemezt fordított sorrendben, a legmagasabb számozott logikai rendelt kezdve. A Linux SCSI réteget nem támogatja a leválasztása egy alsó számozott logikai, miközben egy újabb számozott logikai továbbra is van csatlakoztatva. Ha például kell nem leválasztja logikai 0 Ha logikai 1 továbbra is van csatlakoztatva.

**virtuális lemez show [kapcsolók] &lt;neve >**

Ez a parancs megjeleníti az Azure lemez olvashat.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**virtuális lemez lista [kapcsolók], [virtuális neve]**

A parancs listák Azure lemezt vagy lemezen megadott virtuális géphez csatlakoztatott. Ha egy virtuális gép neve paraméterrel futtatása a virtuális géphez csatlakoztatott összes lemezt adja eredményül. A virtuális gép logikai 1 jön létre, és minden más felsorolt lemez külön-külön csatolt.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Az összes lemez virtuális gép neve paraméter nélkül a parancs végrehajtása adja eredményül.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**virtuális lemez törlés [kapcsolók] &lt;neve >**

Ez a parancs az Azure lemez töröl egy személyes tárban tárolnak. A lemez virtuális gépen kell leválasztva törlés előtt.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**virtuális lemez létrehozása &lt;neve > [forrás elérési útja]**

Ez a parancs feltölti, és az Azure lemez regisztrálja. – blob-URL-címét, – helyét, vagy – affinitás csoport meg kell adni. Ha ez a parancs az [forrás elérési útja], megadott .vhd fájlt töltenek fel, és kép jön létre. Ezután csatolhat ezen a képen egy virtuális géphez virtuális lemezzel csatolni.

Egyes rendszerek folyamat fájl leíró korlátai ír elő. A korlát túllépésekor egy fájl leíró korlát hibát jelenít meg. Futtatását is lehetővé teszi az paranccsal újra -p &lt;szám > paraméter csökkentheti a párhuzamos feltöltések maximális számát. A párhuzamos feltöltések alapértelmezés szerinti maximális száma 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**virtuális lemez feltöltés [kapcsolók] &lt;forrás-elérési útja > &lt;blob-url > &lt;tároló fiókkulcs >**

Ez a parancs segítségével virtuális lemezen feltöltése

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**csatolni virtuális lemez &lt;virtuális neve > &lt;merevlemez-kép-neve >**

Ez a parancs egy meglévő lemez blob-tárolóban lévő csatol egy meglévő virtuális gép telepítését az egy felhőalapú szolgáltatásba.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**virtuális lemez csatolása és új &lt;virtuális neve > &lt;méretét a gb > [blob-URL-cím]**

Ez a parancs adatok lemezen csatol egy Azure virtuális gépen. Ebben a példában 20 mérete az új lemez gigabájtban rögzíteni. Másik lehetőségként használhatja blob URL-címet az utolsó argumentum explicit módon adja meg a cél blob hozhat létre. Ha nem adja meg a blob URL-címet, automatikusan létrejön egy blob-objektumot.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**virtuális lemez leválasztása &lt;virtuális neve > &lt;logikai >**

Ez a parancs leválasztja az Azure virtuális géphez csatlakoztatott adatok meghajtót. &lt;logikai > azonosítja a lemez leválasztott lesz. Lemezen társított, mielőtt leválasztása esetén lemezre listáját, használja virtuális merevlemez-lista &lt;virtuális neve >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Az Azure felhőszolgáltatások kezelésére szolgáló parancsok

Azure felhőszolgáltatások alkalmazások és szolgáltatások webes és dolgozó szerepeket is. Az alábbi parancsok Azure felhőszolgáltatások kezelésére használható.

**szolgáltatás hozzon létre [kapcsolók] &lt;serviceName >**

Ez a parancs létrehoz egy felhőalapú szolgáltatásba

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**szolgáltatás megjelenítése [kapcsolók] &lt;serviceName >**

Ez a parancs megjeleníti az Azure felhőszolgáltatásba részleteit

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**szolgáltatáslistával [kapcsolók]**

Ez a parancs megjeleníti az Azure felhőszolgáltatások.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**szolgáltatás törlési [kapcsolók] &lt;neve >**

Ez a parancs az Azure felhőalapú szolgáltatás törli.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

A törlés megjeleníteni, használja a `-q` paraméter.


## <a name="commands-to-manage-your-azure-certificates"></a>Az Azure tanúsítványok kezelésére szolgáló parancsok

Azure service tanúsítványok az SSL-tanúsítványok a Azure fiókjához kapcsolva. Azure tanúsítványokkal kapcsolatos további tudnivalókért olvassa el a [Tanúsítványok kezelése](http://msdn.microsoft.com/library/azure/gg981929.aspx)című témakört.

**tanúsítvány szolgáltatáslistával [kapcsolók]**

Ez a parancs megjeleníti az Azure tanúsítványokat.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**szolgáltatás-tanúsítvány létrehozása &lt;dns-előtag > &lt;fájl > [jelszó]**

Ez a parancs feltölt egy tanúsítványt. Üres mező a jelszómező tanúsítványok, amelyek nem jelszóval védett.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**szolgáltatás tanúsítványának törlés [kapcsolók] &lt;ujjlenyomat >**

Ez a parancs töröl egy tanúsítványt.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>A web Apps alkalmazások kezelésére szolgáló parancsok

Az Azure-webappokban beállítás egy webhelyen elérhető URI. Web Apps alkalmazások tárolt virtuális gépeken futó, de nem kell figyelni a létrehozás és a virtuális gép alkalmazás részleteit saját magának. Adott részletek Azure-kezeli.

**webhelyen tárolt listában [kapcsolók]**

Ez a parancs megjeleníti a web Apps alkalmazások.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**webhely beállítása [kapcsolók], [név]**

Ez a parancs állítja be a web app [név] beállítási lehetőségei

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**webhely deploymentscript [kapcsolók]**

Ez a parancs létrehoz egy egyéni telepítési parancsfájlt

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**webhely létrehozása [kapcsolók], [név]**

Ez a parancs a web app és a helyi könyvtár hoz létre.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] A webhely neve egyedinek kell lennie. Webhely nem hozható létre a DNS neve megegyezik egy meglévő webhelyet.

**webhely Tallózás [kapcsolók], [név]**

Ez a parancs megnyitja a az webalkalmazást a böngészőben.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**webhely show [kapcsolók], [név]**

Ez a parancs webalkalmazást részletes adatait jeleníti meg.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**webhely törlése [kapcsolók], [név]**

Ez a parancs törli a webalkalmazást.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **webhely felcserélése [kapcsolók], [név]**

Ez a parancs két web app helyek felcseréli.

Ez a parancs támogatja a következő további lehetőséget:

**– kérdések és **– csendes **: megerősítést kér. Az automatikus parancsfájlok használja ezt a beállítást.


**webhely start [kapcsolók], [név]**

Ez a parancs elindítja a webalkalmazást.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**webhely leállítása [kapcsolók], [név]**

Ez a parancs leállítja a webalkalmazást.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**webhely újraindítás [kapcsolók], [név]**

Ez a parancs leállítja, és megkezdi a megadott web App alkalmazásban.

Ez a parancs támogatja a következő további lehetőséget:

**– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.


**webhely helylistájában [kapcsolók]**

Ez a parancs megjeleníti a web app helyek.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>A web app alkalmazás beállításainak kezelésére szolgáló parancsok

**webhely appsetting lista [kapcsolók], [név]**

Ez a parancs megjeleníti az adott hozzá a web app alkalmazás beállítása.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**webhely appsetting hozzáadása [kapcsolók] &lt;keyvaluepair > [név]**

Ez a parancs a web app-app beállítása elsődlegeskulcs-értékének párban hozzáadja.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**webhely appsetting törlése [kapcsolók] &lt;kulcs > [név]**

Ez a parancs törli az adott alkalmazás beállítása a web App alkalmazásban.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**webhely appsetting megjelenítése [kapcsolók] &lt;kulcs > [név]**

Ez a parancs megjeleníti az adott alkalmazás beállítása részleteit

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>A web app tanúsítványok kezelésére szolgáló parancsok

**tanúsítvány listában [kapcsolók], [név]**

Ez a parancs a web app tanúsítványok listáját jeleníti meg.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**webhely-tanúsítvány felvétele az [kapcsolók] &lt;tanúsítvány-path > [név]**

**webhely tanúsítvány törlése [kapcsolók] &lt;ujjlenyomat > [név]**

**a webhely tanúsítvány show [kapcsolók] &lt;ujjlenyomat > [név]**

Ez a parancs megjeleníti a tanúsítvány részleteinek

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>A web app kapcsolat karakterláncok kezelésére szolgáló parancsok

**webhely connectionstring lista [kapcsolók], [név]**

**webhely connectionstring hozzáadása [kapcsolók] &lt;kapcsolat neve > &lt;érték > &lt;típus > [név]**

**webhely connectionstring törlése [kapcsolók] &lt;kapcsolat neve > [név]**

**webhely connectionstring megjelenítése [kapcsolók] &lt;kapcsolat neve > [név]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>A web app alapértelmezett dokumentumok kezelésére szolgáló parancsok

**webhely defaultdocument lista [kapcsolók], [név]**

**webhely defaultdocument hozzáadása [kapcsolók] &lt;dokumentum > [név]**

**webhely defaultdocument törlése [kapcsolók] &lt;dokumentum > [név]**

###<a name="commands-to-manage-your-web-app-deployments"></a>A web app-telepítések kezelésére szolgáló parancsok

**telepítési listában [kapcsolók], [név]**

**webhelyek üzembe megjelenítése [kapcsolók] &lt;commitId > [név]**

**webhely üzembe újratelepítés [kapcsolók] &lt;commitId > [név]**

**webhely üzembe github [kapcsolók], [név]**

**webhely üzembe felhasználói beállítása [kapcsolók], [felhasználónév], [fázis]**

###<a name="commands-to-manage-your-web-app-domains"></a>A web app tartományok kezelésére szolgáló parancsok

**webhely tartományok listája [kapcsolók], [név]**

**webhely tartomány hozzáadása [kapcsolók] &lt;dn > [név]**

**webhely tartomány törlése [kapcsolók] &lt;dn > [név]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>A web app kezelő hozzárendelések kezelésére szolgáló parancsok

**eseménykezelő listában [kapcsolók], [név]**

**webhely-kezelő hozzáadása [kapcsolók] &lt;bővítmény > &lt;processzor > [név]**

**webhely-kezelő törlése [kapcsolók] &lt;bővítmény > [név]**

###<a name="commands-to-manage-your-web-jobs"></a>A webhely feladatok kezelésére szolgáló parancsok

**webhely feladatlista [kapcsolók], [név]**

Ez a parancs a webalkalmazást webes feladatok lista.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– Munkatípus** &lt;munkatípus >: nem kötelező. A webjob típusát. Érvényes érték "indítóval kezdeményezett" vagy "folyamatos". Alapértelmezés szerint az összes típusú webjobs vissza.
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

**a webhely feladat megjelenítése [kapcsolók] &lt;nyomtatási > &lt;feladattípus > [név]**

Ez a parancs megjeleníti az adott webhely feladat részleteit.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve >: szükséges. A webjob neve.
+ **– Munkatípus** &lt;munkatípus >: szükséges. A webjob típusát. Érvényes érték "indítóval kezdeményezett" vagy "folyamatos".
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

**webhely-feladat törlése [kapcsolók] &lt;nyomtatási > &lt;feladattípus > [név]**

Ez a parancs a megadott web-feladat törlése

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve > szükséges. A webjob neve.
+ **– Munkatípus** &lt;munkatípus > szükséges. A webjob típusát. Érvényes érték "indítóval kezdeményezett" vagy "folyamatos".
+ **– kérdések** és **– Csendes**: megerősítést kér. Az automatikus parancsfájlok használja ezt a beállítást.
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

**webhely feladat feltöltés [kapcsolók] &lt;nyomtatási > &lt;feladattípus > <jobFile> [név]**

Ez a parancs a megadott web-feladat törlése

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve >: szükséges. A webjob neve.
+ **– Munkatípus** &lt;munkatípus >: szükséges. A webjob típusát. Érvényes érték "indítóval kezdeményezett" vagy "folyamatos".
+ **– feladat-fájl** &lt;feladat-fájl >: szükséges. A feladat fájlt.
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

**webhely a projekt kezdési [kapcsolók] &lt;nyomtatási > &lt;feladattípus > [név]**

Ez a parancs elindítja a megadott web feladatot.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve >: szükséges. A webjob neve.
+ **– Munkatípus** &lt;munkatípus >: szükséges. A webjob típusát. Érvényes érték "indítóval kezdeményezett" vagy "folyamatos".
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

**webhely feladat befejezésekor [kapcsolók] &lt;nyomtatási > &lt;feladattípus > [név]**

Ez a parancs leállítja a megadott web feladatot. Csak a folyamatos feladatok leállítható.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve >: kötelező. A webjob neve.
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

###<a name="commands-to-manage-your-web-jobs-history"></a>A webhely feladatok előzmények kezelésére szolgáló parancsok

**webhely előzmények feladatlista [kapcsolók], [nyomtatási], [név]**

Ez a parancs megjeleníti a megadott web feladat lefut az előzményeket.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve >: szükséges. A webjob neve.
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

**webhely korábbi megjelenítése [kapcsolók], [nyomtatási], [futtatásazonosító], [név]**

Ez a parancs a részletek, a feladat futtatása a megadott web projektre vonatkozóan kap.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– feladat-neve** &lt;feladat-neve >: szükséges. A webjob neve.
+ **– Futtatás-azonosító** &lt;Futtatás-azonosító >: nem kötelező. A futtatási előzmények azonosítója. Ha nincs megadva, a legújabb megjelenítése futtatása.
+ **– tárolóhely** &lt;tárolóhely >: a tárolóhely újraindításához nevét.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>A web app diagnosztika kezelésére szolgáló parancsok

**webhely napló letöltése [kapcsolók], [név]**

Töltse le a web app diagnosztika tartalmazó .zip fájlok.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**webhely napló képviselőknek [kapcsolók], [név]**

Ez a parancs a terminálablakba a napló streaming szolgáltatás csatlakozik.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**webhely napló beállítása [kapcsolók], [név]**

Ez a parancs a web App alkalmazásban a diagnosztikai beállítások konfigurálása

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>A web app tárházakban kezelésére szolgáló parancsok

**webhely tárházba ág [kapcsolók] &lt;ág > [név]**

**webhely tárházba törlése [kapcsolók], [név]**

**webhely tárházba szinkronizálási [kapcsolók], [név]**

###<a name="commands-to-manage-your-web-app-scaling"></a>A web app méretezés kezelésére szolgáló parancsok

**webhely-skála mód [kapcsolók] &lt;mód > [név]**

**webhely-skála példányok [kapcsolók] &lt;példányok > [név]**


## <a name="commands-to-manage-azure-mobile-services"></a>Azure Mobile Services kezelésére szolgáló parancsok

Azure Mobile szolgáltatások az egyetlen Azure szolgáltatások, amelyek lehetővé teszik az alkalmazások kódmentes funkciókhoz. Mobil szolgáltatások parancsai a következő kategóriák oszthatók:

+ [Mobilszolgáltatás példányok kezelésére szolgáló parancsok](#Mobile_Services)
+ [Mobilszolgáltatás konfigurációs kezelésére szolgáló parancsok](#Mobile_Configuration)
+ [Mobilszolgáltatás táblák kezelésére szolgáló parancsok](#Mobile_Tables)
+ [Parancsfájlok mobilszolgáltatás kezelésére szolgáló parancsok](#Mobile_Scripts)
+ [Ütemezett projektek kezelésére szolgáló parancsok](#Mobile_Jobs)
+ [Ha át kívánja méretezni a mobilszolgáltatás parancsok](#Mobile_Scale)

Az alábbi beállítások a legtöbb Mobile szolgáltatások parancsai vonatkoznak:

+ **-h** vagy **– Súgó**: megjelenítési kimeneti kihasználtsági információk.
+ **-s `<id>` ** vagy **– előfizetés `<id>` **: használja az adott előfizetést, a megadott `<id>`.
+ **-v szolgáltatást úgy** vagy **– részletes**: részletes kimenet írni.
+ **– json**: írása JSON kimeneti.

### <a name="Mobile_Services"></a>Mobilszolgáltatás példányok kezelésére szolgáló parancsok

**mobil helyek [kapcsolók]**

Ez a parancs megjeleníti a földrajzi helyek Mobile szolgáltatás által támogatott.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**mobil létrehozása [kapcsolók], [szolgáltatásnév], [sqlAdminUsername], [sqlAdminPassword]**

Ez a parancs létrehoz egy SQL-adatbázis és a kiszolgáló egy mobilszolgáltatás.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **- r `<sqlServer>` ** vagy **– SQL Server `<sqlServer>` **: használjon egy meglévő SQL-adatbázis kiszolgálója, a megadott `<sqlServer>`.
+ **-d `<sqlDb>` ** vagy **– sqlDb `<sqlDb>` **: használja a meglévő SQL-adatbázishoz megadott `<sqlDb>`.
+ **-l `<location>` ** vagy **– hely `<location>` **: a szolgáltatás létrehozása az egy adott helyen, a megadott `<location>`. Futtassa az elérhető helyek első azure mobil helyek.
+ **– sqlLocation `<location>` **: az SQL server létrehozása az egy adott `<location>`; az alapértelmezett érték a mobilszolgáltatás helyét.

**mobil törlés [kapcsolók], [szolgáltatásnév]**

Ez a parancs az SQL-adatbázis és a kiszolgáló egy mobilszolgáltatás törli.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **d –** vagy **– deleteData**: az összes adat törlése a mobil szolgáltatásból az adatbázisból.
+ **-a** vagy **--deleteAll**: az SQL-adatbázis és a kiszolgáló törlése.
+ **– kérdések** és **– Csendes**: megerősítést kér. Az automatikus parancsfájlok használja ezt a beállítást.

**mobil lista [kapcsolók]**

Ez a parancs megjeleníti a mobil szolgáltatások.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**mobil show [kapcsolók], [szolgáltatásnév]**

Ez a parancs megjeleníti a mobil szolgáltatás kapcsolatos részleteket.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**mobil újraindításra [kapcsolók], [szolgáltatásnév]**

Ez a parancs a mobilszolgáltatás példány újraindul.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**mobil log [kapcsolók], [szolgáltatásnév]**

Ez a parancs mobile szolgáltatás a naplók diagramtípusokat napló kiszűrésével adja eredményül, de `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **- r `<query>` ** vagy **– lekérdezés `<query>` **: végrehajtja a megadott napló lekérdezést.
+ **-t `<type>` ** vagy **– típusú `<type>` **: a visszaadott naplók szűrni bejegyzés `<type>`, ami lehet `information`, `warning`, vagy `error`.
+ **-k `<skip>` ** vagy **– ugorja át `<skip>` **: által megadott sorok számát átugrása `<skip>`.
+ **-p `<top>` ** vagy **– felső `<top>` **: adja eredményül a megadott számú sort, által megadott `<top>`.

> [AZURE.NOTE] A **– lekérdezés** paraméter elsőbbrendű képest **– típusú**, **– ugorja át**, és **– felső**.

**mobil helyreállítása [kapcsolók], [unhealthyservicename], [healthyservicename]**

Ez a parancs megfelelő mobilszolgáltatás egy másik régióbeli áthelyezésével állít helyre a sérült mobilszolgáltatás.

Ez a parancs támogatja a következő további lehetőséget:

**– kérdések** és **– Csendes**: a helyreállítási megerősítés kérése az mellőzése.

**mobil kulcs újragenerálása [kapcsolók], [szolgáltatásnév], [típus]**

Ez a parancs a mobilszolgáltatás helyi menüt megjelenítő billentyű újragenerálása.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Fő típusai `master` és `application`.

> [AZURE.NOTE] Billentyűk követező létrehozásakor, a régi kulcs használó ügyfelek nem fér hozzá a mobilszolgáltatás lehet. A helyi menüt megjelenítő billentyű követező létrehozásakor, frissítenie kell az alkalmazást az új kulcs értékkel.

**mobil kulcs beállítása [kapcsolók], [szolgáltatásnév], [típus], [érték]**

Ez a parancs a mobilszolgáltatás kulcs adott értékre állítja be.


### <a name="Mobile_Configuration"></a>Mobilszolgáltatás konfigurációs kezelésére szolgáló parancsok

**mobil beállítások listája [kapcsolók], [szolgáltatásnév]**

Ez a parancs megjeleníti a mobilszolgáltatás beállítási lehetőségei.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**mobil config első [kapcsolók], [szolgáltatásnév], [billentyűt.]**

Ez a parancs kap egy mobil szolgáltatás, ebben az esetben dinamikus séma egy adott konfiguráció lehetőséget.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**mobil config beállítása [kapcsolók], [szolgáltatásnév], [billentyű], [érték]**

Ez a parancs az egy adott beállítás egy mobil szolgáltatás, ebben az esetben dinamikus séma állítja be.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Mobilszolgáltatás táblák kezelésére szolgáló parancsok

**mobil táblázatos lista [kapcsolók], [szolgáltatásnév]**

Ez a parancs megjeleníti a mobilszolgáltatás szereplő összes táblázat.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**mobil tábla megjelenítése [kapcsolók], [szolgáltatásnév], [táblanév]**

Ez a parancs megjeleníti az egy adott tábla eredménye részleteit.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**mobil tábla létrehozása [kapcsolók], [szolgáltatásnév], [táblanév]**

Ez a parancs létrehoz egy táblázatot.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Ez a parancs támogatja a következő további lehetőséget:

+ **-p `&lt;permissions>` ** vagy **– engedélyek `&lt;permissions>` **: vesszővel tagolt listáját `<operation>` = `<permission>` párokká, ahol `<operation>` van `insert`, `read`, `update`, vagy `delete` és `&lt;permissions>` van `public`, `application` (alapértelmezett), `user`, vagy `admin`.

**mobil adatok olvasása [kapcsolók], [szolgáltatásnév], [táblanév], [query]**

Ez a parancs adatokat olvas táblázat.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-k `<skip>` ** vagy **– ugorja át `<skip>` **: által megadott sorok számát átugrása `<skip>`.
+ **-t `<top>` ** vagy **– felső `<top>` **: adja eredményül a megadott számú sort, által megadott `<top>`.
+ **-l** vagy **– lista**: adatok visszaadására lista formátumban.

**mobil táblázat frissítése [kapcsolók], [szolgáltatásnév], [táblanév]**

Ez a parancs csak a rendszergazdák táblákra engedélyek törlése változik.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-p `&lt;permissions>` ** vagy **– engedélyek `&lt;permissions>` **: vesszővel tagolt listáját `<operation>` = `<permission>` párokká, ahol `<operation>` van `insert`, `read`, `update`, vagy `delete` és `&lt;permissions>` van `public`, `application` (alapértelmezett), `user`, vagy `admin`.
+ **– deleteColumn `<columns>` **: vesszővel tagolt lista és az oszlopok törlése, mint `<columns>`.
+ **– kérdések** és **– Csendes**: oszlopok törlése a megerősítés kérése nélkül.
+ **– addIndex `<columns>` **: vesszővel tagolt listáját a tárgymutató szerepeltetni kívánt oszlopokat.
+ **– deleteIndex `<columns>` **: Ha nem szeretné, az index oszlopok vesszővel elválasztott listája.

**mobil táblázat törlése [kapcsolók], [szolgáltatásnév], [táblanév]**

Ez a parancs törli a táblát.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Adja meg a - q paraméter megerősítés nélkül a táblázat törlése. Ehhez automatizálási parancsfájlok blokkolása megakadályozása érdekében.

**mobil adatok csonkolása [kapcsolók], [szolgáltatásnév], [táblanév]**

Ez a parancs eltávolítása a táblázat összes sornyi adatot.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Parancsfájlok kezelésére szolgáló parancsok

Ebben a szakaszban a parancsok kezelése a kiszolgálón parancsfájlok tartoznak egy mobilszolgáltatás szolgálnak. További tudnivalókért lásd: [a kiszolgálóoldali parancsfájlokat a mobil szolgáltatások használata](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**mobil parancsfájl lista [kapcsolók], [szolgáltatásnév]**

Ez a parancs megjeleníti a regisztrált parancsfájlok, beleértve a táblázat és a Feladatütemező parancsfájlok.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**mobil parancsfájl letöltése [kapcsolók], [szolgáltatásnév], [parancsfájlnév]**

Ez a parancs a Beszúrás parancsfájl letölti a TodoItem táblázat nevű fájlban `todoitem.insert.js` a a `table` almappát.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-p `<path>` ** vagy **– elérési út `<path>` **: az aktuális munkakönyvtár esetén az alapértelmezett mentheti azt, amelyben a fájl helyét.
+ **-f `<file>` ** vagy **– fájl `<file>` **: a parancsfájl mentéséhez a fájl nevét.
+ **-o** vagy **– felülbírálása**: meglévő fájlok felülírása.
+ **-c** vagy **– konzol**: írja meg a parancsfájlt a konzolhoz helyett a fájlba.

**mobil parancsfájl feltöltés [kapcsolók], [szolgáltatásnév], [parancsfájlnév]**

Ez a parancs feltölti nevű parancsfájl `todoitem.insert.js` a a `table` almappát.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

A fájl nevét a táblázat és a művelet nevekből állhatnak. A helyet, ahová a parancs végrehajtása képest a táblázat almappában kell lennie. Is használhatja a **-f `<file>` ** vagy **– fájl `<file>` ** paraméter használatával adja meg, amely tartalmazza a parancsfájl regisztrálhatja a fájl elérési útját és más fájlnevet.


**mobil parancsfájl [kapcsolók], [szolgáltatásnév], [parancsfájlnév] törlése**

Ez a parancs a meglévő Beszúrás parancsfájl eltávolítja a TodoItem táblázatot.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Ütemezett projektek kezelésére szolgáló parancsok

Ebben a részben parancsok használatával ütemezett feladatok tartoznak egy mobilszolgáltatás kezelése. További tudnivalókért olvassa el a [feladatok ütemezése](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx)című témakört.

**mobil feladatlista [kapcsolók], [szolgáltatásnév]**

Ez a parancs megjeleníti az ütemezett feladatokat.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**mobil feladat létrehozása [kapcsolók], [szolgáltatásnév], [nyomtatási]**

Ez a parancs létrehoz egy nevű feladat `getUpdates` , amely ütemezve van óránként.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-i `<number>` ** vagy **– intervallum `<number>` **: A feladat időköze, mint egy egész szám. Az alapértelmezett érték `15`.
+ **-u `<unit>` ** vagy **– intervalUnit `<unit>` **: A mennyisége, az _intervallum_, amely a következő értékek egyike lehet:
    + **a perc** (alapértelmezett)
    + **óra**
    + **nap**
    + **hónap**
    + **nincs lehetőség** (igény szerinti feladatok)
+ **-t `<time>`** **– Kezdés időpontja `<time>` ** A kezdési időpontot, az első futtatása a parancsfájl ISO-formátumban. Az alapértelmezett érték `now`.

> [AZURE.NOTE] Új feladatok jönnek létre letiltott állapotba kerül, mert egy parancsprogramot is fel kell tölteni. A **mobil parancsfájl feltöltése** paranccsal feltöltése egy parancsprogramot és ahhoz, hogy a feladat **mobil feladat módosítása** parancsot.

**mobil feladat frissítés [kapcsolók], [szolgáltatásnév], [nyomtatási]**

A következő parancs lehetővé teszi, hogy a letiltott `getUpdates` feladatot.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-i `<number>` ** vagy **– intervallum `<number>` **: A feladat időköze, mint egy egész szám. Az alapértelmezett érték `15`.
+ **-u `<unit>` ** vagy **– intervalUnit `<unit>` **: A mennyisége, az _intervallum_, amely a következő értékek egyike lehet:
    + **a perc** (alapértelmezett)
    + **óra**
    + **nap**
    + **hónap**
    + **nincs lehetőség** (igény szerinti feladatok)
+ **-t `<time>`** **– Kezdés időpontja `<time>` ** A kezdési időpontot, az első futtatása a parancsfájl ISO-formátumban. Az alapértelmezett érték `now`.
+ **- `<status>` ** vagy **– állapot `<status>` **: A feladat állapota, amely lehet vagy `enabled` vagy `disabled`.

**mobil feladat törlése [kapcsolók], [szolgáltatásnév], [nyomtatási]**

Ez a parancs az getUpdates ütemezett feladat eltávolítása a listájába kiszolgáló.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] A feladat törlésével is törli a feltöltött parancsfájl.

### <a name="Mobile_Scale"></a>Ha át kívánja méretezni a mobilszolgáltatás parancsok

Ebben a részben parancsok használatával egy mobilszolgáltatás méretezni. További tudnivalókért olvassa el a [egy mobilszolgáltatás méretezés](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx)című témakört.

**mobil skála megjelenítése [kapcsolók], [szolgáltatásnév]**

Ez a parancs megjeleníti a skála adatait, beleértve az aktuális számítási mód és a példányok száma.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**mobil méretarányra módosíthatja [kapcsolók], [szolgáltatásnév]**

Ez a parancs a mobilszolgáltatás beosztásának ingyenes prémium módra módosítja.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **- c `<mode>` ** vagy **– computeMode `<mode>` **: A számítási mód kell lennie, vagy `Free` vagy `Reserved`.
+ **-i `<count>` ** vagy **– numberOfInstances `<count>` **: Ha fenntartott üzemmódban használt példányok száma.

> [AZURE.NOTE] Ha Ön számítási mód beállításához `Reserved`, a mobil szolgáltatások ugyanabban a régióban futtatása prémium módban.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Parancsok a mobilszolgáltatási készült előzetes funkciók engedélyezése

**mobil előzetes lista [kapcsolók], [szolgáltatásnév]**

Ez a parancs az előnézeti funkciót a megadott szolgáltatást, és hogy engedélyezve van a jeleníti meg.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**mobil előzetes engedélyezése [kapcsolók], [szolgáltatásnév], [ÖsszetevőNeve]**

Ez a parancs lehetővé teszi, hogy az adott megtekintése funkció egy mobil szolgáltatás. Miután engedélyezte, előzetes verzió szolgáltatások egy mobil szolgáltatás nem tiltható le.

###<a name="commands-to-manage-your-mobile-service-apis"></a>A mobilszolgáltatás API-khoz kezelésére szolgáló parancsok

**mobil api lista [kapcsolók], [szolgáltatásnév]**

Ez a parancs a mobil listáját jeleníti meg a mobil szolgáltatásbeli létrehozott egyéni API-szolgáltatás.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**mobil api létrehozása [kapcsolók], [szolgáltatásnév], [APINÉV]**

Létrehoz egy egyéni mobilszolgáltatás API

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Ez a parancs támogatja a következő további lehetőséget:

**-p** vagy **– engedélyek** &lt;engedélyek >: vesszővel tagolt listáját &lt;módszer > =&lt;jogosultsági > párokká.

**mobil api frissítés [kapcsolók], [szolgáltatásnév], [APINÉV]**

Ez a parancs a megadott mobilszolgáltatás egyéni API frissíti.

Ez a parancs támogatja a következő további lehetőséget:

Ez a parancs támogatja a következő további lehetőségeket:

+ **-p** vagy **– engedélyek** &lt;engedélyek >: vesszővel tagolt listáját &lt;módszer > =&lt;jogosultsági > párokká.
+ **az f -** vagy **– kényszerítése**: felülíró egyéni módosításokat az engedélyek metaadat-fájlt.

**mobil api [kapcsolók], [szolgáltatásnév], [APINÉV] törlése**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Ez a parancs a megadott mobilszolgáltatás egyéni API törli.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>A mobilalkalmazás alkalmazás beállításainak kezelésére szolgáló parancsok

**mobil appsetting lista [kapcsolók], [szolgáltatásnév]**

Ez a parancs a megadott szolgáltatás mobilalkalmazás alkalmazás beállításainak megjelenítése

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**mobil appsetting hozzáadása [kapcsolók], [szolgáltatásnév], [név], [érték]**

Ez a parancs hozzáadása egy egyéni alkalmazás beállítása a mobilszolgáltatás.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**mobil appsetting [kapcsolók], [szolgáltatásnév], [név] törlése**

Ez a parancs eltávolítja a megadott alkalmazás beállítása a mobilszolgáltatás.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**mobil appsetting megjelenítése [kapcsolók], [szolgáltatásnév], [név]**

Ez a parancs eltávolítja a megadott alkalmazás beállítása a mobilszolgáltatás.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Eszköz helyi beállításainak kezelése

Helyi beállítások a következők: az előfizetés azonosítója és alapértelmezett tároló fiók nevére.

**beállítások listája [kapcsolók]**

Ez a parancs megjeleníti a config beállításait.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**config beállítása [kapcsolók] &lt;neve&gt;,&lt;érték&gt;**

Ez a parancs módosítja a config.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Szolgáltatás Bus kezelésére szolgáló parancsok

Ezek a parancsok használatával szolgáltatás Bus fiók kezelése

**SB névtér jelölőnégyzet [kapcsolók] &lt;neve >**

Ellenőrizze, hogy a szolgáltatás bus névtere jogi és érhető el.

**SB névtér létrehozása &lt;neve > &lt;helye >**

Létrehoz egy szolgáltatás Bus névtér.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**SB névtér törlése &lt;neve >**

Távolítsa el a névtér.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**SB névtér lista**

A fiók által létrehozott minden névtér sorolja fel.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**SB névtér helye lista**

Az összes rendelkezésre álló névtér helyek listájának megjelenítéséhez.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**SB névtér megjelenítése &lt;neve >**

Egy adott névtér adatainak megjelenítése

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**SB névtér ellenőrzése &lt;neve >**

Ellenőrizze, hogy a névtér érhető el.

## <a name="commands-to-manage-your-storage-objects"></a>A tároló objektumok kezelésére szolgáló parancsok

###<a name="commands-to-manage-your-storage-accounts"></a>A tároló fiókok kezelésére szolgáló parancsok

**tárterület Ügyféllista [kapcsolók]**

Ez a parancs a tárhely partnereket a előfizetés jeleníti meg.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**tárterület-fiók megjelenítése [kapcsolók]<name>**

Ez a parancs a megadott tárterület-fiókkal, beleértve a URI és a fiók tulajdonságok információkat jelenít meg.

**tárterület-fiók létrehozása [kapcsolók]<name>**

Ez a parancs létrehoz egy tárterület-fiókot, a megadott beállítások alapján.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-e** vagy **– címke** &lt;címke >: a tárterület-fiókom a címke.
+ **-d** vagy **– leírást** &lt;leírás >: A leírás tárterület-fiókot.
+ **-l** vagy **– hely** &lt;neve >: A földrajzi területhez tartozik, amelyben a tárterület-fiók létrehozása.
+ **-a** vagy **– Affinitás csoport** &lt;neve >: A affinitás csoport, amellyel a tárterület-fiók társítani szeretné. 
+ **– típus**: azt jelzi, milyen típusú fiókot létrehozni: bármelyik szabványos tároló redundancia lehetőséget (LRS/ZRS/GRS/RAGRS) vagy a prémium tároló (PLRS).

**tárterület-fiók beállítása [kapcsolók]<name>**

Ez a parancs frissíti a megadott tárterület-fiókot.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Ez a parancs támogatja a következő további lehetőségeket:

+ **-e** vagy **– címke** &lt;címke >: a tárterület-fiókom a címke.
+ **-d** vagy **– leírást** &lt;leírás >: A leírás tárterület-fiókot.
+ **-l** vagy **– hely** &lt;neve >: A földrajzi területhez tartozik, amelyben a tárterület-fiók létrehozása.
+ **– típusa**: azt jelzi, az új fiók típusát: bármelyik szabványos tároló redundancia lehetőséget (LRS/ZRS/GRS/RAGRS) vagy a prémium tároló (PLRS).

**tárterület-fiók törlése [kapcsolók]<name>**

Ez a parancs a megadott tárterület fiók törlése

Ez a parancs támogatja a következő további lehetőséget:

**– kérdések** és **– Csendes**: megerősítést kér. Az automatikus parancsfájlok használja ezt a beállítást.

###<a name="commands-to-manage-your-storage-account-keys"></a>A tároló fiók billentyűk kezelésére szolgáló parancsok

**tárterület-fiók billentyűk lista [kapcsolók]<name>**

Ez a parancs megjeleníti az elsődleges és másodlagos kulcsok a megadott tárterület-fiókom.

**tárterület-fiók kulcsok megújítása [kapcsolók]<name>**

###<a name="commands-to-manage-your-storage-container"></a>A tároló tároló kezelésére szolgáló parancsok

**tárterület tároló lista [kapcsolók], [előtag]**

Ez a parancs megjeleníti tároló tároló megadott tárterület fiókot. A tárhely a kapcsolati karakterlánc vagy a tárhely nevét és a fiók fiókkulcs megadva.

Ez a parancs támogatja a következő további lehetőségeket:

+ **-p** vagy **-előtag** &lt;előtag >: A tárterület a tároló neve előtagot.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: a tárterület parancs futtatása hibakeresési módban.

**tárterület tároló show [kapcsolók], [tároló]**
**tároló tároló létrehozása [kapcsolók], [tároló]**

Ez a parancs a megadott tárterület-fiókom tárolási tároló hoz létre. A tárhely a kapcsolati karakterlánc vagy a tárhely nevét és a fiók fiókkulcs megadva.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-p** vagy **-előtag** &lt;előtag >: A tárterület tároló neve előtagot.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók neve
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület fiókkulcs
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterlánc
+ **– hibakeresési**: a tárterület parancs futtatása hibakeresési módban.

**tárterület tároló törlés [kapcsolók], [tároló]**

Ez a parancs a megadott tárterület tároló törli. A tárhely a kapcsolati karakterlánc vagy a tárhely nevét és a fiók fiókkulcs megadva.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület-tároló létrehozása nevét.
+ **-p** vagy **-előtag** &lt;előtag >: A tárterület tároló neve előtagot.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: a tárterület parancs futtatása hibakeresési módban.

**tárterület-tároló beállítása [kapcsolók], [tároló]**

Ez a parancs a hozzáférés-vezérlési lista a tárterület-tároló állítja be. A tárhely a kapcsolati karakterlánc vagy a tárhely nevét és a fiók fiókkulcs megadva.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-p** vagy **-előtag** &lt;előtag >: A tárterület tároló neve előtagot.
+ **-a** vagy **– fióknevet** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: a tárterület parancs futtatása hibakeresési módban.

###<a name="commands-to-manage-your-storage-blob"></a>A tároló blob kezelésére szolgáló parancsok

**tárterület blob lista [kapcsolók], [tároló], [előtag]**

Ez a parancs a tárhely BLOB listáját a megadott tárterület tároló adja eredményül.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-p** vagy **-előtag** &lt;előtag >: A tárterület tároló neve előtagot.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: a tárterület parancs futtatása hibakeresési módban.

**tárterület blob megjelenítése [kapcsolók], [tároló], [blob]**

Ez a parancs megjeleníti a megadott tárterület blob részleteit.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-p** vagy **-előtag** &lt;előtag >: A tárterület a tároló neve előtagot.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: hibakeresési fut a tárhely parancsot.

**tárterület blob [kapcsolók], [tároló], [blob] törlése**

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-b** vagy **– blob** &lt;blobName >: a tárterület blob törlése nevét.
+ **– kérdések** és **– Csendes**: távolítsa el a megadott tároló blob megerősítés nélkül.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: hibakeresési fut a tárhely parancsot.

**tárterület blob feltöltés [kapcsolók], [fájl], [tároló], [blob]**

Ez a parancs a megadott fájl feltöltése a specified\ tároló blob.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-b** vagy **– blob** &lt;blobName >: töltse fel a tárhely blob nevét.
+ **-t** vagy **– blobtype** &lt;blobtype >: A tárterület blob típusát: lap vagy blokk.
+ **-p** vagy **– Tulajdonságok** &lt;Tulajdonságok >: feltöltött fájlt tároló blob tulajdonságait. Tulajdonságok kulcsfontosságúak érték pár s = és semicolon(;) elválasztva. Elérhető tulajdonságok contentType, contentEncoding, contentLanguage és cacheControl.
+ **-m** vagy a **metaadatok –** &lt;metaadatok >: feltöltött fájlt tároló blob metaadatait. Metaadat-alapú kulcsfontosságúak = érték párokká-d pontosvesszőt (;) elválasztva egymástól.
+ **– concurrenttaskcount** &lt;concurrenttaskcount >: feltöltés egyidejű kérelmek maximális számát.
+ **– kérdések** és **– Csendes**: felülírja a megadott tároló blob megerősítés nélkül.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: hibakeresési fut a tárhely parancsot.

**tárterület blob letöltési [kapcsolók], [tároló], [blob], [cél]**

Ez a parancs a megadott tárterület blob letöltését.

Ez a parancs támogatja a következő további lehetőségeket:

+ **– tároló** &lt;tároló >: a tárterület tároló létrehozása nevét.
+ **-b** vagy **– blob** &lt;blobName >: A tárterület blob nevét.
+ **-d** vagy **– cél** [cél]: A letöltés cél fájl vagy könyvtár elérési útja.
+ **-m** vagy **– checkmd5**: A jelölőnégyzet md5sum a letöltött fájl.
+ **– concurrenttaskcount** &lt;concurrenttaskcount > feltöltés egyidejű kérelmek maximális száma
+ **– kérdések** és **– Csendes**: felülírja a célfájl megerősítés nélkül.
+ **-a** vagy **– fióknév** &lt;fióknév >: A tárterület-fiók nevére.
+ **-k** vagy **– fiókkulcs** &lt;accountKey >: A tárterület-fiók billentyűt.
+ **-c** vagy **– kapcsolat-karakterlánc** &lt;connectionString >: A tárterület-kapcsolati karakterláncot.
+ **– hibakeresési**: hibakeresési fut a tárhely parancsot.

## <a name="commands-to-manage-sql-databases"></a>SQL-adatbázisait kezelésére szolgáló parancsok

Ezek a parancsok használata az Azure SQL-adatbázisok kezelése

###<a name="commands-to-manage-sql-servers"></a>SQL-kiszolgálók kezelése parancsot.

Ezek a parancsok használata az SQL-kiszolgálókon kezelése

**az SQL server létrehozása &lt;administratorLogin > &lt;administratorPassword > &lt;helye >**

Hozzon létre egy adatbázis-kiszolgáló

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**az SQL server megjelenítése &lt;neve >**

Kiszolgáló részletek megjelenítéséhez.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**az SQL server-lista**

A GET-kiszolgálók listája.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**az SQL server Törlés &lt;neve >**

A kiszolgáló törlése

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>SQL-adatbázisait kezelésére szolgáló parancsok

Ezek a parancsok használatával kezelheti a SQL-adatbázisait.

**SQL-adatbázis létrehozása [kapcsolók] &lt;kiszolgálónév > &lt;adatbázisnév > &lt;administratorPassword >**

Létrehoz egy adatbázis-példány

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL-adatbázis megjelenítése [kapcsolók] &lt;kiszolgálónév > &lt;adatbázisnév > &lt;administratorPassword >**

Adatbázis részletek megjelenítéséhez.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**SQL-adatbázis lista [kapcsolók] &lt;kiszolgálónév > &lt;administratorPassword >**

Az adatbázisok listában.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL-adatbázis törlése [kapcsolók] &lt;kiszolgálónév > &lt;adatbázisnév > &lt;administratorPassword >**

Adatbázis törlése

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Az SQL Server-tűzfal szabályok kezelésére szolgáló parancsok

Ezek a parancsok használata az SQL Server-tűzfal szabályok kezelése

**SQL-firewallrule létrehozása [kapcsolók] &lt;kiszolgálónév > &lt;szabálynév > &lt;startIPAddress > &lt;endIPAddress >**

Az SQL Server tűzfal szabály létrehozása.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL-firewallrule megjelenítése [kapcsolók] &lt;kiszolgálónév > &lt;szabálynév >**

Tűzfal szabály részleteinek megjelenítése.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**SQL-firewallrule lista [kapcsolók] &lt;kiszolgálónév >**

A tűzfalszabályokat sorolja fel.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL-firewallrule törlése [kapcsolók] &lt;kiszolgálónév > &lt;szabálynév >**

Ez a parancs a tűzfal szabályok törlése

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>A virtuális hálózatok kezelésére szolgáló parancsok

Ezek a parancsok használatával kezelheti a virtuális hálózatok

**hálózati vnet létrehozása [kapcsolók] &lt;helye >**

Hozzon létre egy virtuális hálózaton.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**a hálózati vnet megjelenítése &lt;neve >**

A virtuális hálózat részlet megjelenítése parancsra.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**vnet hálózatlistához**

A lista minden meglévő virtuális hálózatok.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**hálózati vnet törlése &lt;neve >**

A megadott virtuális hálózati törli.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**hálózati exportálás [útvonal]**

Speciális hálózati konfigurációja a hálózati konfigurálásról helyileg exportálhatja. Az exportált hálózati beállítások DNS-kiszolgáló beállításai, a virtuális hálózat beállításainak, a helyi hálózaton webhely beállításai és a más beállításokat tartalmazza.

**hálózati importálása [útvonal]**

A helyi hálózaton konfiguráció importálása.

**hálózati DNS_kiszolgáló regisztrálni [kapcsolók] &lt;dnsIP >**

A DNS-kiszolgáló, amely a hálózati konfigurálásról névfeloldás a használni kívánt regisztrálni.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**DNS_kiszolgáló hálózatlistához**

A hálózati konfigurálásról regisztrált DNS-kiszolgálók sorolja fel.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**hálózati DNS_kiszolgáló unregister [kapcsolók] &lt;dnsIP >**

A hálózati konfigurálásról eltávolítja a DNS-kiszolgáló bejegyzés.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

