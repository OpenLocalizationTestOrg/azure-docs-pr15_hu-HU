<properties
    pageTitle="Biztonsági mentési és visszaállítási az SQL Server |} Microsoft Azure"
    description="Biztonsági mentési és visszaállítási fut a Azure virtuális gépeken futó SQL Server-adatbázisokhoz kapcsolatos szempontok ismertetése."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Biztonsági mentési és visszaállítási az Azure virtuális gépeken futó SQL Server

## <a name="overview"></a>– Áttekintés

Adatok az SQL Server-adatbázis biztonsági másolatának része egy fontos alkalmazás vagy felhasználó hibák miatt adatvesztés elleni védelem az stratégia. Ez a egyaránt igaz, az SQL Server fut a Azure virtuális gépeken futó (VMs).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Az SQL Server Azure VMs fut használhatja a natív biztonsági mentése és csatlakoztatott használata a biztonságimásolat-fájlok rendeltetési technikákat visszaállítása. Azonban nem csatolhat Azure virtuális géphez, [a virtuális gép méretének](virtual-machines-linux-sizes.md)alapján lemez száma korlátozott. A terhelést a lemez management szempontok is van.

SQL Server 2014-es kezdve biztonsági mentése és visszaállítása a Microsoft Azure Blob-tárolóhoz. Az SQL Server 2016 is biztosít fejlesztések a ezt a beállítást. Ezeken kívül az a Microsoft Azure Blob-tárolóban lévő adatbázisfájlok, SQL Server 2016 lehetővé teszi majdnem pillanatnyi biztonsági másolatok és Azure pillanatképek használata gyors visszaállítása. Ez a cikk áttekintést nyújt az alábbi lehetőségek, és további információk [az SQL Server biztonsági mentése és visszaállítása a Microsoft Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj919148.aspx)találhatók.

>[AZURE.NOTE] Biztonsági másolatának nagyon nagy adatbázisokat lehetőségei vitafórum olvassa el a [Többszörös-adjon az SQL Server adatbázis biztonsági mentése stratégiák az Azure virtuális gépeken futó](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx)című témakört.

Az alábbi szakaszokban SQL Server-Azure virtuális gépen támogatott különböző verziói-specifikus adatait tartalmazza.

## <a name="sql-server-virtual-machines"></a>Az SQL Server virtuális gépeken futó

Az SQL Server-példányt az Azure virtuális gépen fut, az adatbázis fájlok már találhatók, adatok lemezen Azure-ban. Ezek a lemezt élő Azure Blob-tárolóban lévő. Így okát és biztonsági mentése az adatbázis az alábbi módszerek meg, hogy módosítása kissé. Vegye figyelembe az alábbiakat. 

- Már nincs szüksége az adatbázis biztonsági másolatokat, mivel a Microsoft Azure biztosít a védelem a Microsoft Azure szolgáltatás részét képező szerte hardver- vagy hiba.

- Van szüksége az adatbázis biztonsági másolatokat védelem felhasználói hibák ellen, vagy archiválás céljából, a szabályozó okok vagy a felügyeleti célokat.

- A biztonságimásolat-fájlt közvetlenül az Azure tárolhat. További tudnivalókért lásd: az alábbi szakaszok, amely az SQL Server a különböző verzióira vonatkozóan útmutatást.

## <a name="sql-server-2016"></a>Az SQL Server 2016-ban

A Microsoft SQL Server 2016 támogatja az SQL Server 2014-es [biztonsági mentési és visszaállítási Azure okkal](https://msdn.microsoft.com/library/jj919148.aspx) funkcióját. De az alábbi többletfunkciókkal is tartalmazza:

| 2016 adatbővítésre               | Részletek                          |
|---------------------|-------------------------------|
| **Csíkozást**              | Biztonsági másolatot a Microsoft Azure blob-tárolóhoz, ha az SQL Server 2016 támogatja a több BLOB ahhoz, hogy nagy adatbázisok, 12.8 TB legfeljebb mentésével biztonsági.      |
| **Biztonságimásolat-pillanatfelvétel**                | Azure pillanatfelvételek, való a biztonságimásolat-SQL Server-fájl-pillanatfelvétel majdnem pillanatnyi biztonsági másolatok készítése és a gyors visszaállítása az adatbázis-fájlok tárolása az Azure Blob-tároló szolgáltatással biztosít. Ez a képesség lehetővé teszi, hogy a leegyszerűsíti a biztonsági mentés és házirendek visszaállítása. Biztonságimásolat-fájlt-pillanatfelvétel is támogat a pont, az idő visszaállítása. További tudnivalókért lásd: [Az Azure-ban adatbázisfájlok pillanatkép másolatok](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Felügyelt biztonsági ütemezése**            | Az SQL Server felügyelt biztonsági másolat Azure egyéni ütemezések támogatja. További tudnivalókért lásd: az [SQL Server felügyelt biztonsági mentése Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Lásd: az SQL Server 2016 Azure Blob-tárolóhoz használata esetén az funkciók oktatóanyagot, [oktatóprogram: a Microsoft Azure Blob-tároló szolgáltatás használata az SQL Server 2016 adatbázisokkal](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>Az SQL Server 2014-es

Az SQL Server 2014-es megtalálhatók az alábbi többletfunkciókkal:

1. **Biztonsági mentési és visszaállítási az Azure**:

 - *Az SQL Server biztonsági másolat URL-címre* támogatási ekkor megjelenik az SQL Server Management Studio. A vezérlőt, amellyel biztonsági másolatot készíthet Azure SQL Server Management Studio biztonsági mentése és visszaállítása a tevékenységek és karbantartás terv varázsló használatakor most érhető el. További tudnivalókért lásd: az [SQL Server biztonsági másolat URL-címre](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *Az SQL Server felügyelt biztonsági Azure* , amely lehetővé teszi, hogy az automatikus biztonsági mentés kezelés új funkciókat tartalmaz. Ez akkor különösen hasznos automatizálhatja a biztonságimásolat-kezelési az Azure gépen futó SQL Server 2014-es előfordulását. További tudnivalókért lásd: az [SQL Server felügyelt biztonsági mentése Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Az automatikus biztonsági mentés* nyújt további automatizálási automatikusan engedélyezése *Az SQL Server felügyelt biztonsági másolat Azure* meglévő és új adatbázisokra egy SQL Server virtuális Azure-ban. További tudnivalókért lásd: az [Automatikus biztonsági mentés az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-automated-backup.md).
 - Az SQL Server 2014-es Azure biztonsági mentése a beállítások áttekintése lásd: az [SQL Server biztonsági mentése és visszaállítása a Microsoft Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Titkosítás**: SQL Server 2014-es támogatja az adatok titkosítása biztonsági létrehozásakor. Több titkosítási algoritmust és a használatát támogatja osf tanúsítvány vagy aszimmetrikus billentyűt. További tudnivalókért lásd: a [Biztonsági másolat titkosítást](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012

Az SQL Server biztonsági mentése és visszaállítása az SQL Server 2012 részletes információkat talál a [biztonsági mentése és visszaállítása az SQL Server adatbázisok (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Az SQL Server 2012 SP1 összesítő frissítés 2 indulva biztonsági másolat és állíthatja helyre a az Azure Blob-tárolóhoz szolgáltatás. Ez a bővítés készítsen biztonsági másolatot az SQL Server-adatbázis-Azure virtuális gép vagy egy helyi példányt rendszeren futó SQL Server használható. További tudnivalókért lásd: az [SQL Server biztonsági mentése és visszaállítása az Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Az Azure Blob-tároló szolgáltatás előnyei közé tartoznak a 16 lemezre korlátját csatolt lemezre, könnyű kezelés, a biztonságimásolat-fájlt egy másik példányát az Azure virtuális gépen futó SQL Server-példányt, vagy egy helyi példányt az áttelepítési vagy vészhelyreállításra közvetlen elérhetősége kikerülheti. Egy teljes listát az SQL Server biztonsági másolatok Azure blob-tároló szolgáltatást használ, hogy mennyi előnnyel jár című *előnyeit* az [SQL Server biztonsági mentése és visszaállítása az Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Ajánlott eljárás a javaslatok és hibaelhárítási információkat című témakörben talál [biztonsági mentése és visszaállítása az ajánlott eljárások (Azure Blob-tároló szolgáltatás)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

Az SQL Server biztonsági mentése és visszaállítása az SQL Server 2008 R2 olvassa el a [biztonsági másolat készítése és az adatbázis visszaállítása az SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)című témakört.

Az SQL Server biztonsági mentése és visszaállítása az SQL Server 2008 olvassa el a [biztonsági másolat készítése és az adatbázis visszaállítása az SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)című témakört.

## <a name="next-steps"></a>Következő lépések

Ha azt tervezi, az SQL Server-Azure virtuális a telepítéssel, a következő oktatóanyagra a kiépítési útmutató található: [virtuális gép SQL Server Azure Azure erőforrás-kezelővel a kiépítési](virtual-machines-windows-portal-sql-server-provision.md).

Biztonsági mentése és visszaállítása az áttelepítés után használható, bár vannak esetleg könnyebben adatok áttelepítési elérési utak SQL Server-Azure virtuális. Teljes vitafórum áttelepítési lehetőségek és ajánlások olvassa el a [SQL Server-Azure virtuális az adatbázis áttelepítése](virtual-machines-windows-migrate-sql.md)című témakört.

Egyéb [források az Azure virtuális gépeken futó SQL Server rendszert futtató](virtual-machines-windows-sql-server-iaas-overview.md)áttekintése.
