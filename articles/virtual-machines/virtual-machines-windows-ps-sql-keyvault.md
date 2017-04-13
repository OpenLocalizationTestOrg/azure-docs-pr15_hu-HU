<properties
    pageTitle="Integráció Azure kulcs tárolóból elemre az SQL Server Azure VMs (erőforrás-kezelő) a"
    description="Megtudhatja, hogy miként automatizálhatja a beállításait az SQL Server-titkosítás való használatának Azure kulcs tárolóból elemre. Ez a témakör ismerteti, hogyan Azure kulcs tárolóra integrációs használata SQL Server virtuális gépeken futó az erőforrás-kezelő létrehozott."
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
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Integráció Azure kulcs tárolóból elemre az SQL Server Azure VMs (erőforrás-kezelő) a

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasszikus](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>– Áttekintés
Vannak olyan több SQL Server titkosítási funkciókat, például [átlátszó adatok titkosítási (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), a [oszlop szintű titkosítás (törlése)](https://msdn.microsoft.com/library/ms173744.aspx)és a [biztonsági másolat titkosítás](https://msdn.microsoft.com/library/dn449489.aspx). Ezek az űrlapok titkosítási szükség kezelheti és tárolni a cryptographic billentyűkkel titkosítást használ. Az Azure kulcs tárolóból elemre (AKV) szolgáltatás az biztonsági és a biztonságos és könnyen hozzáférhető helyen billentyűk kezelésére szolgál. Az [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) lehetővé teszi, hogy az SQL Server Azure kulcs tárolóból elemre a billentyűk használatához.

Ha a helyszíni SQL Server róla gépek, hogy a lépéseket [követve elérése a számítógépről a helyszíni SQL Server Azure kulcs tárolóból elemre](https://msdn.microsoft.com/library/dn198405.aspx). De az SQL Server Azure VMs, időt takaríthat az *Azure kulcs tárolóból elemre Integration* funkció használatával.

Ezt a szolgáltatást, ha azt automatikusan telepíti az SQL Server-összekötő, konfigurálja a EKM szolgáltató eléréséhez Azure kulcs tárolóból elemre, és létrehoz lehetővé teszi a tárolóból elemre eléréséhez a hitelesítő adatok. Ha korábban már említettük helyszíni dokumentációjában leírt lépéseket nézett, megtekintheti, hogy ez a funkció automatizálja a lépéseket, 2 és 3. Manuálisan is kimutatásadatokat beállítást, ha a fő tárolóból elemre, és a billentyűk. Itt a teljes beállítása a SQL virtuális automatizált. Befejeződése ezt a szolgáltatást a telepítés hajthat végre T-SQL-utasítások kezdje az adatbázisok vagy a biztonsági másolatok titkosítja a szokásos módon.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Engedélyezése és konfigurálása a AKV-integráció
Során kiépítési AKV alkalmazással való integráció engedélyezése, vagy meglévő VMs konfigurálni.

### <a name="new-vms"></a>Új VMs
Ha egy új SQL Server-virtuális gép az erőforrás-kezelő program kiépítési, az Azure portál Azure kulcs tárolóra alkalmazással való integráció engedélyezése lépés. Az Azure kulcs tárolóból elemre a funkció csak az a vállalati, a Fejlesztőeszközök és az SQL Server kiadásai kiértékelés.

![Az SQL Azure kulcs tárolóra integrációja](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Részletes ismertetését megtalálja a kiépítési olvassa el a [rendelkezni az Azure-portálon SQL Server virtuális gép](virtual-machines-windows-portal-sql-server-provision.md)című témakört.

### <a name="existing-vms"></a>Meglévő VMs
Meglévő SQL Server virtuális gépeken futó jelölje be az SQL Server virtuális gépet. Válassza a **Beállítások** lap a **SQL Server konfigurálása** című szakaszát.

![Meglévő VMs SQL AKV integrációja](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Kattintson az **SQL Server konfigurációs** lap automatikus kulcs tárolóra integrációs szakaszában a **Szerkesztés** gombra.

![SQL AKV integráció a meglévő VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Amikor befejezte, kattintson az **OK** gombra a módosítások mentéséhez az **SQL Server konfigurálása** a lap alján.

>[AZURE.NOTE] Sablon használatával AKV integrációs is beállítható. További tudnivalókért lásd: [Azure quickstart útmutató sablon integrációs Azure kulcs tárolóból elemre](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
