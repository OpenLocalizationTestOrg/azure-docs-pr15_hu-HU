<properties
    pageTitle="Egy virtuális SQL Server SQL Server adatbázis áttelepítése |}] A Microsoft Azure"
    description="További tudnivalók az SQL Server egy Azure virtuális gépen egy helyi felhasználói adatbázis áttelepítéséhez."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>SQL Server adatbázis áttelepítése SQL Server egy Azure VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Erőforrás-kezelő modell.


Számos helyi SQL Server adatbázis felhasználó áttelepítése SQL Server egy Azure VM módszerei. Ez a cikk röviden ismertetik a különböző módszerek, ajánljuk a legjobb módszer a különböző helyzeteket és tartalmazzák a [tankönyv](#azure-vm-deployment-wizard-tutorial) segítséget nyújtanak a **központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép** varázsló használatával lesz. 

A leírt az [oktatóprogram](#azure-vm-deployment-wizard-tutorial) **központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép** varázslóval módszer csak a klasszikus telepítési modell szolgál. 

## <a name="what-are-the-primary-migration-methods"></a>Mik azok az elsődleges áttelepítési módszerek?

Az elsődleges áttelepítési módszerek a következők:

- A központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló használata
- Tömörítés használatával helyszíni biztonsági mentéshez és a biztonságimásolat-fájl kézzel másolja a Azure virtuális gép
- Az URL-címről URL-címe és a Azure virtuális gépre történő visszaállítás biztonsági mentés
- Le, majd másolja az adat- és naplófájljai Azure blob-tároló és mellékeljük Azure VM az SQL Server URL-címről
- Helyszíni fizikai számítógép átalakítása a Hyper-V VHD, Azure Blob-tároló feltöltése, és telepíteni, az új virtuális gép használata VHD feltöltve.
- Hajó importálás/exportálás szolgáltatással a Windows merevlemez-meghajtó
- Ha a helyszíni telepítés AlwaysOn, [Azure replika hozzáadása varázsló](virtual-machines-windows-classic-sql-onprem-availability.md) segítségével hozható létre replika Azure és felhasználók Azure adatbázispéldány mutat a feladatátvétel
- [Tranzakciós replikáció](https://msdn.microsoft.com/library/ms151176.aspx) SQL Server segítségével az előfizető konfigurálja az Azure az SQL Server-példányt, és majd tiltsa le a replikációt, a felhasználók Azure adatbázispéldány mutat



## <a name="choosing-your-migration-method"></a>Az áttelepítési módszer kiválasztása

Adatátviteli teljesítmény optimális a Azure VM tömörített biztonságimásolat-fájl használatával történő áttelepítése az adatbázis fájlok esetében általában a legjobb módszer. Ez a lehetőség a [központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló](#azure-vm-deployment-wizard-tutorial) használ. Ez a varázsló az áttelepíteni egy helyi felhasználói adatbázis fut, az SQL Server 2005 vagy az SQL Server 2014-ig nagyobb vagy annál nagyobb, legalább 1 TB méretű tömörített adatbázis biztonságimásolat-fájl esetén az ajánlott módszer.

Az adatbázis-áttelepítési folyamat során a állásidő minimálisra csökkentése érdekében, vagy az AlwaysOn vagy a tranzakciós replikációs beállítás használata.

Ha nem lehetséges, a fenti módszerek használatát, kézzel át az adatbázis. Ezzel a módszerrel akkor általában indul az adatbázis biztonsági másolatának egy példányát követ a az adatbázis biztonsági mentése Azure azokat, és hajtsa végre az adatbázis visszaállítási. Az adatbázis fájlok maguk is átmásolhatja Azure, és csatolja azokat. Vannak számos módszer, amellyel elvégezhető az adatbázis egy Azure VM történő áttelepítése a kézi folyamat.

> [AZURE.NOTE] SQL Server 2014 vagy SQL Server 2016 történő frissítéskor az SQL Server korábbi verzióit, érdemes, hogy változtatásokra van szükség. Azt javasoljuk, hogy a cím az áttelepítési projekt részeként az SQL Server új verziója által nem támogatott szolgáltatások összes függőségét. További információt a verziókra és forgatókönyvek lásd: [SQL Server rendszerre](https://msdn.microsoft.com/library/bb677622.aspx).

Az alábbi táblázat felsorolja az egyes elsődleges áttelepítési módszereket, valamint ismerteti a legmegfelelőbb minden módszer használata esetén.

| Módszer  | Forrás-adatbázis-verzió  |  Cél-adatbázis-verzió | Forrás-adatbázis biztonsági másolat mérete megkötés  | Jegyzetek  |
|---|---|---|---|---|
| [A központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló használata](#azure-vm-deployment-wizard-tutorial) | Az SQL Server 2005 vagy nagyobb | SQL Server 2014 vagy nagyobb | < 1 TB  | Leggyorsabb és legegyszerűbb módszer, használata, ha lehetséges, egy új vagy meglévő SQL Server-példány egy Azure virtuális gépen át | 
| [Használja a varázsló Azure replika hozzáadása](virtual-machines-windows-classic-sql-onprem-availability.md) | Az SQL Server 2012 vagy nagyobb | Az SQL Server 2012 vagy nagyobb | [Borzas VM tárolási korlátját](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Minimálisra csökkenti az állásidőt, ha az AlwaysOn helyszíni telepítés használata |
| [Az SQL Server tranzakciós többszörözés](https://msdn.microsoft.com/library/ms151176.aspx) | Az SQL Server 2005 vagy nagyobb | Az SQL Server 2005 vagy nagyobb | [Borzas VM tárolási korlátját](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Akkor használja, ha az állásidő minimálisra csökkentése érdekében szükséges, és nem rendelkezik az AlwaysOn helyszíni telepítés |
| [Tömörítés használatával helyszíni biztonsági mentéshez és a biztonságimásolat-fájl kézzel másolja a Azure virtuális gép](#backup-to-file-and-copy-to-vm-and-restore) | Az SQL Server 2005 vagy nagyobb | Az SQL Server 2005 vagy nagyobb | [Borzas VM tárolási korlátját](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Csak ha varázslóval nem lehet az, amikor a cél adatbázis-verzió nem kevesebb, mint az SQL Server 2012 SP1 CU2, vagy az adatbázis biztonsági másolat mérete nagyobb, mint 1 TB (az SQL Server 2016 12.8 TB) használata |
| [Az URL-címről URL-címe és a Azure virtuális gépre történő visszaállítás biztonsági mentés](#backup-to-url-and-restore) | Az SQL Server 2012 SP1 CU2 vagy nagyobb | Az SQL Server 2012 SP1 CU2 vagy nagyobb | < SQL Server 2016, egyébként < 1 TB az 12.8 TB | Általában [tartalék URL-cím](https://msdn.microsoft.com/library/dn435916.aspx) használata nem meglehetősen egyszerű, és egyenértékű a teljesítmény, a varázsló segítségével |
| [Le, és másoljuk az adat- és naplófájljai Azure blob-tároló és mellékeljük az SQL Server Azure virtuális gép URL-címről](#detach-and-copy-to-url-and-attach-from-url) | Az SQL Server 2005 vagy nagyobb | SQL Server 2014 vagy nagyobb | [Borzas VM tárolási korlátját](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Ezt a módszert használja, ha meg szeretné [tárolni ezeket a fájlokat az Azure Blob-tároló szolgáltatás használatával](https://msdn.microsoft.com/library/dn385720.aspx) , majd csatolja azokat egy Azure VM, különösen a nagyon nagy adatbázisokat futó SQL Server |
| [Helyszíni számítógép átalakítása a Hyper-V virtuális merevlemezek, Azure Blob-tároló feltöltése, és telepíteni egy új feltöltött virtuális Merevlemezt használó virtuális gép](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | Az SQL Server 2005 vagy nagyobb | Az SQL Server 2005 vagy nagyobb | [Borzas VM tárolási korlátját](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Akkor használja, ha az adatbázisok az adatbázis más felhasználói adatbázisok és/vagy a rendszeradatbázisok függ az áttelepítés együtt [összhangba hozzák a saját SQL Server licenc](../sql-database/sql-database-paas-vs-sql-server-iaas.md), futtatni az SQL Server, vagy egy régebbi verziója a rendszer és a felhasználó áttelepítésekor adatbázis áttelepítésekor. |
| [Hajó importálás/exportálás szolgáltatással a Windows merevlemez-meghajtó](#ship-hard-drive) | Az SQL Server 2005 vagy nagyobb | Az SQL Server 2005 vagy nagyobb | [Borzas VM tárolási korlátját](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | A [Windows importálása és exportálása szolgáltatást](../storage/storage-import-export-service.md) használja, ha kézi copy metódus túl lassú, mint például nagy adatbázisok |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Borzas VM telepítési varázsló-oktatóprogram

Microsoft SQL Server Management Studio **központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép** varázsló segítségével telepítse át egy SQL Server 2005, SQL Server 2008, az SQL Server 2008 R2, az SQL Server 2012, SQL Server 2014 vagy SQL Server 2016 helyi felhasználói adatbázis (akár 1 TB) SQL Server 2014 vagy SQL Server 2016 egy Azure virtuális gép. A varázsló segítségével felhasználói adatbázis áttelepítése az áttelepítési folyamat során a varázsló által létrehozott SQL Server egy Azure VM vagy egy meglévő Azure virtuális gép. Adatbázis SQL Server újabb verziójára telepíti át, amikor az adatbázis automatikusan frissítésre kerül a folyamat során.

A metódus csak a klasszikus telepítési modell. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Töltse le a legújabb verziót a központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló

A legújabb verzió az SQL Server Microsoft SQL Server Management Studio segítségével ellenőrizze, hogy a **központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép** varázsló legújabb verzióját. Ez a varázsló a legújabb verzió tartalmazza a legújabb frissítéseket a klasszikus Azure Portal, és támogatja a legújabb Azure VM képek a galériában (a varázsló régebbi verziói nem működnek). Az SQL Server, [Töltse le](http://go.microsoft.com/fwlink/?LinkId=616025) a legújabb verzióját a Microsoft SQL Server Management Studio letöltése és telepítése az ügyfélgépen a kapcsolatot az adatbázis áttelepítése és az interneten tervez.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Adja meg a meglévő Azure virtuális gép és SQL Server-példányt (ha alkalmazható)

Ha egy meglévő Azure VM, a konfigurációs lépések szükségesek:

- Állítsa be a Azure VM és az SQL Server példány csatlakoztathatóság egy másik számítógépről való [csatlakozáshoz a SSMS egy másik számítógépen lévő SQL Server VM példány](virtual-machines-windows-sql-connect.md)lépéseket követve. Csak az SQL Server 2014 és SQL Server 2016 a galériában képek támogatottak, ha a varázsló segítségével.
- Az SQL Server felhő Adapter szolgáltatás nyitott végpontját konfigurálja 11435 saját kikötője és a Microsoft Azure átjárón. Ez a port az SQL Server 2014 vagy SQL Server 2016 egy Azure a Microsoft VM a kiépítés részeként jön létre. A felhő Adapter is létrehoz a Windows tűzfal szabály 11435 alapértelmezett porton a bejövő TCP-kapcsolatok engedélyezésére. A végpont lehetővé teszi, hogy a varázsló használatát a felhő adapter szolgáltatás a biztonságimásolat-fájlok másolása a helyi példány Azure VM. További tudnivalókért lásd: [Felhő Adapter az SQL Server](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Felhő Adapter végpont létrehozása](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Futtassa a használatát a központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló

1. Nyissa meg a Microsoft SQL Server Management Studio a Microsoft SQL Server 2016, és csatlakozzon az SQL Server példány, amely egy Azure VM áttelepítéséhez felhasználói adatbázist tartalmazó.
2. Kattintson a jobb gombbal az áttelepíteni kívánt adatbázis, feladat pontjára, és kattintson a központi telepítés a Microsoft Azure VM.

    ![Varázsló indítása](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. A bevezetés oldalon kattintson a Tovább gombra.
4. Az adatforrás-beállítások oldalon csatlakozni az SQL Server-példányt tartalmazó adatbázis, amely egy Azure VM át kívánja.
5. A biztonságimásolat-fájlok ideiglenes helyét adja meg. Ha a távoli kiszolgálóhoz csatlakozni, egy hálózati meghajtót kell megadnia.

    ![Adatforrás-beállítások](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Kattintson a Tovább gombra.
7. A Microsoft Azure bejelentkezési lapon kattintson a bejelentkezés és bejelentkezés az Azure fiókját.
8. Jelölje be az előfizetés kívánja használni, és kattintson a Tovább gombra.

    ![Azure-bejelentkezés](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. A telepítési beállítások lapon megadhatja a egy új vagy egy meglévő felhőalapú szolgáltatás és a virtuális gép nevét:

 - Adja meg egy SQL Server 2014 vagy SQL Server 2016 galéria kép segítségével új Azure virtuális gép létrehozása egy új felhőalapú szolgáltatás új felhő szolgáltatás nevét és a virtuális gép nevét.

     - Ha új felhőalapú szolgáltatás nevét adja meg, adja meg a használni kívánt tároló fiók.

     - Ha egy meglévő felhőalapú szolgáltatás nevét adja meg, a tároló fiók visszanyerhetjük, és beírjuk Ön helyett.

 - Adjon meg egy meglévő felhőalapú szolgáltatás és Azure új virtuális gép létrehozása egy meglévő felhőalapú szolgáltatás új virtuális gép nevét. Csak adja meg egy SQL Server 2014 vagy SQL Server 2016 galéria kép.
 - Adja meg egy meglévő felhőalapú szolgáltatás és egy meglévő Azure virtuális gép virtuális gép nevét. Ez kell egy képet egy SQL Server 2014 vagy SQL Server 2016 galéria kép épül.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Kattintson a beállítások gombra
  - Ha megad egy meglévő felhőalapú szolgáltatás és virtuális gép nevét, a felhasználói név és jelszó megadását kéri.

        ![Borzas gép beállításai](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Ha megad egy új virtuális gép nevét, jelöljön ki egy képet a galéria képek közül, és adja meg a következő adatokat kéri:
      - Kép – csak az SQL Server 2014 vagy SQL Server 2016 kijelölése
        - Felhasználónév
        - Új jelszó
        - Jelszó megerősítése
        - Hely
        - Mérete.
    - Ezenkívül Ide kattintva fogadja el az önálló generált tanúsítvány esetében az új Microsoft Azure virtuális gép, és kattintson az OK gombra.

    ![Borzas új számítógép-beállítások](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Adja meg a cél-adatbázis neve, ha az különbözik a forrás-adatbázis nevét. Ha már létezik a céladatbázisban, a rendszer lesz automatikusan növeli az adatbázis nevét, hanem felülírja a meglévő adatbázist.
12. Kattintson a Tovább gombra, és kattintson a Befejezés gombra.

    ![Eredmények](./media/virtual-machines-windows-migrate-sql/results.png)

13. A varázsló befejezése után a virtuális gép csatlakozhat, és győződjön meg arról, hogy az adatbázis áttelepítése megtörtént-e.
14. Ha létrehozott egy új virtuális gép, állítsa be a Azure virtuális gép és az SQL Server példány csatlakozni [a SSMS egy másik számítógépen lévő SQL Server VM példány](virtual-machines-windows-sql-connect.md)lépéseket követve.

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Fájl-és másolás a VM és a helyreállítás biztonsági másolat

Ezt a módszert használja, nem a központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló használatakor vagy mert telepít át az SQL Server, SQL Server 2014 előtt verzióra, vagy a biztonságimásolat-fájl mérete nagyobb, mint 1 TB. Ha a biztonságimásolat-fájl mérete nagyobb, mint 1 TB, kell paritásos, mert a virtuális lemez maximális mérete 1 TB. A kézi módszerrel felhasználói adatbázis áttelepítése használja az alábbi általános lépéseket:

1.  Adatbázis teljes biztonsági másolat készítése egy helyszíni helyre.
2.  Hozzon létre vagy feltölteni kívánt SQL Server verziójának virtuális gép.
3.  A telepítő kapcsolat igényeinek. Lásd: [Kapcsolódás a SQL Server virtuális gépek Azure (erőforrás-menedzser)](virtual-machines-windows-sql-connect.md).
4.  Másoljuk át a biztonsági másolat fájlokat a virtuális Gépet, a távoli asztal, a Windows Intéző vagy a Másolás parancsot a parancssorba.

## <a name="backup-to-url-and-restore"></a>URL-cím és visszaállítás

Az [URL-biztonsági mentési](https://msdn.microsoft.com/library/dn435916.aspx) módszert akkor használja, ha a központi telepítése az SQL Server-adatbázishoz a Microsoft Azure virtuális gép varázsló nem használható, mert a biztonságimásolat-fájl mérete nagyobb, mint 1 TB és telepít át, és SQL Server 2016. Kisebb, mint 1 TB vagy SQL Server, SQL Server 2016 előtti verzióját futtató adatbázisok esetén a varázsló használata ajánlott. Az SQL Server 2016 csíkozott biztonságimásolat-készletek támogatottak, teljesítmény érdekében ajánlott, és egy blob mérete lépjék szükséges. Nagyon nagy adatbázisokat a [Windows Import-Export szolgáltatás](../storage/storage-import-export-service.md) használata ajánlott.

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Leválasztása és URL-címet másolja, és URL-címről csatolása

Ezt a módszert akkor használja, ha szeretné [tárolni ezeket a fájlokat az Azure Blob-tároló szolgáltatás használatával](https://msdn.microsoft.com/library/dn385720.aspx) , és csatolja azokat egy Azure VM, különösen a nagyon nagy adatbázisokat futó SQL Servertől. A kézi módszerrel felhasználói adatbázis áttelepítése használja az alábbi általános lépéseket:

1.  A helyszíni adatbázispéldány adatbázis fájlok leválasztása.
2.  Másolja a leválasztott adatbázisfájlok Azure blob-tároló a [AZCopy parancssori segédprogram](../storage/storage-use-azcopy.md)segítségével.
3.  Az adatbázis fájlok Azure URL-címről csatlakozni az SQL Server példány a Azure VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>VM átalakítása, új virtuális gép telepítését és URL feltöltése

Ezt a módszert Azure virtuális gép helyi SQL Server-példány összes rendszer- és felhasználói adatbázis áttelepítéséhez. Az alábbi általános lépéseket segítségével telepítse át a teljes SQL Server-példány a kézi módszerrel:

1.  Konvertálása fizikai vagy virtuális gépek Hyper-V virtuális merevlemezek használatával [A Microsoft virtuális gép konverter](http://technet.microsoft.com/library/dn873998.aspx).
2.  VHD-fájlok feltöltésére Azure tároló az [Add-AzureVHD parancsmag](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx)segítségével.
3.  Új virtuális gép telepítését a feltöltött virtuális Merevlemezt.

> [AZURE.NOTE] Alkalmazások áttelepítése, fontolja meg [Azure webhelyen helyreállítási](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Hajó merevlemez-meghajtó

A [Windows importálási/exportálási metódus](../storage/storage-import-export-service.md) segítségével nagy mennyiségű adat a fájl átvitele az olyan helyzetekben, ahol a hálózaton keresztül feltöltése elfogadhatatlanul magas vagy nem megvalósítható Azure Blob-tároló. A szolgáltatás egy vagy több merevlemez-meghajtók, hogy adatok az Azure adatközpont, ahol feltöltődik az adatokat tároló fiókját tartalmazó küldeni.

## <a name="next-steps"></a>A következő lépések

További információt az SQL Server rendszert futtató Azure virtuális gépek [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)témakörben talál.

A rögzített kép egy Azure az SQL Server virtuális gép létrehozása című a CSS SQL Server mérnökök blog [Tippek és trükkök a "klónozása" Azure SQL virtuális gépek rögzített képekből](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) kapcsolatban.
