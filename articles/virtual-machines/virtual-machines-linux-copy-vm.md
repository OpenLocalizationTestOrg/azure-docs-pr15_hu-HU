<properties
    pageTitle="Másolatot készíteni az Azure Linux virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként másolatot szeretne készíteni a Azure Linux virtuális gép az erőforrás-kezelő telepítési modell"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Azure futó Linux virtuális gép másolatának létrehozása


Ez a cikk bemutatja, hogyan az Azure virtuális gép (virtuális) használ, az erőforrás-kezelő telepítési modell Linux operációs rendszert futtató másolatot szeretne készíteni. Először egy új tároló keresztül az operációs rendszer és az adatok lemezre másolása, majd állítsa be a hálózati erőforrásokat, majd az új virtuális gép létrehozása.

Is [tölthet fel, és hozzon létre egy egyéni lemez kép virtuális](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Első lépések

Győződjön meg arról, hogy teljesülnek az alábbi előfeltételek, a lépések megkezdése előtt:

- Ha a [Azure CLI] (... / xplat-cli-install.md) letölti és telepíti a számítógépre. 

- Akkor is a meglévő Azure Linux virtuális néhány adatot kell:

| Adatforrás-információk virtuális | Hol találja meg |
|------------|-----------------|
| Virtuális neve | `azure vm list` |
| Erőforráscsoport neve | `azure vm list` |
| Hely | `azure vm list` |
| Tárterület-fiók neve | `azure storage account list -g <resourceGroup>` |
| Tároló neve | `azure storage container list -a <sourcestorageaccountname>` |
| Forrásfájl virtuális virtuális neve | `azure storage blob list --container <containerName>` |



- Szüksége lesz, hogy az új virtuális kapcsolatos lehetőség közül választhat:   <br> -Tároló neve   <br> Virtuális - neve   <br> Virtuális - méret   <br> -vNet neve   <br> -Alhálózat neve   <br> -IP neve   <br> Hálózati kártya – neve
    

## <a name="login-and-set-your-subscription"></a>Bejelentkezés és az előfizetés beállítása

1. Jelentkezzen be a CLI.
        
        azure login

2. Győződjön meg arról, hogy az erőforrás-kezelő módban van.
    
        azure config mode arm

3. A helyes előfizetés beállítása. "Azure-fiók list" is használhatja az összes az előfizetések megjelenítéséhez.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>A virtuális leállítása 

Állítsa le, és a forrás virtuális felszabadítani. "Azure virtuális list" is használhatja az összes a VMs az előfizetését, és az erőforrás veheti a csoport nevét.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>A virtuális másolása


Másolhatja a virtuális a forrás-tárhelyről a cél használatával a `azure storage blob copy start`. Ebben a példában fogjuk másolja a vágólapra a virtuális ugyanazzal a tárterület-fiókkal, de a másik tároló.

A virtuális másolása egy másik tároló ugyanazzal a tárterület-fiókkal, írja be:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Az új virtuális a virtuális hálózat beállítása

Állítsa be a virtuális hálózati és a hálózati kártya az új virtuális. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Az új virtuális létrehozása 

Most már létrehozhat egy virtuális révén a CLI vagy az [erőforrás-kezelő sablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) feltöltött pillanatkép írja be a másolt lemezre az URI megadásával:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Következő lépések

Azure CLI használatáról virtuális új számítógépre kezelése című témakörben talál [Azure CLI parancsok az Azure erőforrás parancsra](azure-cli-arm-commands.md).
