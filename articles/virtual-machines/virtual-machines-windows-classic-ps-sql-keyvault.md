<properties
    pageTitle="Az SQL Server Azure kulcs tárolóból elemre Integration tartományi Azure VMs (klasszikus)"
    description="Megtudhatja, hogy miként automatizálhatja a beállításait az SQL Server-titkosítás való használatának Azure kulcs tárolóból elemre. Ez a témakör ismerteti a klasszikus telepítési modell létrehozása virtuális gépeken futó SQL Server Azure kulcs tárolóra integrációs használatára."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Integráció Azure kulcs tárolóból elemre az SQL Server Azure VMs (klasszikus) a

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasszikus](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>– Áttekintés
Vannak olyan több SQL Server titkosítási funkciókat, például [átlátszó adatok titkosítási (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), a [oszlop szintű titkosítás (törlése)](https://msdn.microsoft.com/library/ms173744.aspx)és a [biztonsági másolat titkosítás](https://msdn.microsoft.com/library/dn449489.aspx). Ezek az űrlapok titkosítási szükség kezelheti és tárolni a cryptographic billentyűkkel titkosítást használ. Az Azure kulcs tárolóból elemre (AKV) szolgáltatás az biztonsági és a biztonságos és könnyen hozzáférhető helyen billentyűk kezelésére szolgál. Az [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) lehetővé teszi, hogy az SQL Server Azure kulcs tárolóból elemre a billentyűk használatához.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

A helyszíni gépeken futó SQL Server rendszert futtat, esetén [lépéssel elérése a számítógépről a helyszíni SQL Server Azure kulcs tárolóból elemre](https://msdn.microsoft.com/library/dn198405.aspx). De az SQL Server Azure VMs, időt takaríthat az *Azure kulcs tárolóra integrációs* funkció használatával. Néhány Azure PowerShell-parancsmagok ahhoz, hogy ez a funkció, a automatizálhatja a konfiguráció szükséges egy SQL virtuális elérése a fő tárolóból elemre.

Ezt a szolgáltatást, ha azt automatikusan telepíti az SQL Server-összekötő, konfigurálja a EKM szolgáltató eléréséhez Azure kulcs tárolóból elemre, és létrehoz lehetővé teszi a tárolóból elemre eléréséhez a hitelesítő adatok. Ha korábban már említettük helyszíni dokumentációjában leírt lépéseket nézett, megtekintheti, hogy ez a funkció automatizálja a lépéseket, 2 és 3. Manuálisan is kimutatásadatokat beállítást, ha a fő tárolóból elemre, és a billentyűk. Itt a teljes beállítása a SQL virtuális automatizált. Befejeződése ezt a szolgáltatást a telepítés hajthat végre T-SQL-utasítások kezdődjön a adatbázisokban vagy a biztonsági mentés titkosítja a szokásos módon.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV-integráció
Azure kulcs tárolóra integráció a PowerShell használatával. A következő szakaszokban a szükséges paramétereket és PowerShell mintaparancsfájl áttekintése.

### <a name="install-the-sql-server-iaas-extension"></a>Az SQL Server IaaS bővítmény telepítése

Első, [az SQL Server IaaS bővítmény telepítése](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>A bemeneti paramétereket ismertetése
Az alábbi táblázat a paramétereket, a következő szakaszban az PowerShell parancsprogramot futtatásához szükséges.

|Paraméter|Leírás|Példa|
|---|---|---|
|**$akvURL**|**A fő tárolóra URL-címe**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Egyszerű szolgáltatás neve**|"5-4e11-af04eb07b669ccf2 fde2b411 - 33d"|
|**$spSecret**|**Egyszerű titkos kulcs**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Hitelesítő adatok neve**: AKV integráció a hitelesítő adatok SQL Server lehetővé teszi a virtuális hozzáférjen a fő tárolóra alkalmazásban hoz létre. Válasszon egy nevet a hitelesítő adatok.|"mycred1"|
|**$vmName**|**Virtuális számítógépnév**: egy korábban létrehozott SQL virtuális nevét.|"myvmname."|
|**$serviceName**|**Szolgáltatás neve**: az SQL-virtuális társított a Felhőbeli szolgáltatástól nevét.|"mycloudservicename."|

### <a name="enable-akv-integration-with-powershell"></a>A PowerShell AKV alkalmazással való integráció engedélyezése
A **New-AzureVMSqlServerKeyVaultCredentialConfig** parancsmag objektumot hoz létre konfigurációs Azure kulcs tárolóból elemre Integration funkció. A **Set-AzureVMSqlServerExtension** integrálás konfigurálja a **KeyVaultCredentialSettings** paramétert. A következő lépések bemutatják, hogyan ezek a parancsok használatához.

1. Az Azure PowerShell először állítsa be a bemeneti paramétereket az egyedi értékeket tartalmazó, ez a témakör az előző szakaszokban ismertetett módon. A következő parancsfájl képen.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Ezután a következő parancsfájl segítségével konfigurálhatja és a AKV alkalmazással való integráció engedélyezése.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

Az SQL IaaS ügynök bővítmény frissíti a SQL virtuális ebben a konfigurációban.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
