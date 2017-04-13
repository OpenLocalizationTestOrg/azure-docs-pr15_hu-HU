<properties 
   pageTitle="Sikertelen vissza VMware virtuális gépeken futó és a helyszíni webhely fizikai kiszolgálók |} Microsoft Azure"
   description="Tudjon meg többet a VMware VMs feladatátvételének után vissza az a helyszíni webhely meghibásodása és Azure fizikai kiszolgálók." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Sikertelen vissza VMware virtuális gépeken futó és a helyszíni webhely fizikai kiszolgálók

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-failback-azure-to-vmware.md)
- [Azure klasszikus portál](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klasszikus Portal (régi)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



Ez a cikk ismerteti, hogyan meghiúsító vissza Azure virtuális gépeken futó az Azure a helyszíni webhelyet. Amikor készen áll a VMware virtuális gépeken futó visszavétele vagy a Windows vagy Linux fizikai kiszolgálók után azok már nem sikerült fölé a a helyszíni webhely használata az [oktatóprogram](site-recovery-vmware-to-azure-classic.md)Azure, kövesse a jelen cikkben utasításokat.



## <a name="overview"></a>– Áttekintés

Ez az ábra ebben az esetben visszaállás architektúrája jeleníti meg.

Ez a felépítés használja a folyamat kiszolgáló helyszíni és egy készült ExpressRoute használja.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Ez a felépítés használja a folyamat server Azure van, és a virtuális Magánhálózaton vagy a készült ExpressRoute származó van.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Tekintse át a teljes listáját portokat és a visszaállás architechture diagram olvassa el az alábbi képen

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Az alábbiakban visszaállás működése:

- Miután az Azure már nem sikerült fölé, nem sikerül a helyszíni webhelyek néhány lépésben:
    - **Szakasz 1**: az Azure VMs reprotect, így induljanak el, esetében vissza, a helyszíni webhely futó VMware VMs amelyek. A virtuális reprotection engedélyezése lép, amely automatikusan létrehozásakor a feladatátvételi védelem csoport eredetileg létrehozott visszaállás védelem csoportba. Ha hozzáadta a feladatátvételi védelem csoport helyreállítási csomagra, majd a visszaállás védelem csoport is automatikusan hozzá lett adva a csomagot.  Reprotect során, megadhatja, hogy milyen visszavétele megtervezése.
    - **Szakasz 2**: után az Azure VMs vannak esetében, amelyek a helyszíni webhely, futtassa a sikertelen vissza az Azure sikertelen lesz.
    - **Szakasz 3**: után vissza az adatok nem sikerült, így induljanak el, Azure esetében, amelyek a helyszíni VMs, akkor nem sikerült vissza, reprotect.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Az eredeti vagy másik helyre visszaállás

Ha nem sikerült egy VMware virtuális, ha továbbra is megmarad a helyszíni nem sikerül vissza az azonos forrásból virtuális fölé. Ebben az esetben csak a delta módosításokat sikertelen lesz vissza. Megjegyzés:

- Ha nem sikerült fizikai kiszolgálón keresztül visszaállás akkor mindig egy új VMware virtuális gép.
    - Adatkapcsolat vissza egy fizikai számítógépre előtt vegye figyelembe, hogy
    - Védett tényleges gépi vissza egy virtuális gép esetén vissza az Azure az VMware keresztül nem fog érkezni.
    - Győződjön meg arról, hogy, hogy megtalálja legalább egyet fő cél Server alkalmazás, a szükséges ESX/ESXi állomások, amelyhez kell visszaállás együtt.
- Ha vissza szeretne a eredeti virtuális nem sikerül az alábbiakra szükség:
    - Ha a virtuális vCenter kiszolgáló kezeli a fő célállomás ESX az VMs adattárhoz access kell rendelkeznie.
    - Ha a virtuális egy ESX állomáson, de nem vCenter kezeli majd a virtuális merevlemezének egy könnyen kezelhető adattárhoz a MT állomás a kell lennie.
    - Ha a virtuális egy ESX állomáson, és nem használható a vCenter majd kell végrehajtania, a MT ESX rengeteg felfedezése mielőtt, reprotect. Ez az érvényes, ha túl van adatkapcsolat vissza fizikai kiszolgálók.
    - Egy másik (ha létezik a helyszíni virtuális), hogy előtt teheti meg egy visszaállás törölheti azt. Ezután visszaállás ekkor hoz létre egy új virtuális, a fő ESX célállomás azonos állomáson.
    
- Ha egy másik helyre az adatok visszaállás visszakerül az azonos adattárhoz és az azonos ESX állomás, hogy a helyszíni fő tároló kiszolgáló által használt. 


## <a name="prerequisites"></a>Előfeltételek

- Annak érdekében, hogy a sikertelen lesz szüksége a VMware környezet VMware VMs és fizikai kiszolgálók biztonsági másolatot. Vissza a fizikai kiszolgáló hibás használata nem támogatott.
- Annak érdekében, hogy visszavétele kell létrehozott egy Azure hálózat védelem kezdetben beállításakor. Visszaállás Azure a hálózatról, amelyben az Azure VMs találhatók a helyszíni webhely készült ExpressRoute vagy a virtuális Magánhálózati kapcsolat szükséges.
- Győződjön meg arról, hogy kell vCenter kiszolgálóra kezelhetők vissza nem szeretné a VMs esetén a megfelelő engedélyekkel a VMs felfedezése vCenter kiszolgálókon. [További információ:](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Ha egy adott időpontban érvényes megtalálható egy virtuális reprotection meghiúsul. A pillanatképek vagy lemez törölheti. 
- Mielőtt, visszavétele kell összetevők számának létrehozása:
    - **Hozzon létre egy folyamat kiszolgálót Azure-ban**. Ez a az Azure virtuális, létrehozása és nyomon futtatása során visszaállás kell. A gép visszaállás befejeződése után törölheti.
    - **Hozzon létre egy fő célalkalmazás kiszolgálót**: A fő tároló kiszolgáló adatokat küld és fogad visszaállás. A helyszíni létrehozott management server a fő cél server van telepítve alapértelmezés szerint. Jó helyen jár attól függően, hogy a sikertelen vissza forgalom mennyiségű szükség lehet történő visszaállás külön fő cél kiszolgálót hozhat létre.
    - Ha szeretne létrehozni egy további fő cél Linux futtató, kell beállítása a Linux virtuális a fő tároló kiszolgáló telepítése előtt alábbiakban leírtak szerint.
- Konfigurációs kiszolgálója szükséges a helyszíni esetén egy visszaállás teheti meg. A virtuális gép során visszaálláshoz léteznie kell, a konfigurációs server-adatbázisban, nem működnek, mely visszaállás nem lesznek. Így győződjön meg arról, hogy a normál ütemezett, a kiszolgáló biztonsági másolat készítése. Abban az esetben, ha katasztrófa szüksége lesz visszaállítása az azonos IP-címet, így visszaállás fog működni.

## <a name="set-up-the-process-server-in-azure"></a>Állítsa be a folyamat server Azure-ban

Egy folyamat server telepítéséhez Azure-ban, hogy az Azure VMs küldhet az adatok helyszíni fő cél kiszolgálóhoz való szükséges. 

1.  A webhely helyreállítási portálon > **Konfigurációs kiszolgálóihoz** új folyamat kiszolgáló hozzáadásához jelölje ki.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Adja meg a folyamat kiszolgáló nevét, és írja be a név és a jelszó való csatlakozáshoz a Azure virtuális rendszergazdaként kell megadnia. A **Konfigurációs kiszolgálója** a helyszíni management server, és töltse ki az Azure hálózati, amelyben a folyamat kiszolgáló telepíthető. A hálózat, amelyben az Azure VMs találhatók kell. Adja meg a választó alhálózat egy egyedi IP-címét, és kezdje el a telepítés.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    A folyamat server telepítése a feladat aktiválódik

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Miután a folyamat kiszolgáló telepítve van is azt a megadott hitelesítő adataival jelentkezzen Azure-ban. A folyamat kiszolgáló párbeszédpanel első bejelentkezéskor fog futni. Írja be az IP-cím a helyszíni management server és a hozzáférési kódot. Az alapértelmezett 443-as port hagyja. Is hagyhatja, az alapértelmezett adatokat a replikáció 9443 port kivéve, ha a helyszíni management server beállításakor kifejezetten módosított ezt a beállítást. 

    >[AZURE.NOTE] A kiszolgáló nem látható a **virtuális tulajdonságai**. Gomb csak akkor látható, amelyre rá van regisztrálva management Server **kiszolgálók** lapján. Tudnivalók a kis időt igénybe 10 – 15 perc a folyamat kiszolgáló jelennek meg.


## <a name="set-up-the-master-target-server-on-premises"></a>A fő tároló kiszolgáló helyszíni beállítása

A fő tároló kiszolgáló visszaállás adatokat kapja. A fő tároló kiszolgáló automatikusan települ a helyszíni management server, de ha, esetén nem működnek vissza az adatokat, előfordulhat, hogy be kell állítania egy további fő tároló kiszolgáló. Ehhez az alábbi képlettel történik:

>[AZURE.NOTE] Ha szeretne egy fő cél server telepítése Linux, kövesse a következő eljárással a.

1. Ha Windows rendszeren telepíti a fő tároló kiszolgáló, az első lépések lap megnyitása a virtuális, amelyen a fő tároló kiszolgáló telepíti, és töltse le a telepítőfájlt a Azure webhely helyreállítási egyesített beállítási varázsló.
2. A telepítő futtatása, és az **első lépések** válassza a **Hozzáadás további folyamat kiszolgálók telepítési méretezése**.
3. A varázsló a megszokott módon mikor [a management server beállítása](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). A **Konfiguráció kiszolgáló adatai** lapon adja meg a fő tároló kiszolgáló, és a jelszó elérése a virtuális IP-címét.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>A fő cél kiszolgálójával egy Linux virtuális beállítása
Állíthatja be a egy Linux virtuális a Cent telepíteni kell a fő tároló kiszolgáló futtatott management server) S 6.6 minimális operációs rendszere, az egyes SCSI merevlemez SCSI azonosítók beolvasása, néhány további csomagok telepítése és alkalmazása egyéni módosításokat.

#### <a name="install-centos-66"></a>CentOS 6.6 telepítése

1.  Telepítse az CentOS 6.6 minimális operációs rendszer a virtuális kiszolgálóra. Az ISO megőrzése DVD-meghajtóba, és indítsa el a rendszer. Ugorja át, a media tesztelése, jelölje be a Nekünk az angol nyelvi, a nyelv, jelölje ki az **Egyszerű tárolók**, ellenőrizze, hogy a merevlemezen ne fontos adatokat, és kattintson az **Igen**gombra, adatok elvetése. Írja be az állomásnév a management Server és a kiszolgáló hálózati kártya kijelölése.  A **Rendszer szerkesztése** párbeszédpanelen jelölje be az** Automatikus csatlakozás** , és adja meg a statikus IP-címet, a hálózati és a DNS-beállítások. Időzóna és a management server eléréséhez legfelső szintű jelszó megadása 
2.  Amikor kérdés jelenik meg a telepítési típusát szeretné, jelölje be a partíciót **Létrehozása egyéni elrendezés** . Kattintás után **következő** **ingyenes** jelölje ki, és kattintson a Létrehozás gombra. Hozzon létre **/**, **/var/crash** és **/ otthoni partíciót** a **FS típusa:** **ext4**. Hozzon létre, a csere partion **FS típusa: felcserélése**.
3.  Ha már meglévő eszközök találhatók, egy figyelmeztetés fog megjelenni. Kattintson a **Formátum** a meghajtó partíciót beállításokkal formázása gombra. Kattintson a **Módosítás lemezre írása** a partíciót alkalmazásához.
4.  Válassza a **telepítés indítási betöltő** > **következő** az indítási betöltő telepítése a legfelső szintű partíciót.
5.  Amikor befejeződött a telepítés gombra, **Indítsa újra**.


#### <a name="retrieve-the-scsi-ids"></a>A SCSI azonosítók beolvasása

1. Telepítése után beolvashatja a virtuális minden SCSI merevlemez-SCSI-azonosítóját. Ehhez a management server virtuális a Leállítás, a virtuális tulajdonságok VMware kattintson a jobb gombbal a virtuális bejegyzés > **Beállítások szerkesztése** > **Beállítások**.
2. Válassza a **Speciális** > **Általános elemet** , és kattintson a **Paraméterek beállítása**gombra. Ez a beállítás a gépen fut de-active lesz. Aktiválja ezt a gép le kell állítani.
3. Ha a sor **lemez. EnableUUID** létezik győződjön meg arról, hogy az értéke **Igaz** (kis-és nagybetűk). Ha már van Mégse gombra, és tesztelje a SCSI parancs vendég operációs rendszer belül, után. 
4.  Ha a sor nem létező kattintson a **Sor hozzáadása** – gombra, és adja hozzá az **Igaz** értéket. A kettős idézőjelek, ne használja.

#### <a name="install-additional-packages"></a>További csomagok telepítése

Töltse le és telepítse a néhány további csomag kell. 

1.  Győződjön meg arról, hogy a fő tároló kiszolgáló csatlakozik az internethez.
2.  Töltse le és telepítse a CentOS tárházba 15 csomagok e parancs futtatásával: **# yum – y xfsprogs perl lsscsi rsync wget kexec-eszközök telepítése**.
3.  Ha az adatforrás gépek esetén védelme Linux wit Reiser futtatja, vagy XFS fájlrendszer a legfelső szintű vagy indítási lehetőséget, majd le kell töltenie és további csomagok telepítése az alábbi képlettel történik:

    - # <a name="cd-usrlocal"></a>CD-re /usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>rpm – ivh kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>rpm – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Egyéni változtatások alkalmazása

Hajtsa végre az alábbi egyéni módosítások után, ha már hajtsa végre a telepítés utáni és a csomagok telepítése:

1.  A 6-64 RHEL egyesített Agent bináris másolja a virtuális. Ez a bináris untar parancs futtatásával: **tar – zxvf <file name> **
2.  Ezzel a paranccsal engedélyezhetik futtatása: **# chmod 755./ApplyCustomChanges.sh**
3.  Futtassa a: **#./ApplyCustomChanges.sh**. Csak futtatnia kell a parancsfájl egyszer. Miután sikeresen futtatható parancsfájl, indítsa újra a kiszolgálóra.


## <a name="run-the-failback"></a>A visszaállás futtatása

### <a name="reprotect-the-azure-vms"></a>Az Azure VMs reprotect

1.  A webhely helyreállítási portálon > **gépek** lapon jelölje ki a virtuális, nem lett sikerült fölé, majd kattintson az **újból védelme**.
2.  A **Diaminta tároló kiszolgáló** , valamint **Folyamat** válassza ki a helyszíni fő tároló kiszolgáló, az Azure virtuális folyamat kiszolgálót.
3.  Jelölje ki a fiókot, a virtuális csatlakozhat beállított.
4.  Jelölje be a védelem Csoport visszaállási verzióját. A példában ha a virtuális PG1, akkor a védett kell PG1_Failback jelölje ki.
5.  Ha azt szeretné, egy másik helyre való, válassza az adatmegőrzési meghajtó és a fő célalkalmazás kiszolgálóhoz beállított adattárhoz. Ha nem sikerül a helyszíni webhelyek a VMware VMs visszaállás védelem tervben használja az azonos adattárhoz a fő tároló kiszolgáló. Ha a kópia helyreállítani kívánt Azure virtuális ugyanannak a helyszíni virtuális, majd a helyszíni virtuális már kell lennie az a fő célalkalmazás kiszolgálójával azonos adattárhoz. Ha nincs virtuális helyszíni reprotection során jön létre egy újat.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Kattintás után **az OK gombra** a feladat reprotection megkezdéséhez való replikáció a virtuális Azure a helyszíni webhely kezdődik. A **feladatok** lap követheti a végrehajtását.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Futtassa a helyszíni webhely feladatátvevő

Reprotection után a virtuális helyez át a védelem Csoport visszaállási verzióját, és automatikusan felkerül a helyreállítási csomagot, ha van ilyen az Azure való áttérés a használt.

1.  A **Helyreállítási tervek** lapon jelölje ki a a helyreállítási csomagot a visszaállás csoportot tartalmazó, és kattintson a **feladatátvevő** > **Nem tervezett feladatátvételre**.
2.  **Erősítse meg című** témakörében ellenőrizze a feladatátvevő irányának (az Azure), és jelölje ki a feladatátvételi (legújabb) használni kívánt helyreállítási pontot. Ha engedélyezte a **Több-virtuális** az replikációs tulajdonságai konfigurálásakor visszaállíthatja az alkalmazás legújabb verzióját vagy összeomlik egységes helyreállítási pont. A jelölőnégyzet be van jelölve a feladatátvételi indítása gombra.
3.  Feladatátvételkor webhely helyreállítási az Azure VMs leáll. Adott visszaállás befejeződött, a várt módon ellenőrzése után is ellenőrizheti, hogy az Azure VMs van már állítsa le a várt módon működik.

### <a name="reprotect-the--on-premises-site"></a>A helyszíni webhely reprotect

Visszaállás befejeződése után az adatok vissza a webhelyen, a helyszíni lesz, de nem fogja védeni. Azure esetében, amelyek indításához újra tegye a következőket:

1.  A webhely helyreállítási portálon > **gépek** lapon jelölje ki a sikertelen VMs készíteni, és kattintson a **újra védelme**. 
2.  Miután megbizonyosodott róla, hogy a replikáció Azure van a várt módon működnek, törölheti az Azure VMs (jelenleg nem fut), amely vissza nem sikerült Azure-ban.


### <a name="common-issues-in-failback"></a>Gyakori problémák visszaállás

1. Ha végezze el az írásvédett felhasználói vCenter feltárás és védelme virtuális gépeken futó, sikerrel, és feladatátvevő működik. A Reprotect időpontjában meghiúsul, mivel az nem lehet a datastores talált. A jelenség, nem jelenik meg a datastores szerepel a felsorolásban, újra védelme közben. Megoldásához vCenter hitelesítő adatok frissítése a megfelelő engedélyekkel rendelkező fiókkal, és próbálkozzon újra a feladatot. [További információ](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Ha egy Linux virtuális visszaállás, és indítsa el a helyszíni, látni fogja, hogy a hálózati kezelője csomag eltávolítása a számítógépről. Ennek oka az, ha a virtuális van helyreállított Azure-ban, a hálózati kezelője csomag törlődik.
3. Ha egy virtuális statikus IP-címet is be van állítva, és keresztül nem sikerült az Azure, az IP-cím szerezte be a DHCP keresztül. Ha nem sikerül helyszíni vissza keresztül, a virtuális továbbra is használja a DHCP szerezheti be az IP-címet. Bejelentkezési manuálisan kell a gép, és szükség esetén a statikus cím visszatérhet az IP-cím beállítása.
4. ESXi 5.5 ingyenes edition vagy a vSphere 6 hipervizor ingyenes edition használatakor feladatátvevő sikeres tenné, de visszaállás nem fog sikerülni. Lehetővé teszi a ned vagy kiértékelés licenc visszaállás ahhoz, hogy frissítsen.

## <a name="failing-back-with-expressroute"></a>Hibás vissza az készült ExpressRoute

Vissza a virtuális Magánhálózati kapcsolatot vagy a készült Azure ExpressRoute keresztül sikertelen lehet. Ha szeretné használni a készült ExpressRoute Megjegyzés: a következőket:

- Készült ExpressRoute a hálózaton Azure virtuális melyik adatforrás gépek fail fölé, és milyen Azure VMs találhatók után a feladatátadás létre kell hozni.
- Adatok egy nyilvános végpontot Azure tároló fiókkal van replikált. Állítson be nyilvános peering készült ExpressRoute be a cél adatok központban a webhely helyreállítási replikáció készült ExpressRoute használni.



