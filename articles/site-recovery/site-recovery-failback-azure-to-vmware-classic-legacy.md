<properties 
   pageTitle="Sikertelen vissza VMware virtuális gépeken futó és VMware (régi) az Azure fizikai kiszolgálók |} Microsoft Azure" 
   description="Ez a cikk ismerteti az Azure az Azure-webhely helyreállításhoz van már replikált VMware virtuális gép visszavétele." 
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
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Sikertelen vissza VMware virtuális gépeken futó és fizikai kiszolgálók az Azure VMware az Azure-webhely helyreállításhoz (régi)

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-failback-azure-to-vmware.md)
- [Azure klasszikus portál](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klasszikus Portal (régi)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogy replikált a helyszíni webhelyről Azure után vissza VMware virtuális gépeken futó és a Windows vagy Linux fizikai kiszolgálók az Azure a helyszíni webhely nem működnek.

>[AZURE.NOTE] Ez a cikk ismerteti a régi példa. Ez a cikk csak meg kell használni a képernyőn megjelenő utasításokat, [alábbi régebbi](site-recovery-vmware-to-azure-classic-legacy.md)utasításokat követve Azure replikált, ha. Ha a replikáció használata a [bővített funkciójú telepítési](site-recovery-vmware-to-azure-classic-legacy.md) beállítása, majd kövesse az [Ebben](site-recovery-failback-azure-to-vmware-classic.md) a cikkben utasításokat visszavétele. 


## <a name="architecture"></a>Architektúra

Ez az ábra a feladatátvétel és visszaállás forgatókönyv jelöli. A kék feladatátvételkor használt kapcsolatok. A piros a kapcsolatok visszaállás során. A vonalak, nyilak nyissa meg az interneten keresztül.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Előzetes teendők 

- Ön a VMware VMs vagy a fizikai kiszolgálón keresztül nem sikerült, és kell futnia Azure-ban.
- Ne feledje, hogy, csak nem vissza VMware virtuális gépeken futó és a Windows vagy Linux fizikai kiszolgálók az Azure VMware virtuális gépeken futó a helyszíni elsődleges webhelyen.  Akkor vissza fizikai gép esetén nem működnek, ha Azure való áttérés az Azure virtuális konvertálja, és VMware csomópontoktól egy VMware virtuális konvertálja.

Íme, hogyan állíthatja be visszaállás:

1. **Visszaállás összetevők beállítása**: vContinuum kiszolgáló beállítása kell a helyszíni, mutasson a a fiókkonfigurációs kiszolgáló virtuális Azure-ban. Akkor fogja is állíthatja be a folyamat kiszolgáló az Azure virtuális az adatok küldése a helyszíni fő tároló kiszolgáló. A folyamat kiszolgáló regisztrálása a fiókkonfigurációs kiszolgáló, amely a feladatátvételi kezeli. Egy helyszíni fő célalkalmazás kiszolgálón telepítenie. Ha szüksége van egy Windows fő tároló kiszolgáló, akkor automatikusan beállítani vContinuum telepítésekor. Ha szüksége van Linux kell állítsa be manuálisan a üzemeltető kiszolgálón.
2. **Védelem és visszaállás engedélyezése**: az összetevők beállítása után a vContinuum kell védelem bekapcsolása az Azure VMs fölé sikertelen volt. Fogja a VMs készenléti ellenőrzéséhez, és futtassa az Azure feladatátvevő a helyszíni webhely. Visszaállás befejeződése után, reprotect helyszíni gépek, hogy induljanak el, esetében, amelyek Azure.



## <a name="step-1-install-vcontinuum-on-premises"></a>Lépés: 1: VContinuum a helyszíni telepítéséhez

Kell vContinuum kiszolgáló helyszíni telepítése, és mutasson a a fiókkonfigurációs kiszolgáló.

1.  [Töltse le a vContinuum](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Töltse le a [vContinuum módosítása](http://go.microsoft.com/fwlink/?LinkID=533813) verziója.
3. Telepítse a legújabb vContinuum. A **Kezdőlap** lapon kattintson a **Tovább**gombra.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  A varázsló első lapján adja meg a CX kiszolgáló IP-címét, és az CX portot. Jelölje ki a **HTTPS használatát**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Keresse meg a fiókkonfigurációs kiszolgáló IP-cím meg a fiókkonfigurációs kiszolgáló virtuális **Irányítópult** lapja Azure-ban.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  A kiszolgáló keresse meg a fiókkonfigurációs kiszolgáló virtuális **Végpontok** lapján nyilvános HTTPS-port Azure-ban.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. **CS jelszó a részletek** lapon adja meg a jelszó lejegyzett lefelé a fiókkonfigurációs kiszolgáló regisztrálásakor. Ha nem emlékszik rá jelölje be, **C:\\programfájlok (x86)\\InMage rendszerek\\személyes\\connection.passphrase** virtuális konfigurációs kiszolgálón.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  A **Célhely kiválasztása** lapon adja meg, amelyre a vContinuum server telepítése, és kattintson a **telepítés**gombra.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Telepítés befejezése után elindíthatja az vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Lépés: 2: Telepítése egy folyamat server Azure-ban 

Egy folyamat server telepítéséhez Azure-ban, hogy az Azure-ban VMs küldhet az adatokat vissza egy helyszíni fő tároló kiszolgáló szükséges. 

1.  Azure **Konfigurációs kiszolgálóihoz** lapon válassza az új folyamat kiszolgáló hozzáadása.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Adja meg a folyamat kiszolgáló nevét, és nevét és jelszavát a virtuális gép rendszergazdaként csatlakozni. Jelölje ki a kívánt a folyamat kiszolgáló regisztrálása konfigurációs kiszolgálója. Ugyanazon a kiszolgálón, hogy használja védelme és a nem sikerül a virtuális gépeken futó keresztül kell.
3.  Adja meg az Azure hálózati, amelyben a folyamat kiszolgáló telepíthető. A kiszolgáló azonos hálózaton kell tenni. Adjon meg egy egyedi IP-cím kijelölt alhálózat, és kezdje el a telepítés.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  A folyamat kiszolgáló üzembe induljanak egy feladatot.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  A folyamat server Azure-ban telepítik után jelentkezhet be a kiszolgálóra, a megadott hitelesítő adataival. A folyamat kiszolgáló regisztráljon regisztrálta a helyszíni folyamat kiszolgáló megegyező módon. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] A visszaállás során regisztrálva kiszolgálók nem látható a webhely helyreállítási virtuális tulajdonságai csoportban. Csak azok a konfigurációs kiszolgáló, amelyhez regisztrálva van a **kiszolgálók** lapján látható. Csak azok a lapon megjelenik a folyamat kiszolgáló körülbelül 10 – 15 percet igénybe vehet.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Lépés 3: A fő tároló kiszolgáló helyszíni telepítése

Az adatforrás virtuális gépeken futó operációs rendszertől függően telepítenie kell ezeket a Linux vagy Windows-fő tároló kiszolgáló helyszíni.

### <a name="deploy-a-windows-master-target-server"></a>A fő cél Windows server telepítése

A Windows fő cél már kapcsolt, a vContinuum beállítással. Ha telepíti a vContinuum, a fő kiszolgáló is ugyanarra a gépre telepített és a konfigurációs kiszolgálója regisztrált.

1.  Telepítési első lépésként hozzon létre egy üres a gép a helyszíni az alakzatot, amelynek meg szeretné helyreállítani az VMs az Azure ESX állomáson.

2.  Győződjön meg arról, hogy nincsenek-e a virtuális csatolt legalább két lemez – az operációs rendszer szolgál egyik, és a többi az adatmegőrzési meghajtó szolgál.

3.  Telepítse az operációs rendszer.

4.  A vContinuum telepíteni a kiszolgálón. Ezzel is befejezte a fő cél server telepítése.

### <a name="deploy-a-linux-master-target-server"></a>A fő cél Linux server telepítése

1.  Telepítési első lépésként hozzon létre egy üres a gép a helyszíni az alakzatot, amelynek meg szeretné helyreállítani az VMs az Azure ESX állomáson.

2.  Győződjön meg arról, hogy nincsenek-e a virtuális csatolt legalább két lemez – az operációs rendszer szolgál egyik, és a többi az adatmegőrzési meghajtó szolgál.

3.  Telepítse a Linux operációs rendszer. A Linux fő cél rendszer ne használja a legfelső szintű vagy adatmegőrzési tárhelyek LVM. Linux fő tároló kiszolgáló alapértelmezés szerint LVM partíciót/lemez feltárás elkerülése érdekében van beállítva.
4.  Az partíciók hozható létre:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Végezheti el az alábbi közzé a telepítési útmutatást a fő cél telepítés megkezdése előtt.


#### <a name="post-os-installation-steps"></a>Bejegyzés OS telepítési lépései

Úgy juthat az SCSI ID's az egyes merevlemez-meghajtó SCSI Linux virtuális gépen, engedélyezze a paraméter "lemez. EnableUUID = TRUE "az alábbi képlettel történik:

1. Állítsa le a virtuális gépet.
2. Kattintson a jobb gombbal a bal oldali panelen a virtuális bejegyzés > **Beállítások szerkesztése elemre**.
3. Kattintson a **Beállítások** fülre. Válassza a **Speciális\>általános elem** > **Konfigurációs paramétereket**. A **Konfigurációs paramétereket** beállítást csak érhető el, a gép leállítása.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Annak vizsgálata, hogy **lemezzel sor. EnableUUID** létezik. Ha nem, és értéke **hamis,** majd állítsa be **Igaz** (nem kis-és nagybetűk). Ha létezik és értéke igaz, kattintson a **Mégse gombra** , és tesztelje a SCSI parancs a vendég operációs rendszer belül, után be. Ha nem létező **Sor hozzáadása**gombra.
5. Adja hozzá a lemezt. A **név** oszlopban EnableUUID. Az érték értéke igaz. Ne legyen a fenti értékeket a kettős idézőjelek együtt.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Töltse le és telepítse a további csomagok

Megjegyzés: Győződjön meg arról, hogy a rendszer internetkapcsolattal rendelkező letöltése és a további csomagok telepítése előtt.

\#yum -y xfsprogs perl lsscsi rsync wget kexec-eszközök telepítése

Ez a parancs CentOS 6.6 tárházba ezekről a csomagokról 15 letölti és telepíti őket:

BC-1.06.95-1.el6.x86\_64. rpm

busybox-1.15.1-20.el6.x86\_64. rpm

elfutils-könyvtárral lenne-0.158-3.2.el6.x86\_64. rpm

kexec-eszközök-2.0.0-280.el6.x86\_64. rpm

lsscsi-0,23-2.el6.x86\_64. rpm

lzo-2.03-3.1.el6\_5.1.x86\_64. rpm

Perl-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-modul-beépíthető-3.90-136.el6\_6.1.x86\_64. rpm

Perl-Pod-Kilépés-1.04-136.el6\_6.1.x86\_64. rpm
    
Perl-Pod-egyszerű – 3.13-136.el6\_6.1.x86\_64. rpm

Perl-könyvtárral lenne-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-verzió – 0.77-136.el6\_6.1.x86\_64. rpm

rsync-3.0.6-os-12.el6.x86\_64. rpm

klassz kis 1.1.0 1.el6.x86\_64. rpm

wget-1.12-5.el6\_6.1.x86\_64. rpm

Megjegyzés: Ha a forrás gépet használ a legfelső szintű vagy indítási eszköz Reiser vagy XFS formázáshoz, majd alábbi csomagok kell letölti és telepíti a Linux fő célalkalmazás védelme előtt.

\#CD-re /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#rpm - ivh kmod-reiserfs-0.0-1.el6.elrepo.x86\_64. rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#rpm - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Egyéni konfigurációs módosítások alkalmazása

A módosítások alkalmazása előtt ellenőrizze, hogy befejezte az előző szakaszban, majd tegye a következőket:


1. A 6-64 RHEL egyesített Agent bináris másolása az újonnan létrehozott OS.

2. Ezzel a paranccsal a bináris untar futtatása: **tar - zxvf \<Fájlnév\> **

3. Ezzel a paranccsal engedélyezhetik futtatása: \# **chmod 755./ApplyCustomChanges.sh**

4. Futtassa a: ** \# ./ApplyCustomChanges.sh**. Futtassa a parancsfájlt csak egyszer a kiszolgálón. Indítsa újra a kiszolgálóra, a parancsfájl futtatása után.



### <a name="install-the-linux-server"></a>A Linux server telepítése


1. [Töltse le](http://go.microsoft.com/fwlink/?LinkID=529757) a fájlt.
2. Másolja a vágólapra a fájlt a Linux fő cél virtuális géphez segédprogrammal egy sftp ügyfél lehetőség. Váltakozva jelentkezzen be a Linux fő célalkalmazás virtuális gép és wget használni szeretné, hogy a telepítőcsomag letöltését a megadott hivatkozást
3. A Linux fő cél kiszolgáló virtuális gépet használ bejelentkezik egy ssh ügyfél lehetőség
4. Ha a Linux fő cél kiszolgálón keresztül a virtuális Magánhálózati kapcsolat telepítve a Azure hálózathoz kapcsolódik majd használja a belső IP-címet a kiszolgáló, amely a virtuális gép **Irányítópult** fülre, majd a port való csatlakozáshoz a Linux fő tároló kiszolgáló biztonságos rendszerhéjával 22 találhat.
5. Ha egy nyilvános hálózati kapcsolaton keresztül csatlakozik a Linux fő cél kiszolgálóhoz a Linux fő tároló kiszolgáló nyilvános virtuális IP-címet használja (a lapról virtuális gépeken futó **Irányítópult** ), és a nyilvános végpont által létrehozott ssh a Linux servder bejelentkezni.
6. Bontsa ki a fájlokat a gzipped Linux fő tároló kiszolgáló installer tar archívumból futtatásával: *"tar xvzf – Microsoft – automatikus rendszer-Helyreállítás\_UA\_8.2.0.0\_RHEL6-64\"* , amely tartalmazza a telepítőfájl könyvtárból.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Ha a telepítő egy másik mappába a fájlok átvitele a könyvtár kicsomagolta, amely a tar tartalmának archiválása kibontása volt. A könyvtár elérési útját, futtassa a "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Amikor a rendszer kéri, válassza ki a elsődleges szerepkörbe válassza a **2 (fő cél)**. A többi interaktív telepítési beállítások hagyja meg az alapértelmezett értékeket.
9. Várja meg a telepítés folytatásához és jelennek meg a Config Host felületet. A Host konfigurációs segédprogram Linux fő sarget Server parancssori segédprogram. Nem méretezze át a ssh ügyfél segédprogram ablakot. A nyílbillentyűk segítségével válassza ki a **globális** lehetőséget, és nyomja le az ENTER billentyűt.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. A mező **IP** adja meg a fiókkonfigurációs kiszolgáló (találja meg a fiókkonfigurációs kiszolgáló virtuális **Irányítópult** lapja a) belső IP-címét, és nyomja le az ENTER BILLENTYŰT. Írja be a **Port** a **22** , és nyomja le az ENTER BILLENTYŰT. 
11.  Hagyja bejelölve a **Használata HTTPS** , **Igen**, és nyomja le az ENTER BILLENTYŰT.
12.  Adja meg a fiókkonfigurációs kiszolgáló a létrehozott jelszót. Használata gitt ügyfél Windows számítógépről történő ssh a Linux virtuális géphez Shift + Insert a vágólap tartalmának beillesztése is használhatja. A jelszó használatával Ctrl-C helyi vágólapra másolása, és illessze be a Shift + Insert segítségével. Nyomja le az ENTER BILLENTYŰT.
13.  A JOBBRA billentyűvel nyissa meg a zárja be, és nyomja le az ENTER BILLENTYŰT. Várja meg a telepítés befejezéséhez.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Ha valamiért nem sikerült regisztrálni a Linux fő tároló kiszolgáló a fiókkonfigurációs kiszolgáló műveleteket hajthat végre, újra futtatásával a fogadó konfigurációs segédprogram /usr/local/ASR/Vx/bin/hostconfigcli (első kell hozzáférési engedélyek beállítása a címtárhoz chmod a felügyelő felhasználóként futtatásával).


#### <a name="validate-master-target-registration"></a>Fő cél regisztrációs ellenőrzése

Ellenőrizheti, hogy a fő tároló kiszolgáló sikeresen regisztrálva van-e a kiszolgáló Azure webhely helyreállítási tárolóból elemre a > **Konfigurációs kiszolgálója** > **Server részleteket**.

>[AZURE.NOTE] A fő tároló kiszolgáló regisztrálás után ha kitörölt a virtuális gép előfordulhat, hogy az Azure vagy végpontokhoz nem megfelelően konfigurált, konfigurációs hibák megjelenik ennek oka a fő cél konfiguráció által az Azure dndpoints lép fel, ha telepíti a fő cél Azure-ban, bár ez nem egy helyszíni fő tároló kiszolgáló helyszíni igaz. Ez nem befolyásolja a visszaállás, és a hibák figyelmen kívül hagyhatja. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Lépés: 4: A virtuális gépeken futó a helyszíni webhely védelme

VMs védelme a helyszíni webhely előtt vissza sikertelen lesz szüksége.

### <a name="before-you-begin"></a>Első lépések

A virtuális fölé Azure sikertelen, amikor egy extra temp meghajtóra, oldal fájl oszlopaihoz. Egy további meghajtóra, amely általában nem kötelező: az által a sikertelen virtuális, mivel előfordulhat, hogy már rendelkezik egy dedikált, oldal fájl meghajtó fölé. A virtuális gépeken futó fordított védelme megkezdése előtt győződjön meg arról, hogy a meghajtó offline állapotba, hogy nem első védett szeretne. Ehhez az alábbi képlettel történik:

1.  Nyissa meg a számítógép-kezelés, és válassza ki a tároló kezelése lapon, hogy a lemez online sorolja fel, és a géphez csatlakoztatott.
2.  Jelölje ki a ideiglenes lemezt a géphez csatlakoztatott, és válassza az offline állapotba. 

### <a name="protect-the-vms"></a>A VMs védelme

1. Az Azure-portálon annak érdekében, hogy azok esetén nem sikerült fölé a virtuális gép állapotának ellenőrzése

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Indítsa el a számítógépen lévő vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Kattintson a **Új Dokumentumvédelem** gombra, és válassza ki a művelet rendszer, a 

4.  Az új ablakban, a Megnyitás jelölje be az **operációs rendszer típusa** > a VMs visszavétele kívánt**Első részleteket** . Az **elsődleges kiszolgáló részletek**azonosítása, és válassza ki a védelemmel ellátni kívánt virtuális gépeken futó. VMs átadása előtt mintha vCenter host néven szerepelnek.
5.  Ha kijelöl egy virtuális gép védelme (és ezt már sikerült fölé a Azure) egy előugró ablak két bejegyzés jelenik a virtuális gép biztosít. Ennek oka az, a fiókkonfigurációs kiszolgáló észleli a virtuális gépeken futó regisztrált azt a két példánya. Távolítsa el a bejegyzést a helyszíni virtuális, hogy a helyes virtuális megvédheti szüksége. A helyes Azure virtuális bejegyzés azonosítása, jelentkezzen be a Azure virtuális, és nyissa meg a C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. A fájl drscout.conf mely a host azonosítóját. A vContinuum párbeszédpanelen hagyja meg a bejegyzést, amelynek a host azonosító megtalálható a virtuális. Az összes többi bejegyzés törlése. Jelölje ki a megfelelő virtuális IP-címét is hivatkozhat. Az IP cím tartomány helyszíni a helyszíni virtuális lesz.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. A vCenter kiszolgálón állítsa le a virtuális gépen. A VMs helyszíni is törölheti.
7. Ezután adja meg a helyszíni MT kiszolgáló, amelyhez a VMs védelemmel ellátni kívánt. Ehhez a vCenter kiszolgáló, amely a visszavétele szeretne csatlakozni.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Jelölje ki a fő célalkalmazás kiszolgáló, amelyhez a a virtuális helyreállítani kívánt állomás alapján.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Adja meg az egyes a virtuális gépeken futó a replikáció lehetőséget. Ehhez kell jelölje ki a helyreállítási egymás adattárhoz, amelyhez a VMs állíthatók lesz. A táblázat összefoglalja a különböző beállításokat kell megadnia az egyes virtuális.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **A beállítás** | **A javasolt értéket beállítás**
    ---|---
    A folyamat kiszolgáló IP-címe | Jelölje ki a folyamat kiszolgálót rendszerbe Azure-ban
    Adatmegőrzési mérete MB-ban| 
    Adatmegőrzési érték | 1
    Nap/óra | Napok
    Egységesebb intervallum | 1
    Jelölje be a cél adattárhoz | Az adattárhoz érhető el a helyreállítási webhelyen. A tárolót kell elég hely, és a ESX állomáshoz visszaállítása a virtuális gépen szeretné elérhetővé tenni.


10. A virtuális gép helyszíni webhelyre való áttérés után fog szerezni tulajdonságainak konfigurálása Az tulajdonságokat az alábbi táblázatban soroljuk fel.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **A tulajdonság** | **Részletek**
    ---|---
    Hálózati konfigurációja| Az összes hálózati adapteren észlelt jelölje ki azt, és állítsa be a virtuális gép visszaállás IP-címének **módosítása** gombra. 
    Hardverkonfiguráció| Adja meg a Processzor- és a memóriában a virtuális. Beállítások védelemmel ellátni kívánt összes VMs alkalmazhatók. Helyes értékek azonosítása Processzor és memóriát, olvassa el a IAAS VMs szerepkör méretét, és megtekintheti magmintákat és a rendelve memória számát.
    Megjelenítendő név | A helyszíni visszaálláshoz után átnevezheti a virtuális gépeken futó vCenter készlet megjelenő fogja. Az alapértelmezett neve a virtuális gép számítógép állomásnév. Azonosítja a virtuális nevét, a virtuális listára, kattintson a védelem csoport is hivatkozhat.
    Hálózati Címfordítást konfigurálása | Az alábbi részletes ismertetése

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Hálózati Címfordítást beállításainak konfigurálása

1. Ahhoz, hogy a virtuális gépeken futó védelmét, két kommunikációs csatornák kell létrehozni. Az első csatorna a virtuális gép és a folyamat kiszolgáló között van. A csatorna gyűjti össze a virtuális az adatokat, és elküldi a folyamat kiszolgáló, amely a fő tároló kiszolgáló által küld az adatokat. Ha a folyamat kiszolgáló és a virtuális gép védeni kívánt is ugyanazon a hálózaton Azure virtuális, akkor nem kell hálózati Címfordítást beállítások. Más módon adja meg a hálózati Címfordítást beállításokat. Azure megtekintheti a folyamat kiszolgáló nyilvános IP-címét. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. A második pedig a folyamat és a fő tároló kiszolgáló közötti. A lehetőség, amellyel a hálózati címfordító Eszközig, vagy nem attól függ, hogy vannak VPN-alapú kapcsolattal, vagy internetes kommunikáció. Nem jelöli ki a hálózati címfordító Eszközig, VPN használata, de csak akkor, ha az internetkapcsolat használ.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Ha még nem törli a megadott módon helyszíni virtuális gépeken futó, és az adattárhoz vissza az még nem működnek, a régi VMDK tartalmazza, majd szüksége lesz ahhoz, hogy az új helyen jön létre virtuális visszaállás. Ehhez jelölje ki a **Speciális** beállításokat, és adja meg a **Mappa nevét beállításainak**visszaállítása egy másik mappába. Hagyja az alapértelmezett beállításokkal az egyéb beállításokat. A Mappabeállítások alkalmazása az összes kiszolgálón.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Futtassa a felkészülés ellenőrizze, hogy győződjön meg arról, hogy vissza a helyszíni a védeni kívánt készen állnak-e a virtuális gépeken futó. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Várja meg, hogy befejezéséhez. Ha sikeresen lévő összes VMs megadhatja a védelem terv nevét. Kattintson a **védelem** a kezdéshez.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Figyelheti a folyamatban lévő vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. A **Védelem megtervezése aktiválása** fejeződik lépés után figyelheti a virtuális védelem a webhely helyreállítási portálon.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. A virtuális parancsra, és a meghívási folyamat figyeléshez pontos állapotát tekintheti meg.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>A visszaállás terv előkészítése

Egy visszaállás-csomag vContinuum, hogy az alkalmazás bármikor visszatérhet a helyszíni webhely is nem előkészítheti a. Ezekről a csomagokról helyreállítási nagyban hasonlítanak ahhoz a helyreállítási tervek webhely helyreállítás.

1.  Indítsa el a vContinuum, és válassza a **Manage csomagok** > **helyreállítása.** A tervek VMs átadni használt listája látható. Az azonos csomagok helyreállítása is használhatja.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Jelölje ki a védelem csomagot, és a VMs azt a helyreállítani kívánt összes. Válassza a minden virtuális további információra kíváncsi, beleértve a cél ESX kiszolgáló és a forrás virtuális lemez láthatja. Kattintson a **Tovább** gombra a törölt varázsló elindításához, majd válassza ki a helyreállítani kívánt VMs.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Több beállítások alapján visszaállíthatja, de azt javasoljuk, hogy használja **Legújabb címkét** , és válassza az **alkalmazás az összes VMs** annak érdekében, hogy a legfrissebb adatokat a virtuális számítógépről szolgálnak.
4. Futtassa a **készenléti ellenőrzés.** A gyakrabban ellenőrizze, hogy ha a megfelelő paramétereket vannak beállítva, hogy virtuális-helyreállítás engedélyezése. Ha a sikeresek összes ellenőrzést, kattintson a **Tovább** gombra. Ha nem talál, és a hibák megoldásához.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  **Virtuális konfigurációs** győződjön meg róla, hogy a helyreállítási beállítások helyesen vannak-e beállítva. A virtuális beállításokat módosíthatja, ha módosítani szeretné.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Tekintse át a virtuális gépeken futó hasznosítani kell, és adja meg a helyreállítási módját listáját. Figyelje meg, hogy VMs jelennek meg a számítógép host nevét használja. A számítógép állomásnév hozzárendelése a virtuális gép nehéz lehet.
Feleltesse meg a neveket, nyissa meg a virtuális gépeken futó **Irányítópult** Azure-ban, és jelölje be a virtuális állomásnév.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Adja meg a terv nevét, és válassza a **később helyreállítása**. Azt javasoljuk, mivel a kezdeti védelmet nem lehet teljes később visszaállításához. 
11. Kattintson a **helyreállítása** a terv mentése vagy elindítani a helyreállítás, ha állíthatja helyre a most és később nem jelölt ki. Érdemes helyreállítási állapotát tekintheti meg, ha a terv van mentve.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Virtuális gépeken futó helyreállítása

A terv adatait a létrehozása után visszaállíthatja a virtuális gépeken futó. Mielőtt érdemes, a virtuális gépeken futó elvégezte a szinkronizálást. Ha a replikáció Állapot oszlopban a OK gombra a védelmet, nincs befejezve, és a Készletben küszöbértékét teljesült. Virtuális tulajdonságok állapot ellenőrzése

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Kapcsolja ki az Azure virtuális gépeken futó, a helyreállítás elindítása előtt. Ez biztosítja nem osztott agy nem, és hogy felhasználók csak akkor tud hozzáférni az alkalmazás egy példányát. 


1.  Indítsa el a mentett csomagot. VContinuum **Monitor** válassza a tervek. Ez megjeleníti a már futtatott tervek.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  A **helyreállítási** válassza ki a tervet, és kattintson a **Start**gombra. Figyelheti a helyreállítási. Miután VMs van kapcsolva a csatlakozhat őket a vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Az Azure visszaállás után újra védelme

Visszaállás befejezése után fogja valószínűleg védelemmel ellátni kívánt a virtuális gépeken futó újra. Ehhez az alábbi képlettel történik:

1.  Ellenőrizze, hogy a virtuális gépeken futó helyszíni dolgozik, és hogy az alkalmazások érhetők el.
2.  Az Azure webhely helyreállítási portálon válassza ki a virtuális gépeken futó, és törölje őket. Jelölje ki a virtuális gépeken futó védelme letiltása. Ezzel biztosíthatja, hogy a VMs nincs több védelemmel ellátva.
3.  Azure-ban törlése a sikertelen Azure virtuális gépeken futó keresztül
4.  Törölje a régi virtuális gép a vSpehere. Ezek a korábban sikertelen fölé Azure VMs.
5.  A webhely helyreállítási portálon védelme a VMs, amelyeket nemrégiben nem sikerült fölé. Miután esetén a védett helyreállítási csomagra is hozzáadhatja.
 
## <a name="next-steps"></a>Következő lépések



- [Információ](site-recovery-vmware-to-azure-classic.md) esetében, VMware virtuális gépeken futó és fizikai kiszolgálók amelyek Azure a bővített funkciójú példányban használatával.

 
