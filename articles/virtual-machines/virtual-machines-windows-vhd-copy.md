<properties
    pageTitle="A speciális virtuális másolatának létrehozása az Azure-ban |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy speciális Windows virtuális fut az Azure, az erőforrás-kezelő telepítési modell másolatát."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>A speciális Windows virtuális Azure-ban futó másolatának létrehozása 

Ez a cikk bemutatja, hogyan a virtuális másolatának létrehozása egy speciális Windows virtuális Azure-ban futó a AZCopy eszközzel. A virtuális példányát használatával hozzon létre egy új virtuális majd. 

- Ha szeretne másolni egy általános virtuális, megtudhatja, [hogy miként hozhat létre egy meglévő általános Azure virtuális egy virtuális képet](virtual-machines-windows-capture-image.md).

- Ha egy virtuális egy helyszíni virtuális, például a Hyper-V, a [feltöltése a Windows virtuális az Azure szeretné egy helyszíni virtuális](virtual-machines-windows-upload-image.md)lásd a készült a feltölteni kívánt.


## <a name="before-you-begin"></a>Első lépések

Győződjön meg róla, hogy:

- A **forrás- és céltáblák tároló fiókok**adatait van. A forrás virtuális kell tárterület-fiók és tároló neveket. A tároló neve általában a **VHD**lesz. Meg kell cél tároló fiókkal rendelkezik. Ha még nincs egyet, létrehozhat egyet vagy a portálon (**További szolgáltatások** > tároló fiókok > Hozzáadás) vagy a [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) parancsmaggal. 

- Azure [PowerShell 1.0](../powershell-install-configure.md) van (vagy újabb) telepítve.

- Letöltött és a [AzCopy eszköz](../storage/storage-use-azcopy.md)telepített. 


## <a name="deallocate-the-vm"></a>A virtuális felszabadítása

A virtuális, amely szabadítja fel a másolni kívánt virtuális felszabadítani. 

- **Portálon**: kattintson a **virtuális gépeken futó** > **myVM** > leállítása
- **A PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` a virtuális **myVM** az erőforrás csoport **myResourceGroup**nevű felszabadítja.

Leállítva **(felszabadítása)** **Leállítva** módosítja a **állapota** a virtuális az Azure-portálon.


## <a name="get-the-storage-account-urls"></a>A tároló fiók URL-címek első

Az URL-címeit, a forrás- és céltáblák tároló számlák van szüksége. Az URL-címek néz: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Ha már tudja a tárterület-fiók és a tároló nevét, az adatok az URL-cím létrehozása a zárójelek között csak cserélheti. 

Ismerkedés az URL-címet használhatja az Azure portálja vagy az Azure Powershell:

- **Portálon**: kattintson a **További szolgáltatások** > **tároló fiókok**  >  <storage account> **BLOB** - és a forrásfájl virtuális valószínűleg a **VHD** tároló. A tároló kattintson a **Tulajdonságok** parancsra, és másolja a szöveget, címkézett **URL-CÍMÉT**. A forrás- és tárolók az URL-címe lesz szüksége. 

- **A PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` kap az információkat, a virtuális nevű **myVM** az erőforrás csoport **myResourceGroup**a. A találatok között keresse meg a **profilját tároló** szakaszban a **Virtuális Uri**. A Uri első része a tároló URL-CÍMÉT, és az utolsó részét a virtuális OS virtuális nevét.

## <a name="get-the-storage-access-keys"></a>A tároló hívóbetűk beszerzése

Keresse meg a hívóbetűk a forrás- és a tárterület-fiókok. Hívóbetűk kapcsolatos további tudnivalókért olvassa el a [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)című témakört.

- **Portálon**: kattintson a **További szolgáltatások** > **tároló fiókok**  >  <storage account> **Minden elérhető beállítás** > **hívóbetűk**. Másolja a vágólapra a billentyű felirata **azonosító1**.
- **A PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` az erőforrás csoport **myResourceGroup**megkapja a tárterület a tárterület-fiók **mystorageaccount** billentyűjét. Másolja a vágólapra a billentyű felirata **azonosító1**.


## <a name="copy-the-vhd"></a>A virtuális másolása 

Fájlok használata AzCopy tároló fiókok között másolhatja. Az a cél tároló Ha a megadott tároló nem létezik, azt létrejön meg. 

AzCopy, nyisson meg egy parancssort, a helyi számítógépre és nyissa meg azt a mappát, amelyen telepítve van a AzCopy. *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*hasonló lesz. 

Szeretné másolni az összes fájlt tároló belül, akkor használja a **/S** kapcsolót. Ez az operációs rendszer virtuális és az összes adat lemezt másolni, ha ugyanabban a tárolóban is használható. Ez a példa bemutatja, hogyan másolja az összes fájl a tárhely fiók **mysourcestorageaccount** tároló **mysourcecontainer** tároló **mydestinationcontainer** **mydestinationstorageaccount** tároló fiók. Cserélje a tárterület-fiókok és tárolók azoknak a saját. Csere `<sourceStorageAccountKey1>` és `<destinationStorageAccountKey1>` saját billentyűkkel.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Ha csak szeretne másolni egy adott virtuális tároló több fájlokkal, adja meg a fájl nevét a /Pattern váltás. Ebben a példában a csak a **myFileName.vhd** nevű fájlt program másolja.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Amikor elkészült, a következőhöz hasonló üzenet jelenik meg:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Hibaelhárítás

- Használatakor AZCopy, ha megjelenik a hiba "a kiszolgáló nem sikerült hitelesíteni a kérést. Győződjön meg arról, hogy engedélyezési fejléc értékének megfelelően többek között az aláírás lett létrehozva." és 2 billentyűt vagy a másodlagos tároló billentyűt, próbálkozzon az elsődleges vagy 1st tároló billentyűt.


## <a name="next-steps"></a>Következő lépések

- [A virtuális merevlemez-OS meghajtó legyen-e egy virtuális példányát](virtual-machines-windows-create-vm-specialized.md)csatolásával hozhat létre egy új virtuális.












