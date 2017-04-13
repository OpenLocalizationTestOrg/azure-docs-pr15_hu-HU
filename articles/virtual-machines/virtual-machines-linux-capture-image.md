<properties
    pageTitle="Rögzíthet egy Linux virtuális sablonként használandó |} Microsoft Azure"
    description="Megtudhatja, hogy miként rögzítheti és generalize Linux-alapú Azure virtuális gép (virtuális) létrehozása az Azure erőforrás-kezelő telepítési modell a képet."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Azure futó Linux virtuális gép rögzítése

Kövesse a jelen cikkben generalize, és rögzítse a Azure Linux virtuális gép (virtuális) az erőforrás-kezelő telepítési modell. A virtuális általánosításakor, távolítsa el a személyes fiók adatait, és felkészülés a virtuális képként használandó. Majd készítsen egy általános virtuális merevlemez (virtuális) képet az operációs rendszer, a csatolt adatok lemezt VHD és [erőforrás-kezelő sablon](../azure-resource-manager/resource-group-overview.md) új virtuális telepítésekhez. 

A kép felhasználásával VMs létrehozásához minden új virtuális hálózati erőforrások beállítása és üzembe rögzített virtuális képének a sablon (JavaScript Objektumjelrendszer vagy JSON, fájl) használatával. Ezzel a módszerrel, hogy bizonyos egy virtuális az aktuális szoftver konfigurációval hasonlóan képek használata a Microsoft Azure piactéren.

>[AZURE.TIP]A meglévő Linux virtuális másolatának létrehozását a biztonsági másolat és hibakeresés speciális állapotába, című témakörben olvashat [Linux futó Azure virtuális gép másolatának létrehozása](virtual-machines-linux-copy-vm.md). És ha a Linux virtuális egy helyszíni virtuális a feltölteni kívánt, olvassa el a [tölthet fel, és hozzon létre egy Linux virtuális egyéni lemezre kép](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Első lépések

Győződjön meg arról, hogy teljesülnek az alábbi előfeltételek:

* **Az erőforrás-kezelő telepítési modell készült azure virtuális** – Ha még nem hozott létre a Linux virtuális, használhatja a [portálon](virtual-machines-linux-quick-create-portal.md), az [Azure CLI](virtual-machines-linux-quick-create-cli.md)vagy az [erőforrás-kezelő sablonok](virtual-machines-linux-cli-deploy-templates.md). 

    Állítsa be a virtuális, szükség szerint. Például [hozzáadása adatok lemezt](virtual-machines-linux-add-disk.md),-frissítések telepítése, és -alkalmazások telepítése. 
* **Azure CLI** - telepítse az [Azure CLI](../xplat-cli-install.md) helyi számítógépen.

## <a name="step-1-remove-the-azure-linux-agent"></a>Lépés: 1: Azure Linux ügynök eltávolítása

Először futtassa a Linux virtuális **deprovision** paraméter a **waagent** parancsot. Ez a parancs a fájljait és adatait, hogy készen áll a generalizing a virtuális törli. Részletekért olvassa el az [Azure Linux ügynök útmutatója](virtual-machines-linux-agent-user-guide.md).

1. Csatlakozás a Linux virtuális egy SSH ügyfélprogramban.

2. A SSH ablakban írja be a következő parancsot:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Ez a parancs csak egy virtuális, amelyeket be szeretne rögzíteni képként futtatható. Nem garantálja, hogy a kép összes bizalmas információkat nincs bejelölve, vagy alkalmas további terjesztés céljából.

3. Írja be az **y** továbbra is. Hozzáadhat a **-kényszerítése** paraméter elkerülése érdekében a megerősítést kérő ezt a lépést.

4. A parancs befejeződése után írja be a **Kilépés a**. Ebben a lépésben a SSH ügyfél bezárása.

    
## <a name="step-2-capture-the-vm"></a>Lépés: 2: A virtuális rögzítése

Az Azure CLI segítségével generalize, és rögzítse a virtuális. Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméternevek **myResourceGroup** **myVnet**és **myVM**tartalmazza.

5. A helyi számítógépen nyissa meg az Azure CLI és a [bejelentkezési Azure-előfizetéséhez](../xplat-cli-connect.md). 

6. Győződjön meg arról, hogy az erőforrás-kezelő módban van.

    ```
    azure config mode arm
    ```

7. Állítsa le a virtuális, amely már leépítve a következő parancs használatával:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Az alábbi paranccsal a virtuális generalize:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Most futtassa az **azure virtuális rögzítése** parancs, amely a virtuális rögzíti. A következő példában a kép VHD rögzítésének **MyVHDNamePrefix**kezdődő neveket, és a **-t** beállítás, adja meg a sablon **MyTemplate.json**elérési útját. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]A virtuális képfájlokat a eredeti virtuális használt azonos tárolási fiók alapértelmezés szerint létrejönnek. A *tároló ugyanazzal a fiókkal* segítségével tárolása a VHD minden új VMs hoz létre a képet. 

6. Keresse meg a rögzített kép helyét, nyissa meg a JSON-sablon szövegszerkesztőben. A **storageProfile**keresse meg a **képet** , a **rendszer** tárolóban található a **uri** . Ha például az operációs rendszer lemez kép URI hasonlít`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>3 lépés: A rögzített képet egy virtuális létrehozása
Most már az sablonnal a kép segítségével létrehozhat egy Linux virtuális. Ezeket a lépéseket az Azure CLI és a JSON fájlsablon, a virtuális létrehozása az új virtuális hálózat rögzített használatával megjelenítése.

### <a name="create-network-resources"></a>Hálózati erőforrások létrehozása

A sablon használatához, először be kell állítania egy virtuális hálózati és a hálózati kártya az új virtuális. Azt javasoljuk, hogy ezek az erőforrások erőforráscsoport a virtuális kép tárolási helye a hozzon létre. Parancsok futtatása a következő, azaz helyettesíti be nevet az erőforrások és a megfelelő Azure helyet (ezek a parancsok a "centralus" jelöli) hasonló:

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Ismerkedés a hálózati kártya azonosítója

A kép egy virtuális a rögzítés közben mentett JSON telepítéséhez szükséges a adaptert azonosítója Szerezze be azt a következő parancs futtatásával:

    azure network nic show MyResourceGroup1 myNIC

Az **azonosító** kimeneti hasonlít`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Hozzon létre egy virtuális
Ekkor a következő parancsot a virtuális készítése a rögzített virtuális képet. Az **-f** paraméter használatával adja meg a mentett sablont JSON fájl elérési útját.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

A parancs a kéri kiszolgálóneveket virtuális új nevet, a rendszergazdai felhasználónevével és a jelszó és a korábban létrehozott hálózati azonosítója.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

A következő példában látható sikeres telepítés:

    + Sablon beállításokat és a paraméterek inicializálása
    + A telepítési információ létrehozása: létrehozott sablont telepítési xxxxxxx
    + Várakozás a telepítés befejezéséhez adatok: DeploymentName: MyDeployment adatok: ResourceGroupName: MyResourceGroup1 adatok: ProvisioningState: adatok sikeres: időbélyeg: xxxxxxx adatok: mód: növekményes adatok: név típusú érték


    data:    ------------------  ------------  -------------------------------------

    adatok: vmName karakterlánc myNewVM


    adatok: vmSize karakterlánc Standard_D1


    adatok: adminUserName karakterlánc myAdminuser


    adatok: SecureString meghatározatlan adminPassword


    adatok: networkInterfaceId karakterlánc /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic információ: csoport telepítési OK létrehozása parancs

### <a name="verify-the-deployment"></a>A telepítési ellenőrzése

Most már SSH a virtuális géphez, létrehozott, ellenőrizze a telepítési és az új virtuális használatának megkezdése. Csatlakozás SSH keresztül, keresse meg a létrehozott, a következő parancs futtatásával virtuális IP-címe:

    azure network public-ip show MyResourceGroup1 myIP

A nyilvános IP-cím szerepel a parancs. Alapértelmezés szerint csatlakozik a Linux virtuális port 22 SSH szerint.

## <a name="create-additional-vms"></a>További VMs létrehozása
Használatával a rögzített képet, és a sablonok használata a lépéseket az előző szakaszban további VMs. Egyéb lehetőségek VMs létrehozása a kép közé tartoznak, quickstart útmutató sablon használatával, és futtassa az **azure virtuális létrehozása** parancsot.

### <a name="use-the-captured-template"></a>A rögzített sablon használata

A rögzített képet, és a sablon használatához kövesse az alábbi lépéseket, (az előző szakaszban részletes):

* Győződjön meg arról, hogy a virtuális kép ugyanazt a virtuális virtuális üzemeltető tároló fiók.
* A JSON sablonfájlt másolva, és adjon egy egyedi nevet az új virtuális virtuális (vagy VHD) operációs rendszer lemez. Például a **storageProfile**, a **virtuális**az **uri**, adja meg egy egyedi nevet a virtuális Merevlemezt, hasonlít **osDisk**`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Hozzon létre egy hálózati kártya ugyanabban vagy a virtuális másik hálózathoz.
* A módosított JSON sablonfájl használatával hozhat létre egy telepítési az erőforráscsoport, amelyen beállította a virtuális hálózat.

### <a name="use-a-quickstart-template"></a>Quickstart útmutató sablon használata

Ha azt szeretné, hogy a hálózat beállítása a automatikusan Ha hoz létre egy virtuális a képet, megadhatja azokat az erőforrásokat sablonba. A GitHub [101 virtuális-a-felhasználó-kép sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) például látható. Ezt a sablont a saját kép és a szükséges virtuális hálózati, a nyilvános IP-cím és a hálózati erőforrások egy virtuális hoz létre. A sablonnal az Azure-portálon ismertetését megtalálja megtudhatja, [hogy miként hozhat létre virtuális gép az erőforrás-kezelő sablon használatával egyéni kép](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Használja a azure virtuális létrehozása parancs

Általában legegyszerűbb hozzon létre egy virtuális a kép egy erőforrás-kezelő sablonnal. Azonban hozhat létre a virtuális _imperatively_ **– Kérdések** és az **azure virtuális létrehozása** parancs használatával (**– kép-urn**) paraméter. Ha ezt a módszert is átadhatja a **kétdimenziós** (**--os-merevlemez-virtuális**) paraméterrel adja meg az új virtuális az operációs rendszer .vhd fájl helyét. Ez a fájl a tárterület-fiókot, a kép virtuális fájlt tároló VHD tárolót kell lennie. A parancs másolja a virtuális az új virtuális automatikusan a **VHD** tároló.

Mielőtt operációs rendszert futtató **azure virtuális létrehozása** a képpel, végezze el az alábbi lépéseket:

1.  Hozzon létre egy erőforrás csoportot, vagy azonosítsa a telepítéshez meglévő erőforráscsoport.

2.  Hozzon létre egy nyilvános IP-cím erőforrás és a hálózati erőforrás az új virtuális. Megtudhatja, hogy egy virtuális hálózat, a nyilvános IP-cím és a hálózati kártya létrehozása a CLI használatával lásd: Ez a cikk korábbi részében. (**azure virtuális hozzon létre** egy hálózati kártya is létrehozhat, de további paraméterekkel virtuális hálózati és alhálózat szükséges.)


Futtassa az új OS virtuális fájl és a meglévő képet is URL-címe átadja parancs. Ebben a példában a méretet Standard_A1 virtuális jön létre a kelet-Amerikai Egyesült Államok területhez tartozik.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

A lehetőségekről további parancs futtatása `azure help vm create`.

## <a name="next-steps"></a>Következő lépések

A VMs együtt a CLI című témakörben a tevékenységek [Deploy és Azure erőforrás-kezelő sablonok és az Azure CLI használatával kezelheti a virtuális gépeken futó](virtual-machines-linux-cli-deploy-templates.md).
