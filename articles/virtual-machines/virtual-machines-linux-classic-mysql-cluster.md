<properties
    pageTitle="Clusterize MySQL-terheléselosztás értékkészletet |} Microsoft Azure"
    description="Egy Linux MySQL fürt létrehozott és a klasszikus telepítési modellt a Azure-terheléselosztás, a magas elérhetőségének beállítása"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>MySQL Linux clusterize terheléselosztás készletek használata

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ez a cikk célja, feltárására és az különböző módszerekkel érhető el a Microsoft Azure felfedezése MySQL-kiszolgáló magas elérhetősége alapozó, könnyen hozzáférhető Linux-alapú szolgáltatásainak üzembe szemlélteti. Ezt a megközelítést bemutató videó [csatorna 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL)érhető el.

A megosztott semmi két-csomópont egyszeres MySQL magas elérhetősége megoldást DRBD, Corosync és szívritmusszabályzót alapján szerkezeti azt. MySQL egyszerre csak egy csomópont működik. Olvasásra és írásra a DRBD erőforrás korlátozódik is csak egy csomópont egyszerre.

Nincs szükség a virtuális megoldás, például HÉ nem óta azt adja meg a ciklikus funkciókat és a végpont észlelése, eltávolítása és biztonságos helyreállítás: a virtuális használata a Microsoft Azure kiegyensúlyozott készletek parancsot. A virtuális egy globálisan átirányítható IPv4-címet, a felhőalapú szolgáltatást első létrehozásakor a Microsoft Azure rendeli.

Vannak más lehetséges architektúrákban, többek között a NBD fürt, Percona és Galera, valamint több köztes megoldásairól, legalább egy MySQL- [Virtuális üzemi](http://vmdepot.msopentech.com)egy virtuális gép szerint. Mindaddig, amíg a megoldások egyedi csoportos vagy a szórás és a hogy bizonyos, és nem a megosztott tároló vagy több hálózati kapcsolaton, az esetek egyszerűen bevezetését tervezi a Microsoft Azure kell lennie.

Természetesen más termékek, például a PostgreSQL- és OpenLDAP hasonlóan történik a következő fürtképző architektúrákban kiterjeszthető. Például ez terheléselosztási eljárás a megosztott semmi sikeresen tesztelve volt több diaminta OpenLDAP, és a csatorna 9 blogbejegyzés megtekintés.

## <a name="getting-ready"></a>Felkészülés

Szüksége lesz egy legalább két (2) VMs létrehozhatnak érvényes előfizetés a Microsoft Azure fiók (ebben a példában XS használt), egy hálózati és alhálózat, egy affinitás csoport és az elérhetőség beállítva, valamint képes hozhat létre új VHD ugyanabban a régióban, mint a felhőbeli szolgáltatástól, és a Linux VMs szeretne csatolni.

### <a name="tested-environment"></a>Tesztelt környezet

- Ubuntu 13.10
  - DRBD
  - MySQL-kiszolgáló
  - Corosync és szívritmusszabályzót

### <a name="affinity-group"></a>Affinitás csoport

Bejelentkezés az Azure klasszikus portál Görgetés lefelé beállítások és affinitás új csoport létrehozása a megoldás egy affinitás csoport hozza létre. Hozzárendelt erőforrások létrehozott később a affinitás csoporthoz fog kiosztani.

### <a name="networks"></a>Hálózatok

Új hálózat hoz létre, és alhálózat jön létre a hálózaton belül. Csak egy /24 alhálózat belül 10.10.10.0/24 hálózat választottunk.

### <a name="virtual-machines"></a>Virtuális gépeken futó

Az első Ubuntu 13.10 virtuális Endorsed Ubuntu gyűjteménye használatával létrehozott és nevű `hadb01`. A folyamat, amely a hadb jön létre egy új felhőszolgáltatásba. Azt hívja meg ezzel a módszerrel a megosztott, a terheléselosztás természeti, amely a szolgáltatás áll elő, amikor a további erőforrások hozzáadása azt mutatja be. Az kibocsátása `hadb01` Eseménytelen és befejeződött a portálon. Végpont SSH automatikusan létrejön, és a létrehozott hálózatba ki van jelölve. Azt is beállíthatja, hogy hozzon létre egy új elérhetőségének beállítása a VMs.

Miután létrejött a első virtuális (értelemben, létrehozásakor a felhőbeli szolgáltatástól) létrehozása a második virtuális Folytatás `hadb02`. A második virtuális fogjuk is használni Ubuntu 13.10 virtuális a gyűjteményből az portálon, de azt fogja dönt, hogy egy meglévő felhőalapú szolgáltatást használ `hadb.cloudapp.net`, helyett újat hozna létre. A hálózati és elérhetőségének beállítása automatikusan választják nekünk. Zárólap SSH létrejön, is.

Mindkét VMs létrehozását követően azt fogja ajánljuk figyelmébe a SSH portot `hadb01` (22-es TCP) és `hadb02` (Azure által automatikusan hozzárendelt)

### <a name="attached-storage"></a>Csatolt tárhely

Azt az új lemez csatolása mindkét VMs, és hozzon létre új 5 GB lemezt a folyamat. A lemez fog tárolni a virtuális tárolóban a fő operációs rendszer lemez használatban. Miután lemezre hozza létre és csatolt, akkor nincs szükség az Amerikai Linux indítani, mint a kernel jelenik meg az új eszközre (általában `/dev/sdc`, érdemes `dmesg` a kimeneti)

Minden egyes virtuális Folytatás hozhat létre egy új partíciót használatával `cfdisk` (elsődleges, Linux partíciót), és írja be az új partíciót táblát. **Ne hozzon létre egy formázáshoz ezt a partíciót** .

## <a name="setting-up-the-cluster"></a>A fürt beállítása

A két Ubuntu VMs szükséges APT telepítheti Corosync, szívritmusszabályzót és DRBD. Használatával `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Nem telepíthető a MySQL adott időben** . Debian és Ubuntu telepítési parancsfájlokat a MySQL-adatkönyvtárának elindítja a `/var/lib/mysql`, de a címtárban fog felváltotta a DRBD fájlrendszer, mivel később ehhez szükséges.

Ezen a ponton azt is ellenőrizze (használatával `/sbin/ifconfig`), hogy mindkét VMs használja a 10.10.10.0/24 alhálózat címek és, hogy azok pingelése egymást név szerint. Ha azt szeretné, akkor használhatja `ssh-keygen` és `ssh-copy-id` , hogy mindkét VMs SSH kommunikálhat jelszó kérése nélkül.

### <a name="setting-up-drbd"></a>DRBD beállítása

Azt hoz létre, amely használja, az alapul szolgáló DRBD erőforrást `/dev/sdc1` kapcsol partíciót egy `/dev/drbd1` kell tudni az erőforrás ext3 formázva, és az elsődleges és másodlagos csomópontok használt. Ehhez nyissa meg a `/etc/drbd.d/r0.res` , és másolja a következő erőforrás-definíciót. Mindkét VMs tegye a következőket:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Ezt követően az erőforrás használatával inicializálni `drbdadm` a mindkét VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

És végül kattintson az elsődleges (`hadb01`) az DRBD erőforrás (elsődleges) tulajdonjogát kényszerítése:

    sudo drbdadm primary --force r0

Ha megvizsgálja/eljárás/drbd tartalmának (`sudo cat /proc/drbd`) a mindkét VMs, meg kell jelennie `Primary/Secondary` a `hadb01` és `Secondary/Primary` a `hadb02`, ezen a ponton a megoldást követik. Az 5 GB szabad a rendszer szinkronizálja a 10.10.10.0/24 hálózaton vevőknek költség nélkül.

Amikor a rendszer szinkronizálja a lemezt a fájlrendszer készíthet a `hadb01`. Tesztelési célú ext2 használatos, de az alábbi útmutatás hoz létre egy ext3 formázáshoz:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>A DRBD erőforrás csatlakoztatása

A `hadb01` most, hogy készen áll a DRBD erőforrások csatlakoztatásához. Debian és származtatott használata `/var/lib/mysql` MySQL-féle adatok könyvtár. Mert azt még nem telepítette a MySQL, hogy miként a könyvtár létrehozása, és csatlakoztassa a DRBD erőforrás. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>MySQL beállítása

Most már készen áll a MySQL telepítése `hadb01`:

    sudo apt-get install mysql-server

A `hadb02`, két lehetőség közül választhat. Mysql-kiszolgáló, amely /var/lib/mysql létrehozása, és töltse ki az új adatok címtár, és ezután folytassa a tartalomjegyzék eltávolítása telepítheti. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

A második lehetőséget, ha való áttérés `hadb02` , és telepítse a mysql-kiszolgáló nincs (telepítési parancsfájlokat a meglévő telepítés láthatja, és nem érintéses)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Ha nem segítségével DRBD most, az első lehetőséget könnyebben bár arguably kisebb elegáns. Beállítása után ez, indítsa el a MySQL-adatbázis dolgoznak. A `hadb02` (vagy amelyik a kiszolgálókat az egyik aktív, DRBD megfelelően):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Figyelmeztetés**: az utolsó nyilatkozat hatékony letiltja az alábbi táblázatban a legfelső szintű felhasználói hitelesítés. Ez a gyártási minőségű helyébe adja meg az utasításokat, és csak szemléltető célokra megtalálható.

Meg kell engedélyezése a hálózat MySQL-Ha azt szeretné, hogy a VMs kívüli lekérdezéseket, amelyhez ez az útmutató szolgál. Nyissa meg mindkét VMs `/etc/mysql/my.cnf` , és keresse meg `bind-address`, 0.0.0.0 a 127.0.0.1 módosítása. Miután mentette a fájlt, a kibocsátás egy `sudo service mysql restart` az aktuális elsődleges gépen.

### <a name="creating-the-mysql-load-balanced-set"></a>A MySQL-terhelés létrehozása kiegyensúlyozott beállítása

Azt fogja térjen vissza az-portálra, és keresse meg a `hadb01` virtuális, majd a végpontok. Hogy hozzon létre egy új végpontot, MySQL (TCP 3306) válassza a legördülő menü és osztásfeliratok *létrehozása új terheléselosztás beállítása* párbeszédpanel. A terheléselosztása végpont fog hívása `lb-mysql`. Azt hagyja a beállítások a legtöbb egyedül kivéve azt fogja csökkentő 5 (másodperc, minimális) idő

Az endpoint létrehozását követően megyünk `hadb02`, végpontok, és hozzon létre egy új végpontot, de azt választja ki `lb-mysql`, a legördülő menüből válassza a MySQL. Az ebben a lépésben az Azure CLI is használhatja.

Ez a jelenleg felkínálunk minden szükséges az a fürt kézi művelet.

### <a name="testing-the-load-balanced-set"></a>Tesztelje a betöltés kiegyensúlyozott beállítása

Vizsgálatok egy külső számítógépről hajtható végre, ebben az esetben a MySQL-jegyzetlapon, valamint az alkalmazások (például egy Azure webhelyet futtatott phpMyAdmin) azt használt MySQL-féle parancssori eszköz a másik Linux jelölőnégyzetet:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Manuális feladat átvétele

Ön mégis szimulálhatja feladatátadás most MySQL leáll, váltás DRBD meg az elsődleges és indítsa újra a MySQL.

A hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Ezután a hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Miután, feladatátvevő manuálisan kell ismételnie a távoli lekérdezést, és kell működik tökéletesen.

## <a name="setting-up-corosync"></a>Corosync beállítása

Corosync, az alapul szolgáló fürt infrastruktúra szívritmusszabályzót működéséhez szükséges. Szívveréséhez v1 és v2 felhasználók (és más módszereket, például Ultramonkey) Corosync a CRM funkciók felosztott, míg szívritmusszabályzót további hasonló Hearbeat marad a használható funkciók körét.

A fő kényszer Corosync az Azure, hogy egyedi kommunikáció fölé Corosync inkább az adás fölé csoportos, de csak a Microsoft Azure hálózati támogatja egyedi.

Szerencsére Corosync munka egyedi mód van, és a csak valós korlátozás, hogy mivel minden csomópontok egymással nem kommunikál *automagically*, meg kell határoznia a csomópontok a konfigurációs fájlokban, beleértve az IP-címét. Az egyedi, és csak módosítása kötést létrehozni a cím, a csomópont-listák és a naplózási könyvtár ábrázolásakor a Corosync példa fájlokat (Ubuntu használja `/var/log/corosync` a példában a fájlok használata közben `/var/log/cluster`) és kvórum eszközök engedélyezése.

**Megjegyzés: a `transport: udpu` az alábbi irányelv és a manuális megadott IP-címek a csomópontok**.

A `/etc/corosync/corosync.conf` mindkét csomópontok:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Hogy a konfigurációs fájl másolása a mindkét VMs és Corosync nyissa meg mindkét csomópontok a:

    sudo service start corosync

Hamarosan a szolgáltatás indítása után a fürt létre kell hozni az aktuális kicsengetés, és kvórum kell hoznak létre. Azt is jelölje be ezt a funkciót naplók megtekintésével vagy:

    sudo corosync-quorumtool –l

Célszerű követnie az olyan eredménye az alábbi képen hasonló:

![corosync-quorumtool - l minta kimeneti](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Szívritmusszabályzót beállítása

Szívritmusszabályzót a fürt figyelemmel az erőforrások, határozza meg, amikor elsődleges Ugrás lefelé, és ezek az erőforrások váltani formátumú másodlagos zónák használja. Erőforrások definiálható parancsfájlokat halmazának vagy LSB (init hasonló) parancsfájlok, más elemei között.

A "saját" az DRBD erőforrás, a csatlakozási pont és a MySQL-szolgáltatás szívritmusszabályzót szeretnénk. Ha szívritmusszabályzót kapcsolhatja be- és kikapcsolása DRBD, csatlakoztatni, /, és a kezdési és befejezési MySQL a megfelelő sorrendben, ha valami hibás fordul elő, ha az elsődleges, a telepítő umount befejeződött.

Szívritmusszabályzót első telepítésekor a konfigurációban legyen elég, egyszerű valami hasonló:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Jelölje be, ha futtatásával `sudo crm configure show`. Ezután hozzon létre egy fájlt (mondjuk `/tmp/cluster.conf`) az alábbi forrásokat:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

És most betöltése a konfiguráció (csak kell ehhez az egyik csomópontra):

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Győződjön meg arról, hogy elindítja-e szívritmusszabályzót mindkét csomópontjai rendszerindításkor:

    sudo update-rc.d pacemaker defaults

Néhány másodperc után és használatával `sudo crm_mon –L`, ellenőrizze, hogy a csomópontok egyike vált, a diaminta a fürt, összes erőforrás működik. Csatlakozási és ps segítségével ellenőrizze, hogy az erőforrások futnak.

Az alábbi kép mutatja `crm_mon` egy csomópont leállítva (Kilépés a vezérlőelem-C használata)

![leállítva crm_mon csomópont](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

És a képernyőképen mindkét csomópontok egy minta és a több kisegítő:

![crm_mon műveleti főadat/kisegítő](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Tesztelés

Hogy készen áll a egy automatikusan átveszi szimulációk. Ennek két módja van: lágy és kemény. A lágy módon használja a fürt leállítás függvény: ``crm_standby -U `uname -n` -v on``. A használja ezt a minta, a kisegítő kerül. Ne feledje, hogy ez vissza kívánja állítani kikapcsolva (crm_mon a program tájékoztatja, egy csomópont hiányos készenléti)

A merevlemez módja a elsődleges virtuális (hadb01) leáll a portálon keresztüli vagy módosítása a paraméterben megadott futtatási szint a virtuális (tehát leállítja, Leállítás) a, majd azt is segítve Corosync és szívritmusszabályzót által-jelzés mesteralakzat folyamatos lefelé. Hogy tesztelje ezt (hasznos a karbantartás windows), de azt is lehetséges a forgatókönyv csak a virtuális rögzítésével.

## <a name="stonith"></a>STONITH

A virtuális leállítása az Azure CLI helyett egy STONITH parancsfájlt, amely meghatározza a fizikai eszközök keresztül hiba kell lennie. Használható `/usr/lib/stonith/plugins/external/ssh` alap-és engedélyezése STONITH a fürt konfigurációban. Azure CLI globálisan telepítése, és a Közzététel beállításai/profil be kell tölteni a fürt felhasználó számára.

Az erőforrás elérhető [GitHub](https://github.com/bureado/aztonith)a minta kódját. A fürt konfigurálása az alábbi módon hozzáadásával módosítani kell `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Megjegyzés:** a parancsfájl nem hajtja végre, felfelé/lefelé ellenőrzések. Az eredeti SSH erőforrás volna 15 ping szempontú vizsgálatok, de az Azure virtuális helyreállítási idő további változó lehet.

## <a name="limitations"></a>Korlátozások

Az alábbi korlátozások vonatkoznak:

- A linbit DRBD erőforrás parancsfájl szívritmusszabályzót célú erőforrásként DRBD kezelő `drbdadm down` amikor leállítja a csomópont, még akkor is, ha a csomópont mindössze készenléti fog. Ez nem ideális óta a kisegítő fog nem lehet szinkronizálása az DRBD erőforrás a fő módja írás közben. Ha a mesteralakzatot nem sikerül graciously, a kisegítő keresztül egy régebbi formázáshoz állam vehet igénybe. Kétféleképpen potenciális ez megoldása:
  - Kényszerítése egy `drbdadm up r0` a via egy helyi (nem clusterized) figyelő fürt csomópontjait, vagy
  - Ügyelve arra, hogy a linbit DRBD szerkesztési parancsfájl `down` nem hívja meg, a `/usr/lib/ocf/resource.d/linbit/drbd`.
- Terheléselosztó szükséges válaszolni, így alkalmazások kell fürt tartsa szem előtt, és az időtúllépés; alternatív kell legalább 5 másodpercig más architektúrákban is segíthetnek, például az alkalmazás sorban várakozó, lekérdezés middlewares stb.
- Biztosítani kell írni egy sane tempójában befejeződött, és gyorsítótárát kiürítette a lehető nagymértékű adatvesztést memória gyakran lemezre MySQL finombeállítása
- Írja be a virtuális függhet teljesítmény interconnect a virtuális váltás, mivel ez a módszer szerint DRBD bizonyos az eszközön
