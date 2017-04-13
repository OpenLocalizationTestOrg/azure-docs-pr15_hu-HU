<properties
   pageTitle="Technikai útmutató: az Azure a helyszíni helyreállítási |} Microsoft Azure"
   description="Azure a helyszíni infrastruktúra rendszerek ismertetése és helyreállítási tervezése cikk"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Azure tűrőképessége technikai útmutató: az Azure a helyszíni helyreállítás

Azure egy átfogó készlet segítségével, így az Azure-helyszíni adatközponthoz kiterjesztését magas rendelkezésre állásának és katasztrófa helyreállítási célokra szolgáltatásokat nyújtja:

* __Hálózati__: A virtuális magánhálózati biztonságosan kiterjesztése a felhőbe a helyszíni hálózaton.
* __Számítja ki__: segítségével a Hyper-V a helyszíni ügyfelek is "emelje fel és shift" Azure meglévő virtuális gépek (VMs).
* __Tárterület__: Azure tároló StorSimple kiterjed a fájlrendszerben. A biztonsági másolat Azure szolgáltatás nyújt a biztonsági másolat fájlok és SQL-adatbázisait Azure tárolóhoz.
* __Adatbázis-replikáció__: SQL Server 2014 (vagy újabb) elérhetőségét a csoportok, alkalmazhat magas rendelkezésre állásának és katasztrófa helyreállítási a helyszíni adatok.

##<a name="networking"></a>Hálózati

Azure virtuális hálózati logikailag elszigetelt szakasz létrehozása az Azure-ban és a biztonságos csatlakoztassa a helyszíni adatközponthoz vagy egy ügyfél egyetlen számítógépre IPsec kapcsolat segítségével is használhatja. Virtuális hálózattal kihasználhatja az méretezhető, igény szerinti infrastruktúra Azure-ban adja meg a csatlakozási adatok és az alkalmazások a helyszíni, beleértve a Windows Server, nagyszámítógépeket és UNIX futó rendszerek. [Azure hálózati dokumentáció](../virtual-network/virtual-networks-overview.md) talál további információt.

##<a name="compute"></a>Számítási

Ha helyszíni a Hyper-V használata esetén is "emelje fel és shift" meglévő Azure virtuális gépek, és a Windows Server 2012 (vagy újabb rendszerű), a virtuális megváltoztatása nélkül, vagy a virtuális konvertálása formátumok szolgáltatók. További információ című témakörben [olvashat a lemez és az Azure virtuális gépeken futó VHD](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Azure webhely helyreállítás

Ha vészhelyreállítás szolgáltatásként (DRaaS), az Azure [Azure webhely helyreállítási](https://azure.microsoft.com/services/site-recovery/)biztosít. Azure webhely helyreállítási VMware, a Hyper-V és fizikai kiszolgálók átfogó védelmet nyújt. Az Azure-webhely helyreállításhoz is használhatja egy másik helyszíni kiszolgálót és Azure a helyreállítási webhelyen. Azure webhely helyreállítási kapcsolatos további tudnivalókért olvassa el a [Azure webhely helyreállítási dokumentáció](https://azure.microsoft.com/documentation/services/site-recovery/)című témakört.

##<a name="storage"></a>Tárhely

Az Azure biztonsági mentési helyként a helyszíni adatok többféle lehetőség kínálkozik.

###<a name="storsimple"></a>StorSimple

StorSimple biztonságosan és átlátszó egyesíti a felhőbeli tárhelyről helyszíni alkalmazásait. Egy egyetlen készülék, amely biztosítja a nagy teljesítményű többszintű helyi és felhőbeli tárhelyről, élő archiválás, felhőalapú adatvédelem, és a helyreállítás is kínál. További tudnivalókért lásd: a [StorSimple termék lapját](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure biztonsági mentése

Azure biztonsági másolat lehetővé teszi, hogy a felhőben biztonsági másolatok a megszokott biztonsági eszközök a Windows Server 2012 (vagy újabb), a Windows Server 2012 Essentials (vagy újabb), és a System Center 2012 adatok védelme Manager (vagy újabb). Ezeket az eszközöket a biztonságimásolat-kezelési, amely független a tárolóhely a biztonsági másolatait, akár egy helyi lemez Azure tároló munkafolyamat szükséges. Miután a felhőbe adatok biztonsági másolat, jogosult felhasználók könnyen visszaállíthatja biztonsági mentést bármelyik kiszolgálóra.

Növekményes biztonsági másolatokat, és csak azokat a módosításokat, fájlok átkerülnek a felhőben. Ezzel az elemzéssel hatékonyan rendelkezésre álló tárterület méretének, csökkentheti a sávszélesség és támogatja az adatok több verziójának helyreállítása pont és az idő. További szolgáltatások használatát, például adatok adatmegőrzési házirendek, adatok tömörítés és szabályozásának adatátvitel is választhat. Az egyértelmű előnye, hogy a biztonsági másolatok is automatikusan "telephelyen kívül" Azure a használják a biztonsági másolat helyét tartalmaz. Ez megszünteti a biztonsága és védelme a helyszíni biztonsági másolat további követelményeknek.

További tudnivalókért lásd: [Mi az Azure biztonsági másolat?](../backup/backup-introduction-to-azure-backup.md) és [Azure biztonsági másolat konfigurálása DPM adatokhoz](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Adatbázis

AlwaysOn rendelkezésre állási csoportok, az adatbázis-tükrözéshez, a napló szállítási és a biztonsági mentés használatával az informatikai hibrid környezetben az SQL Server-adatbázisok katasztrófa helyreállítási megoldást telepítve van, és az Azure Blob-tárolóhoz visszaállítása. Az alábbi lehetőségeket használni fut a Azure virtuális gépeken futó SQL Server.

AlwaysOn rendelkezésre állási csoportok is használható, ahol találhatók az adatbázis-kópiák a mind a helyszíni hibrid-IT környezetben, és a felhőben. Ez a következő ábrán látható.

![Az SQL Server AlwaysOn rendelkezésre állási csoportok hibrid cloud architektúra](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Adatbázis-tükrözéshez is is kiterjedhet helyszíni kiszolgálók és a tanúsítvány-alapú beállítása a felhőben. Az alábbi ábra szemlélteti a fogalom.

![Az SQL Server adatbázis egy hibrid felhő architektúrában tükrözése](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Log szállítási egy helyszíni adatbázist szinkronizálni az Azure virtuális gép az SQL Server-adatbázis használható.

![SQL Server napló szállítási egy hibrid felhő architektúrában](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Végül akkor is adatbázis biztonsági mentése egy helyszíni közvetlenül az Azure Blob-tárolóhoz.

![Biztonsági másolatot az SQL Server Azure blobtárolóhoz egy hibrid felhő architektúrában](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

További tudnivalókért lásd: [Azure virtuális gépeken futó SQL Server magas rendelkezésre állásának és katasztrófa helyreállítási](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) és [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Ellenőrzőlisták a helyszíni helyreállítási Microsoft Azure-ban

###<a name="networking"></a>Hálózati

  1. Olvassa el ezt a dokumentumot a hálózat című szakaszát.
  2. Virtuális hálózat segítségével biztonságosan csatlakoztathassa a helyszíni a felhőbe.

###<a name="compute"></a>Számítási

  1. Tekintse át a jelen dokumentum számítási szakaszát.
  2. VMs áthelyezése a Hyper-V és Azure között.

###<a name="storage"></a>Tárhely

  1. Tekintse át a dokumentum tárolási szakaszát.
  2. A felhőbeli tárhelyről StorSimple szolgáltatások előnyeit.
  3. Az Azure biztonsági másolat szolgáltatással.

###<a name="database"></a>Adatbázis

  1. Tekintse át a jelen dokumentum adatbázis szakaszát.
  2. Fontolja meg az SQL Server Azure VMs meg a biztonsági másolatot.
  3. Állítsa be AlwaysOn rendelkezésre állási csoportok.
  4. Állítsa be a tanúsítvány-alapú adatbázis-tükrözéshez.
  5. Log szállítási használja.
  6. Azure Blob-tárolóhoz készítsen biztonsági másolatot a helyszíni adatbázisok.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a [technikai útmutató Azure tűrőképessége](./resiliency-technical-guidance.md)sorozat része. A sorozat a következő cikk [helyreállítási adatsérülésekkel vagy véletlen törlését](./resiliency-technical-guidance-recovery-data-corruption.md).