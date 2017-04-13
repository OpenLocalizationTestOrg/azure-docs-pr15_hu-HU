<properties
    pageTitle="Az Oracle GoldenGate VMs beállítása |} Microsoft Azure"
    description="Állítsa be és Azure Windows Server VMs Oracle GoldenGate végrehajtása nagy rendelkezésre állásának és katasztrófa helyreállítás lépéseinek."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />


#<a name="configuring-oracle-goldengate-for-azure"></a>Az Oracle GoldenGate Azure konfigurálása


Ebben az oktatóanyagban bemutatja, hogyan Oracle GoldenGate beállítása magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó környezetben. Az oktatóprogram szolgáltatásaival kapcsolatos [kétirányú replikációs](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) az RAC Oracle-adatbázishoz, és van szükség az, hogy mindkét webhelyen aktívak.

Oracle GoldenGate támogatja az adatok terjesztési és adatok integrálása. Lehetővé teszi, hogy az adatok terjesztési és adatok megoldás szinkronizálási keresztül az Oracle-Oracle replikációs konfiguráció beállítása, és egy rugalmas magas elérhetősége megoldás. Az Oracle GoldenGate egészíti ki az Oracle-adatok Guard ahhoz, hogy a vállalati szintű adatok eloszlását és nulla-legrövidebb leállás frissítések és áttelepítések való replikáció funkciókhoz. Részletes tudnivalókért lásd: [Az Oracle-GoldenGate használatával az Oracle-adatok Guard](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Az Oracle GoldenGate tartalmazza a következő fő összetevőket: kibontásához adatok szivattyúból Replicat, útjának vagy a fájlok pontjainak, kezelő és adatgyűjtő kibontásához. Kétirányú replikációs van két hely között, szüksége létrehozása és minden összetevő mindkét webhelyen. Az Oracle GoldenGate architektúra részletes információkat a [Oracle GoldenGate útmutató](http://docs.oracle.com/goldengate/1212/gg-winux/index.html)című témakörben.

Ebben az oktatóanyagban feltételezi, hogy már rendelkezik az Oracle-adatbázis magas elérhetőségét, és a helyreállítás fogalmak, valamint az [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html)elméleti és gyakorlati ismeretek. További tudnivalókért lásd: az [Oracle-webhelyet](http://www.oracle.com/technetwork/database/features/availability/index.html).

Ezeken kívül az oktatóprogram feltételezi, hogy az alábbi előfeltételek már végrehajtotta:

- A magas elérhetősége és katasztrófa helyreállítási szempontok című részben a [Oracle virtuális gép képek - egyéb megfontolások](virtual-machines-windows-classic-oracle-considerations.md) témakörben, már áttekintése. Figyelje meg, hogy a Azure jelenleg önálló Oracle-adatbázis-példányok, de nem az Oracle valós alkalmazás fürt (Oracle RAC) támogatja.

- Az Oracle GoldenGate szoftver letöltötte az [Oracle-letöltések](http://www.oracle.com/us/downloads/index.html) webhelyről. A termék Pack Oracle fúziós köztes – adatok integrálása van kijelölve. Ezután ki van kijelölve az Oracle GoldenGate Oracle v11.2.1 Media csomag a a Microsoft Windows x64 (64 bites) 11 g Oracle-adatbázishoz. Ezután töltse le az Oracle GoldenGate V11.2.1.0.3 az Oracle 11g 64 bites Windows 2008-on (64 bites).

- Az Oracle vállalati Edition használata a Windows Server Azure-ban létrehozott két virtuális gépeken futó (VMs). Győződjön meg arról, hogy a virtuális gépeken futó [egy felhőalapú szolgáltatásba](virtual-machines-linux-load-balance.md) , és ugyanazon a [Virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/) annak érdekében, hogy az állandó magánjellegű IP-cím fölé elérhetik egymást.

- Az Azure klasszikus portált a webhely A "MachineGG1" és "MachineGG2" webhely B állított be a virtuális számítógép nevét.

- Létrehozta próba-adatbázisok "TestGG1" a webhely a és b webhely "TestGG2"

- Jelentkezzen be a Windows server a Rendszergazdák csoport tagjának vagy a **ORA_DBA** csoport tagjának.

Ebben az oktatóanyagban lesz:

1. A webhelyen A és B webhely adatbázis beállítása  

    1. Végezze el az adatok első betöltése

2. A webhely és a webhely B előkészítése az adatbázis-replikáció:

3. Az összes szükséges objektumok DDL replikáció támogatása létrehozása

4. GoldenGate Manager beállítása a webhelyen A és B hely

5. A webhelyen A és B webhely kibontása csoport létrehozása és adatok szivattyú folyamatok

    1. A webhelyen A kivonat és az adatok szivattyú folyamatok létrehozása

    2. A webhely B GoldenGate ellenőrzés táblázat létrehozása

    3. Adja hozzá a REPLICAT B webhelyen

    4. A webhely B kivonat és az adatok szivattyú folyamatok létrehozása

    5. GoldenGate ellenőrzés táblázat létrehozása A webhelyen

    6. A webhely REPLICAT hozzáadása

    7. A webhely és a webhely B trandata hozzáadása

    8. Kivonat és az adatok szivattyú folyamatok elindításához A webhelyen

    9. A webhely B kivonat és az adatok szivattyú folyamatok elindításához

    10. A webhelyen A REPLICAT-végrehajtás elindítása

    11. A webhely B REPLICAT-végrehajtás elindítása

6. Ellenőrizze a kétirányú replikációs folyamat

>[AZURE.IMPORTANT] Ebben az oktatóanyagban van beállítva, és az alábbi szoftver beállítások tesztelése:
>
>|                        | **A webhely-adatbázis**              | **B webhelyadatbázis**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Az Oracle megjelenés**     | Oracle11g Release 2 – (11.2.0.1) | Oracle11g Release 2 – (11.2.0.1) |
>| **Számítógép neve**       | MachineGG1                       | MachineGG2                       |
>| **Operációs rendszer**   | Windows 2008 R2 rendszerben                  | Windows 2008 R2 rendszerben                  |
>| **Az Oracle biztonsági AZONOSÍTÓK**         | TESTGG1                          | TESTGG2                          |
>| **A replikáció séma** | SCOTT                            | SCOTT                            |

Az Oracle-adatbázis és Oracle GoldenGate későbbi kiadásokban előfordulhat, hogy néhány további módosításokat végrehajtásához szükséges. A legfrissebb verziót információkért dokumentációjában olvasható [Az Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) és az [Oracle-adatbázis](http://www.oracle.com/us/corporate/features/database-12c/index.html) Oracle-webhelyen. Példa, a Megjelenés 11.2.0.4 forrásadatbázis és újabb DDL rögzítése aszinkron a logmining kiszolgáló által végzett, és nincs speciális eseményindítók, táblázatot vagy más adatbázis-objektumok telepítésére van szükség. Az Oracle GoldenGate frissítések felhasználói alkalmazások leállítása nélkül lehet elvégezni. Az eseményindító DDL és támogató objektumok kivonat az Oracle-11g forrás adatbázishoz, amely 11.2.0.4 verziónál korábbi integrált üzemmódban van szükség. Részletes, olvassa el az [telepítése és konfigurálása az Oracle GoldenGate Oracle-adatbázishoz](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. a webhelyen A és B webhely adatbázis beállítása
Ez a szakasz ismerteti, hogyan végre szeretne hajtani az adatbázis Előfeltételek helyen és a webhely b Az ebben a szakaszban a lépéseket kell elkészíti mindkét webhelyen: A webhely és a webhely b

Először is, távoli asztal helyen és a webhely B az Azure klasszikus portálon keresztül. Nyissa meg a Windows parancssor, és az Oracle GoldenGate telepítőfájlokat otthoni könyvtár létrehozása:

    mkdir C:\OracleGG

Ezután csomagolja ki, és az Oracle GoldenGate szoftverek telepítése, ebben a mappában. Ezt a lépést követően a GoldenGate szoftver parancs értelmező (GGSCI) kezdeményezhető hajtja végre a következő parancsot:

    C:\OracleGG\.\ggsci

[GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) konfigurálható számos parancs futtatása, a vezérlő és a monitor Oracle GoldenGate is használhatja.

Ezután a következő parancsot a csomagból almappákat létrehozásához:

    GGSCI (Hostname) 1> CREATE SUBDIRS

A következő parancsot zárja be a GGSCI parancssort:

    GGSCI (Hostname) 1> EXIT

Ezután létrehozásához szükséges egy adatbázis-felhasználó, az Oracle GoldenGate Manager, a kivonat és a replikáció folyamatok által használt. Ne feledje, hogy minden folyamathoz felhasználónként létrehozása, vagy csak egyetlen közös felhasználó beállítása. Ebben az oktatóanyagban hozzunk létre egy felhasználó, aki mint ggate nevezik. Ezután kínálunk, hogy a felhasználó a szükséges jogosultságokkal. Figyelje meg, hogy a webhelyen A és b webhelyen az alábbi műveleteket kell végrehajtania

Nyisson meg SQL\*plusz parancsablakban adatbázis rendszergazda jogosultságokkal rendelkező **SYSDBA csoport**, például: segítségével a webhelyen A és B webhelyen:

    Enter username: / as sysdba

És futtatásával:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Ezután keresse meg a INIT\<DatabaseSID\>. Az ORACLE %-os fájl ORA\_KEZDŐLAP %\\adatbázis mappába A és B webhely és a következő adatbázis paraméterek hozzáfűzése INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Egy teljes listát az összes Oracle GoldenGate GGSCI parancs olvassa el a [Reference for Oracle GoldenGate Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm)című témakört.

### <a name="perform-initial-data-load"></a>Végezze el az adatok első betöltése

A kiindulási adatok betöltés az adatbázis több lehetőség is kínálkozik követve végezheti el. Ha például is használhatja az [Oracle GoldenGate közvetlen betöltés](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) vagy normál exportálása és importálása segédprogramok táblázat-adatok exportálása a webhelyen A webhely b

Az Oracle GoldenGate replikációs folyamat bemutatására, ebben az oktatóanyagban bemutatja, hogyan tábla létrehozása A webhely és a B hely az alábbi parancsok használatával.

Első lépésként nyissa meg az SQL\*plusz ablak parancsot, és -készlet táblázat létrehozása a webhelyen a és B a webhely-adatbázisok a következő parancs futtatásával:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Kényszer ezután hozzáadása az újonnan létrehozott táblázat a webhelyen A és B webhely adatbázisok:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Ezután jogosultságokat összes meg az új készlet táblát a felhasználó ggate A webhelyen, és a webhely B:

    grant all on scott.inventory to ggate;

Ezután hozzon létre, és egy adatbázis eseményindító engedélyezése, INVENTORY_CDR_TRG, győződjön meg arról, hogy az új tábla a tranzakciók rendszer rögzíti, ha a felhasználó nem ggate az újonnan létrehozott táblázatot a. Ehhez a művelethez a webhelyen A és b webhely

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. A webhelyen és előkészítése webhely B adatbázis-replikáció:
Ez a szakasz ismerteti, hogyan helyen és a webhely B Felkészülés az adatbázis-replikáció. Az ebben a szakaszban a lépéseket kell elkészíti mindkét webhelyen: A webhely és a webhely b

Először is, távoli asztal helyen és a webhely B az Azure klasszikus portálon keresztül. Váltás az adatbázis archivelog mód használata az SQL * plusz parancsablakban:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Ezután engedélyezése minimális [kiegészítő naplózás](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) az alábbi képlettel történik:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Ezután készítse elő az adatbázis DDL (adatok definition language) a replikáció támogatása:

    SQL> alter system set recyclebin=off scope=spfile;

Ezután, leállítási, majd indítsa újra az adatbázist:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. a DDL replikáció támogatása az összes szükséges objektumok létrehozása
Ez a szakasz a parancsfájlok, akkor használja az összes szükséges objektumok DDL replikáció támogatása létrehozásához. A parancsfájlok ebben a szakaszban a helyen és a webhely b megadott futtatásához szükséges

Nyissa meg a Windows parancssor, és keresse meg az Oracle GoldenGate mappát, például C:\\OracleGG. Indítsa el az SQL\*plusz adatbázis rendszergazdai jogokkal **SYSDBA csoport** használata a webhelyen A és b webhely, például a parancssor

Futtassa a következő parancsfájlok:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Az Oracle GoldenGate eszköz egy tábla szintű bejelentkezési DDL (adatok definition language) támogatási van szükség. Ez az érveket, kiegészítő hozzáadása TRANDATA parancsával tábla szintű naplózás engedélyezése. Nyissa meg az Oracle GoldenGate parancs értelmező ablakot, jelentkezzen be az adatbázist, és kattintson a Hozzáadás TRANDATA parancsot:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. GoldenGate Manager beállítása a webhelyen A és B hely
Az Oracle GoldenGate Manager működik, mint a többi GoldenGate folyamatot, pontosan naplófájl kezelése és a jelentés indítása számos hajt végre.

Az Oracle GoldenGate Manager folyamat tartományi helyen és a webhely b van szüksége Ehhez hajtsa végre az alábbi lépéseket a webhelyen A és b webhely

Nyissa meg a Windows ablak parancsot, és az Oracle GoldenGate parancs értelmező kezdeményezése:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Naplózza a GGSCI munkamenet egy adatbázisba, hogy az adatbázis befolyásoló parancsok hajthat végre:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Az állapotot, és időbeli eltérés (ha szükséges) az összes felettes, kivonat és Replicat folyamat rendszeren:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzni a következő adatokat:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Az állapotot, és időbeli eltérés (ha szükséges) az összes felettes, kivonat és Replicat folyamat rendszeren:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Naplózza a GGSCI munkamenet egy adatbázisba, hogy az adatbázis befolyásoló parancsok hajthat végre:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

A kezelő megkezdéséhez:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. kivonat létrehozása a webhelyen A és B webhely csoport és az adatok szivattyú folyamatok

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>A webhelyen A kivonat és az adatok szivattyú folyamatok létrehozása

Meg kell a kivonat és az adatok szivattyú folyamatok létrehozása A webhelyen, és a webhely b távoli asztal helyen és a webhely B az Azure klasszikus portálon keresztül. GGSCI parancs értelmező ablak megnyitása. A következő parancsokat a webhely válasz:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzése a következő információkat: GGSCI (MachineGG1) 18 > Szerkesztés a paraméterek ext1 kivonat ext1 FELHASZNÁLÓAZONOSÍTÓ ggate jelszó ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate táblázat scott.inventory, GETBEFORECOLS (a frissítés KEYINCLUDING (prod_category, qty_in_stock, last_dml), a törlés KEYINCLUDING (prod_category, qty_in_stock, last_dml));

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzése a következő adatokat:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>A webhely B GoldenGate ellenőrzés táblázat létrehozása

Ezután kell a webhely b ellenőrzés tábla hozzáadása Ehhez kell nyissa meg a GoldenGate parancs értelmező ablakot, és futtatása:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Majd, adjon hozzá az ellenőrzés táblázat az adatbázis tulajdonosa ggate esetén:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Adja meg a jelölőnégyzet pont táblázat nevét a cél kiszolgálón, amely webhely B Ebben a lépésben a GLOBALS fájlt. A webhely B: GLOBALS fájl szerkesztése

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Ezután a CHECKPOINTTABLE paraméter hozzáfűzése a meglévő GLOBALS fájlt:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Utolsó lépésként mentse és zárja be a GLOBALS paraméter fájlt.


###<a name="add-replicat-on-site-b"></a>Adja hozzá a REPLICAT B webhelyen
Ez a szakasz ismerteti, hogyan webhely b "REP2" REPLICAT folyamat hozzáadása

A webhely B: Replicat csoport létrehozása hozzáadása REPLICAT paranccsal

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzése a következő adatokat:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>A webhely B kivonat és az adatok szivattyú folyamatok létrehozása

Ez a szakasz ismerteti, hogy miként hozhat létre egy új kivonat folyamat "EXT2" és az új adatok szivattyú folyamat "DPUMP2" webhely B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzni a következő adatokat:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzése a következő adatokat:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>GoldenGate ellenőrzés táblázat létrehozása A webhelyen

Nyissa meg az Oracle GoldenGate parancs értelmező ablakot, és az ellenőrzés tábla létrehozása:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Is szeretne adni a jelölőnégyzet pont tábla nevét a GLOBALS fájlt webhelyen válaszok parancsra.

Nyissa meg az Oracle GoldenGate parancs értelmező ablakot, és a webhely A: GLOBALS fájl szerkesztése

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Mentse és zárja be a GLOBALS paraméter fájlt.

###<a name="add-replicat-on-site-a"></a>A webhely REPLICAT hozzáadása

Ez a szakasz ismerteti, hogy miként vehet fel egy REPLICAT folyamat "REP1" webhelyen válaszok parancsra.

A következő parancsot a nevét, valamint egy pontosan a társított checkpointtable hoz létre egy Replicat csoport rep1.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Nyissa meg a Paraméterek szerkesztése paranccsal paraméter fájlt, és majd hozzáfűzése a következő adatokat:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>A webhely és a webhely B trandata hozzáadása

A táblázat szintjén kiegészítő naplózás engedélyezése a Hozzáadás TRANDATA parancs használatával. Nyissa meg az Oracle GoldenGate parancs értelmező ablakot, jelentkezzen be az adatbázis, és kattintson a Hozzáadás TRANDATA parancsot.

Távoli asztal MachineGG1, nyissa meg az Oracle GoldenGate parancs értelmező, és futtassa:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Távoli asztal MachineGG2, nyissa meg az Oracle GoldenGate parancs értelmező, és futtassa:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

A naplózási táblázat szintű kiegészítő állapotával kapcsolatos adatok megjelenítése:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>A webhely és a webhely B trandata hozzáadása

A táblázat szintjén kiegészítő naplózás engedélyezése a Hozzáadás TRANDATA parancs használatával. Nyissa meg az Oracle GoldenGate parancs értelmező ablakot, jelentkezzen be az adatbázis, és kattintson a Hozzáadás TRANDATA parancsot.

Távoli asztal MachineGG1, nyissa meg az Oracle GoldenGate parancs értelmező, és futtassa:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Távoli asztal MachineGG2, nyissa meg az Oracle GoldenGate parancs értelmező, és futtassa:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Kivonat és az adatok szivattyú folyamatok elindításához A webhelyen

Indítsa el a kivonat folyamat ext1 a webhely válasz:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Az adatok szivattyú folyamat dpump1 kezdése webhely válasz:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Információk jeleníthetők meg róluk a kivonat csoport ext1: GGSCI (MachineGG1) 32 > adatok kibontása ext1 KIBONTÁSÁHOZ EXT1 utolsó lépések 2013-11-25 08:03 állapot-ellenőrzés eltérés fut: 00:00:00 (frissített 00:00:02 korábbi) napló olvasható ellenőrzés Oracle ismételt naplók 2013-11-25 08:03:18 Seqno 6 RBA 3230720 Állapotváltozás 0.1074371 (1074371)

Az állapotot, és időbeli eltérés (ha szükséges) az összes felettes, kivonat és Replicat folyamat rendszeren:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>A webhely B kivonat és az adatok szivattyú folyamatok elindításához

B: a webhely a kivonat folyamat ext2 kezdése

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Webhely B: az adatok szivattyú folyamat dpump2 kezdése

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Az állapotot, és időbeli eltérés (ha szükséges) az összes felettes, kivonat és Replicat folyamat rendszeren:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>A webhelyen A REPLICAT-végrehajtás elindítása

Ez a szakasz ismerteti a megkezdéséhez REPLICAT "REP1" webhelyen válaszok parancsra.

A webhely válasz: a Replicat-végrehajtás elindítása

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Replicat csoport állapotának megjelenítése:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>A webhely B REPLICAT-végrehajtás elindítása

Ez a szakasz ismerteti a megkezdéséhez REPLICAT "REP2" a webhely b

A webhely B: a Replicat-végrehajtás elindítása

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Replicat csoport állapotának megjelenítése:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Ellenőrizze, hogy a kétirányú replikációs folyamat

Az Oracle GoldenGate konfigurációjának ellenőrzése, hogy sor beszúrása az adatbázisba webhely válaszokhoz távoli asztali webhely válaszokhoz Megnyitás SQL be a * plusz parancs és a Futtatás: SQL > select név, v$ adatbázisból;

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Ezután jelölőnégyzetet, ha a sor replikált-e a webhely b Ehhez a webhely b nyissa meg a távoli asztali be az SQL plusz ablakban, és futtatásával:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Új rekord webhely B: beszúrása

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

A webhely és a jelölőnégyzetet, ha a replikáció végeztek távoli asztali:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>További források
[Az Oracle Azure virtuális gép képe](virtual-machines-linux-classic-oracle-images.md)
