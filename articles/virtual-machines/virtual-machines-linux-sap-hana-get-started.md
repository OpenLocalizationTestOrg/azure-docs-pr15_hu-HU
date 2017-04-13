<properties
   pageTitle="Manuális telepítés az SAP HANA Azure VMs a quickstart útmutató útmutató |} Microsoft Azure"
   description="Quickstart útmutató útmutató a Azure VMs SAP HANA manuális telepítése"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Quickstart útmutató útmutató a Microsoft single-példány SAP HANA Azure VMs a manuális telepítése

## <a name="introduction"></a>– Bevezetés

Quickstart útmutató útmutatóban állíthat be egy olyan egyetlen-példány SAP HANA prototípusának/bemutató rendszert, a Azure VMs manuális telepítés SAP NetWeaver 7.5 és az SAP HANA SP12 segítséget nyújt.
Az útmutató megvalósító, hogy ismeri a Azure IaaS alapszolgáltatásait – például a virtuális gépeken futó vagy virtuális hálózatok, akár az Azure portálon vagy a Powershell/CLI többek között a json-sablonok csoportban telepítéséről-e az olvasót. Továbbá várhatóan, hogy ismeri a SAP HANA, SAP NetWeaver és útmutatást adunk a telepítés helyszíni-e az olvasót.

Várhatóan, az SAP-Azure Alapdokumentum a cikk végén általános adatok szakaszának említett tudatában-e az olvasót.

A korlátozás nem munkakörnyezetben miatt ez az útmutató fog nem foglalkozik témaköröket HA, például biztonsági másolatot készíteni, DR, a nagy teljesítmény és a speciális biztonsági megfontolások.

A minta beállítása végezték két virtuális gépeken futó használata egy elosztott SAP NetWeaver telepítési keresztül az Azure erőforrás-kezelő modell elvégezheti az SAP-Linux-Azure csak Azure erőforrás-kezelő és a Klasszikus modell nem támogatott a szerint. Azure kezelő kapcsolatos további információkra mutató hivatkozások találhatók, ez a cikk végén az általános adatok szakaszának.

Azok a két teszt VMs a minta telepítéshez használni:

* (írjon be egy DS3) szeretné üzemeltetni a NW 7.5 ASC példány + ProClarity Analytics Server Hana-appsrv
* (írjon be egy GS4) Hana-dbsrv állomáshoz HANA SP12
* mindkét VMs tartozott, egy Azure virtuális hálózati (azure-hana-próba-vnet)
* Mindkét esetben operációs rendszer SLES 12 SP1


Legyen tudatában annak, azonban, hogy kezdve július 2016 SAP HANA csak a teljesen támogatott OLAP (FF) munkakörnyezetben Azure virtuális típus GS5. Tesztelés céljából, ahol egy van nem meglepő hivatalos SAP-támogatás, nem kell aggódnia valamit kisebb, mint például a GS4 használni.
Mi mindig kell használni az SAP HANA Azure a HANA adatokat, és jelentkezzen be fájlok - tárhelyet Azure prémium lásd: a "lemez beállítási" szakasz lejjebb. Kapcsolatban további részleteket, amelyen az SAP termékek támogat a Azure ellenőrizze az általános adatok szakaszának Ez a cikk végén.


Az útmutató kétféleképpen webhelyen való manuális telepítéséhez SAP HANA Azure VMs ismerteti:

* az "adatbázis-példány" lépésben egy elosztott NetWeaver telepítés részeként telepítse az SAP HANA keresztül SAP szoftver kiépítési Manager (SWPM)
* használja a HANA életciklus-kezelő eszköz hdblcm SAP HANA telepítése, és ezt követően telepítse NetWeaver

Akkor is SWPM használni, és egy egyetlen virtuális minden összetevő (SAP HANA, SAP alkalmazáskiszolgáló, ASC példány SAP grafikus) telepítéséhez. Ez a beállítás nem a jelen cikkben ismertetett, de az elemeket, amelyeket figyelembe kell venni megegyeznek.

A telepítés megkezdése előtt olvassa el a szakasz után a feladatlisták alatt az Azure próba VMs beállításával kapcsolatos történik a alapértelmezett Azure virtuális konfiguráció használata esetén, amelyek több egyszerű hibák elkerülése érdekében.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Feladatlista SAP HANA telepítési keresztül SAP SWPM

Ez a egy egyszerű tevékenység-ellenőrzőlistát a kulcs egyetlen-példány SAP HANA manuális telepítés bemutató vagy prototípusának elkészítéséhez pursposes elosztott SAP NW 7.5 rendszerű módon SAP SWPM keresztül a kapcsolódó elemek telepítése. Az egyes elemek magyarázata és a teljes cikk részletesen képernyőképek űrlapon látható:

* Hozzon létre egy Azure virtuális hálózat, amely a két teszt VMs foglalja 
* az operációs rendszer SLES 12 SP1 két Azure VMs üzembe Azure erőforrás-kezelő modell keresztül 
* két szabványos tároló lemez csatolása az alkalmazás kiszolgáló virtuális (pl. 75GB és 500GB)
* négy lemez csatolása a HANA adatbázis-kiszolgáló virtuális – az alkalmazás kiszolgáló virtuális például 2 szabványos tároló lemezt + 2 prémium tároló lemezt (pl. 2x512GB)
* attól függően, hogy a mérete, illetve átviteli követelmények csatolása a több lemezre, és hozzon létre vagy sávos kötet lvm vagy mdadm használatával OS szintjén a virtuális belül
* Hozzon létre XFS fájl rendszerek, a csatolt lemezen / logikai kötet
* Csatlakoztassa az új XFS fájlrendszer OS szintjén. Egy formázáshoz segítségével az SAP-szoftvert és a másik egy, például a sapmnt könyvtárat és esetleg a biztonsági másolatok. Az SAP HANA DB a kiszolgáló csatlakoztatási a XFS fájlrendszer /hana, valamint az összes szükséges, hogy a legfelső szintű formázáshoz, amely nem túl nagy a Linux Azure VMs megtelik elkerülése érdekében /usr/sap prémium tároló lemezen
* Adja meg a helyi IP-címek, a vizsgálat VMs/stb/hosts
* Írja be a nofail paraméter /etc/fstab
* az SAP HANA-SLES – 12 Megjegyzés (kernel jólétéhez szakaszában lefelé további részleteket lásd) kernel paramétereinek beállítása
* térköz beállítása felcserélése
* -volna egy grafikus desktop telepíthető a vizsgálat VMs. Egyéb esetben használata egy távoli sapinst telepítése
* az SAP-szolgáltatás piactérről SAP Szoftverletöltés
* az alkalmazás kiszolgáló virtuális az SAP ASC példány telepítése
* a sapmnt könyvtár NFS keresztül között a vizsgálat VMs megosztása (alkalmazáskiszolgáló virtuális az NFS-kiszolgáló)
* többek között a via SWPM HANA a virtuális adatbázis-kiszolgáló adatbázis-példány telepítése
* az alkalmazás kiszolgáló virtuális a ProClarity Analytics Server telepítése
* SAP MC indítása és csatlakozás pl. keresztül SAP grafikus / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Feladatlista SAP HANA telepítési keresztül hdblcm

Ez a egy egyszerű tevékenység-ellenőrzőlistát a kulcs egyetlen-példány SAP HANA manuális telepítés bemutató vagy prototípusának elkészítéséhez pursposes elosztott SAP NW 7.5 rendszerű módon SAP SWPM keresztül a kapcsolódó elemek telepítése. Az egyes elemek magyarázata és a teljes cikk részletesen képernyőképek űrlapon látható:

* Hozzon létre egy Azure virtuális hálózat, amely a két teszt VMs foglalja 
* az operációs rendszer SLES 12 SP1 két Azure VMs üzembe Azure erőforrás-kezelő modell keresztül 
* két szabványos tároló lemez csatolása az alkalmazás kiszolgáló virtuális (pl. 75GB és 500GB)
* négy lemezt csatolni virtuális - 2 szabványos tárterület például az alkalmazás kiszolgáló virtuális + 2 prémium tároló lemezt (pl. 2x512GB) HANA adatbázis-kiszolgálóra
* attól függően, hogy a mérete, illetve átviteli követelmények csatolása a több lemezre, és hozzon létre vagy sávos kötet lvm vagy mdadm használatával OS szintjén a virtuális belül
* Hozzon létre XFS fájl rendszerek, a csatolt lemezen / logikai kötet
* Csatlakoztassa az új XFS fájlrendszer OS szintjén. Egy formázáshoz segítségével az SAP-szoftvert és a másik egy, például a sapmnt könyvtárat és esetleg a biztonsági másolatok. Az SAP HANA DB a kiszolgáló csatlakoztatási a XFS fájlrendszer /hana, valamint az összes szükséges, hogy a legfelső szintű formázáshoz, amely nem túl nagy a Linux Azure VMs megtelik elkerülése érdekében /usr/sap prémium tároló lemezen
* Adja meg a helyi IP-címek, a vizsgálat VMs/stb/hosts
* Írja be a nofail paraméter /etc/fstab
* az SAP HANA-SLES – 12 Megjegyzés (kernel jólétéhez szakaszában lefelé további részleteket lásd) kernel paramétereinek beállítása
* térköz beállítása felcserélése
* Ha - grafikus asztali telepítése a vizsgálat VMs. Egyéb esetben használata egy távoli sapinst telepítése
* az SAP-szolgáltatás piactérről SAP Szoftverletöltés
* a HANA DB Server virtuális 1001 csoportazonosító csoport "sapsys" létrehozása
* SAP – HANA telepítése az adatbázis-kiszolgáló virtuális hdblcm használatával
* az alkalmazás kiszolgáló virtuális az SAP ASC példány telepítése
* a sapmnt könyvtár NFS keresztül között a vizsgálat VMs megosztása (alkalmazáskiszolgáló virtuális az NFS-kiszolgáló)
* többek között a via SWPM HANA a virtuális adatbázis-kiszolgáló adatbázis-példány telepítése
* az alkalmazás kiszolgáló virtuális a ProClarity Analytics Server telepítése
* SAP MC indítása és csatlakozás pl. keresztül SAP grafikus / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Azure VMs előkészítése az SAP HANA manuális telepítése

Tudnivalók az Azure VMs előkészítése az SAP HANA manuális telepítés fejezet öt szakaszból, amely magában foglalja a következő témakörök áll:

* Lemezen beállítása
* Kernel paraméterei
* FILESYSTEMS
* hosts/stb /
* / stb/fstab


### <a name="disk-setup"></a>Lemezen beállítása

A legfelső szintű formázáshoz, a egy Azure virtuális Linux gép korlátozott méretű van. Így szükség egy virtuális SAP futó további lemezterületet csatolása. Az SAP alkalmazás kiszolgáló virtuális tiszta prototípusának/bemutató környezetben használt esetében, nem kell aggódnia Azure szabványos tároló lemezt használni. Mivel az SAP HANA szabványú adatbázisból származó adatok, és jelentkezzen be fájlok - Azure prémium tároló lemezre még akkor is, egy tesztkörnyezetben fekvő kell használni.

Hogyan csatolhat lemezt egy Linux virtuális néhány részleteit [Itt](virtual-machines-linux-add-disk.md)

Amikor Azure lemez gyorsítótárazás – egy "Nincs" a HANA tranzakció naplók tárolására szolgáló lemez kell használnia. HANA adatfájlok hagyható használatához olvassa el a gyorsítótár. HANA formátumát, attól függ, hogy a teljes használatát mintát Azure lemez szint mennyi olvasási gyorsítótára a memóriában adatbázissá hatékonyabbá tehetők a teljesítmény (pl. HANA kezdési és adatok beolvasása a lemezről történő memória).

Azure prémium tároló részleteit [Itt](../storage/storage-premium-storage.md)

[Itt](https://github.com/Azure/azure-quickstart-templates) megtalálta hozhat létre VMs json mintasablonok.
A "101 virtuális-egyszerű – linux" látható, hogy miként alapsablont szeretne például többek között a tárhely rész, amely 100GB adatot lemezen felveszi.

[Ez a cikk](virtual-machines-linux-sap-on-suse-quickstart.md) a Powershell, illetve CLI SUSE képként keresése információkat és a sürgős keresztül UUID lemezen csatolhat tartalmazza.


A rendszer és átviteli követelmények méretétől függően szükség lehet csatolni egy helyett több lemezre, és később hozzon létre egy adott lemezre OS szintjén beállítása csíkok. A két szempontból miért egyik hozna létre egy csíkok több Azure lemezre beállítása a következők:

* átviteli növelése
* egy egyetlen formázáshoz > 1TB van szüksége, mint Azure lemezre méretkorlátot 1TB (kezdve 2016 július)


A két fő eszközök csíkozást konfigurálása kapcsolatos további információkért található:

[Mdadm segítségével történő beállításáról a Linux rendszerhez szoftver raid az Azure virtuális a cikk](virtual-machines-linux-configure-raid.md)

[Egy Linux Azure virtuális logikai mennyiségi Manager beállításával kapcsolatos cikk](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

A próba környezet két Azure szabványos tároló lemez az SAP-app kiszolgálója virtuális csatolt volt. Egy használt tárolásához (például NetWeaver 7.5, SAP grafikus, SAP HANA... telepítéshez SAP-szoftverek ), a másik van elég hely minden szükséges lehet (például biztonsági másolatot készíteni, tesztadatokat) és az SAP azonos fekvő tartozó összes VMs között megosztani kívánt sapmnt könyvtár (pl. SAP profil).

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

Az alkalmazás kiszolgáló ellentétes virtuális négy lemezen az SAP HANA kiszolgáló virtuális volt csatolt. Például a két lemez használtak, hagyja az SAP szoftver (egy is oszthatják meg az SAP szoftver lemez NFS keresztül), és problémákat elég hely, például a biztonsági mentés előtt. A további két lemezen Azure prémium tároló lemezre SAP HANA adatokat, és jelentkezzen be a fájlokat, valamint a /usr/sap könyvtár megtartása volt.


### <a name="kernel-parameters"></a>Kernel paraméterei


SAP – HANA szükséges Linux kernel bizonyos beállításokat, amelyek nem vesznek részt a szabványos Azure gyűjtemény képeket és kell manuálisan kell beállítani. Nincs Megjegyzés SAP, amelynek a beállításait ismerteti. 


SAP – Megjegyzés SAP HANA DB: Ajánlott SLES 12 OS beállításai / SLES az SAP-alkalmazások 12: [SAP Megjegyzés 2205917](https://launchpad.support.sap.com/#/notes/2205917)

Egy SAP HANA futó SLES kapcsolatos további témakör reagrding-gyorsítótár megtalálható [Itt](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) fejezet 6.1 Kernel:-gyorsítótár korlát

Is van a lap-gyorsítótár korlát [SAP Megjegyzés 1557506](https://service.sap.com/sap/support/notes/1557506) vonatkozó SAP megjegyzést

SLES 12 rendelkezik egy új eszköz, amely felváltotta a régi sapconf segédprogramot. "Beállítva-adm" és különleges SAP HANA profil áll rendelkezésre. Egy található ez az eszköz az alábbi hivatkozásokat követve olvashat bővebben.

SLES dokumentációjában az sap-hana adm beállítva profil megtalálható [Itt](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

Beállítva adm SLES dokumentációjában az sap-hana - profil javítása rendszerek fejezet 6.2 beállítva adm - az SAP-Munkaterhelésekből található [Itt](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Itt egyik láthatja, hogyan "beállítva-adm" megváltozott a transparent_hugepage, valamint a szükséges SAP HANA megfelelően numa_balancing értékeket.


Annak az SAP HANA kernel beállításainak állandó senkinek SLES 12 grub2 használata. További információt a grub2 megtalálható [Itt](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

A képernyőképen hogyan kernel beállításainak volt a konfigurációs fájl megváltozott és majd "lefordított" grub2-mkconfig keresztül


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Másik lehetőségként a via Yast és a indítási betöltő kernel paraméter beállításainak módosítása lenne.


### <a name="filesystems"></a>FILESYSTEMS 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Itt láthatja egy a kiszolgálón SAP alkalmazás virtuális fölött a két csatolt Azure szabványos tároló lemezen létrehozott két fájlrendszer. Mindkét filesystems típusú XFS és /sapdata és /sapsoftware csatlakoztatva.

Még nem feltétlenül ebben az esetben kell. Másik lehetőség a lemezterületet struktúrájához hogyan.
A legfontosabb méretarány a legfelső szintű formázáshoz elfogy a hely elkerülése érdekében. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Az SAP HANA DB virtuális kapcsolatos fontos tudni, hogy egy adatbázis telepítéskor sapinst (swpm) keresztül, amely csak egyszerű "tipikus" telepítési lehetőséghez telepíti elemek alapértelmezés szerint /hana és /usr/sap. Az alapértelmezett SAP HANA napló biztonsági másolatának pl. /usr/sap alatt áll.
Mielőtt kulcsról, így elkerülheti a legfelső szintű formázáshoz elfogy a hely hasonló. Ezért egyik győződjön meg arról, hogy van-e elég szabad hely a /hana és /usr/sap swpm keresztül telepíthető az SAP HANA előtt.

SAP – [Ez a cikk](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) ismerteti az SAP HANA szabványos formázáshoz elrendezését 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Ha telepíti az SAP NetWeaver egy szabványos SLES 12 Azure gyűjteménye, hogy nincs-e felcserélése lemezterület üzenet lesz. Ez az üzenet volna egy sikerült pl. manuálisan felcserélése fájl hozzáadása a dokumentum nn, mkswap és swapon ismertetett módon. Csak keresése "Hozzáadása egy felcserélése fájl manuálisan" [Ez a cikk](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Egy másik, hogy konfigurálása felcserélése helyet a Linux virtuális agent keresztül. További részletek találhatók [Itt](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>hosts/stb /

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Másik fontos szempontja SAP telepítés megkezdése előtt is vegye fel a/Etc/Hosts fájlba állomásnevek és az SAP VMs IP-címét. Az egyik kell egy Azure virtuális hálózaton belül SAP VMs telepítése, és alkalmazza a belső IP-címek.

### <a name="etcfstab"></a>/ stb/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

A tesztelés fázisban meg kapcsolni a nofail paraméter hozzáadásának lehetőségét fstab célszerű lehet. Ha valamilyen probléma azzal, hogy a lemez a virtuális szeretné továbbra is lyncen, majd a indítási folyamat nem lefagyhat. Ha pedig egy kell figyelni ahogy ebben az esetben, a további lemezterület nem érhető el és folyamatok sikerült töltse fel a legfelső szintű formázáshoz. Abban az esetben, ha /hana lenne hiányzó SAP HANA, hogy egyáltalán nem indítsa el.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>A grafikus Gnome asztali telepítése SLES 12

A fejezet két secitions, amely magában foglalja a következő témakörök áll:

* Gnome asztali és a SLES 12 xrdp telepítése
* Java-alapú SAP MC használata Firefox SLES 12 fut

Egy Xterminal, VNC például alternatívák is használhatja, de szept 2016 kezdve ez a cikk csak ismerteti xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Gnome asztali és a SLES 12 xrdp telepítése

Különösen azok számára, akik a Microsoft Windows hátteret, és közvetlenül elérhető jelentését az SAP Linux VMs grafikus asztali használni a Firefox, Sapinst, SAP grafikus, SAP MC vagy HANA Studio futtatása és esetleg csatlakoztatása a virtuális RDP keresztül egy Microsoft Windows rendszerű számítógépen van cél egyszerűen. Ez nem lehet például adatbázis-kiszolgálóval megfelelő azt is ok tiszta prototípusának/bemutató környezetben. Az alábbiakban a lépések követésével telepítse az Gnome asztali az Azure SLES 12 virtuális meg:

Telepítse az gnome asztali (például a gitt ablak) a következő parancsot:

   egyszerű gnome - t mintázat zypper

Telepítse a xrdp való csatlakozáshoz a virtuális RDP keresztül:

   a xrdp zypper

/etc/sysconfig/windowmanager szerkesztése, és állítsa az alapértelmezett windows-kezelő Gnome:

   DEFAULT_WM = "gnome"

Győződjön meg arról, hogy xrdp újraindítása után automatikusan elindul az chkconfig futtatása: 

  chkconfig-3-as szintű xrdp a

abban az esetben, ha a RDP kapcsolat problémát kell próbálja meg újraindítani (például házon kívül gitt ablak):

  /ETC/xrdp/xrdp.SH újraindítása

abban az esetben, ha az xrdp újraindítást, mint a fent említett nem működik a jelölőnégyzetet, ha van .pid fájl, és távolítsa el azt:

  Jelölje be a /var/run, és keresse meg a xrdp.pid   
  Távolítsa el azt, és próbálkozzon újra az újraindítás



### <a name="sap-mc"></a>SAP – MC


A grafikus Java-alapú SAP MC ki a Gnome telepítése után az Azure SLES 12 virtuális futó Firefox indításához asztali az egyik előző szakaszban leírtak szerint kap egy hiba miatt hiányzó Java-tallózó beépülő modul.

Az URL-cím, az SAP MC indításához <server>: 5 < instance_number > 13-mal

További részletek találhatók [Itt](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

A fenti a képernyőképet egy megtekintheti, hogyan hibaüzenet ábrája a táblagéptől mikor a Java-tallózó bővítmény nem található. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

A probléma megoldására egy lehetőséget is egyszerűen telepítse a hiányzó bővítményt Yast keresztül.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Az SAP MC URL-cím ismétlődő kéri a beépülő modul aktiválása egy kis párbeszédpanel az első alkalommal be állapotba kerül.


Előfordulhat, hogy bukkan fel, amelyek egy további probléma a hiányzó fájl kapcsolatos hibaüzenet jelenik: valószínű nagyon Java 1.8, amelyre szükség van a SAP grafikus 7.4 telepítésével kapcsolatos javafx.properties

A via Yast látható IBM Java verziója nem tartalmazza a ezt a fájlt. A megoldás, Oracle Java letölthető.
Egy cikk, amely folytatott beszélgetést, erről a problémáról megtalálható [Itt](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Egy NetWeaver 7.5 telepítés részeként manuális SAP HANA-telepítés SWPM keresztül


Képernyőképek az alábbi lista bemutatja, a kulcs az SAP NetWeaver 7.5 és az SAP HANA SP12 SWPM (sapinst) keresztül. Egy NW 7.5 részeként telepítési SWPM tartalmaz a lehetőségeket is telepítheti a HANA adatbázis egyetlen példányt.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

A minta vizsgálathoz környezet egyetlen ABAP app kiszolgálója lett telepítve. A beállítás "Elosztott rendszer" telepítenie az ASC-példányt, mind az elsődleges alkalmazás server-példányt az egyik Azure virtuális és az SAP HANA egy másik Azure virtuális az adatbázis-rendszer használták.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Miután az ASC példány telepítve van az alkalmazás kiszolgálón virtuális, és állítsa a az SAP MC megosztandó virtuális SAP HANA adatbázis-kiszolgálóval rendelkezik a sapmnt könyvtár, például az SAP profil könyvtárában tartalmazó "zöld".
A KCS2 telepítési lépésben a adatokhoz való hozzáférés szükséges. A legjobb úgy, hogy használata NFS Yast használatával állítható be.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Az alkalmazás a kiszolgáló a sapmnt könyvtár virtuális kell oszthatók meg keresztül NFS "no_root_squash" és "rw" beállítások használatával. "Ro" és "root_squash", amely problémák vezethet, ha telepíti az adatbázis-példányt az alapértelmezett érték.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Az SAP HANA adatbázis-kiszolgáló virtuális a sapmnt ossza meg a alkalmazás kiszolgálóról virtuális (pl. segítségével Yast) kell beállítani a "NFS-ügyfél" keresztül van.


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Ezután senkinek az SAP HANA adatbázis-kiszolgáló virtuális bejelentkezni, és elvégezheti a következő lépés a elosztott NW 7.5 telepítés – az "Adatbázis-példány" SWPM indításához.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Nem valójában túl nagy "szokásos" telepítés kiválasztása után írja be az SAP HANA telepítésével kapcsolatos. Emellett elérési útját egy installatiom médiafájlokat rendelkezik adja meg a biztonsági AZONOSÍTÓT DB, az állomásnév, a példány számot és a KCS2 Sys rendszergazdai jelszavát.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Következő lépésként írja be jelszavát a DBACOCKPIT sémában.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Ezután megtalálható a kérdést a SAPABAP1 séma jelszót.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Végén kell csak zöld jelölők a KCS2 telepítési folyamatot minden szakaszát elé, és a kis üzenetpanel, amely megjelenik kell üzenetsávján a "végrehajtás … Adatbázis-példány befejeződött".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

A sikeres telepítése után az SAP MC is jelenjen meg az adatbázis-példányt, "zöld" és az SAP HANA folyamatok (például hdbindexserver, hdbcompileserver) teljes listája


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

A képernyőképen a /hana/shared, amely SWPM HANA telepítése során létre fájlszerkezet részei. Nem volt lehetőség különböző elérési útja. Ezért fontos, így további lemezterületet /hana előtt az SAP HANA telepítési keresztül SWPM a legfelső szintű formázáshoz elfogy a szabad terület elkerülése érdekében a csatlakoztatni.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Itt se láthassa a célt szolgálja előtt a /usr/sap könyvtár című témakörben leírt módon.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Az elosztott ABAP telepítés utolsó lépésként az "elsődleges alkalmazás Server-példányt"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

A ProClarity Analytics Server, valamint a SAP grafikus használ telepítése után egy ellenőrizheti a tranzakció "dbacockpit", amely minden jobbra és az SAP HANA telepítési keresztül.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

A módosított első lépés esetleg az SAP-app kiszolgálója virtuális SAP HANA Studio telepítése, és az SAP HANA példányához virtuális DB kiszolgálón futó.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Manuális SAP HANA telepítés HANA életciklus-kezelő eszköz hdblcm keresztül


SAP HANA telepítése SWPM keresztül egy elosztott telepítés részeként mellett található is először telepítenie kell HANA önálló hdblcm használatával, és telepítse pl. SAP NetWeaver 7.5 később possibe. Az alábbi képernyőképek listáját jeleníti meg, hogyan működik.

Íme három információforrások a HANA hdblcm eszközről:

[A helyes SAP HANA HDBLCM a tevékenység kiválasztása](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP – HANA életciklus-kezelő eszközök](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP – HANA Server telepítési és frissítés útmutató](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Problémákat tapasztal, később az alapértelmezett csoport azonosító beállítása futó elkerülése érdekében a \<HANA biztonsági AZONOSÍTÓK\>adm felhasználói (az hdblcm eszközzel létrehozott) egy új csoport neve "sapsys" csoport azonosítójú 1001 hdblcm használatával SAP HANA telepítése előtt meg kell határoznia.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

1 "a telepítés új rendszer" lesz egy egyszerű start menüjében, ha rendelkezik egy elem kijelölése hdblcm az első alkalommal indítása



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Ez a kép egy a fő beállítások előtt írt be, amelyek látható. Fontos - könyvtárak amelynek voltak elnevezése HANA naplókban és adatok kötet, valamint a telepítési hely elérési útja (/ hana/a következő példában a megosztott), és/usr/sap nem a legfelső szintű formázáshoz részének kell lennie, de az Azure adatok lemezre, amely a virtuális mintha csatlakozna, az Azure virtuális beállítása szakaszban leírtak tartoznak. A kockázat, a legfelső szintű formázáshoz fogyni előfordulhat, hogy a helyet, így elkerülhető.
Egy is megtekintheti, hogy a HANA felügyeleti felhasználó 1005 felhasználói azonosító, és a sapsys csoport (1001-azonosító), amely a telepítés előtt adták része.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Egy ellenőrizheti a HANA \<HANA biztonsági AZONOSÍTÓK\>/stb/passwd felhasználó adatait adm (Ez a példa a azdadm)



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

SAP HANA hdblcm használatával telepítése után azt is láthatja az SAP HANA Studio eszközben. A SAPABAP1 séma pl. összes SAP NetWeaver táblát tartalmazó egyelőre nem érhető el.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

SAP – HANA telepítése után egy telepítheti SAP NetWeaver fölött. Ez a példa azt végezték használata "elosztott telepítés" keresztül SWPM a megfelelő fenti szakaszban leírt módon.
Ha csak a via SWPM egy adatbázis-példány telepítése rendelkezik egyező adatok bevitele, mielőtt a hdblcm (pl. hostname (állomásnév), HANA biztonsági AZONOSÍTÓK, példány számot). SWPM majd használhatja a meglévő HANA telepítési és további sémák hozzáadása.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Ez a SWPM telepítési lépés a kép azok egy nem adja meg az adatokat a DBACOCKPIT séma kapcsolatban.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Kattintson a SWPM vár adja meg az adatokat a SAPABAP1 séma ismertetése.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Az adatbázis-példány SWPM telepítés befejeződése után se láthassa a SAPABAP1 séma HANA Studio alkalmazásban.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

És végül az SAP alkalmazás kiszolgálót és az SAP grafikus telepítése után egy kell lennie az HANA DB példány tranzakció "dbacockpit" ellenőrizheti.




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Általános információk SAP Azure-tanúsítványok SAP HANA futó Azure és az SAP szoftver letöltése

* a Windows operációs rendszer Azure Klasszikus módú az SAP futó kapcsolatos általános SAP Azure dokumentum: [SAP használata Windows virtuális gépeken Azure-ban](virtual-machines-windows-classic-sap-get-started.md)

* információ a meglévő SAP-sablonok a vevők használatát: [Azure quickstart útmutató sablonok az SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* SAP futó Linux OS az Azure Azure erőforrás-kezelő modell kapcsolatos általános SAP Azure dokumentum: [Segítségével SAP Linux (VMs) virtuális gépeken](virtual-machines-linux-sap-get-started.md)

* SAP HANA hardver tanúsított címtár, amely felsorolja a támogatott Azure virtuális milyen gyártási: [Tanúsítvánnyal SAP HANA® hardver-címtár](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* információ a virtuális gép méretű, különösen a Linux munkaterhelésekből: [az Azure virtuális gépeken futó méretben](virtual-machines-linux-sizes.md)

* SAP megjegyzés, amely felsorolja az összes támogatott SAP-termékek Azure és Azure virtuális típusok támogatja az SAP: [SAP Megjegyzés 1928533](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP SAP megjegyzést "fokozott figyelése" az Azure a Linux VMs: [SAP Megjegyzés 2191498](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP – HANA Azure "Nagy példányok" kínál. Ha meg szeretné érteni, hogy ez nem SAP HANA futó Azure VMs kapcsolatban, de a hibrid környezet hol az SAP-alkalmazás kiszolgálók fut-e az Azure VMs, de az SAP HANA fut a gépi kiszolgálókon fontos: [SAP Megjegyzés 2316233](https://launchpad.support.sap.com/#/notes/2316233/E)

* SAP jegyzeteit Linux SAPOSCOL kapcsolatos információk: [SAP Megjegyzés 1102124](https://launchpad.support.sap.com/#/notes/1102124/E)

* Mértékek figyelése a Microsoft Azure SAP kulcs: [SAP Megjegyzés 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Azure kezelő kapcsolatos információk: [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)

* Sablonok keresztül Linux VMs telepítéséről: [Deploy és Azure erőforrás-kezelő sablonok és az Azure CLI használatával kezelheti a virtuális gépeken futó](virtual-machines-linux-cli-deploy-templates.md)

* Azure erőforrás-kezelő és klasszikus között modellek telepítési összehasonlítása: [Azure erőforrás-kezelő klasszikus telepítési összehasonlítása: az erőforrások állapotát és üzembe modellek megértéséhez](../resource-manager-deployment-model.md)

* Töltse le NetWeaver 7.5 Linux/HANA az SAP-szolgáltatás Marketplace webhelyről:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Töltse le a HANA SP12 Platform Edition az SAP-szolgáltatás piactér:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

