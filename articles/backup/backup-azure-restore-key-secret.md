<properties
    pageTitle="Azure biztonsági mentéssel titkosított VMs kulcs kulcs tárolóból elemre, és a titkos kulcs visszaállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként állíthatja helyre a kulcs tárolóra billentyűt és titkos Azure biztonsági másolat PowerShell használatával"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Kulcs kulcs tárolóból elemre, és a titkos kulcs visszaállítása titkosított VMs Azure biztonsági másolat használata
Ez a cikk folytatott beszélgetést használatáról Azure virtuális biztonsági másolat a titkosított Azure VMs visszaállítása végrehajtásához, ha a billentyű és a titkos nem léteznek a fő tárolóból elemre. Ezeket a lépéseket is használható, ha meg szeretné őrizni a kulcs (fő titkosítási kulcs) és a titkos (BitLocker titkosítási kulcs) külön másolatát a visszaállított virtuális.

## <a name="pre-requisites"></a>Előzetes követelmények

1. **Biztonsági másolat titkosított VMs** - titkosított Azure VMs mentett Azure biztonsági másolat használata. Olvassa el a cikk [biztonsági mentése és visszaállítása a PowerShell használatá Azure VMs kezelése](backup-azure-vms-automation.md) biztonsági másolatot készíteni a titkosított Azure VMs olvashat.

2. Már **Azure kulcs tárolóra konfigurálása** – ellenőrizze, hogy a kulcsfontosságú tárolóból elemre, amelyhez kulcsok és titkos kulcsok kell visszaállíthatók. Fő tárolóra kezelésével kapcsolatos részletekért, olvassa el a cikk [Első lépések az Azure kulcs tárolóból elemre](../key-vault/key-vault-get-started.md) .

## <a name="setup-recovery-services-vault"></a>A telepítő helyreállítási szolgáltatások tárolóból elemre 
Az alábbi lépésekkel használatával jelentkezzen be a PowerShell és helyreállítási szolgáltatások tárolóra környezet beállítása

### <a name="log-in-to-azure-powershell"></a>Bejelentkezés az Azure PowerShell 

Jelentkezzen be az alábbi parancsmag használatával Azure-fiók

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Set helyreállítási szolgáltatások tárolóra környezetben

Miután bejelentkezett, használja a következő parancsmagot a listát a rendelkezésre álló előfizetések

```
PS C:\> Get-AzureRmSubscription
```

Jelölje ki azt az előfizetést, amelyben erőforrások érhetők el

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

A tárolóból elemre környezet használatával helyreállítási szolgáltatások tárolóból elemre, amelyben engedélyezett a biztonsági másolat volt az titkosított VMs beállítása

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Ismerkedés a helyreállítási pont 

Jelölje ki a tárolóból elemre, amely a titkosított Azure virtuális gép tároló

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Ez a tároló használ, térjen vissza a megfelelő virtuális gép elem mentése

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Első tömb helyreállítási pontok az a változó rp biztonsági elemhez

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Titkosított virtuális gép visszaállítása
Kulcs, és a titkos visszaállítása a titkosított virtuális, kövesse az alábbi lépéseket.

### <a name="restore-key"></a>Kulcs visszaállítása

A tömb $rp fenti, az index 0 legújabb helyreállítási pontnak idő fordított sorrendben van rendezve. Például: [0] $rp kijelöli a legújabb helyreállítási pont.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Ezzel a parancsmaggal sikeresen futtatása után kap keletkezett blob-fájl a megadott mappába a hol fut a számítógépen. A blob-fájl a titkosított formában fő titkosított kulcs jelöli.

A fő tárolóra, az alábbi parancsmag használatával visszaállíthatja az kulcs vissza. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Titkos visszaállítása

Titkos adatok helyreállítás helyétől fentiekben kapott visszaállítása

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
A szöveg előtt vault.azure.net eredeti fő tárolóra nevét jelenti. Titkos kulcsok után a szöveg és a titkos nevét. 

Letölthető titkos neve és értéke a fenti futtatása parancsmag kimenete abban az esetben, ha a titkos néven használni kívánt. Egyéb esetben az alábbi $secretname módosuljon az új titkos nevét. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

A titkos kulcs címkéinek beállítása a abban az esetben, ha virtuális kell, valamint visszaállíthatók. A címke DiskEncryptionKeyFileName érték a használni kívánt titkos nevét kell tartalmaznia. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
DiskEncryptionKeyFileName értéke megegyezik a fent titkos névből. DiskEncryptionKeyEncryptionKeyURL értékét szerezhető be a fő tárolóból elemre a billentyűk vissza visszaállítása, és a [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) parancsmaggal után   

A fő tárolóra titkos hátsó beállítása

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Virtuális gép visszaállítása
A fenti PowerShell-parancsmagok segít visszaállításához billentyű és a titkos vissza a fő tárolóból elemre, ha készített biztonsági másolatot a titkosított virtuális Azure virtuális biztonsági másolat használata. Miután visszaállításáról, olvassa el a cikk a titkosított VMs visszaállítása [biztonsági mentése és visszaállítása a PowerShell használatá Azure VMs kezelése](backup-azure-vms-automation.md) .
