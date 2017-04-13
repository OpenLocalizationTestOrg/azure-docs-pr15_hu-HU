<properties
    pageTitle="A webhely helyreállítási feladatátvevő |} Microsoft Azure" 
    description="Azure webhely helyreállítási koordináták a replikáció, feladatátvevő és helyreállítási virtuális gépeken futó és fizikai kiszolgálók. További tudnivalók: Azure vagy egy másodlagos adatközponthoz való áttérés." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>A webhely helyreállítási feladatátvevő

A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

## <a name="overview"></a>– Áttekintés

Ez a témakör nyújt tájékoztatást és visszaállítása (adatkapcsolat feletti és hiányában vissza) utasításokat virtuális gépeken futó és a webhely helyreállítási védett tényleges kiszolgálók. 

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.


## <a name="types-of-failover"></a>Feladatátvevő típusai

Védelem engedélyezve van a virtuális gépeken futó és a fizikai kiszolgálók és azok is replikálása után futtathatók feladatátadás, szükség szerint. Webhely helyreállítási feladatátvevő típusú számos használatát támogatja.

**Feladatátvevő** | **Mikor érdemes futtatása** | **Részletek** | **Folyamatábra**
---|---|---|---
**Teszt feladatátvevő** | A replikáció vonatkozó stratégia érvényesítése, vagy hajtsa végre a katasztrófa helyreállítási részletező futtatása | Az adatok elvesztését sem legrövidebb leállás.<br/><br/>Nincs hatással a replikáció<br/><br/>Nincs hatással a termelési környezetén | Indítsa el a feladatátvételi<br/><br/>Adja meg, hogyan próba gépek fog csatlakozni hálózatok feladatátvétel után<br/><br/>Nyomon kövesse a **feladatok** lap. Próba gépek jönnek létre, és nyissa meg a másodlagos helye<br/><br/>Azure - csatlakoztatása a gép az Azure-portálon<br/><br/>Másodlagos webhely - a gép host és a felhő elérése<br/><br/>Tesztelés végrehajtása, és automatikusan karbantartása feladatátvevő beállításainak tesztelése.
**Tervezett feladatátvevő** | A megfelelőségi követelmények betartását futtatása<br/><br/>Tervezett karbantartásokkal kapcsolatos információk futtatása<br/><br/>Mutasson az adatokra ismert kiesések – például egy várható áramszünet vagy nagyon gyenge hálózati minőség időjárás-jelentések futó feladatok megtartása meghiúsító futtatása<br/><br/>Futtatásával visszaállás feladatátvétel után az elsődleges, másodlagos való | Az adatvesztés<br/><br/>Legrövidebb leállás ideig tart a virtuális gép az elsődleges a Leállítás, és jelenítse meg a másodlagos helyénél során keletkezett.<br/><br/>Virtuális gépeken futó hatása cél gépek válik a forrás gépek feladatátvétel után lépnek. | Indítsa el a feladatátvételi<br/><br/>Nyomon kövesse a **feladatok** lap. Forrás gépek le<br/><br/>A másodlagos helyen indítása replika gépek<br/><br/>Azure - csatlakozás a replika géphez az Azure-portálon<br/><br/>Másodlagos webhely - elérni az azonos állomáson és felhőbeli ugyanazon a gépen<br/><br/>A feladatátvételi véglegesítése
**Tervezett feladatátvevő** | Futtatni feladatátvevő ilyen típusú reaktív módon miatt nem várt esemény, például a power üzemszünetek, illetve vírus támadások elsődleges webhely elérhetetlenné válik <br/><br/> Meg tudja nyitni egy tervezett feladatátvevő történik, ha az elsődleges webhely nem érhető el. | Az adatok elvesztését függő replikációs gyakoriság beállítások<br/><br/>Adatok naprakész utolsó szinkronizálásakor megfelelően lesznek | Indítsa el a feladatátvételi<br/><br/>Nyomon kövesse a **feladatok** lap. Tetszés szerint próbálkozzon virtuális gépeken futó leállítás, és a legfrissebb adatok szinkronizálása<br/><br/>A másodlagos helyen indítása replika gépek<br/><br/>Azure - csatlakozás a replika géphez az Azure-portálon<br/><br/>Másodlagos hely elérési az azonos állomáson és felhőbeli ugyanazon a gépen<br/><br/>A feladatátvételi véglegesítése


Feladatátadás támogatott típusú telepítési igényektől függ.

**Feladatátvevő iránya** | **Teszt feladatátvevő** | **Tervezett feladatátvevő** | **Tervezett feladatátvevő**
---|---|---|---
Elsődleges VMM webhely másodlagos VMM webhelyre | Támogatott | Támogatott | Támogatott 
Másodlagos VMM webhely elsődleges VMM webhelyre | Támogatott | Támogatott | Támogatott
Felhőalapú a felhőbe (egyetlen VMM server) |  Támogatott | Támogatott | Támogatott
Azure VMM webhely | Támogatott | Támogatott | Támogatott 
Azure VMM webhelyre | Nem támogatott | Támogatott | Nem támogatott 
Azure-webhelyére a Hyper-V | Támogatott | Támogatott | Támogatott
Azure a Hyper-V-webhelyre | Nem támogatott | Támogatott | Nem támogatott
Azure VMware webhely | Támogatott (fokozott helyzet)<br/><br/> Nem támogatott (régi helyzet) |Nem támogatott | Támogatott
Azure fizikai kiszolgáló | Támogatott (fokozott helyzet)<br/><br/> Nem támogatott (régi helyzet) | Nem támogatott | Támogatott

## <a name="failover-and-failback"></a>Feladatátvétel és visszaállás

Attól függően, hogy a üzembe sikertelen a virtuális gépeken futó helyszíni másodlagos webhelyre vagy Azure-fölé. A gép, amely fölé Azure nem sikerül az Azure virtuális gép jön létre. Sikertelen lehet egy virtuális egyetlen számítógépre vagy a fizikai server, illetve a helyreállítási terv fölé. Helyreállítási tervek áll egy vagy több rendelt védett virtuális gépeken futó vagy a fizikai kiszolgálók tartalmazó csoportok. Azok megszokott feladatátvevő téve több gépek, amelyek szerint a csoport átveszi is legyenek. [További információ:](site-recovery-create-recovery-plans.md) helyreállítási csomagokról. 

Miután feladatátvevő befejeződik, és a gép működik és egy másodlagos helyen vannak vegye figyelembe, hogy:

- Ha Ön nem sikerült fölé Azure-után a feladatátvevő gépek nem védett, vagy replikáló az elsődleges és másodlagos helyen. A virtuális gépeken futó Azure geo replikált tárhely, amely biztosítja tűrőképessége, de nem replikációs vannak tárolva.
- Ha elvégezte a másodlagos webhelyek, a másodlagos hely feladatátvevő gépek nem védett vagy replikálása tervezett áttérni.
- Ha egy tervezett áttérni egy másodlagos webhelyen, a másodlagos hely feladatátvevő gépek után elvégzett-e védelemmel ellátva.
 

### <a name="failback-from-secondary-site"></a>Másodlagos webhelyről visszaállás

Elsődleges másodlagos webhelyről csomópontoktól áttérni az elsődleges másodlagos ugyanezt az eljárást használja. Miután az elsődleges másodlagos adatközponthoz való áttérés lekötött és teljes, kezdeményezhet fordított replikációs, az elsődleges webhely elérhetővé. Fordított replikációs kezdeményez replikációs között a másodlagos webhelyet, és az elsődleges, a delta adatok szinkronizálásával. Fordított replikációs védett állapotba elérhetővé teszi a virtuális gépeken futó azonban a másodlagos adatközponthoz továbbra is az aktív helyét. Az elsődleges webhely kezdeményezéséhez az aktív helyre az elsődleges, másodlagos tervezett feladatátvevő kezdeményez egy másik fordított replikációs követ.

### <a name="failback-from-azure"></a>Az Azure visszaállás

Ha, hogy nem sikerült fölé Azure a virtuális gépeken futó tűrőképessége Azure virtuális gépeken futó funkciók védik. Az eredeti elsődleges webhely jeleinek aktív helyét, futtassa a tervezett feladatátvevő. Az eredeti helyén, vagy egy másik helyre is sikertelen, ha az eredeti webhely nem érhető el. Replikálása csomópontoktól elsődleges helye után újra el egy fordított replikációs kezdeményez.

### <a name="failover-considerations"></a>Feladatátvevő kapcsolatos szempontok

- **IP-cím feladatátvétel után**– alapértelmezés szerint egy sikertelen fölé gépi egy másik IP-címet, mint a forrás gép lesz. Ha szeretné megőrizni a azonos IP-cím lásd: 
    - **Másodlagos webhely**– Ha éppen feladat átvétele másodlagos webhelyre, és meg szeretné őrizni egy IP cím [olvassa el](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) a cikk. Figyelje meg, hogy segítségével megőrizheti a nyilvános IP-címet, ha az Internetszolgáltató támogatja.
    - **Azure**– Ha az Azure fölé adatkapcsolat esetén a virtuális gép tulajdonságainak **konfigurálása** lapján hozzárendelni kívánt IP-címet is megadhat. Egy nyilvános IP-cím nem megőrzi az Azure való áttérés után. Továbbra is a belső címeket használt RFC 1918 cím szóközöket.
- **Részleges feladatátvevő**– Ha nem sikerül egy webhely része fölé szeretne, hanem egy teljes webhelyet vegye figyelembe, hogy: 
    - **Másodlagos webhely**– Ha nem sikerül egy elsődleges, másodlagos webhelyre webhely része fölé, és szeretne csatlakozni az elsődleges webhelyek, a webhely virtuális Magánhálózati kapcsolat használata alkalmazások infrastruktúra összetevők futtatása a elsődleges webhelyen, a másodlagos webhelyen keresztül nem sikerült csatlakozni. Ha egy teljes alhálózat átvétele segítségével megőrizheti a gép virtuális IP-címét. Ha Ön nem egy részleges alhálózat fölé a gép virtuális IP-cím meg nem tartania, mivel az alhálózathoz nem lehet megosztani a helyek között.
    - **Azure**– Ha sikertelen Azure részleges hely fölé, és szeretne csatlakozni az elsődleges webhely, is használhatja a webhely VPN Azure-infrastruktúra-összetevők, az elsődleges webhely futó alkalmazás fölé egy sikertelen csatlakozni. Megjegyzendő, hogy ha keresztül, a teljes alhálózat sikertelen segítségével megőrizheti a gép virtuális IP-címet. Ha Ön nem egy részleges alhálózat fölé a gép virtuális IP-cím meg nem tartania, mivel az alhálózathoz nem lehet megosztani a helyek között.
 
- **Betűjele**– Ha meg szeretné őrizni a virtuális gépeken betűjele feladatátvétel után is beállíthatja a virtuális gép SAN házirendjét **a**. Virtuális gép lemezt automatikusan online állapotba. [További információ:](https://technet.microsoft.com/library/gg252636.aspx).
- **Útvonal ügyfél kérések**– webhely helyreállítási Azure forgalom Manager működik ügyfél kérelmek feladatátvétel után az alkalmazás.




## <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

A próba feladatátvétel futtatásakor, a rendszer kéri, jelölje be a hálózati beállítások tesztelése replika gépekhez. Ha több lehetőség.  

**Teszt feladatátvételi beállítás** | **Leírás** | **Feladatátvevő ellenőrzése** | **Részletek**
---|---|---|---
**Azure átadni – hálózati nélkül** | Azure hálózati cél nem jelöli ki. | Tesztelje a virtuális gép feladatátvevő ellenőrzést elindítja az Azure-ban a várt módon | Az összes vizsgálat virtuális gépeken futó helyreállítása tervben adhatók meg egy egy felhőalapú szolgáltatásba, és csatlakozhat egymást<br/><br/>Az Azure-hálózat gépek feladatátvétel után nem kapcsolódik.<br/><br/>Felhasználók csatlakozhatnak a próba gépek nyilvános IP-címmel
**Azure átadni – a hálózat** | Jelölje be a cél Azure hálózati | Feladatátvevő ellenőrzi, hogy próba gépek kapcsolódik a hálózathoz | Hozzon létre egy Azure hálózat, amely a Azure gyártási hálózatról elszigetelt, és állítsa be a replikált virtuális gépen a várt módon működnek a infrastruktúra alakítható ki.<br/><br/>A próba virtuális gép az alhálózat alhálózat, amelyen alapul a sikertelen fölé virtuális gép várhatóan csatlakozhat, ha a tervezett vagy nem tervezett feladatátvétel.
**Másodlagos VMM webhely átadni – hálózati nélkül** | Virtuális hálózaton nem jelöli ki. | Feladatátvevő ellenőrzi, hogy próba gépek jönnek létre.<br/><br/>A próba virtuális gépen, amelyen a kópia virtuális gép létezik a host azonos állomáson jön létre. Azt nem adja hozzá a felhőben, ahol a replika virtuális gép is található. | <p>A sikertelen fölé a számítógép nem csatlakozik olyan hálózat.<br/><br/>A gép csatlakoztatható virtuális hálózathoz után lett létrehozva
**Másodlagos VMM webhely átadni – a hálózat** | Jelöljön ki egy meglévő virtuális hálózatot egy | Feladatátvevő ellenőrzi, hogy vannak-e létre virtuális gépeken futó | A próba virtuális gépen, amelyen a kópia virtuális gép létezik a host azonos állomáson jön létre. Azt nem adja hozzá a felhőben, ahol a replika virtuális gép is található.<br/><br/>A gyártási hálózatról, amely magában elszigetelt virtuális hálózat létrehozása<br/><br/>Egy virtuális alapú hálózat használata ajánlott (a termelési nem használt) külön logikai hálózat létrehozása a VMM erre a célra. Logikai hálózat használatos hozhat létre virtuális hálózatok feladatátvevő tesztelése céljából.<br/><br/>A logikai hálózatnak a hálózati adaptereken virtuális gépeken futó szolgáltatója a Hyper-V kiszolgálók legalább egyikével társított kell lennie.<br/><br/>Virtuális logikai hálózatok a hálózati helyek vesz fel a logikai hálózati elszigetelt kell lennie.<br/><br/>Ha logikai Windows hálózati virtualizációs-alapú hálózat használata esetén a Azure webhely helyreállítási automatikusan létrehozza a elszigetelt virtuális hálózatok.
**Másodlagos VMM webhely átadni – a hálózat létrehozása** | Ideiglenes próba hálózati **Logikai hálózati** és a hozzá kapcsolódó hálózati helyek megadott beállítás alapján automatikusan létrejön | Feladatátvevő ellenőrzi, hogy vannak-e létre virtuális gépeken futó | Akkor használja ezt a lehetőséget, ha a helyreállítási terv egynél több virtuális hálózatot használ, amelyben. Használata Windows hálózati virtualizációs hálózatok, ezt a beállítást automatikusan hozhat létre virtuális hálózatok azonos beállítású (alhálózat és IP-cím készletek) a replika virtuális gép a hálózaton. Automatikusan törlődnek a virtuális hálózatok a próba feladatátvételi befejeződése után.</p><p>A próba virtuális gépen, amelyen a kópia virtuális gép létezik a host azonos állomáson jön létre. Azt nem adja hozzá a felhőben, ahol a replika virtuális gép is található.

>[AZURE.NOTE] Az IP-cím próba feladatátvételkor virtuális géphez megadott ugyanaz, mint az IP-cím meg szeretné kapni mikor tesz a tervezett vagy nem tervezett feladatátvevő (feltéve, hogy az IP-cím érhető el a próba feladatátvevő hálózati., Ha ugyanazt a címet nem érhető el a próba feladatátvevő hálózaton virtuális gép érhető el a próba feladatátvevő hálózaton egy másik IP-cím kap.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Futtassa a helyszíni próba feladatátvevő Azure

Ez az eljárás a helyreállítási terv próba feladatátvevő futtatása ismerteti. Másik lehetőségként a feladatátvétele egyetlen számítógépen futtathatja a **virtuális gépeken futó** lapon.

1. Válassza a **Helyreállítás tervek** > *recoveryplan_name*. Kattintson a **feladatátvevő** > **próba feladatátvevő**.
2. A **Próba feladatátvevő erősítse meg** lapon adja meg, hogyan replika gépek fog csatlakozni az Azure-hálózat feladatátvétel után.
3. Ha Ön éppen adatkapcsolat fölé Azure, és adatok titkosítása engedélyezve van a felhőben, a **Titkosítási kulcs** jelölje ki a szolgáltató a telepítés során adatok titkosítása engedélyezésekor kiadott tanúsítványt. 
4. Feladatátvevő nyilvántartása a **feladatok** lap. Látnia kell a próba replika gép az Azure-portálon megjelenítéséhez.
5. Elérheti replika gépek Azure-ban a helyszíni kezdeményezése webhelyről egy RDP-kapcsolat a virtuális géphez. port 3389 nyitva a virtuális gép végpont kell.
5. Amikor elkészült, amikor a feladatátvételi eléri a **teljes tesztelés** fázis, kattintson a **Teljes vizsgálati** befejezéséhez.
5. **Jegyzetek** rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.
8. Kattintson **a vizsgálat feladatátvételi befejeződött** automatikusan jelenjenek meg a tesztkörnyezetben. Ez befejeződése után a próba feladatátvételt C**omplete** állapotát jelennek meg.

> [AZURE.NOTE] Ha a teszt feladatátvétel továbbra is fennáll, több mint két hétig azt fogja kell elvégeznie hatályba. Azok az elemek és a vizsgálat feladatátvételkor automatikusan létrehozott virtuális gépeken futó is törli.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>A próba feladatátvétel lebonyolítása elsődleges a helyszíni webhely másodlagos helyszíni webhelyre

Végezze el a próba feladatátvevő, beleértve a tartományvezérlőnek másolása, és úgy, hogy a próba-DHCP és a DNS-kiszolgálók a tesztkörnyezetben futtatásához dolog számos kell. Ez a probléma több módon teheti:

- Használata egy meglévő hálózat tesztelése feladatátvevő futtatni szeretné, ha a hálózati Felkészülés az Active Directory, a DHCP és a DNS.
- A beállítás használatával automatikusan létrehozza a virtuális hálózatok próba feladatátvevő futtatni szeretné, ha adja hozzá a csoport-1 előtt kézi lépést a helyreállítási tervben szeretné használni a próba feladatátvételi, majd adjon hozzá az infrastruktúra-erőforrások az automatikusan létrehozott hálózati a próba feladatátvételi futtatása előtt.

#### <a name="things-to-note"></a>Megjegyzés: szempontok

- Amikor egy másodlagos webhely esetében, amelyek, a kópia gép által használt hálózati típusú nem kell logikai hálózat tesztelése feladatátvevő használt típusának megfelelő, de nem működik az egyes kombinációk. Ha a kópia használ DHCP és virtuális-alapú elkülönítési, a virtuális hálózat a kópia nem kell a statikus IP-cím készlet. Így Windows hálózati virtualizációs használata a próba feladatátvételi nem működik, mert nincs cím készletek érhetők el. Ezenkívül próba feladatátvevő nem fognak működni, ha a kópia hálózati nincs elkülönítési, és a próba-hálózaton Windows hálózati virtualizációs. Ennek oka az, a nincs elkülönítési hálózati nem tartozik a Windows hálózati virtualizációs hálózati létrehozásához szükséges alhálózat.
- Melyik kópiában virtuális gépeken futó csatlakoztatott módjának virtuális hálózatok megfeleltetve, feladatátvevő függ, hogy hogyan a virtuális hálózathoz van konfigurálva, a VMM konzolban után:
    - **Virtuális hálózatnak elkülönítési és virtuális elkülönítési nélkül**– Ha DHCP a virtuális hálózathoz van megadva, a kópia virtuális gép a virtuális Értékét a megadott beállítástól a hálózati hely, a társított logikai hálózaton kell csatlakoztatni. A virtuális gép kap IP-címének a rendelkezésre álló DHCP-kiszolgálóról. A cél virtuális hálózat definiált statikus IP cím készlet nem szükséges. Statikus IP-cím készletét a virtuális hálózat használatakor a replika virtuális gép csatlakozik a virtuális Értékét a megadott beállítástól a hálózati hely, a társított logikai hálózaton. A virtuális gép kap IP-címének a készletből, a virtuális hálózat definiált. Ha egy statikus IP-cím készlet a cél virtuális hálózaton nem definiálja, IP-cím lefoglalásához sikertelen lesz. Az IP-cím készlet fogja használni a védelem és helyreállítási a forrás- és célwebhelyek VMM kiszolgálókon kell létrehozni.
    - **Windows-hálózaton virtuális hálózati virtualizációs**– Ha ezt a beállítást, statikus kell meghatározni a cél virtuális hálózat virtuális hálózaton van beállítva, függetlenül attól, hogy helyesek-e a forrás virtuális hálózat használata DHCP vagy statikus IP-cím cím készlet. DHCP határozza meg, ha a cél VMM kiszolgálóhoz használni kívánt egy útja, és a szükséges a készletből meghatározott IP-címet a cél virtuális hálózathoz. Ha a forráskiszolgálóval a statikus IP-cím készlet felhasználása van megadva, a a cél VMM kiszolgálóhoz lefoglalhat a IP-címet a készletből. Mindkét esetben IP-cím lefoglalásához meghiúsul, ha egy statikus IP-cím készlet nincs megadva.

#### <a name="run-test"></a>Teszt futtatása

Ez az eljárás a helyreállítási terv próba feladatátvevő futtatása ismerteti. Azt is megteheti a feladatátvételi egyetlen virtuális gép vagy fizikai kiszolgáló futtathatja a **virtuális gépeken futó** lapon.

1. Válassza a **Helyreállítás tervek** > *recoveryplan_name*. Kattintson a **feladatátvevő** > **próba feladatátvevő**.
2. **Tesztelése feladatátvevő megerősítése** lapon adja meg, hogyan virtuális gépeken futó kell csatlakoznia hálózatok a próba feladatátvétel után.
3. Feladatátvevő nyilvántartása a **feladatok** lap. A feladatátvételi eléri a** teljes tesztelés** fázis, kattintson a **Teljes tesztelje** a próba feladatátvételi befejezéshez.
4. Kattintson a **jegyzetek** rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.
4. Befejezése után győződjön meg arról, hogy a virtuális gépeken futó sikeres indítása érdekében.
5. Annak ellenőrzése, hogy virtuális gépeken futó sikeres indítása érdekében, után jelenjenek meg a elszigetelt környezetben próba áttérni befejezéséhez. Ha automatikusan létre virtuális hálózatok, karbantartása törli az összes a próba virtuális gépeken futó és tesztelje a hálózatok.

> [AZURE.NOTE] Ha a teszt feladatátvétel továbbra is fennáll, több mint két hétig azt fogja kell elvégeznie hatályba. Azok az elemek és a vizsgálat feladatátvételkor automatikusan létrehozott virtuális gépeken futó is törli.


#### <a name="prepare-dhcp"></a>DHCP előkészítése

Ha a virtuális gépeken futó részt próba feladatátvevő DHCP használatához próba DHCP-kiszolgálót, létre kell hozni a próba feladatátvevő céljából létrehozott elszigetelt hálózaton belül.


### <a name="prepare-active-directory"></a>Felkészülés az Active Directory
Próba feladatátvevő tesztelése alkalmazás futtatásához szüksége lesz az Active Directory munkakörnyezetben másolatát a próba-környezetben. Hajtsa végre a [feladatátvevő megfontolások az active directory tesztelése](site-recovery-active-directory.md#considerations-for-test-failover) szakaszban további információt. 


### <a name="prepare-dns"></a>Felkészülés a DNS

Felkészülés a DNS-kiszolgáló a próba feladatátvételi az alábbi képlettel történik:

- **DHCP**– virtuális gépeken futó DHCP használja, ha a teszt DNS IP-címét frissíteni kell a a próba útja. Ha egy Windows hálózati virtualizációs hálózati típusú használata esetén a VMM kiszolgáló végpontként a útja. Ezért DNS IP-címét frissíteni kell a próba feladatátvevő hálózaton. Ebben az esetben a virtuális gépeken futó regisztrálja magukat a releváns DNS-kiszolgálóhoz.
- **Statikus cím**– virtuális gépeken futó a statikus IP-címet használja, ha a teszt DNS-kiszolgáló IP-címét frissíteni kell próba feladatátvevő hálózaton. Előfordulhat, hogy módosítania kell az IP-címet, a próba virtuális gépeken futó DNS. A következő mintaparancsfájl erre a célra használható: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Futtassa a tervezett feladatátvevő (másodlagos elsődleges)

 Ez az eljárás a helyreállítási terv tervezett feladatátvevő futtatása ismerteti. Másik lehetőségként a feladatátvételi egyetlen virtuális géphez futtathatja a **virtuális gépeken futó** lapon.

1. Megkezdése előtt győződjön meg arról, hogy az összes a virtuális gépeken futó átadni kívánt befejeződött a kezdeti replikáció.
2. Válassza a **Helyreállítás tervek** > *recoveryplan_name*. Kattintson a **feladatátvevő** > **feladatátvevő tervezett**. 
3. A **Tervezett feladatátvevő megerősítése **lapon válassza ki a forrás- és célwebhelyek helyeken. Megjegyzés: a feladatátvevő irányát.

    - Előző feladatátadás dolgozott a várt módon működik, és a forráslista vagy a Céllista helyen található összes virtuális gép kiszolgálóra, a feladatátvevő irányba részletek is információt csak. 
    - Ha mind a forrás- és a helyek a virtuális gépeken futó aktív, a **Módosítás szövegirány** gomb jelenik meg. Ez a gomb segítségével módosíthatja, és adja meg, amelyben a feladatátvételi történjen irányának.

5. Ha Ön éppen adatkapcsolat fölé Azure, és adatok titkosítása engedélyezve van a felhőben, a **Titkosítási kulcs** jelölje ki a szolgáltató telepítéskor VMM kiszolgálói adatok titkosítása engedélyezésekor kiadott tanúsítványt. 
6. Tervezett feladatátvevő megkezdésekor első lépésként kapcsolja ki a virtuális gépeken futó ahhoz, hogy az adatvesztés. A **feladatok** lapon hajtsa végre az feladatátvevő végrehajtását. Ha hiba történik, a (vagy egy virtuális gépen vagy parancsfájl, amelyben szerepel a helyreállítási terv), című témakörében a helyreállítási terv tervezett feladatátvételének leáll. A feladatátvételi újra kezdeményezhet.
8. Replika virtuális gépeken futó létrehozása után legyenek a jóváhagyás állapota függőben. Kattintson a **Jóváhagyás** véglegesítse a feladatátvételi gombra. 
9. Replikáció töltse ki a virtuális gépeken futó indítási a másodlagos helyére. 

## <a name="run-an-unplanned-failover"></a>Feladatátvételhez futtatása

Ez az eljárás a helyreállítási terv feladatátvételhez futtatása ismerteti. Azt is megteheti a feladatátvételi egyetlen virtuális gép vagy fizikai kiszolgáló futtathatja a **virtuális gépeken futó** lapon.

1. Válassza a **Helyreállítás tervek** > *recoveryplan_name*. Kattintson a **feladatátvevő** > **nem tervezett feladatátvételre**. 
3. Az **Erősítse meg a tervezett feladatátvevő **lapon válassza ki a forrás- és célwebhelyek helyeken. Megjegyzés: a feladatátvevő irányát.

    - Előző feladatátadás dolgozott a várt módon működik, és a forráslista vagy a Céllista helyen található összes virtuális gép kiszolgálóra, a feladatátvevő irányba részletek is információt csak. 
    - Ha mind a forrás- és a helyek a virtuális gépeken futó aktív, a **Módosítás szövegirány** gomb jelenik meg. Ez a gomb segítségével módosíthatja, és adja meg, amelyben a feladatátvételi történjen irányának.

4. Ha Ön éppen adatkapcsolat fölé Azure, és adatok titkosítása engedélyezve van a felhőben, a **Titkosítási kulcs** jelölje ki a szolgáltató telepítéskor VMM kiszolgálói adatok titkosítása engedélyezésekor kiadott tanúsítványt. 
5. Jelölje ki a **virtuális gépeken futó leállíthatja, és a legfrissebb adatok szinkronizálása** adja meg, hogy webhely helyreállítási próbálja meg a védett virtuális gépeken futó leállítás, és szinkronizálja az adatokat, hogy a legújabb az adatok felett sikertelen lesz. Ha nem jelöli ki ezt a lehetőséget, vagy a kísérlet nem sikerül a feladatátvételi lesz a virtuális gép legújabb elérhető helyreállítási ponttól.
6. A **feladatok** lapon hajtsa végre az feladatátvevő végrehajtását. Figyelje meg, hogy akkor is, ha a hibákról feladatátvételhez alatt, a helyreállítási terv fut, amíg nem fejeződik be.
7. A feladatátvétel, után a virtuális gépeken futó **függőben lévő véglegesítése** állapotban vannak. Kattintson a **Jóváhagyás** véglegesítse a feladatátvételi gombra.
8. Ha több helyreállítási pontok szeretne beállítani a replikáció, módosítás helyreállítási pont közül választhat használni a helyreállítási pont, amely nem a legújabb (alapértelmezés szerint a legfrissebb van használatban). Miután további véglegesítése után helyreállítási pontok törlődnek.
9. Replikációs befejeződése után a virtuális gépeken futó indítási, és futtatja a másodlagos helyére. Azonban ezek nem védett, vagy replikáló. Ha az elsődleges webhely ismét az azonos mögöttes infrastruktúra számára érhető el, kattintson a **Fordított bizonyos** való replikáció fordított megkezdéséhez. Ez biztosítja, hogy minden adat replikált-e az elsődleges webhelyek, és hogy a virtuális gép készen áll a feladatátvevő újra. Miután feladatátvételhez vonz egy általános az adatátvitel replikációs visszavonása Az áthelyezés a felhőbe kezdeti replikációs beállításainak beállított ugyanazzal a módszerrel fogja használni.

## <a name="failback-from-secondary-to-primary"></a>Az elsődleges másodlagos visszaállás

 Az elsődleges másodlagos helyre való áttérés replikált virtuális gépeken futó webhely helyreállítás nem védett és a másodlagos helyre most az az elsődleges éppen dolgozik. Kövesse ezeket a műveleteket, az eredeti elsődleges webhelyek sikertelen lesz. Ez az eljárás a helyreállítási terv tervezett feladatátvevő futtatása ismerteti. Másik lehetőségként a feladatátvételi egyetlen virtuális géphez futtathatja a **virtuális gépeken futó** lapon.

1. Válassza a **Helyreállítás tervek** > *recoveryplan_name*. Kattintson a **feladatátvevő** > **feladatátvevő tervezett**.
2. A **Tervezett feladatátvevő megerősítése **lapon válassza ki a forrás- és célwebhelyek helyeken. Megjegyzés: a feladatátvevő irányát. Ha sikerült a feladatátvételi az elsődleges számíthat, mint és minden virtuális gépeken futó Ez az információ csak a másodlagos helyen.
3. Ha éppen nem működnek az Azure vissza **Adatszinkronizálás**jelölje be a beállítások:

    - **Adatszinkronizálás átadása (csak szinkronizálása saját konfigurációjuk delta módosítás esetén) előtt**– Ez a beállítás kis méretűre állítása a virtuális gépeken futó legrövidebb leállás, akkor szinkronizálja őket leállítása nélkül. Hajtja végre a következőket:
        - 1 fázis: Azure virtuális gép pillanatfelvételt, és másolja a helyszíni a Hyper-V állomáshoz. A gép továbbra is fennáll, futó Azure-ban.
        - 2 fázis: Leállítja a virtuális gép Azure-ban, hogy nincs egyesíthető változtatás ott előfordul. A módosítások végleges készlete átkerülnek a helyszíni kiszolgálót és a helyszíni virtuális gép el kell indítani.
    

    - **Szinkronizálás adatok csak feladatátvételkor (teljes letöltés)**– használja ezt a lehetőséget, ha korábban már Azure a hosszú időn keresztül. Ezt a beállítást, hogy a lemez a legtöbb módosítását, és azt nem szeretné, hogy időt ellenőrző számítási megakadályozási oka gyorsabb. Végrehajtott a lemezen a letöltését. Ez akkor hasznos, ha a helyszíni virtuális gép törölték.
    
    > [AZURE.NOTE] Azt javasoljuk, ha korábban már Azure ideig használja ezt a beállítást (egy hónap vagy több) vagy a helyszíni virtuális törölve lett. Ez a beállítás nem számításokat bármely ellenőrző.
    
5. Ha Ön éppen adatkapcsolat fölé Azure, és adatok titkosítása engedélyezve van a felhőben, a **Titkosítási kulcs** jelölje ki a szolgáltató telepítéskor VMM kiszolgálói adatok titkosítása engedélyezésekor kiadott tanúsítványt. 
5. Alapértelmezés szerint az utolsó helyreállítási pontot használja, de **Módosítása helyreállítási pont** adhat meg egy másik helyreállítási pont. 
6. Kattintson a jelet a visszaállás indításához.  A **feladatok** lapon hajtsa végre az feladatátvevő végrehajtását. 
7. az adatok az átadása előtt szinkronizálni, miután az eredeti adatok szinkronizálása befejeződik, és készen áll a virtuális gépeken futó az Azure-leállítása lehetőséget választotta f kattintson a **projektek**  >  <planned failover job name> **Teljes feladatátvevő**. Ez az Azure gépi leállítja, a legfrissebb módosítások át a helyszíni virtuális gép és indítja el.
8. Azt is megteheti, miután bejelentkezik a virtuális gépen érvényesítés érhető el is. 
9. A virtuális gép Függőben állapot a jóváhagyás szerepel. Kattintson a **Jóváhagyás** véglegesítse a feladatátvételi gombra.
10. Most már befejezéséhez a visszaállás jelölje be a **Fordított bizonyos** védelme a virtuális gép az elsődleges webhely indításához.



## <a name="failback-to-an-alternate-location"></a>Visszaállás egy másik helyre

Ha már telepítette a védelmet a [Hyper-V webhely- és Azure](site-recovery-hyper-v-site-to-azure.md) kell azt jelenti, hogy visszaállás az Azure való között egy alternatív helyszíni helyét. Ez akkor hasznos, ha módosítani szeretné a helyszíni hardveren beállítása. Itt látható, hogyan teheti.

1. Ha állít be az új hardver telepítse a Windows Server 2012 R2 és a Hyper-V szerepkör a kiszolgálón.
2. Hozzon létre egy virtuális hálózati kapcsoló, az eredeti kiszolgálón van ilyen nevű.
3. Válassza a **Védett elemek** -> **Védelem csoport**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> visszavétele, és válassza a **Tervezett feladatátvevő**szeretne.
4. **Erősítse meg tervezett című** témakörében válassza a **Létrehozás helyszíni virtuális gép, ha még nem létezik**. 
5. **A Host Name** válassza az új a Hyper-V host kiszolgáló, amelyen el szeretné helyezni a virtuális gépen.
6. Az adatok szinkronizálását, javasoljuk, hogy válassza a **szinkronizálás az adatokat a átadása előtt**lehetőséget. A kis méretűre állítása a virtuális gépeken futó legrövidebb leállás, akkor szinkronizálja őket leállítása nélkül. Hajtja végre a következőket:

    - 1 fázis: Azure virtuális gép pillanatfelvételt, és másolja a helyszíni a Hyper-V állomáshoz. A gép továbbra is fennáll, futó Azure-ban.
    - 2 fázis: Leállítja a virtuális gép Azure-ban, hogy nincs egyesíthető változtatás ott előfordul. A módosítások végleges készlete átkerülnek a helyszíni kiszolgálót és a helyszíni virtuális gép el kell indítani.
    
7. Kattintson a jelet a feladatátvételi (visszaállás) megkezdéséhez.
8. Miután a kezdeti szinkronizálás befejeződik, és készen áll a Azure virtuális gép leállítása, kattintson a **feladatok** > <planned failover job> > **Teljes feladatátvevő**. Ezzel leállítja az Azure számítógépen, a legfrissebb módosítások át a helyszíni virtuális gép, és elindítja.
9. Ellenőrizze, hogy minden megfelelően működik a virtuális helyszíni gépen alakzatot naplózható. Kattintson a **véglegesítse** a feladatátvételi befejezéséhez.
10. Jelölje be a **Fordított bizonyos** védelme a helyszíni virtuális gép indításához.

    >[AZURE.NOTE] A visszaállás feladat adatszinkronizálás lépés a Mégse gombra, ha a helyszíni virtuális sérültek állapotban lesz. Ennek oka az, a legfrissebb adatokat az Azure virtuális lemezek a helyszíni adatok lemezre, és a szinkronizálás untill adatok szinkronizálásának példányban végrehajtotta, a lemez adatok nem feltétlenül konzisztens állapotban. Ha a On-prem virtuális akkor indul el, miután adatok szinkronizálásának megszakad, akkor előfordulhat, hogy nem elindulni. Töltse ki az adatok szinkronizálásának áttérni újra elindítani.
 
