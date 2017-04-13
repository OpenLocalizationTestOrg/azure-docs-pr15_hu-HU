<properties
    pageTitle="Az Oracle-adatok Guard VMs beállítása |} Microsoft Azure"
    description="Állítsa be, és az Oracle-adatok Guard végrehajtása nagy rendelkezésre állásának és katasztrófa helyreállítás Azure virtuális gépeken lépéseinek."
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

#<a name="configuring-oracle-data-guard-for-azure"></a>Az Oracle adatok Guard az Azure konfigurálása


Ebben az oktatóanyagban bemutatja, hogyan állíthatja be és Oracle adatok őr végrehajtása nagy elérhetőségét, és a helyreállítás az Azure virtuális gépeken futó környezetben. Az oktatóprogram RAC Oracle-adatbázisok egyirányú replikációs koncentrál.

Oracle adatok Guard támogatja az adatok védelme és a helyreállítás Oracle-adatbázishoz. Érdemes egy egyszerű, a nagy teljesítményű, vészhelyreállítás, adatvédelem és a teljes Oracle-adatbázishoz magas elérhetősége Esőcsepp megoldás.

Ebben az oktatóanyagban feltételezi, hogy már rendelkezik az Oracle-adatbázis magas elérhetőségét, és a helyreállítás fogalmak elméleti és gyakorlati ismeretek. Tudnivalókért lásd: az [Oracle-webhely](http://www.oracle.com/technetwork/database/features/availability/index.html) , valamint az [Oracle-adatok Guard fogalmak és felügyeleti útmutató](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Ezeken kívül az oktatóprogram feltételezi, hogy az alábbi előfeltételek már végrehajtotta:

- A magas elérhetősége és katasztrófa helyreállítási szempontok című részben a [Oracle virtuális gép képek - egyéb megfontolások](virtual-machines-windows-classic-oracle-considerations.md) témakörben, már áttekintése. Azure jelenleg támogatja önálló Oracle-adatbázis-példányok, de nem az Oracle valós alkalmazás fürt (Oracle RAC).


- Azure ugyanarra a megadott Oracle Enterprise Edition kép platformra használatával létrehozott két virtuális gépeken futó (VMs). Ellenőrizze, hogy a virtuális gépeken futó, hogy [egy felhőalapú szolgáltatásba](virtual-machines-windows-load-balance.md) , és annak érdekében, hogy az állandó magánjellegű IP-cím fölé elérhetik egymást virtuális ugyanabba a hálózatba. Ezenkívül ajánlott a VMs helyezze az azonos [elérhetőségének beállítása](virtual-machines-windows-manage-availability.md) helyezhet őket külön hibafa tartományok és frissítése a tartományok Azure engedélyezni. Az Oracle-adatok őr csak az Oracle-adatbázis vállalati kiadásban érhető el. Az egyes 2 GB memóriát és 5 GB szabad lemezterület kell rendelkeznie. A megadott virtuális méretű platformon a legfrissebb információkért lásd: [Azure virtuális gép méretét](virtual-machines-windows-sizes.md). Ha további kötet a VMs van szüksége, további lemezt csatolhat. Megtudhatja, [hogy miként csatolni virtuális géphez adatok lemezen](virtual-machines-windows-classic-attach-disk.md)információt.



- Beállított virtuális gép neve "Gép1", az elsődleges virtuális és a készenléti virtuális "Machine2" az Azure klasszikus portált a.

- Mutasson az azonos oracle legfelső szintű telepítési útvonal az elsődleges és készenléti virtuális gépeken futó, például a környezeti **ORACLE_HOME** változó állított be `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Jelentkezzen be a Windows server a **rendszergazdák** csoport tagjának vagy a **ORA_DBA** csoport tagjának.

Ebben az oktatóanyagban lesz:

A fizikai készenléti adatbázis környezet megvalósítása

1. Elsődleges adatbázis létrehozása

2. Az elsődleges adatbázis előkészítése készenléti az adatbázis létrehozása

    1. Kényszerített naplózás engedélyezése

    2. Jelszó-fájl létrehozása

    3. Ismét a készenléti: a naplózási konfigurálása

    4. Archiválás

    5. Elsődleges adatbázis inicializálni paraméterek beállítása

Fizikai készenléti adatbázis létrehozása

1. Hozzon létre egy inicializálni paraméter fájlt készenléti adatbázis

2. Az adatbázis támogatja az elsődleges és készenléti gépeken figyelő és tnsnames konfigurálása

    1. Listener.ora mindkét bejegyzéseinek mindkét adatbázist tároló kiszolgáló konfigurálása

    2. Elsődleges és a készenléti adatbázis bejegyzéseinek tartása, állítsa be az elsődleges és készenléti virtuális gépeken tnsnames.ora. 

    3. Indítsa el a figyelő, és jelölje be a tnsping mindkét virtuális gépeken mindkét szolgáltatásokhoz.

3. A készenléti példány nomount állapotban megkezdése

4. RMAN használja, az adatbázis klónozhatja, és készenléti adatbázis létrehozása

5. Indítsa el a fizikai készenléti adatbázis felügyelt helyreállítási módban

6. A fizikai készenléti adatbázis ellenőrzése

> [AZURE.IMPORTANT] Ebben az oktatóanyagban állítsa be, és a következő hardver- és konfigurációs tesztelése:
>
>|                      | **Elsődleges adatbázis**                      | **Adatbázis**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Az Oracle megjelenés**   | Oracle11g vállalati kiadás (11.2.0.4.0) | Oracle11g vállalati kiadás (11.2.0.4.0) |
>| **Számítógép neve**     | Gép1                                  | Machine2                                  |
>| **Operációs rendszer** | Windows 2008 R2 rendszerben                           | Windows 2008 R2 rendszerben                           |
>| **Az Oracle biztonsági AZONOSÍTÓK**       | TESZT                                      | TESZT\_STBY                                |
>| **A memória**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Szabad lemezterület**       | Min 5 GB                                  | Min 5 GB                                  |

Az Oracle-adatbázishoz, és az Oracle-adatok őr későbbi kiadásokban előfordulhat, hogy néhány további módosításokat végrehajtásához szükséges. A legújabb verzió adatait dokumentációjában [Adatok Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) és az [Oracle-adatbázis](http://www.oracle.com/us/corporate/features/database-12c/index.html) Oracle-webhelyen.

##<a name="implement-the-physical-standby-database-environment"></a>A fizikai készenléti adatbázis környezet megvalósítása
### <a name="1-create-a-primary-database"></a>1. a elsődleges adatbázis létrehozása

- Hozzon létre egy elsődleges adatbázist "TESZTELÉSE" elsődleges virtuális gépen. Tudnivalókért lásd: létrehozása és konfigurálása az Oracle-adatbázishoz.
- Lásd: az adatbázis nevét, csatlakoznia kell az adatbázis SYSDBA csoport szerepkör az SQL-SYS felhasználóként * plusz parancssort, és a Futtatás a következő utasítás:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Ezután a lekérdezés a neveket a adatbázisfájlok dba_data_files rendszer nézetben:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. a fő adatbázis előkészítése készenléti az adatbázis létrehozása

Készenléti adatbázis létrehozása, előtt javasolt megfelelően van-e beállítva az elsődleges adatbázis biztosítása. A következő figyel lépéseket kell elvégeznie:

1. Kényszerített naplózás engedélyezése
2. Jelszó-fájl létrehozása
3. Ismét a készenléti: a naplózási konfigurálása
4. Archiválás
5. Elsődleges adatbázis inicializálni paraméterek beállítása

#### <a name="enable-forced-logging"></a>Kényszerített naplózás engedélyezése

Készenléti adatbázis végrehajtásához szükséges ahhoz, hogy "Kényszerített naplózás" az elsődleges adatbázisban. Ez a beállítás biztosítja, hogy akkor is, ha egy "nologging" művelet befejeződött, az hatályba naplózás elsőbbséget, és összes művelet a művelet ismétlése naplók be van jelentkezve. Ezért VÁLLALUNK arról, hogy minden, az elsődleges adatbázis be van jelentkezve, és a készenléti való replikáció az összes művelet az elsődleges adatbázis is tartalmaz. Kötelező a naplózás engedélyezése futtassa a alter database utasítás:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Jelszó-fájl létrehozása

Engedélyezni szállítási és archivált naplók az elsődleges kiszolgáló készenléti kiszolgálóra, a sys jelszót az elsődleges és a készenléti kiszolgálón azonosnak kell lennie. Ezért hozzon létre egy a fő adatbázis jelszavát fájlt, és másolja a vágólapra a készenléti kiszolgálóra.

>[AZURE.IMPORTANT] Oracle-adatbázishoz 12c használatakor van **SYSDG**, amelynek használatával felügyelheti Oracle adatok Guard új felhasználót. További tudnivalókért lásd: [változások az Oracle-adatbázishoz 12 c megjelenés](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Ezenkívül, győződjön meg arról, hogy az ORACLE\_KEZDŐLAP környezet már meghatározottak gép1. Ha nem, definiálása-környezet változóként a környezeti változók párbeszédpanelen. Ez a párbeszédpanel eléréséhez indítsa el **a segédprogram** kattintson duplán a rendszer ikonra, kattintson a **Vezérlőpult**; Ezután kattintson a **Speciális** fülre, és válassza a **Környezeti változók**. A környezet változók beállításához **Rendszer változók**csoportban kattintson az **Új** gombra. Után állítsa be a környezetet változók, zárja be a meglévő Windows parancssort, és nyissa meg egy újat.

Futtassa a következő utasítás váltani szeretne az Oracle\_kezdőlap címtár, például C:\\OracleDatabase\\termék\\11.2.0\\dbhome\_1\\adatbázis.

    cd %ORACLE_HOME%\database

Ezután hozzon létre egy jelszót segédprogrammal jelszó fájl létrehozása, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). Ugyanazon a Windows parancssorba a gép1 futtassa a következő parancsot, mint **SYS**jelszavát a jelszó értékének beállítása:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Ez a parancs létrehoz egy jelszót, nevű fájlt, PWDTEST.ora az ORACLE a\_OTTHONI\\adatbáziskönyvtár. A fájl %-os ORACLE kell másolása\_KEZDŐLAP %\\adatbázis-címtár Machine2 manuálisan.

#### <a name="configure-a-standby-redo-log"></a>Ismét a készenléti: a naplózási konfigurálása

Akkor kell, hogy az elsődleges megfelelően is fogadhasson a művelet ismétlése, amikor egy készenléti válik, állítsa be készenléti ismételt naplót. Előre létrehozása őket itt is lehetővé teszi, hogy a készenléti mégis naplók automatikusan létrejön a készenléti. Fontos készenléti ismételt naplók (SRL) állítható be az azonos méretű, a Web App mégis naplók. Az aktuális készenléti mégis naplófájlok méretének pontosan egyeznie kell az aktuális elsődleges adatbázis online mégis naplófájlok méretét.

Futtassa a következő utasítást a SQL-\*plusz gép1 a parancssor parancsot. A v$ naplófájlban mégis naplófájlok információkat tartalmazó rendszer nézet.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Ne feledje, hogy 52428800 50 megabájtnál.

Ezután, az SQL-\*plusz ablakban, futtassa a következő kimutatások készenléti mégis napló fájl új csoport hozzáadása készenléti adatbázishoz, és adjon meg egy számot, amely azonosítja a csoportot, a csoport záradék használata. Csoport számok használatának felügyelete készenléti naplófájlcsoportok egyszerűbbé teheti:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Ezután a Futtatás a következő rendszer nézet lista információ mégis naplófájlok. Ez a művelet is ellenőrzi, hogy a készenléti naplófájlcsoportok létrehozott:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Archiválás

A következő utasításokat az elsődleges adatbázis helyének ARCHIVELOG módban, és az automatikus archiválás futtatásával archiválásának ezután engedélyezése. Archiválás naplózási mód engedélyezheti csatlakoztatása az adatbázist, majd kattintson a a archivelog parancs végrehajtása.

Első lépésként jelentkezzen SYSDBA csoport. Futtassa a Windows parancssort:

    sqlplus /nolog

    connect / as sysdba

Ezután a leállítás az a SQL-adatbázis\*plusz parancssort:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Ezután hajtsa végre az indítási csatlakoztatási parancs az adatbázis csatlakoztatásához. Ezzel biztosíthatja, hogy az Oracle társít a példány a megadott adatbázis.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Ezután futtatása:

    SQL> alter database archivelog;
    Database altered.

Ezután futtassa a Alter database utasítás a megnyitott záradék elérhetővé szeretne tenni az adatbázist a szokásos használata:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Elsődleges adatbázis inicializálni paraméterek beállítása

Az adatok Guard konfigurálásához kell létrehozása és konfigurálása a készenléti paraméterek egy normál pfile (inicializálni paraméter szövegfájl) első. Amikor készen áll a pfile, kell átalakíthatja a kiszolgálón tárolt paraméter fájl (SPFile fájl).

Megadhatja, hogy a paraméterek használata a INIT adatok Guard környezetet. ORA fájlt. Ha ebben az oktatóanyagban számított, frissítenie kell az elsődleges adatbázis INIT. ORA úgy, hogy mindkét szerepkörök tulajdonosai: elsődleges vagy készenléti.

    SQL> create pfile from spfile;
    File created.

Ezután módosítani szeretne a pfile a készenléti paramétert szeretne adni. Ehhez nyissa meg a INITTEST. % ORACLE helyét a fájl ORA\_KEZDŐLAP %\\adatbázis. Ezután a következő utasítások hozzáfűzése a INITTEST.ora fájlt. A INIT az elnevezésük is egységes. A fájl ORA INIT\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Az előző utasítás időtartomány három fontos beállítási elemet tartalmazza:
-   **LOG_ARCHIVE_CONFIG...:** Meghatározhatja az egyedi adatbázis azonosítók, ez az utasítás használatával.
-   **LOG_ARCHIVE_DEST_1...:** A helyi archiválás mappa helyét, ez az utasítás használatával meghatározhatja. Azt javasoljuk, hozzon létre egy új könyvtárat az adatbázis archiválási igényeknek, és adja meg a helyi archiválás helyét utasítással kifejezetten Oracle alapértelmezett mappa % ORACLE_HOME%\database\archive használata helyett.
-   **LOG_ARCHIVE_DEST_2... LGWR ASZINKRON...:** meghatározhatja, hogy egy aszinkron naplózási író folyamat (LGWR) tranzakció mégis adatok összegyűjtése és továbbítja az készenléti célhelyre. Ebben az esetben a DB_UNIQUE_NAME adja meg az adatbázist a cél készenléti kiszolgálón egy egyedi nevet.

Ha készen áll az új paraméter fájl, kell a SPFile fájl készítése.

Az adatbázis először leállítása:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Ezután parancsot indítási nomount az alábbi képlettel történik:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Ezen a ponton hozzon spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Ezután leállítás az adatbázist:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Ezután az indítási parancs segítségével egy példány indítása:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Fizikai készenléti adatbázis létrehozása
Ebben a szakaszban a lépéseket, hogy el kell végeznie a Machine2 előkészítése a fizikai készenléti adatbázis koncentrál.

Először meg kell távoli asztal Machine2 az Azure klasszikus portálon keresztül.

Ezt követően a kiszolgálón készenléti (Machine2), hozzon létre a szükséges mappákat a készenléti adatbázis, például C:\\\<YourLocalFolder\>\\próba. Ebben az oktatóanyagban számított, miközben győződjön meg arról, hogy a mappastruktúra megfelel-e a mappastruktúra meg szeretné tartani az összes szükséges fájlt, például a controlfile, adatfájlok, redologfiles, udump, bdump és cdump gép1. Ezeken kívül definiálása az ORACLE\_OTTHONI és ORACLE\_Machine2 alap környezeti változó. Ha nem,-környezet változóként a környezeti változók párbeszédpanelen adja meg őket. Ez a párbeszédpanel eléréséhez indítsa el **a segédprogram** kattintson duplán a rendszer ikonra, kattintson a **Vezérlőpult**; Ezután kattintson a **Speciális** fülre, és válassza a **Környezeti változók**. A környezeti változók beállításához a **Rendszer változói**csoportban kattintson az **Új** gombra. A környezet változók beállítása, után kell zárja be a meglévő Windows parancssort, és nyissa meg a módosítások megtekintéséhez egy újat.

Ezután kövesse az alábbi lépéseket:

1. Hozzon létre egy inicializálni paraméter fájlt készenléti adatbázis

2. Az adatbázis támogatja az elsődleges és készenléti gépeken figyelő és tnsnames konfigurálása

    1. Listener.ora mindkét bejegyzéseinek mindkét adatbázist tároló kiszolgáló konfigurálása

    2. Az elsődleges és készenléti virtuális gépeken futó elsődleges és a készenléti adatbázis bejegyzéseinek tartása tnsnames.ora konfigurálása

    3. Indítsa el a figyelő, és jelölje be a tnsping mindkét virtuális gépeken mindkét szolgáltatásokhoz.

3. A készenléti példány nomount állapotban megkezdése

4. RMAN használja, az adatbázis klónozhatja, és készenléti adatbázis létrehozása

5. Indítsa el a fizikai készenléti adatbázis felügyelt helyreállítási módban

6. A fizikai készenléti adatbázis ellenőrzése

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. a készenléti adatbázis paraméter inicializálni fájlként előkészítése

Ez a szakasz bemutatja, hogyan előkészítése az adatbázis paraméter inicializálni fájlként. Ehhez először másolja a vágólapra a INITTEST. ORA fájl gépi 1 Machine2 manuálisan. Látnia kell a INITTEST megjelenítéséhez. Az ORACLE % ORA a fájl\_KEZDŐLAP %\\adatbáziskönyvtár mindkét géphez. Módosítsa a INITTEST.ora Machine2 az alábbiakban meghatározott készenléti szerepkör tagjának beállítja a fájlt:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Az előző utasítás időtartomány két fontos beállítási elemet tartalmazza:

-   **\*. LOG_ARCHIVE_DEST_1:** Machine2 a c:\OracleDatabase\TEST_STBY\archives mappa létrehozása manuálisan kell.
-   **\*. LOG_ARCHIVE_DEST_2:** (ez nem kötelező). Ezeket a beállításokat megadta előfordulhat, hogy lehet szükség, ha az elsődleges gépi karbantartási és a készenléti gép lesz az elsődleges adatbázis.

Ezután akkor indítsa el a készenléti példányt. Adatbázis-kiszolgálón készenléti írja be az Oracle-példány létrehozása: Windows szolgáltatás hozzon létre egy Windows parancssorba a következő parancsot:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

A **Oradim** parancs az Oracle-példányt hoz, de nem indul el, hogy. Megtalálhatja a c:\\OracleDatabase\\termék\\11.2.0\\dbhome\_1\\címtár ELEMRE.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Az adatbázis támogatja az elsődleges és készenléti gépeken figyelő és tnsnames konfigurálása
Mielőtt készenléti adatbázis létrehozása, meg kell győződjön meg arról, hogy az elsődleges és készenléti adatbázisokat a konfigurációban is beszélhet az egymást. Ehhez kell beállítani, a figyelő és a TNSNames manuálisan vagy a hálózati konfigurációja segédprogrammal NETCA. Ez egy kötelező tevékenység használata esetén a helyreállítás-kezelő segédprogram (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Listener.ora mindkét bejegyzéseinek mindkét adatbázist tároló kiszolgáló konfigurálása

Távoli asztali gép1 és a Szerkesztés alatt megadott listener.ora fájlként. A listener.ora fájl szerkesztésekor mindig ellenőrizze, hogy a nyitó és záró zárójelet egy vonalba ugyanabban az oszlopban. A listener.ora fájl megtalálható a következő mappába: c:\\OracleDatabase\\termék\\11.2.0\\dbhome\_1\\hálózati\\felügyeleti\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Következő, a távoli asztali Machine2 és szerkesztése a listener.ora fájl az alábbi képlettel történik: # listener.ora hálózati konfigurációs fájl: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Az elsődleges és készenléti virtuális gépeken futó elsődleges és a készenléti adatbázis bejegyzéseinek tartása tnsnames.ora konfigurálása

Távoli asztali gép1 és a Szerkesztés alatt megadott tnsnames.ora fájlként. A tnsnames.ora fájl megtalálható a következő mappába: c:\\OracleDatabase\\termék\\11.2.0\\dbhome\_1\\hálózati\\felügyeleti\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Távoli asztali Machine2 és szerkesztése a tnsnames.ora fájl az alábbi képlettel történik:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Indítsa el a figyelő, és jelölje be a tnsping mindkét virtuális gépeken mindkét szolgáltatásokhoz.

Nyissa meg a Windows új parancssor elsődleges és a készenléti virtuális gépeken futó, és futtassa az alábbi utasításokat:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>A készenléti példány nomount állapotban megkezdése
Az adatbázis támogatja a készenléti virtuális gépen (MACHINE2) állíthatja be a környezetet.

Első lépésként másolhatja a jelszó (gép1) elsődleges számítógépről a készenléti gép (Machine2) manuálisan. Ez a szükséges **sys** jelszavát meg kell egyezniük mindkét gépen.

Ezután nyissa meg a Windows parancssort Machine2, és beállítása a környezeti változók, mutasson a készenléti adatbázishoz az alábbi képlettel történik:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Ezután az készenléti adatbázis indítása nomount állapotú, és akkor a spfile hoz létre.

Az adatbázis indítása:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>RMAN használja, az adatbázis klónozhatja, és készenléti adatbázis létrehozása
A helyreállítási Manager segédprogram (RMAN) segítségével bármely a fizikai készenléti adatbázis létrehozása az elsődleges adatbázis biztonsági másolatot készíthet.

A készenléti virtuális gép (MACHINE2) és a Futtatás a RMAN segédprogram teljes kapcsolat megadásával távoli asztali karakterlánc, mind a cél (elsődleges adatbázis, gép1) és (készenléti adatbázis, Machine2) SEGÉDSZOLGÁLTATÁSA példányok.

>[AZURE.IMPORTANT] Ne használja az operációs rendszer hitelesítés nincs adatbázis a készenléti kiszolgáló gépen még.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Indítsa el a fizikai készenléti adatbázis felügyelt helyreállítási módban
Ebben az oktatóanyagban bemutatja, hogyan hozhat létre a fizikai készenléti adatbázist. További információkat logikai készenléti adatbázis az Oracle dokumentációjában olvasható.

Nyisson meg SQL\*plusz kérdés parancsot, és a következőképpen engedélyezése az adatok Guard a készenléti virtuális gép vagy kiszolgálón (MACHINE2):

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Amikor adatbázist nyit meg a készenléti módban **CSATLAKOZTATÁSA** az archiválási napló szállítási továbbra is fennáll, és a felügyelt helyreállítási folyamat továbbra is a napló alkalmazása készenléti-adatbázisban. Ezzel biztosíthatja, hogy az adatbázis továbbra is a elsődleges adatbázissal naprakész. Figyelje meg, hogy az adatbázis nem lesznek elérhetők jelentési célokra ez idő alatt.

Az adatbázis megnyitásakor **Csak OLVASHATÓ** módban a archívumba jelentkezzen szállítási továbbra is. De a felügyelt helyreállítási folyamat leáll. Ennek hatására a készenléti adatbázis egyre elavult lesz, amíg a felügyelt helyreállítási folyamat folytatódik. Hozzáférhet az adatbázis jelentési célból ez idő alatt, de az adatok nem feltétlenül tükrözi frissen megváltozott.

Általánosságban elmondható azt javasoljuk, az adatbázis kordában mód **CSATLAKOZTATNI** szeretné tartani az adatokat az készenléti adatbázis naprakész-e az elsődleges adatbázis hiba esetén. Jó helyen jár tárolhatja a készenléti adatbázis **Csak OLVASHATÓ** módban attól függően, hogy az alkalmazás követelmények jelentési célból. A következő lépések bemutatják, hogyan engedélyezhető az adatok őr használata SQL írásvédett módban\*plusz:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>A fizikai készenléti adatbázis ellenőrzése
Ez a szakasz bemutatja, hogyan lehet rendszergazdaként magas elérhetősége konfigurációjának ellenőrzése.

Nyisson meg SQL\*plusz parancssorablakot és archivált jelölőnégyzet ismételt napló a készenléti virtuális gép (Machine2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Nyisson meg SQL * plusz parancssor ablakban és a Váltás naplófájlok (gép1) elsődleges gépen:

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Jelölje be az archivált mégis napló a készenléti virtuális gép (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Ellenőrizze, hogy minden olyan távolság a készenléti virtuális gép (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Egy másik ellenőrzési módszer is az adatbázis áttérni és tesztelje a Ha az elsődleges adatbázis csomópontoktól lehetséges. Az adatbázis elsődleges adatbázisként való aktiválásához használja az alábbi utasításokat:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Nem engedélyezte a az eredeti elsődleges adatbázis flashback, ha azt ajánljuk, hogy az eredeti elsődleges adatbázis, és hozza létre ismét készenléti adatbázisként.

Azt javasoljuk, hogy engedélyezze az elsődleges flashback adatbázis és a készenléti adatbázisokat. Feladatátvevő történik, ha az elsődleges adatbázis fellobban vissza az átadása előtt időt, és gyorsan átalakíthatók készenléti adatbázishoz.

##<a name="additional-resources"></a>További források
[Az Oracle Azure virtuális gép képe](virtual-machines-windows-classic-oracle-images.md)
