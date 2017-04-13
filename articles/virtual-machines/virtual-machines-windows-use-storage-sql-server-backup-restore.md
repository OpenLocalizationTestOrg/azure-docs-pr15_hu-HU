<properties
    pageTitle="Azure tárolási használata SQL Server biztonsági mentése és visszaállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként biztonsági másolatot készíthet az SQL Server Azure-tárterületet. Ebből a cikkből megtudhatja, a tárolóhoz Azure SQL-adatbázisait mentésével előnyeit."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Azure tároló használata SQL Server biztonsági mentése és visszaállítása

## <a name="overview"></a>– Áttekintés

Az SQL Server 2012 SP1 CU2 kezdve, most írhat SQL Server biztonsági mentés közvetlenül az Azure Blob-tároló szolgáltatás. Biztonsági másolat, és visszaállíthatja a Azure Blob-szolgáltatásból egy helyszíni SQL Server-adatbázis vagy SQL Server-adatbázis-Azure virtuális gépen ezt a funkciót is használhatja. Biztonsági mentés a felhőbe elérhetősége korlátlan geo replikált helyszínen tárolási és és a adatainak áttelepítési állapotával Kezeléstechnikai a felhőből előnyei kínál. Biztonsági mentése és VISSZAÁLLÍTÁSA kimutatások Transact-SQL nyelvben vagy SMO állíthatnak be.

Az SQL Server 2016 új funkciók; vezet be. használhatja a [biztonságimásolat-fájlt-pillanatfelvétel](http://msdn.microsoft.com/library/mt169363.aspx) majdnem pillanatnyi biztonsági mentés és a nagyon fontos visszaállítása hajthat végre.

Ebből a témakörből megtudhatja, miért érdemes választani Azure tároló SQL biztonsági másolatok és az érintett összetevők majd ismerteti. A cikk végén az erőforrásokat forgatókönyvek és további információt a használat ezt a szolgáltatást az SQL Server biztonsági másolat is használhatja.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Az SQL Server biztonsági másolatok az Azure Blob-szolgáltatás előnyei

Vannak olyan, amely, ha az SQL Server mentésével arcra több problémáit. Ezekkel a kihívásokkal-tároló kezelése lapon kockázata tárterület, hozzáférés helyszínen tárhely és a hardverkonfigurációja tartalmazzák. Ezekkel a kihívásokkal számos SQL Server biztonsági másolatok a Azure Blob-tár szolgáltatás használatával foglalkozik. Fontolja meg a következő előnyökkel jár:

- **Könnyen használható**: a biztonsági másolatok tárolása Azure BLOB lehet egy kényelmes rugalmas és könnyen elérhető a helyszínen lehetőséget. Az SQL Server biztonsági másolatok helyszínen tároló létrehozása ugyanolyan egyszerű, mint a már meglévő parancsfájlok/feladatokat a **Biztonsági másolat az URL-cím** szintaxissal módosítása lehet. Helyszínen tároló általában távolságra a termelési adatbázis helyét, hogy egyetlen katasztrófa, amelyek csökkenthetik is a helyszínen és a gyártási adatbázis helyekre kell lennie. Kiválasztása a [geo-replikáció az Azure BLOB](../storage/storage-redundancy.md), ha egy további réteget, amely hatással lehet a teljes terület katasztrófa védelmet.
- **Biztonsági másolat archív**: az Azure Blob-tárolóhoz szolgáltatás kínál jobb megoldás, mint a gyakran használt szalag beállítás biztonsági másolatok archiválását. Előfordulhat, hogy a szalag tároló védelme a médiafájlokat a helyszínen létesítmény és mértékek fizikai pihenőhelyek. A biztonsági másolatok tárolása Azure Blob-tárolóhoz biztosít az azonnali, könnyen hozzáférhető, és egy tartós az archiválás lehetőséget.
- **Felügyelt hardveres**: nincs általános hardver-kezelés az Azure szolgáltatásaival nem. Azure szolgáltatás kezelése a hardver, és adja meg a geo replikációs redundancia és a hardver hibák elleni védelem.
- **Korlátlan tároló**: Azure BLOB-való közvetlen biztonsági engedélyezésével gyakorlatilag korlátlan tárolóhoz hozzáférése van. Másik lehetőségként biztonsági mentés az Azure virtuális gép lemez munkalapjai gépi mérete alapján. Nincs lemez csatolhat az Azure virtuális géphez biztonsági másolatok számát korlátozott. Ez a korlát nagyon nagy példány 16 lemezen, és kevesebb kisebb előfordulását.
- **Biztonsági másolat elérhetősége**: Azure BLOB tárolt biztonsági fel bárhol és bármikor érhetők el, és könnyen visszaállítása egy helyszíni SQL Server vagy egy másik SQL Server fut a az Azure virtuális gép, az adatbázis csatolása vagy leválasztása nélkül, vagy le, és a virtuális csatolása számára is elérhető.
- **Költség**: csak a formázás szolgáltatás vásárolni. Helyszínen és biztonsági másolat archív lehetőség költséghatékony lehet. Az [Azure árak Számológép](http://go.microsoft.com/fwlink/?LinkId=277060 "Számológép árak"), illetve az [Azure árak cikk](http://go.microsoft.com/fwlink/?LinkId=277059 "árak a cikk") további információt talál.
- **Tárterület pillanatképek**: Ha adatbázisfájlok tárolja az Azure blob, és használja az SQL Server 2016, [biztonságimásolat-fájlt-pillanatfelvétel](http://msdn.microsoft.com/library/mt169363.aspx) segítségével gyakorlatilag pillanatnyi biztonsági mentés és a nagyon fontos visszaállítása végrehajtása.

További részletekért olvassa el [az SQL Server biztonsági mentése és visszaállítása az Azure Blob-tároló szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=271617).

Az alábbi két szakaszok az Azure Blob tárhelyszolgáltatáshoz, beleértve a szükséges SQL Server-összetevők vezet be. Fontos megtudhatja, hogy az összetevők és a kapcsolati sikeresen biztonsági mentése és visszaállítása a Azure Blob-tároló szolgáltatásból.

## <a name="azure-blob-storage-service-components"></a>Azure Blob-tároló szolgáltatás összetevői

A következő Azure összetevők előbb biztonsági másolatot készít az Azure Blob-tároló szolgáltatás használnak.

| Összetevő               | Leírás                          |
|---------------------|-------------------------------|
| **Tárterület-fiók** | A tárterület-fiók az összes tárterület-szolgáltatások kiindulási pontként. Az Azure Blob-tárolóhoz szolgáltatásainak használatához először létre kell hoznia Azure tárterület-fiókkal. Azure Blob-tároló szolgáltatással kapcsolatos további tudnivalókért lásd: [az Azure Blob-tároló szolgáltatás használata](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **A tároló** | Tároló BLOB halmazának csoportja biztosít, és BLOB tetszőleges számú tárolhat. Írja be az SQL Server-Azure Blob-szolgáltatás biztonsági rendelkeznie legalább a legfelső szintű tároló létrehozott. |
| **BLOB** | Bármilyen típusú és méretű fájlok. BLOB címmel rendelkező, az alábbi URL-cím formátumban: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Lap BLOB kapcsolatos további tudnivalókért lásd: a [lap BLOB és ismertetése tiltása:](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>Az SQL Server-összetevők

A következő SQL Server-összetevők előbb biztonsági másolatot készít az Azure Blob-tároló szolgáltatás használnak.

| Összetevő               | Leírás                          |
|---------------------|-------------------------------|
| **URL-CÍME** | Egy URL-címet adja meg, hogy egy egyedi biztonságimásolat-fájlt szeretne egységes erőforrás azonosító (URI). Adja meg a helyet, és az SQL Server biztonsági másolat nevét az URL-cím szolgál. Az URL-tényleges blob, nem csak a tároló kell mutatnia. Ha a blob nem létezik, akkor jön létre. Ha egy meglévő blob van megadva, biztonsági mentése nem sikerül, kivéve, ha a > a FORMÁZÁSI lehetőség van megadva. Az alábbi képen az URL-cím volna ad meg a biztonsági másolat parancs: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS ajánlott, de a nem kötelező. |
| **Hitelesítő adatok** | A csatlakozás és a hitelesítési Azure Blob-tárhelyszolgáltatáshoz szükséges adatokat tárolja a hitelesítő adatait.  Ahhoz, hogy az SQL Server írni az Azure Blob- vagy visszaállítása biztonsági másolatok készítése belőlük létre kell hozni egy SQL Server hitelesítő adatait. További tudnivalókért lásd: [Az SQL Server hitelesítő adatait](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Ha úgy dönt, hogy másolja a vágólapra, és töltse fel a biztonságimásolat-fájlt az Azure Blob-tároló szolgáltatással, kell használnia blob laptípus a tárolási lehetőség, ha szeretné használni ezt a fájlt a visszaállítás műveletekhez. A továbbfejlesztett fájlblokkolás blob-típusból származó VISSZAÁLLÍTÁSA a hiba miatt sikertelen lesz.

## <a name="next-steps"></a>Következő lépések

1. Ha még nem rendelkezik egy Azure-fiók létrehozása Ha az Azure értékelése, fontolja meg az [ingyenes próbaverziót](https://azure.microsoft.com/free/).

1. Ezután hajtsa végre az alábbi oktatóanyagok, amelyek végigvezetik a tárterület-fiók létrehozása és a visszaállítás közül.

    - **Az SQL Server 2014-es**: [oktatóprogram: az SQL Server 2014-es biztonsági mentése és visszaállítása a Microsoft Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **Az SQL Server 2016**: [oktatóprogram: a Microsoft Azure Blob-tároló szolgáltatás használata az SQL Server 2016 adatbázisokkal](https://msdn.microsoft.com/library/dn466438.aspx)

1. Tekintse át az [SQL Server biztonsági mentése és visszaállítása a Microsoft Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj919148.aspx)kezdődő dokumentációt.

Ha bármilyen problémája lenne, olvassa el a témakör [Az SQL Server biztonsági mentés ajánlott eljárások a URL-cím és a hibaelhárítás](https://msdn.microsoft.com/library/jj919149.aspx).

Más SQL Server biztonsági másolatot készíteni, és -beállításainak visszaállítása, lásd: a [biztonsági mentés és visszaállítás az Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
