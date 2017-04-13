<properties
    pageTitle="Cassandra futtatni Linux Azure |} Microsoft Azure"
    description="Hogyan Cassandra fürt futtatásához Linux az Azure virtuális gépeken futó Node.js alkalmazásból"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Cassandra Linux Azure fut, és használja a Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>– Áttekintés
Microsoft Azure szolgáltatás egy megnyitása a felhőből platform, mind a Microsoft futó operációs rendszerek, alkalmazáskiszolgálók, üzenetben köztes, valamint SQL és NoSQL adatbázisok mindkét kereskedelmi, és nyissa meg a forrás modellekből tartalmazó jól, nem Microsoft-szoftverek. Összeállítását, többek között az Azure nyilvános felhőket rugalmassá szolgáltatások és elő kell készítenie ügyeljen szándékos architektúra mindkét alkalmazások kiszolgálók jól tároló rétegek szerint. Cassandra's elosztott tároló architektúra természetesen segíti a könnyen hozzáférhető rendszerek, amely fürt hibák hibatűrők épület. Cassandra cassandra.apache.org; a Apache szoftver Foundation által fenntartott NoSQL adatbázis felhő skálával Cassandra Java írott, és így futtatja a Windows, valamint a Linux mindkét, platformokon.

Ez a cikk elsősorban szeretne megjeleníteni a Ubuntu Cassandra telepítési feljebb helyezése a Microsoft Azure virtuális gépeken futó és virtuális hálózatokat egyetlen és több adat központ fürt. A gyártási optimalizált munkaterhelésekből fürt telepítésének megegyezik kívül a jelen cikk hatókör több szabad csomópont konfigurálása, a megfelelő csengetés topológia tervezés és a szükséges replikációs, adatok összhangot, átviteli és magas rendelkezésre állási követelmények adatmodellezése igényel.

Ez a cikk érvényesek, mi megjelenítése alapvető megközelítése részt a Cassandra fürt épület Docker, tölthetők le vagy, amelynek a műveletet egyszerűbbé teheti az infrastruktúra telepítési Puppet képest.  

## <a name="the-deployment-models"></a>A telepítési modellek
Microsoft Azure hálózat lehetővé teszi, hogy a központi elszigetelt magánjellegű fürt, lehet, hogy, amelyek az access korlátozott finom nyomtatott hálózati biztonsági eléréséhez.  Mivel ez a cikk a Cassandra telepítésének alapvető szinten lévő megjelenítésével kapcsolatos, azt nem összpontosít konzisztencia szintet és a teljesítmény optimális tárhely megtervezése. Az alábbiakban a hálózati a hipotetikus fürt követelményei:

- Külső rendszerek nem tud hozzáférni a Cassandra adatbázist belüli vagy kívüli Azure
- Cassandra fürt működnie kell az mögött egy terheléselosztó thrift forgalmának beállítása
- Az egyes adatközpont egy továbbfejlesztett fürt elérhetőség két csoportok Cassandra csomópontok terjesztése
- Zárolni a fürt úgy, hogy csak az alkalmazás kiszolgálófarm fér hozzá az adatbázis közvetlenül
- Eltérő SSH nyilvános hálózati végpontok
- Az egyes Cassandra csomópontok szüksége van egy rögzített belső IP-cím

Cassandra elvégezhető egyetlen Azure területére, vagy a terhelést a elosztott jellegét alapján több területre. Több elem régió telepítési modell valószínűleg kihasználják felhasználóknak közelebbi egy adott földrajzi keresztül az azonos Cassandra infrastruktúra szolgálnak. Cassandra meg beépített csomópont replikációs tart a szinkronizálás több fő ír több adatközpontokban származó és bemutatja az adatok alkalmazások egységes nézet. Több elem régió telepítési is segíthetnek a, a szélesebb Azure service kimaradások a kockázat kezelési. Cassandra's hangolható egységesebb és topológiát segítséget nyújt az értekezlet-alkalmazások igényekkel Készletben.

### <a name="single-region-deployment"></a>Egyetlen régió telepítési
Hogy egyetlen régió telepítéshez indítása, és több területre adatmodell létrehozása az learnings harvest. Azure virtuális hálózat elkülöníteni alhálózat létrehozásához, hogy az imént említett lekérdezéstípusokról hálózati biztonsági követelményeknek is teljesülnie lesz.  A folyamat létrehozása a egyetlen régió példányban ismertetett használja Ubuntu 14.04 LTS és Cassandra 2.08; azonban a folyamat is egyszerűen elfogadni a Linux változatok. Az alábbiakban néhány egyetlen régió helyezési rendszeres jellemzőit.  

**Magas elérhetősége:** A szám 1 látható Cassandra csomópontok telepítik két elérhetőségének beállítása, hogy a csomópontok több hibafa tartományok magas elérhetőség között helyezkednek. Minden egyes elérhetőségének beállítása attribútummal VMs 2 hibafa tartományok van rendelve.  Microsoft Azure használja, amely a hibafa közben, amely a frissítési tartomány (például a állomás vagy Vendég OS javítása és frissítések, az alkalmazás frissítése) (például hardveres és szoftveres hibák) a váratlan lefelé időbeosztás kezelése a tartomány kezelésére, ütemezett idő lefelé használt. [Vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége](http://msdn.microsoft.com/library/dn251004.aspx) is találhat hibafa szerepe, és frissítése a tartományokat magas elérhetősége elérése.

![Egyetlen régió telepítési](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Ábra 1: Egyetlen régió telepítési

Figyelje meg, hogy az írás időben Azure nem engedélyezi a közvetlen hozzárendelése egy adott hibafa tartományra; VMs csoportja tehát még és a telepítés modellt, az 1, valószínű statisztikailag, hogy a virtuális gépeken futó két hibafa tartomány helyett négy lehet rendelni.

**Thrift forgalom terheléselosztás:** Ügyfél-tárak Thrift belül az érintett webkiszolgálóra a fürt keresztül egy belső terheléselosztó csatlakozni. Ehhez a való felvételét a belső terheléselosztó "adatok karakterláncot" az alhálózathoz (lásd az ábrát 1) a felhőbeli szolgáltatástól szolgáltatója a Cassandra fürt környezetében. A belső terheléselosztó is meg van határozva, miután a minden csomópont szükséges hozzá kell adni a korábban beállított betöltés terheléselosztó nevű terheléselosztása csoportja széljegyzetekkel terheléselosztása végpontot. [Azure belső terheléselosztás ](../load-balancer/load-balancer-internal-overview.md)talál további információt.

**Mag fürt:** Fontos mag legtöbb könnyen hozzáférhető csomópontjait válassza az új csomópontokat fog kommunikálni a seed (mag) csomópontot a fürt topológiájának fel. Egyetlen hiba pont elkerülése érdekében az egyes elérhetőségének beállítása egy csomópont rendező csomópontok van kijelölve.

**Varianciaanalízis ismétlések és egységesebb szint:** Cassandra a beépített magas rendelkezésre állás és az adatok tartóssági jellemzi a replikáció tényező (RF – a példányszámot, minden egyes sor a fürt-on tárolt), és egységesebb szint (az olvasott/írt legyen az eredmény visszatér a hívó kópiák száma). Varianciaanalízis ismétlések KEYSPACE (hasonlóan egy relációs adatbázisban) létrehozása során van megadva, mivel a konzisztencia szint közben a CRUD lekérdezés kiállító van megadva. [Konzisztencia konfigurálása](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) konzisztencia részletekért és kvórum számítási képlete Cassandra dokumentációjában találhat.

Cassandra támogatja az adatok integritását modellek – egységesebb és az esetleges konzisztencia; két típusa a varianciaanalízis ismétlések és egységesebb szint közös meghatározza, ha az adatok lesznek egységes, amint írási művelet befejeződött, és végül konzisztens lesz. Például KVÓRUM megadása, mint a konzisztencia szint fognak mindig biztosítja az adatok konzisztencia közben bármilyen konzisztencia szinten, ahhoz, hogy eléréséhez szükség szerint kópiák száma alatti KVÓRUM (például egy) adatok ahányat egységes eredményez.

A 8-csomópont fürt alább látható, a 3-as és KVÓRUM replikációs most (2 csomópontokat is és olvasható konzisztencia) olvasási/írási konzisztencia szint, észre a hibát alkalmazás indítása előtt replikációs csoportonként legfeljebb 1 csomópont elméleti funkcióvesztést pusztítani. Ez feltételezi, hogy az összes a fő szóközöket is van kiegyensúlyozott olvasási/írási kérelmek.  A paraméterek fogjuk használni a telepített fürt a következők:

Egyetlen régió Cassandra fürt konfigurálása:

| Fürt paraméter | Érték | Megjegyzések: |
| ----------------- | ----- | ------- |
| A csomópontok (N) száma | 8   | A fürt csomópontjai száma |
| A replikáció tényező (RF) | 3 | Egy adott sort replikái száma |
| Egységesebb szint (írás) | QUORUM[(RF/2) +1) = 2], a képlet eredménye lefelé lesz kerekítve. | Legfeljebb 2 kópiák ír, a hívó; a visszajelzés elküldése előtt végül következetesen 3 replika íródott. |
| Egységesebb szint (olvasás) | KVÓRUM [(RF/2) + 1 = 2], a képlet eredménye lefelé lesz kerekítve. | 2 kópiák felolvassa visszajelzésről kíván a hívó féllel. |
| Replikációs stratégia | NetworkTopologyStrategy az [Adatok replikációs](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra dokumentációjában talál további információt a lásd: | A telepítési topológia értelmez, és menti a kópiák csomóponton, hogy az összes másolatnál be az azonos állvány nem befejezése |
| Snitch | GossipingPropertyFileSnitch látható [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra dokumentáció További információ | NetworkTopologyStrategy snitch fogalma használ, a topológia megértéséhez. GossipingPropertyFileSnitch az egyes csomópontok megfeleltetése adatközpont és állvány folyamatának jobb felügyelete adja vissza. A fürt pletykákat használja fel ezt az információt propagálása. Ez a sokkal egyszerűbb viszonyított PropertyFileSnitch dinamikus IP-beállítással |


**Cassandra fürt azure szempontjai:** Microsoft Azure virtuális gépeken futó képesség Azure Blob-tárolóhoz használ a lemez adatmegőrzési; Azure tároló magas tartóssági minden lemez 3 replikái menti. Ez azt jelenti, hogy minden adatsor Cassandra tábla beszúrt 3 kópiák már tárolja, és így adatok konzisztencia van már vett érdekli még akkor is, ha a replikáció tényező (RF) érték 1. A főbb probléma való replikáció tényező jelentő, 1 áll, hogy az alkalmazás fog tapasztalni a legrövidebb leállás még akkor is, ha nem sikerül egy egy Cassandra csomópontra. Azonban csomópont Azure háló vezérlő által ismert problémákat (pl. hardver, rendszerhibákat szoftver) nem működik, ha azt egy új csomópontjának elfoglalt helyét, használja az azonos tároló meghajtók kiépítése lesz. Új csomópontot, hogy felváltsa a régi kiépítési néhány percig tart.  Hasonlóképpen tervezett karbantartás tevékenységekhez, mint a vendégként való bekapcsolódáshoz OS módosítások, Cassandra frissíti és a fürt csomópontok közbeni hajt végre Azure háló vezérlő alkalmazás módosításait.  Közbeni is eltarthat néhány csomópontok le egyszerre, és így a fürt léphetnek fel néhány partíciók rövid állásidőt. Jó helyen jár az adatok nem elvesznek a beépített Azure tároló redundancia miatt.  

Számítógépekhez készült Azure magas elérhetőségét (például: 99,9, amely egyenértékű a 8.76 óra évvel; részletekért lásd: a [Magas elérhetősége](http://en.wikipedia.org/wiki/High_availability) ) nem igénylő rendszerbe állított akkor futtathat RF = 1 és egységesebb szint = egyet.  Alkalmazások magas elérhetősége követelményeknek, RF = 3 és egységesebb szint = KVÓRUM fog elviselni a csomópontok a replikák közül egyik lefelé időpontját. RF = 1, a hagyományos telepítések (pl. helyszíni) az adatvesztést eredő problémák lemez meghibásodása miatt nem használhatók.   

## <a name="multi-region-deployment"></a>Több elem régió telepítési
A több elem régió példányban meg minden olyan külső szerszámok nélkül beépített Cassandra tartozó adatok-központ-et replikációs és következetességét modell fentebb ismertetett megkeresheti. Az igazán eltér a hagyományos relációs adatbázisok adatbázis-tükrözéshez több fő írások telepítő hol lehet igazán összetett. A használati forgatókönyvek, többek között az alábbi Cassandra beállítása több területen is segíthetnek:

**Közelség-alapú telepítés:** Több elem bérlői alkalmazásokat a bérlői felhasználók megfeleltetésének törlése-a-terület vette kell figyelembe által a több elem régió fürt alacsony késések. Példa egy tanulási kezelése oktatási intézmények rendszerek esetében a megfelelő kampuszok szolgáló kelet-Amerikai Egyesült Államokban és a nyugati USA-beli régióban elosztott fürt telepítheti és analytics tranzakció alapú. Az adatok lehet helyileg egységes, az idő olvasása és írása és is ahányat egységes mindkét azon részeire lépkedhet végig. További példákat, például a médiafájlok terjesztési, e-kereskedelmi és semmit, és minden sűrített geo felhasználói alap kiszolgáló egy jó használatieset-e telepítési modell.

**Magas elérhetősége:** A redundancia szoftver- és; magas elérhetősége elérése kulcsfontosságú tényező Nézze meg a Microsoft Azure épület megbízható felhő rendszerek további információt. A Microsoft Azure a csak megbízható igaz redundancia elérésének módja több területre fürt telepítésével. Alkalmazások telepíthetők aktív-aktív vagy aktív-passzív módban, és ha lefelé, a régiók egyik Azure forgalom Manager forgalom irányíthatja át a active területhez tartozik.  Az egyetlen régió telepítés elérhetősége 99,9, ha két területi telepítés el tud érni 99.9999 számítja ki a képletet egy elérhetősége: (1-(1-0.999) *(1-0.999))*100); olvassa el a fenti további információt.

**Vészhelyreállítás:** Több területre Cassandra fürt, megfelelő használhatóságát, ha állhat ellen katasztrofális adatok központ kimaradások. Ha egy régió nem működik, az alkalmazás más régiók rendszerbe állított elindíthatja a végfelhasználók felszolgálásához. Bármely más üzleti folytonosságot esetében, például az alkalmazás nem lehet az adatokat a aszinkron során eredő adatvesztést az alternatív. Cassandra azonban a helyreállítás sokkal zavartalanabbá, mint a hagyományos adatbázis helyreállítási folyamat által készített idő. Ábra 2 a tipikus több területre telepítési modell nyolc csomópontokkal egyes régiókra jeleníti meg. A két régió Tükörmargók képe egymás azonos szimmetriasíkjához; valós látványtervek terhelést típusa (pl. tranzakció alapú vagy analitikai), Készletben, RTO, adatok egységesebb és rendelkezésre állási követelmények függ.

![Több régió telepítési](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Ábra 2: Több területre Cassandra telepítési

### <a name="network-integration"></a>Hálózati integrációval
A saját hálózatokhoz a két régió található rendszerbe virtuális gépeken futó csoportja egymással a virtuális Magánhálózati alagutas használatával kommunikál. A virtuális Magánhálózati alagutas kiépítéstől a hálózati telepítési folyamat során két szoftver átjárók csatlakozik. A két régió van hasonló hálózat architektúrája értelmez "webes" és "adatok" alhálózat; Azure hálózati lehetővé teszi, hogy szükség szerint annyi alhálózat létrehozását, és szükség szerint hálózati biztonsági alkalmazása a hozzáférés-vezérlési listák. A fürt topológia tervezésekor többek az adatok központ kommunikációs késleltetés és a hálózati forgalmat kell figyelembe kell venni a economic hatását.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Több elem adatközpont telepítéshez adatok konzisztencia
Átviteli és elérhetőséget fürt topológia hatása a felhasználónevek telepítések kell meghatározva. A RF és egységesebb szint kell választhatók ki úgy, hogy a kvórum nem attól, hogy a rendelkezésre álló összes adatközpontokban.
Magas konzisztencia igénylő rendszer egy LOCAL_QUORUM konzisztencia szint (az Olvasás és írás) Győződjön meg arról, hogy eleget a a helyi olvasása és írása a helyi csomópontok, adatok aszinkron replikált távoli adatok központjának közben.  Táblázat 2 áttekintheti a több elem régió fürt az írás be ismertetett beállításainak részleteit.

**Két-régió Cassandra fürt konfigurálása**


| Fürt paraméter | Érték | Megjegyzések: |
| ----------------- | ----- | ------- |
| A csomópontok (N) száma | 8 + 8 | A fürt csomópontjai száma |
| A replikáció tényező (RF) | 3 | Egy adott sort replikái száma |
| Egységesebb szint (írás) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] a képlet eredményét lefelé lesz kerekítve. | 2 csomópontok írandó az első adatközpont szinkron; a további 2 csomópontok kvórum szükséges írandó aszinkron az 2nd adatközpont. |
| Egységesebb szint (olvasás) | LOCAL_QUORUM ((RF/2) + 1) = 2, a képlet eredményét lefelé lesz kerekítve. | Olvasás kérések teljesülnek csak egy területről; 2 csomópontok olvasnak, mielőtt a visszajelzés elküldése az ügyfélnek. |
| Replikációs stratégia | NetworkTopologyStrategy az [Adatok replikációs](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra dokumentációjában talál további információt a lásd: | A telepítési topológia értelmez, és menti a kópiák csomóponton, hogy az összes másolatnál be az azonos állvány nem befejezése |
| Snitch | GossipingPropertyFileSnitch látható [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra dokumentáció További információ | NetworkTopologyStrategy snitch fogalma használ, a topológia megértéséhez. GossipingPropertyFileSnitch az egyes csomópontok megfeleltetése adatközpont és állvány folyamatának jobb felügyelete adja vissza. A fürt pletykákat használja fel ezt az információt propagálása. Ez a sokkal egyszerűbb viszonyított PropertyFileSnitch dinamikus IP-beállítással |


##<a name="the-software-configuration"></a>A SZOFTVER KONFIGURÁCIÓ
A telepítés során a következő szoftver verzióival használhatók:

<table>
<tr><th>Szoftver</th><th>Forrás</th><th>Verzió</th></tr>
<tr><td>JRE </td><td>[8 JRE](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Az Oracle-licenc manuális elfogadását JRE le van szüksége, mivel egyszerűsítheti, töltse le a szükséges szoftvert az asztalra, a fürt példányhoz előanyagát azt fog szervezni Ubuntu sablon képbe később feltöltése.

A fenti Szoftverletöltés be egy jól ismert letöltési könyvtár (például a Windows %TEMP%/downloads vagy ~/Downloads a legtöbb Linux terjesztését és Mac gépre) a helyi számítógépen.

### <a name="create-ubuntu-vm"></a>UBUNTU VIRTUÁLIS LÉTREHOZÁSA
Ebben a lépésben a folyamat azt hoz létre Ubuntu képe a előzetesen szükséges szoftvert, hogy a kép több Cassandra csomópontok kialakítási felhasználhassa őket.  
####<a name="step-1-generate-ssh-key-pair"></a>LÉPÉS: 1: SSH kulcs pár készítése
Azure szüksége van egy nyilvános kulcs PEM vagy DER kódolású kiépítési időben X509. Hogyan használja SSH Linux Azure a címen található útmutatást követve nyilvános és titkos kulcs két készítése. Ha putty.exe tartományként egy SSH ügyfél vagy a Windows vagy Linux rendszerhez, van-e a kódolt PEM konvertálása RSA titkos kulcs használatával puttygen.exe; PPK formátumba Ez az utasításokat a fenti az weblapon találhatók.

####<a name="step-2-create-ubuntu-template-vm"></a>LÉPÉS: 2: Virtuális Ubuntu sablon létrehozása
A virtuális sablon létrehozásához jelentkezzen be az Azure klasszikus portált, és használja a következő sorrendben: kattintson a új számítási, virtuális gép, FROM GALÉRIA, UBUNTU, Ubuntu Server 14.04 LTS, és kattintson a jobbra mutató nyílra. Hogyan hozhat létre egy Linux virtuális leíró oktatóanyagban létrehozása egy virtuális gépen futó Linux című témakör tartalmaz.

#1 "a virtuális gép beállítások" képernyőn, írja be az alábbi adatokat:

<table>
<tr><th>A MEZŐ NEVE              </td><td>       MEZŐÉRTÉK               </td><td>         MEGJEGYZÉSEK:                </td><tr>
<tr><td>VERZIÓ MEGJELENÉSI DÁTUMA    </td><td> Adja meg a dátumot, az a drow lefelé</td><td></td><tr>
<tr><td>VIRTUÁLIS GÉP NEVE    </td><td> esetén-sablon                </td><td> A hostname (állomásnév): a virtuális: az </td><tr>
<tr><td>RÉTEG                     </td><td> NORMÁL                        </td><td> Az alapértelmezett hagyja              </td><tr>
<tr><td>MÉRETE                     </td><td> A1                              </td><td>Jelölje ki a virtuális; IO igényeinek megfelelően erre a célra hagyja az alapértelmezett </td><tr>
<tr><td> ÚJ FELHASZNÁLÓ NEVÉT.           </td><td> localadmin                      </td><td> "felügyeleti" fenntartva felhasználónév Ubuntu 12. xx lévő és után</td><tr>
<tr><td> HITELESÍTÉS      </td><td> Kattintson a jelölőnégyzet                 </td><td>Ha szeretné-e egy SSH kulccsal biztonságos ellenőrzése </td><tr>
<tr><td> TANÚSÍTVÁNY             </td><td> a nyilvános kulcs a tanúsítvány neve </td><td> A korábban létrehozott nyilvános kulcs</td><tr>
<tr><td> Új jelszó   </td><td> erős jelszó </td><td> </td><tr>
<tr><td> Jelszó megerősítése   </td><td> erős jelszó </td><td></td><tr>
</table>

A #2 "virtuális gép konfigurációs" képernyőn, írja be az alábbi adatokat:

<table>
<tr><th>A MEZŐ NEVE             </th><th> MEZŐÉRTÉK                       </th><th> MEGJEGYZÉSEK:                                 </th></tr>
<tr><td> FELHŐALAPÚ SZOLGÁLTATÁS  </td><td> Hozzon létre egy új, felhőalapú szolgáltatásba    </td><td>Felhőbeli szolgáltatástól egy tároló számítási erőforrások, mint a virtuális gépeken futó</td></tr>
<tr><td> FELHŐALAPÚ SZOLGÁLTATÁS DNS NEVE </td><td>ubuntu-template.cloudapp.net   </td><td>Nevezze el a gép agnostic betöltés terheléselosztó</td></tr>
<tr><td> RÉGIÓ/AFFINITÁS CSOPORT/VIRTUÁLIS HÁLÓZATI </td><td>    Nyugati Amerikai Egyesült Államok </td><td> Jelölje ki a régió, amelyek a webalkalmazások hozzáférni a Cassandra fürthöz</td></tr>
<tr><td>TÁRTERÜLET-FIÓK </td><td>   Alapértelmezett használata </td><td>Az alapértelmezett tároló vagy előre létrehozott tárterület-fiókkal használja az egy adott régióban</td></tr>
<tr><td>ELÉRHETŐSÉG BEÁLLÍTÁSA </td><td>  Nincs lehetőség </td><td>  Hagyja üresen a mezőt</td></tr>
<tr><td>VÉGPONTOK   </td><td>Alapértelmezett használata </td><td>  Az alapértelmezett SSH konfiguráció használata </td></tr>
</table>

Kattintson a jobbra mutató nyíl, hagyja az alapértelmezett beállításokat a #3 képernyőn, és a "" gombot a virtuális kiépítési folyamat befejezéséhez kattintson. Néhány perc múlva a virtuális neve "ubuntu-sablon" egy "operációs rendszert futtató" állapot kell lennie.

###<a name="install-the-necessary-software"></a>A SZÜKSÉGES SZOFTVER TELEPÍTÉSE
####<a name="step-1-upload-tarballs"></a>LÉPÉS: 1: Feltöltés tarballs
A korábban letöltött szoftver scp vagy pscp használ, másolja be a következő parancs formátumban ~/downloads címtár:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Ismételje meg a fenti parancs JRE, valamint a Cassandra bittel mint.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>LÉPÉS: 2: Felkészülés a címtár-struktúra és a archívumok kibontása
Jelentkezzen be a virtuális és a címtár-szerkezet létrehozása, és bontsa ki a szoftver felügyelő felhasználóként az alábbi bash parancsfájl használatával:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Ha a parancsprogram beillesztése vim ablak, győződjön meg arról, ha el szeretné távolítani a kocsi vissza ("\r") a következő parancsot:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Lépés 3: Stb/profil szerkesztése
Hozzáfűzés a végén a következő:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Lépés: 4: A gyártási System JNA telepítése
A következő parancs jelsorrend: A következő parancsot a jna-3.2.7.jar telepítése és jna-platform-3.2.7.jar /usr/share.java Directoryhoz sudo apt get libjna-java telepítése

Szimbolikus hivatkozás létrehozása $CASS_HOME/tár könyvtár, hogy Cassandra indítási parancsfájl megtalálják a következő kancsó:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Lépés az 5: Cassandra.yaml konfigurálása
A konfiguráció szükséges az összes a virtuális gépeken futó [azt fogja működését ennek során a tényleges kiépítési] megfelelően minden virtuális cassandra.yaml szerkesztése:

<table>
<tr><th>A mező neve   </th><th> Érték  </th><th> Megjegyzések: </th></tr>
<tr><td>fürtnév </td><td>  "CustomerService"   </td><td> Használja a nevet, amely a üzembe tükrözi.</td></tr>
<tr><td>listen_address  </td><td>[hagyja üresen a mezőt]   </td><td> "Localhost" törlése </td></tr>
<tr><td>rpc_addres   </td><td>[hagyja üresen a mezőt]  </td><td> "Localhost" törlése </td></tr>
<tr><td>mag   </td><td>"10.1.2.4, 10.1.2.6 10.1.2.8." </td><td>Amely mag jelöli a rendszer minden IP-címek listája.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Ezzel a funkcióval a NetworkTopologyStrateg által az adatközpont és a állvány a virtuális gép behívásakor</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Lépés a 6: A virtuális kép rögzítéséhez
Jelentkezzen be a virtuális gép (hk hitelesítésszolgáltatók, template.cloudapp.net) hostname (állomásnév) és a korábban létrehozott SSH titkos kulcs használatával. Lásd: hogyan használata SSH Azure bejelentkezni, a paranccsal ssh vagy putty.exe olvashat a Linux.

Hajtsa végre az alábbi műveletsor készítsen egy képet:
#####<a name="1-deprovision"></a>1. deprovision
A paranccsal "sudo waagent – deprovision + felhasználói" virtuális gép példány meghatározott adatok történő eltávolításához. Látható, [hogyan Linux virtuális gép rögzítéséhez](virtual-machines-linux-classic-capture-image.md) használata sablonként további részleteket a kép rögzítési folyamat.

#####<a name="2-shutdown-the-vm"></a>2: a virtuális leállítása
Győződjön meg arról, hogy a virtuális gép kiemelt, és a LEÁLLÍTÁSA hivatkozásra a alsó parancssávon.

#####<a name="3-capture-the-image"></a>3: készítsen egy képet
Győződjön meg arról, hogy a virtuális gép kiemelt, és a rögzítés hivatkozásra az alsó parancssávon. A következő képernyőn nevezze el egy kép (például hk-cas-2-08-ub-14-04-2014071), a megfelelő kép leírása és a "" jelet a rögzítési folyamat befejezéséhez kattintson.

Ez eltarthat néhány másodpercig, és a képet a képgyűjtemény saját képek szakaszában érhető el kell. A forrás virtuális automatikusan delated lesz, miután a kép sikeresen rögzíthető.

##<a name="single-region-deployment-process"></a>Egyetlen régió telepítési folyamatot
**Lépés: 1: a virtuális hálózat létrehozása** Jelentkezzen be az Azure klasszikus portált, és hozzon létre egy virtuális hálózati az attribútumok megjelenítése a táblázatban. A lépések részletes leírását a folyamat [konfigurálása az Azure klasszikus portálon Cloud-Only virtuális hálózat](../virtual-network/virtual-networks-create-vnet-classic-portal.md) című témakört.      

<table>
<tr><th>Virtuális attribútumnév</th><th>Érték</th><th>Megjegyzések:</th></tr>
<tr><td>név</td><td>vnet-esetén-nyugati-szolgáltatás</td><td></td></tr>
<tr><td>Régió</td><td>Nyugati Amerikai Egyesült Államok</td><td></td></tr>
<tr><td>A DNS-kiszolgálók </td><td>Nincs lehetőség</td><td>Figyelmen kívül hagyja meg, hogy nem a DNS-kiszolgáló</td></tr>
<tr><td>Virtuális Magánhálózattal pont-webhely konfigurálása</td><td>Nincs lehetőség</td><td> Figyelmen kívül hagyja a</td></tr>
<tr><td>A webhely VPN konfigurálása</td><td>Nnone</td><td> Figyelmen kívül hagyja a</td></tr>
<tr><td>Címterület használatára</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Kezdő IP</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Adja hozzá a következő alhálózat:

<table>
<tr><th>név</th><th>Kezdő IP</th><th>CIDR</th><th>Megjegyzések:</th></tr>
<tr><td>webes</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>A webes farm alhálózat</td></tr>
<tr><td>adatok</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Az adatbázis-csomópontok alhálózat</td></tr>
</table>

Adatok és a webes alhálózat keresztül hálózati biztonsági csoportokat a körét alapján való Ez a cikk nem lehetnek védettek.  

**Lépés: 2: virtuális gépeken futó kiépítése** A korábban létrehozott képet használ, azt létrehoz a következő virtuális gépeken futó felhőalapú Server "hk-c-svc-nyugati", majd kösse őket a megfelelő alhálózat alább látható módon:

<table>
<tr><th>Számítógép neve    </th><th>Alhálózat </th><th>IP-cím </th><th>Elérhetőség beállítása</th><th>Adatközpont/állvány</th><th>Seed (mag)?</th></tr>
<tr><td>HK-c1-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.4   </td><td>HK-c-aset-1    </td><td>Adatközpont WESTUS állvány = = rack1 </td><td>igen</td></tr>
<tr><td>HK-c2-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.5   </td><td>HK-c-aset-1    </td><td>Adatközpont WESTUS állvány = = rack1 </td><td>nem </td></tr>
<tr><td>HK-c3-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.6   </td><td>HK-c-aset-1    </td><td>Adatközpont WESTUS állvány = = rack2 </td><td>igen</td></tr>
<tr><td>HK-c4-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.7   </td><td>HK-c-aset-1    </td><td>Adatközpont WESTUS állvány = = rack2 </td><td>nem </td></tr>
<tr><td>HK-c5-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.8   </td><td>HK-c-aset-2    </td><td>Adatközpont WESTUS állvány = = rack3 </td><td>igen</td></tr>
<tr><td>HK-c6-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.9   </td><td>HK-c-aset-2    </td><td>Adatközpont WESTUS állvány = = rack3 </td><td>nem </td></tr>
<tr><td>HK – c7-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.10  </td><td>HK-c-aset-2    </td><td>Adatközpont WESTUS állvány = = rack4 </td><td>igen</td></tr>
<tr><td>HK-c8-nyugati-szolgáltatás   </td><td>adatok   </td><td>10.1.2.11  </td><td>HK-c-aset-2    </td><td>Adatközpont WESTUS állvány = = rack4 </td><td>nem </td></tr>
<tr><td>HK-F1-nyugati-szolgáltatás   </td><td>webes    </td><td>10.1.1.4   </td><td>HK-w-aset-1    </td><td>                       </td><td>A #HIÁNYZIK</td></tr>
<tr><td>HK-w2-nyugati-szolgáltatás   </td><td>webes    </td><td>10.1.1.5   </td><td>HK-w-aset-1    </td><td>                       </td><td>A #HIÁNYZIK</td></tr>
</table>

A fenti listáját VMs létrehozásához a következő folyamat szerint alakulnak:

1.  Hozzon létre egy üres felhőszolgáltatásba egy adott régióban
2.  A virtuális készítése a korábban rögzített képet, és csatolja a virtuális hálózat; a korábban létrehozott Ismételje meg ezt az összes VMs
3.  Egy belső terheléselosztó hozzáadása a felhőalapú szolgáltatásba, és csatolja a "adatok" alhálózat
4.  Minden egyes virtuális korábban létrehozott egy terheléselosztása végpontot keresztül csatlakozik a korábban létrehozott belső terheléselosztó terheléselosztása thrift forgalmához hozzáadása

A fenti eljárás futtatható Azure klasszikus portálon; a Windows gépi (Ha nincs Windows géphez access Azure virtuális gép használata), használata az alábbi PowerShell parancsprogramot hozhatók létre automatikusan összes 8 VMs.

**Lista 1: A virtuális gépeken futó kiépítéséhez PowerShell-parancsprogramot**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Lépés 3: Minden virtuális Cassandra konfigurálása**

Jelentkezzen be a virtuális, és hajtsa végre a következő:

* Az adatok központ és a állvány tulajdonságainak meghatározásához $CASS_HOME/conf/cassandra-rackdc.properties szerkesztése:

       dc =EASTUS, rack =rack1

* Cassandra.yaml segítségével konfigurálhatja az alábbi rendező csomópontok szerkesztése:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Lépés: 4: Indítsa el a VMs, és a fürt tesztelése**

Jelentkezzen be a csomópontok (pl. hk-c1-nyugati – us) közül, és futtassa a következő parancsot a fürt állapotának megtekintése:

       nodetool –h 10.1.2.4 –p 7199 status

Meg kell jelennie a megjelenítés egy 8-csomópont fürt alatt egy hasonló:

<table>
<tr><th>Állapot</th><th>Cím  </th><th>Betöltése   </th><th>Tokenek </th><th>A tulajdonosa </th><th>A Host azonosító  </th><th>Állvány</th></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.4   </td><td>87.81 KB   </td><td>256    </td><td>38.0 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack1</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.5   </td><td>41.08 KB   </td><td>256    </td><td>68.9 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack1</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.6   </td><td>55.29 KB   </td><td>256    </td><td>68.8 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack2</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.7   </td><td>55.29 KB   </td><td>256    </td><td>68.8 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack2</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.8   </td><td>55.29 KB   </td><td>256    </td><td>68.8 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack3</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.9   </td><td>55.29 KB   </td><td>256    </td><td>68.8 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack3</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.10  </td><td>55.29 KB   </td><td>256    </td><td>68.8 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack4</td></tr>
<tr><th>TÖRÖLJE  </td><td>10.1.2.11  </td><td>55.29 KB   </td><td>256    </td><td>68.8 %  </td><td>Globálisan egyedi azonosítója (eltávolítja)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Az egyetlen régió fürt tesztelése
A fürt ellenőrzéséhez kövesse az alábbi lépéseket:

1.    A belső terheléselosztó IP-címét (például beszerzése parancs a Powershell Get-AzureInternalLoadbalancer parancsmag használata esetén  10.1.2.101). Az alábbi parancs a alább látható módon: Get-AzureLoadbalancer – ServiceName "hk-c-svc-nyugati-hu" [együtt IP-címének a belső terheléselosztó részleteinek megjelenítése]
2.  Jelentkezzen be a webes farm (pl. hk-F1-nyugati – us) virtuális gitt használatával vagy ssh
3.  Hajtsa végre a $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4.  Ellenőrizze, hogy működik-e a fürt a következő CQL parancsokat használhatja:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Meg kell jelennie olyanra, mint az alábbi képernyő:

<table>
  <tr><th> customer_id </th><th> Utónév </th><th> Utónév </th></tr>
  <tr><td> 1 </td><td> János </td><td> Jakab </td></tr>
  <tr><td> 2 </td><td> Verebélyi </td><td> Jakab </td></tr>
</table>

Felhívjuk a figyelmét arra, hogy a 4 lépésben létrehozott keyspace egy replication_factor 3 SimpleStrategy használja. SimpleStrategy ajánlott egyetlen adatok központ telepítések mivel több adatok NetworkTopologyStrategy középre telepítések. 3 egy replication_factor meg fogalmat ad csomópont hibák hibatűrést.

##<a id="tworegion"> </a>Több területre telepítési folyamatot
A program kihasználhatja az egyetlen régió telepítés befejeződött, és ismételje meg ugyanezt az eljárást a második régió telepítése. A fő különbség az egy vagy több régió telepítési, régió közötti kommunikáció; VPN alagutas beállítása azt nyílik meg a hálózati telepítés, a VMs kiépítése, és állítsa be a Cassandra.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Lépés: 1: A virtuális hálózat létrehozása a 2. a régió
Jelentkezzen be az Azure klasszikus portált, és hozzon létre egy virtuális hálózati az attribútumok megjelenítése a táblázatban. A lépések részletes leírását a folyamat [konfigurálása az Azure klasszikus portálon Cloud-Only virtuális hálózat](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) című témakört.      

<table>
<tr><th>Attribútumnév    </th><th>Érték    </th><th>Megjegyzések:</th></tr>
<tr><td>név    </td><td>vnet-esetén – kelet-szolgáltatás</td><td></td></tr>
<tr><td>Régió  </td><td>Kelet-Amerikai Egyesült Államok</td><td></td></tr>
<tr><td>A DNS-kiszolgálók     </td><td></td><td>Figyelmen kívül hagyja meg, hogy nem a DNS-kiszolgáló</td></tr>
<tr><td>Virtuális Magánhálózattal pont-webhely konfigurálása</td><td></td><td>     Figyelmen kívül hagyja a</td></tr>
<tr><td>A webhely VPN konfigurálása</td><td></td><td>      Figyelmen kívül hagyja a</td></tr>
<tr><td>Címterület használatára   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Kezdő IP </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Adja hozzá a következő alhálózat:
<table>
<tr><th>név    </th><th>Kezdő IP    </th><th>CIDR   </th><th>Megjegyzések:</th></tr>
<tr><td>webes </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>A webes farm alhálózat</td></tr>
<tr><td>adatok    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Az adatbázis-csomópontok alhálózat</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Lépés: 2: Hozzon létre helyi hálózatok
Azure virtuális hálózat a helyi hálózaton, beleértve a magánjellegű felhő vagy egy másik Azure terület távoli webhelyre leképezi proxy cím szóköz található. A proxy-címterület használatára van kötve távoli átjáró útválasztási hálózat hálózati megfelelő helyre. [Konfigurálása a VNet VNet kapcsolat](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) az utasításokat VNET-VNET kapcsolat létrehozása című témakört.

Hozzon létre két helyi hálózat per az alábbiakat:

| Hálózati név | Virtuális Magánhálózati átjáró címe | Címterület használatára | Megjegyzések: |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-Map-to-East-us | 23.1.1.1  | 10.2.0.0/16   | A helyi hálózaton létrehozásakor adjon helyőrző átjáró címét. Az átjáró valós címe az átjáró létrehozása után meg van adva. Győződjön meg arról, hogy a címterületet pontosan egyezik a megfelelő távoli VNET; Ebben az esetben a VNET a kelet-USA-beli tartományban lévő létre. |
| HK-lnet-Map-to-West-us | 23.2.2.2  | 10.1.0.0/16   | A helyi hálózaton létrehozásakor adjon helyőrző átjáró címét. Az átjáró valós címe az átjáró létrehozása után meg van adva. Győződjön meg arról, hogy a címterületet pontosan egyezik a megfelelő távoli VNET; Ebben az esetben a VNET az USA-beli nyugati régió létre. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Lépés 3: Térkép "Helyi" hálózat a megfelelő VNETs
Az Azure klasszikus portálról jelölje ki az egyes vnet kattintson a "Konfigurálás" gombra, jelölje be a "Csatlakozás a helyi hálózathoz", és válassza a helyi hálózat per az alábbiakat:


| Virtuális hálózati | Helyi hálózaton |
| --------------- | ------------- |
| HK-vnet-nyugati-szolgáltatás | HK-lnet-Map-to-East-us |
| HK-vnet-kelet-szolgáltatás | HK-lnet-Map-to-West-us |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Lépés: 4: VNET1 és VNET2 átjárók létrehozása
Kattintson a virtuális mindkét hálózatban az irányítópulton létrehozása ÁTJÁRÓ, amely elindítja a virtuális Magánhálózati átjáró kiépítési folyamat. Néhány perc múlva az irányítópulton az összes olyan virtuális hálózat megjelenjen-e a tényleges átjáró címet.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Lépés 5: Frissítés "Helyi" hálózatok a megfelelő "átjáró" címek###
A helyőrző gateway IP-cím lecserélése valós IP-címét az imént kiépített átjárók mindkét a helyi hálózat szerkesztése. A következő leképezés használata:

<table>
<tr><th>Helyi hálózaton    </th><th>Virtuális hálózati átjáró</th></tr>
<tr><td>HK-lnet-Map-to-East-us </td><td>Átjáró hk-vnet-nyugati-Amerikai</td></tr>
<tr><td>HK-lnet-Map-to-West-us </td><td>Átjáró hk-vnet-kelet-Amerikai</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Lépés a 6: Frissítse a megosztott billentyűt
Frissítse a IPSec kulcsot minden VPN átjáró [használata mindkét átjáró kulcsa szakét] használja az alábbi Powershell parancsprogramot: Set-AzureVNetGatewayKey - VNetName hk-vnet-kelet-hu - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-nyugati – hu - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>7 lépés: Az VNET-VNET kapcsolatot létesíteni
Az Azure klasszikus portálról átjárók közötti kapcsolatot létesíteni a virtuális mindkét hálózatban "IRÁNYÍTÓPULT" menüjének használatával. A "Csatlakozás" menüelemek használatához az alsó eszköztáron. Néhány perc múlva az irányítópulton megjelenjen-e a kapcsolat adatai grafikusan.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>8 lépés: A virtuális gépeken futó létrehozása területen #2
Hozzon létre a Ubuntu képe, a következő lépésekkel vagy a Másolás a virtuális képfájl Azure tárolási fiókjában található régióban #2 #1 régió telepítési leírt módon, és a kép elkészítéséhez. Ezen a képen létrehozása és használata az alábbi listában szereplő virtuális gépeken futó be egy új felhőszolgáltatásba hk-c-svc-kelet-nekünk:


| Számítógép neve | Alhálózat | IP-cím | Elérhetőség beállítása | Adatközpont/állvány | Seed (mag)? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-kelet-szolgáltatás | adatok  | 10.2.2.4   | HK-c-aset-1      | Adatközpont EASTUS állvány = = rack1 | igen |
| HK-c2-kelet-szolgáltatás | adatok  | 10.2.2.5   | HK-c-aset-1      | Adatközpont EASTUS állvány = = rack1 | nem  |
| HK-c3-kelet-szolgáltatás | adatok  | 10.2.2.6   | HK-c-aset-1      | Adatközpont EASTUS állvány = = rack2 | igen |
| HK-c5-kelet-szolgáltatás | adatok  | 10.2.2.8   | HK-c-aset-2      | Adatközpont EASTUS állvány = = rack3 | igen |
| HK-c6-kelet-szolgáltatás | adatok  | 10.2.2.9   | HK-c-aset-2      | Adatközpont EASTUS állvány = = rack3 | nem  |
| HK – c7-kelet-szolgáltatás | adatok  | 10.2.2.10  | HK-c-aset-2      | Adatközpont EASTUS állvány = = rack4 | igen |
| HK-c8-kelet-szolgáltatás | adatok  | 10.2.2.11  | HK-c-aset-2      | Adatközpont EASTUS állvány = = rack4 | nem  |
| HK-F1 – kelet-szolgáltatás | webes   | 10.2.1.4   | HK-w-aset-1      | A #HIÁNYZIK                    | A #HIÁNYZIK |
| HK-w2-kelet-szolgáltatás | webes   | 10.2.1.5   | HK-w-aset-1      | A #HIÁNYZIK                    | A #HIÁNYZIK |


Mint terület #1 ugyanezeket a lépéseket, de 10.2.xxx.xxx címterület használja.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Lépés a 9: Minden virtuális Cassandra konfigurálása
Jelentkezzen be a virtuális, és hajtsa végre a következő:

1. Adatok központ és a állvány tulajdonságainak meghatározásához formátumban $CASS_HOME/conf/cassandra-rackdc.properties szerkesztése: adatközpont EASTUS állvány = = rack1
2. Cassandra.yaml beállítása rendező csomópontok szerkesztése: mag: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>10 lépés: Cassandra indítása
Jelentkezzen be az egyes virtuális, és indítsa el a Cassandra a háttérben a következő parancs futtatásával: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>A több elem régió fürt tesztelése
Most által Cassandra lett telepítve az egyes Azure régióban 8 csomópontokkal 16 csomóponthoz. A csomópontok szerepelnek, a közös fürt nevét és a rendező csomópont konfigurálása a azonos fürt. A következő eljárással a fürt tesztelése:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Lépés: 1: A belső betöltés terheléselosztó IP beszerzése mindkét a régiók PowerShell használatával
- Get-AzureInternalLoadbalancer - ServiceName "hk-c-svc-nyugati-hu"
- Get-AzureInternalLoadbalancer - ServiceName "hk-c-svc-kelet-hu"  

    Megjegyzés: az IP-címek (például - 10.1.2.101 kelet - nyugati 10.2.2.101) jelenik meg.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Lépés: 2: Hajtsa végre a következő a nyugati régió hk-F1-nyugati-kapcsolatfelvételi programba való bejelentkezés után
1.    Hajtsa végre a $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2.  Hajtsa végre a következő CQL parancsokat:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Meg kell jelennie olyanra, mint az alábbi képernyő:

| customer_id | Utónév | Utónév |
| ----------- | --------- | -------- |
| 1           | János      | Jakab      |
| 2           | Verebélyi      | Jakab      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>3 lépés: A következő végrehajtása a keleti régióban hk-F1 – kelet-kapcsolatfelvételi programba való bejelentkezés után:
1.    Hajtsa végre a $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2.  Hajtsa végre a következő CQL parancsokat:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Az azonos megjelenítése a nyugati régió naplójában kell megjelennie:


| customer_id | Utónév | Utónév   |
|------------ | --------- | ---------- |
| 1           | János      | Jakab        |
| 2           | Verebélyi      | Jakab        |


Néhány további beszúrása végrehajtani, és látható, hogy azok első replikált nyugati-velünk a fürt része.

## <a name="test-cassandra-cluster-from-nodejs"></a>Teszt Cassandra fürt Node.js
Közül a Linux VMs jön létre a "webhely", az első csoportba tartozó korábban, hogy hajtja végre egy egyszerű Node.js parancsprogramot, olvassa el a korábban beszúrt adatok

**Lépés: 1: Node.js és Cassandra ügyfél telepítése**

1. Node.js és npm telepítése
2. Csomópont csomag "cassandra-ügyfél" telepítése npm használatával
3. Hajtsa végre a következő parancsfájlt, amely megjeleníti a json karakterláncot, a beolvasott adatok rendszerhéj parancssorba:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Elfogadásáról
Microsoft Azure, amely lehetővé teszi a mind a Microsoft, valamint Megnyitás szoftver fut, ahogy a gyakorlat rugalmas platformot. Könnyen hozzáférhető Cassandra fürt egyetlen adatközpont, több hibafa tartományok fürt csomópontok terjesztése keresztül telepíthető. Cassandra fürt is elvégezhető katasztrófa próba rendszerekhez több földrajzilag mennie Azure területek között. Azure és Cassandra közös lehetővé teszi, hogy nagymértékben méretezhető, könnyen hozzáférhető szerkezetének és katasztrófa helyreállítható felhőszolgáltatások szükség szerint az aktuális internet mérete szolgáltatások.  

##<a name="references"></a>Hivatkozások##
- [http://cassandra.Apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
