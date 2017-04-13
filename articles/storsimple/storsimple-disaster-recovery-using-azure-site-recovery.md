<properties 
   pageTitle="DR automatizálása az Azure-webhely helyreállítás segítségével StorSimple fájlmegosztások |} Microsoft Azure"
   description="Gyakorlati tanácsok a StorSimple tárterület is fájlmegosztások katasztrófa helyreállítási megoldást létrehozásához és lépéseit ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatikus vészhelyreállítás megoldást is StorSimple fájlmegosztások Azure-webhely helyreállítási használata

## <a name="overview"></a>– Áttekintés

Microsoft Azure StorSimple hibrid felhőalapú tárolási megoldást, amely megszünteti a bonyodalmainak gyakran társított fájlmegosztások strukturálatlan adatokat. StorSimple felhőbeli tárhelyről kiterjesztése a helyszíni megoldás és az automatikus rétegek adatokat tároló helyszíni és felhőbeli tárhelyről használja. Integrált adatvédelem, helyi és felhőbeli pillanatképek, egy sprawling tárterület-infrastruktúrát szükségtelenné.

[Azure-webhely helyreállítási](../site-recovery/site-recovery-overview.md) egy Azure-alapú szolgáltatás, amely szerint orchestrating replikációs, feladatátadási és helyreállítási a virtuális gépeken futó (DR) helyreállítási lehetőségeket biztosít a katasztrófa. Azure webhely helyreállítási replikációs technológiák bizonyos egységes, védelme és egyszerűen átveszi virtuális gépeken futó és a személyes és nyilvános vagy szolgáltatott felhőket alkalmazások számos használatát támogatja.

Azure webhely helyreállítási virtuális gép replikációs és StorSimple felhő pillanatkép funkciók használ, a teljes fájlelérési kiszolgálói környezetben védelme. Egy állásidőt azzal, ha egyetlen kattintással is használhatja a fájlmegosztások online állapotba Azure-ban mindössze néhány perc alatt.

A dokumentum részletesen bemutatja, hogyan katasztrófa helyreállítási megoldás létrehozása a fájlmegosztások is StorSimple tárolására, és végezze el a tervezett, a tervezett és tesztelése feladatátadás egykattintásos helyreállítási-csomagot használ. Lényegében látható, hogyan módosíthatja a helyreállítási megtervezése az az Azure webhely helyreállítási tárolóból elemre StorSimple feladatátadás ahhoz, hogy katasztrófa esetek során. Ezeken kívül azt ismerteti, támogatott konfigurációk és előfeltételekről. A dokumentum tartalma feltételezi, hogy már jól ismert Azure webhely helyreállítási és StorSimple architektúrákban alapjait.

## <a name="supported-azure-site-recovery-deployment-options"></a>Támogatott Azure webhely helyreállítási telepítési lehetőségek

Ügyfelek fájlkiszolgálókhoz telepítheti fizikai kiszolgálók vagy virtuális gépeken futó (VMs) a Hyper-V vagy VMware rendszeren futó, és ki StorSimple tároló faragottnak kötet fájlmegosztások majd létrehozása. Azure webhely helyreállítási megvédheti fizikai, mind a virtuális telepítések másodlagos webhelyre vagy Azure. A dokumentum bemutatja az Azure virtuális Hyper-V szolgáltatást úgy is fájlkiszolgálóra a helyreállítási helyként és fájlmegosztások StorSimple tárolón DR megoldást részleteit. Más esetben a fájl kiszolgálói virtuális van, amelyben egy VMware virtuális vagy a fizikai gépi hasonlóan kell végrehajtani.

## <a name="prerequisites"></a>Előfeltételek

Egy egyetlen kattintással katasztrófa helyreállítási megoldás StorSimple tárterület is fájlmegosztások Azure webhely helyreállítási használó előfeltételei a következők:

-   A helyszíni virtuális is a Hyper-V vagy VMware vagy a fizikai gépi Windows Server 2012 R2 fájl kiszolgáló

-   Azure StorSimple manager regisztrált StorSimple tároló eszköz a helyszíni

-   Az Azure StorSimple manager (Ez lehet állapotban kell tartani leállítással) létrehozott StorSimple felhő készülék

-   A StorSimple tárolóeszközre konfigurált kötet is fájlmegosztások

-   A Microsoft Azure előfizetés létrehozott [azure webhely helyreállítási szolgáltatások tárolóból elemre](../site-recovery/site-recovery-vmm-to-vmm.md)

Ezenkívül a helyreállítási webhely Azure esetén futtassa az [Azure virtuális gép készenléti értékelési eszköz](http://azure.microsoft.com/downloads/vm-readiness-assessment/) VMs annak érdekében, hogy azok Azure VMs és Azure webhely helyreállítási services szolgáltatással kompatibilis a.

Elkerülése érdekében a késés olyan problémák (amely miatt esetleg magasabb költségek), ellenőrizze, hogy a StorSimple felhő készülék automatizálási fiók és tároló létrehozása fiókok ugyanabban a régióban.

## <a name="enable-dr-for-storsimple-file-shares"></a>StorSimple fájlmegosztások DR engedélyezése  

A helyszíni környezet minden összetevő kell ahhoz, hogy teljes replikációs és helyreállítási szolgáltatása fogja védeni. Ez a szakasz ismerteti, hogy miként:

-   (Nem kötelező) az Active Directory és a DNS-replikáció beállítása

-   Azure webhelyen helyreállítás használata ahhoz, hogy a fájl kiszolgálói virtuális védelméről

-   Védelem StorSimple mennyiségének engedélyezése

-   A hálózat beállítása

### <a name="set-up-active-directory-and-dns-replication-optional"></a>(Nem kötelező) az Active Directory és a DNS-replikáció beállítása

Ha azt szeretné, az Active Directory és a DNS-fut, úgy, hogy a DR webhelyen elérhető legyenek gépek védelmét, szüksége kifejezetten védelmük (úgy, hogy a fájl kiszolgálók hitelesítéssel feladatait után érhetők el). Kétféleképpen lehet ajánlott az ügyfelet a helyszíni környezet komplexitását alapján.

#### <a name="option-1"></a>A beállítás 1

Ha az ügyfélnek kisszámú alkalmazások, egy tartományvezérlőnek a teljes a helyszíni webhely, és akkor javasoljuk, hogy a tartomány vezérlő gép bizonyos (Ez a webhely és a webhely-Azure alkalmazható) másodlagos webhelyre Azure webhely helyreállítási replikációs segítségével a teljes webhelyen keresztül adatkapcsolat.

#### <a name="option-2"></a>A beállítás 2

Ha az ügyfélnek alkalmazások sok van, az Active Directory erdő fut, és néhány alkalmazások fölé egyszerre fog adatkapcsolat, akkor azt javasoljuk, hogy egy további tartományvezérlőnek a DR webhelyen beállításának (másodlagos webhely vagy Azure-ban).

Olvassa el [az Active Directory és az Azure-webhely helyreállítás DNS automatikus DR megoldás](../site-recovery/site-recovery-active-directory.md) utasításokat, amikor egy tartományvezérlőnek elérhetővé tétele a DR webhelyen. Ehhez a dokumentumhoz a hátralévő azt feltételezi, a tartományvezérlőnek érhető el a DR webhelyet.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Azure-webhely helyreállítás használata ahhoz, hogy a fájl kiszolgálói virtuális védelméről

Ezt a lépést kell, hogy arra készül, a fájl kiszolgálói a helyszíni környezet, létrehozása és készítse elő az Azure webhely helyreállítási tárolóból elemre és a virtuális gép fájlvédelmi engedélyezése.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>A fájl kiszolgálói a helyszíni környezet előkészítése

1.  Állítsa a **felhasználói fiókok felügyelete** **sose értesítse**. Szükség, így a iSCSI célok csatlakozás után fail: Azure webhely helyreállítási alapoktól Azure automatizálási parancsfájlokat is használhatja.

    1.  Nyomja le a Windows billentyű + kérdések, és keressen a **felhasználói fiókok**.

    2.  Kattintson a **Beállítások módosítása felhasználói fiókok felügyelete**.

    3.  Húzza a sávot a képernyő aljára, **Sose értesítse**felé.

    4.  Kattintson az **OK gombra** , és válassza az **Igen gombra** .

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  A virtuális ügynököt a minden fájl kiszolgáló VMs. Szükség, így akkor is Azure automatizálási parancsprogramokat futtassanak a sikertelen VMs fölé.

    1.  [Töltse le a agent](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Nyissa meg a Windows PowerShell rendszergazdai mód (Futtatás rendszergazdaként), és írja be a letöltési helye nyissa meg a következő parancsot:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] A fájlnév függően változhat.

1.  Kattintson a **Tovább**gombra.

2.  Fogadja el a **Feltételek a szerződést** , és kattintson a **Tovább**gombra.

3.  Kattintson a **Befejezés gombra**.


1.  Hozzon létre fájlmegosztások kívül StorSimple tároló faragottnak kötet használatával. További tudnivalókért lásd: [a StorSimple Manager szolgáltatással kötet kezelése](storsimple-manage-volumes.md).

    1.  A helyszíni VMs nyomja le a Windows billentyű + Q és **iSCSI**keresése.

    2.  Jelölje ki a **iSCSI kezdeményező**.

    3.  A **beállítás** lapon jelölje ki, és másolja a kezdeményező nevét.

    4.  Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com/).

    5.  Jelölje ki a **StorSimple** fülre, és válassza a a StorSimple kezelő szolgáltatás, amely tartalmazza a fizikai eszközt.

    6.  Mennyiségi container(s) létrehozása, és hozza létre a kötet(ek). (Ezek kötet jelennek meg a fájl hozzádni VMs kiszolgálón). Másolja a kezdeményező nevét, és adjon meg egy megfelelő nevet az Access vezérlő rekordok kötet létrehozásakor.

    7.  Jelölje be az **konfigurálása** lap Megjegyzés lefelé az eszköz IP-címét.

    8.  A helyszíni VMs újra nyissa meg a **iSCSI kezdeményező** és a gyors csatlakozás szakaszban adja meg a IP. Kattintson a, **Gyors csatlakozás** (az eszköz most kell csatlakoztatni).

    9.  Nyissa meg az Azure adatkezelési portált, és kattintson a **kötet, és az eszközök** fülre. Kattintson az **automatikus beállítása**gombra. Az éppen létrehozott mennyiségi jelenjenek meg.

    10. A portálon, válassza az **eszközök** fülre, és válassza a **Hozzon létre egy új virtuális eszközt.** (Ez az eszköz virtuális lesz a feladatátvételek alkalmával). Az új virtuális eszköz többletköltségek elkerülése érdekében offline állapotban is kell tartani. A virtuális eszköz offline állapotba helyezni, a portálon lépjen a **virtuális gépeken futó** szakaszt, és leállíthatja azt.

    11. Térjen vissza a helyszíni VMs, és nyissa meg a merevlemez-kezelés (nyomja le a Windows billentyű + X billentyűkombinációt, és kattintson a **Lemez kezelése**).

    12. Megfigyelheti, hogy néhány további lemezre (attól függően, hogy a létrehozott kötet száma). Kattintson a jobb gombbal a legelső, jelölje ki a **Lemezen inicializálni**, és kattintson **az OK gombra**. Kattintson a jobb gombbal a **Unallocated** szakaszt, jelölje be az **Új egyszerű kötet**, hozzárendelheti egy betűjele és a varázsló.

    13. Az összes lemez esetében ismételje meg az l. **Ez a gép** a Windows Intéző láthatja az összes lemez.

    14. Segítségével a fájl- és tároló szolgáltatások szerepkör fájlmegosztások a következő kötet.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Hozhat létre, és Felkészülés az Azure webhely helyreállítási tárolóból elemre.

Olvassa el az [Azure webhely helyreállítási dokumentáció](../site-recovery/site-recovery-hyper-v-site-to-azure.md) a fájl kiszolgálói virtuális védelme előtt Azure webhely helyreállítási használatbavételéhez.

#### <a name="to-enable-protection"></a>Ahhoz, hogy védelme

1.  A helyszíni VMS-en keresztül Azure webhely helyreállítási védelemmel ellátni kívánt választani a iSCSI target(s):

    1.  Nyomja le a Windows billentyű + Q és a Keresés **iSCSI**.

    2.  Jelölje ki a **iSCSI kezdeményező beállítása**.

    3.  Válassza le a korábban csatlakoztatott StorSimple eszközt. Másik lehetőségként válthat a fájl kiszolgálói ki néhány percig engedélyezésekor a védelmet.

    > [AZURE.NOTE] Ennek hatására a fájlmegosztások átmenetileg nem érhető el

1.  A [virtuális gép védelem bekapcsolása](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) az Azure webhely helyreállítási portálról fájl kiszolgáló virtuális.

2.  Amikor megkezdi a kezdeti szinkronizálás, újra a cél újra. Nyissa meg a iSCSI kezdeményező, jelölje ki a StorSimple eszközt, és kattintson a **Csatlakozás**gombra.

3.  Miután befejeződött a szinkronizálás, és a virtuális állapota **védett**, jelölje ki a virtuális, jelölje be a **beállítás** lapon és a virtuális hálózatához árnyalatait (Ez a hálózat, amely fölé VM(s) sikertelen lesz része a). Ha a hálózati nem jelenik meg, az azt jelenti, hogy a szinkronizálás továbbra is fog.

### <a name="enable-protection-of-storsimple-volumes"></a>Védelem StorSimple mennyiségének engedélyezése

Az **alapértelmezett biztonsági másolatot készít a mennyiségi engedélyezése** lehetőséget a StorSimple kötet nem választotta, ha a StorSimple kezelő szolgáltatás **Biztonsági házirendek** léphet, és hozzon létre egy alkalmas biztonsági házirendet, az összes kötet. Azt javasoljuk, hogy a biztonsági mentés gyakoriságát meg a helyreállítási-pont cél (Készletben), amelyeket szeretne az alkalmazáshoz megtekintéséhez.

### <a name="configure-the-network"></a>A hálózat beállítása

A fájl kiszolgáló virtuális hálózat beállításainak konfigurálása Azure webhely helyreállítási, hogy a virtuális hálózatok feladatátvétel után a megfelelő DR hálózat csatolt.

Választhat a virtuális **VMM felhő** vagy a **Védelem csoportjában** a hálózati beállításokat, az alábbi ábrán látható módon.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

Az automatikus rendszer-Helyreállítás automatizálhatja a fájlmegosztások feladatátvevő folyamata a helyreállítási terv is létrehozhat. Egy zavarok fordul elő, ha hozhat fájlmegosztások néhány percet, mindössze egy kattintással. Ha engedélyezni szeretné a automatizálási, szüksége lesz Azure automatizálási fiókkal.

#### <a name="to-create-the-account"></a>A fiók létrehozása

1.  Az Azure klasszikus portált, és nyissa meg az **automatizálási** szakasz.

1.  Hozzon létre egy új automatizálási fiókot. Az azonos geo/régiót, amelyben a StorSimple felhő készülék és a tárterület-fiókok létrehozott maradjon is.

2.  Kattintson az **Új** &gt; **Alkalmazás szolgáltatások** &gt; **automatizálási** &gt; **Runbook** &gt; **A gyűjtemény** a szükséges runbooks importálhatja az automatizálási fiókjába.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Adja hozzá a következő runbooks a **Vészhelyreállítás** ablakból a gyűjteményben:

    -   Nem sikerül StorSimple mennyiségi tárolók keresztül

    -   Próba-feladatátvevő (TFO) után StorSimple mennyiségének karbantartása

    -   Csatlakoztassa a kötet StorSimple eszközön feladatátvétel után

    -   Indítsa el a virtuális készülék StorSimple

    -   Azure virtuális egyéni parancsfájl-bővítmény eltávolítása

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Az összes olyan parancsfájl közzététele, jelölje ki a runbook az automatizálási fiókot, és nyissa meg a **Szerző** fülre. Ezt a lépést követően a **Runbooks** lap jelenik meg az alábbi képlettel történik:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Az automatizálási fiók nyissa meg az **eszközök** fülre, majd kattintson **A beállítás hozzáadása** &gt; **Hozzáadása hitelesítő adatait**, és adja hozzá a Azure hitelesítő adatait – az eszköz AzureCredential nevet.

    Használja a Windows PowerShell hitelesítő adatokat. A hitelesítő adatok, egy szervezeti azonosító felhasználónév és a jelszót az Azure előfizetés való hozzáférést, és a letiltott többtényezős hitelesítés tartalmazó kell. Ebben az esetben a felhasználó nevében során a feladatátadás hitelesítést végezni, és jelenítse meg a fájl kiszolgálói kötet a DR webhelyen.

1.  Automatizálási fiók kattintson az **eszközök** fülre, és válassza a **Beállítás hozzáadása** &gt; **hozzáadása változó** , és adja hozzá a következő változók. Megadhatja, hogy ezek az eszközök titkosításához. Ezek a változók helyreállítási terv-specifikus. Ha a helyreállítási megtervezése (amely a következő lépésben létrehoz) neve TestPlan, akkor a változók kell TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName és így tovább.

    -   *RecoveryPlanName* **-StorSimRegKey**: a StorSimple kezelő szolgáltatás a regisztrációs billentyűjét.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: az Azure előfizetése nevére.

    -   *RecoveryPlanName* **-Erőforrásnév**: a StorSimple eszköz tartalmazó StorSimple erőforrás nevét.

    -   *RecoveryPlanName* **-DeviceName**: az eszköz, amelynek át kell venni.

    -   *RecoveryPlanName* **-TargetDeviceName**: A StorSimple felhő készülék, amelyben a tárolók át kell venni olyan.

    -   *RecoveryPlanName* **-VolumeContainers**: ezen az eszközön, amely fölé, akkor nem kell mennyiségi tárolók vesszővel tagolt karakterlánc Ha például volcon1 volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: A szolgáltatás neve a célként megadott eszköz (Ez a **virtuális gép** szakaszában találhatók: a szolgáltatás neve megegyezik a DNS-neve).

    -   *RecoveryPlanName* **-StorageAccountName**: A tárterület fiók nevére, amelyben tárolni a parancsprogram (amelyet futtatni a sikertelen virtuális felett). Ez lehet minden egyes hely a parancsfájl ideiglenes tárolásához rendelkező tároló fiókkal.

    -   *RecoveryPlanName* **-StorageAccountKey**: A hívóbetű a fenti tárterület-fiókom.

    -   *RecoveryPlanName* **-ScriptContainer**: a tároló, amelyben a parancsfájl kíván tárolni a felhőben nevét. Ha a tároló nem létezik, létrejön.

    -   *RecoveryPlanName* **-VMGUIDS**: után egy virtuális védheti, Azure webhely helyreállítási oszt ki minden virtuális egy egyedi azonosítója, amellyel a sikertelen részleteit virtuális fölé. A VMGUID juthat a **Helyreállítási-szolgáltatások** lapon jelölje ki, és kattintson a **Védett elem** &gt; **Védelem csoportok** &gt; **gépek** &gt; **tulajdonságait**. Ha több VMs, majd a GUID hozzáadása egy vesszővel tagolt karakterláncként.

    -   *RecoveryPlanName* **-AutomationAccountName** –, amelyben a runbooks és eszközök hozzáadta automatizálási fiók nevét.

    Például ha a helyreállítási előfizetésére fileServerpredayRP, majd az **eszközök** fülre jelenjenek meg az alábbi képlettel történik az összes eszköz hozzáadása után.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Nyissa meg a **Helyreállítási** szakaszában, és válassza ki a korábban létrehozott Azure webhely helyreállítási tárolóból elemre.

2.  Válassza a **Helyreállítás tervek** lapot, és új helyreállítási terv létrehozása az alábbi képlettel történik:

    egy.  Adjon meg egy nevet, és jelölje ki a megfelelő **Védelem csoportjában**.

    b.  Válassza ki a VMs a védelem csoportban a helyreállítási terv szerepeltetni kívánt.

    c billentyűkombinációt.  A helyreállítás terv hoz létre, jelölje ki azt a helyreállítási terv testreszabási nézet megnyitásához.

    d.  Jelölje ki az **összes csoport kikapcsolás**, kattintson az **Indexek**gombra, és válassza a **minden csoport Leállításig elsődleges oldalát parancsfájl hozzáadása**.

    e.  Jelölje be az automatizálási (ahol hozzáadott fiókba a runbooks), és válassza a a **túl-StorSimple – mennyiségi-tárolók meghiúsító** runbook.

    f.  Kattintson a **csoport 1: Start**, válassza ki a **virtuális gépeken futó**és a VMs, amelyeket a helyreállítási terv védeni szeretne hozzáadni.

    g.  Kattintson a **csoport 1: Start**válassza a **parancsfájlt**, és adja hozzá a következő parancsfájlok sorrendben mint **után 1-es csoport** lépéseket.

    - Kezdés-StorSimple – ezek olyan virtuális-készülék runbook
    - Nem sikerül felett-StorSimple – mennyiségi-tárolók runbook
    - Csatlakozási kötet-után-átváltó runbook
    - Eltávolítja az egyéni-parancsprogram-bővítmény runbook

1.  Kézi művelet után a fenti 4 parancsfájlok hozzáadása ugyanazon **csoport 1: utáni lépéseket** szakaszban. Ez a művelet az a pont, amelynél ellenőrizheti, hogy minden megfelelően működik. Ez a művelet kell csak egy részének (csak ezt választó **Tesztelése feladatátvevő** jelölőnégyzetet) teszt feladatátvevő mellékletként.

2.  A kézi művelet után adja meg a Lemezkarbantartó parancsfájlt, ugyanezt az eljárást, amelyet a többi runbooks használt használatával. Mentse a helyreállítási csomagot.

    > [AZURE.NOTE] Próba feladatátvevő fut, ellenőriznie kell minden, a művelet manuális lépésénél, mert a StorSimple kötet, amely a célként megadott eszköz volna lett példányát a Lemezkarbantartó részeként is törli a kézi művelet befejezése után.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Végezze el a tesztet feladatátvevő

Olvassa el a próba feladatátvételkor az [Active Directory DR megoldás](../site-recovery/site-recovery-active-directory.md) kézikönyv az Active Directory adott kapcsolatos szempontok. A helyszíni telepítés ne zavarják egyáltalán, ha a teszt feladatátadás. A csatolt volt a helyszíni virtuális StorSimple kötet a Azure StorSimple felhő készülékre vannak klónozva. Egy virtuális tesztelése céljából állapotba Azure-ban, és a virtuális klónozott kötet csatolt.

#### <a name="to-perform-the-test-failover"></a>A próba feladatátvételi végrehajtásához

1.  Az Azure klasszikus portálon válassza a webhely helyreállítási tárolóból elemre.

1.  Kattintson a helyreállítás csomagot, a fájl kiszolgálói virtuális készült.

2.  Kattintson a **vizsgálat feladatátvevő**.

3.  Jelölje ki a virtuális hálózat a próba feladatátvevő megkezdéséhez.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  A másodlagos környezet esetén, az ellenőrzések hajthatják végre.

2.  Az adatérvényesítés befejezése után kattintson a **Teljes ellenőrzések**. A tesztkörnyezetben feladatátvevő törölve lesznek, és a TFO művelet befejeződik.

## <a name="perform-an-unplanned-failover"></a>Feladatátvételhez végrehajtása

Során feladatátvételhez StorSimple kötet vannak nem sikerült fölé a virtuális eszköz virtuális kópia a Azure a hozzák és a kötet csatolt a virtuális.

#### <a name="to-perform-an-unplanned-failover"></a>Feladatátvételhez végrehajtásához

1.  Az Azure klasszikus portálon válassza a webhely helyreállítási tárolóból elemre.

1.  Kattintson a fájl kiszolgálói virtuális által létrehozott helyreállítási csomagot.

2.  Kattintson a **feladatátvevő** , és válassza a **Nem tervezett feladatátvételre**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Jelölje be a cél hálózatba, és kattintson a az ellenőrzés ikon a feladatátvevő megkezdéséhez ✓.

## <a name="perform-a-planned-failover"></a>Tervezett feladatátvevő végrehajtása

Tervezett feladatátvételkor a helyszíni fájl a virtuális van leállítása server és a felhő StorSimple eszközön mennyiségének biztonsági mentés pillanatképként származik. StorSimple kötet vannak nem sikerült fölé a virtuális eszköz virtuális kópia állapotba Azure és a kötet csatolt a virtuális.

#### <a name="to-perform-a-planned-failover"></a>Tervezett feladatátvevő végrehajtásához

1.  Az Azure klasszikus portálon válassza a webhely helyreállítási tárolóból elemre.

1.  Kattintson a helyreállítás csomagot, a fájl kiszolgálói virtuális készült.

2.  Kattintson a **feladatátvevő** , és válassza a **Tervezett feladatátvevő**.

3.  Jelölje be a cél hálózatba, és kattintson a az ellenőrzés ikon a feladatátvevő megkezdéséhez ✓.

## <a name="perform-a-failback"></a>Végezze el a visszaállás

Egy visszaálláshoz során StorSimple mennyiségi tárolók vannak nem sikerült fölé vissza a fizikai eszközök után biztonsági származik.

#### <a name="to-perform-a-failback"></a>Egy visszaállás végrehajtásához

1.  Az Azure klasszikus portálon válassza a webhely helyreállítási tárolóból elemre.

1.  Kattintson a helyreállítás csomagot, a fájl kiszolgálói virtuális készült.

2.  Kattintson a **feladatátvevő** , és válassza ki a **tervezett feladatátvevő** vagy **nem tervezett feladatátvételre**.

3.  Kattintson a **Módosítás irányba**.

4.  Jelölje ki a megfelelő adatszinkronizálás és virtuális létrehozási beállítások.

5.  Kattintson az ellenőrzés ikon a visszaállás megkezdéséhez ✓.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Ajánlott eljárások

### <a name="capacity-planning-and-readiness-assessment"></a>A kapacitás tervezés és a felkészülés értékelése


#### <a name="hyper-v-site"></a>A Hyper-V webhely

A [felhasználó kapacitás Csapattervező eszköz](http://www.microsoft.com/download/details.aspx?id=39057) segítségével a server, a tárhely és a hálózati infrastruktúrát a Hyper-V replika környezetben.

#### <a name="azure"></a>Azure

A VMs annak érdekében, hogy azok kompatibilisek Azure VMs és Azure helyreállítási Webhelyszolgáltatások futtatását is lehetővé teszi az [Azure virtuális gép készenléti értékelési eszköz](http://azure.microsoft.com/downloads/vm-readiness-assessment/) . A felmérés Readiness eszköz virtuális konfigurációk ellenőrzi, és a tájékoztató Azure kompatibilis konfigurációk esetén. Ha például azt figyelmeztetést 127 GB-nál nagyobb a c meghajtó esetén.


Kapacitás tervezése tevődnek össze legalább két fontos folyamatot:

-   Megfeleltetés helyszíni a Hyper-V VMs az Azure virtuális méretű (például A6, A7, A8 és A9).

-   Annak megállapítása, hogy a szükséges Internet-sávszélesség.

## <a name="limitations"></a>Korlátozások

- Jelenleg csak 1 StorSimple eszköz át lehet venni (az egyetlen StorSimple felhő készülék). Több StorSimple eszközre nyúló fájlkiszolgálóra eset még nem támogatott.

- Nem sikerül egy virtuális védelem engedélyezése során, győződjön meg arról, hogy le van választva a iSCSI célok.

- Az összes közös tartó mennyiségi tárolók keresztül biztonsági házirendek miatt csoportosított mennyiségi tárolók sikertelen lesz fölé együtt.

- A kiválasztott mennyiségi tárolók az összes kötet fölé sikertelen lesz.

- Vegye fel a legfeljebb 64 TB-nél több kötet nem kell sikertelen fölé, mert egy egyetlen StorSimple felhő készülék maximális kapacitása 64 TB.

- Ha a tervezett/tervezett feladatátvételt sikertelen, és a VMs készült Azure-ban, majd nem letisztult másolatot a VMs. Ehelyett végezze el a visszaállás. Ha törli a VMs majd a helyszíni VMs sem kapcsolható be újra.

- Után feladatátvevő Ha nem látja a kötet, nyissa meg a VMs, nyissa meg a merevlemez-kezelés, a lemez újraellenőrzése és majd füleket online.

- Bizonyos esetekben a DR webhelyen meghajtó betűk lehet ugyanaz, mint a levelek helyszíni. Ez akkor fordulhat elő, ha szüksége lesz a feladatátvételi befejeződése után manuálisan a hiba kijavításához.

- Többtényezős hitelesítés az Azure hitelesítőadat megjelenésére eszközként automatizálási fiók ennek célszerű lehet. Ha ez a hitelesítés nem le van tiltva, parancsfájlok automatikus futásának nem használható, és a helyreállítási terv sikertelen lesz.

- Feladatátvevő feladat időtúllépése: A StorSimple parancsfájl időtúllépést okoz, ha mennyiségi tárolók feladatátvételének a parancsprogram (jelenleg 120 perc) / Azure webhely helyreállítási határértéknél több időt vesz igénybe.

- Biztonsági mentési feladat időtúllépése: A StorSimple parancsfájl időtúllépés történik, ha a biztonsági mentés mennyiségének a parancsprogram (jelenleg 120 perc) / Azure webhely helyreállítási határértéknél több időt vesz igénybe.
 
    > [AZURE.IMPORTANT] A biztonsági mentés kézi futtatása az Azure portálról, és futtassa újra a helyreállítási csomagot.

- Feladat időtúllépése klónozhatja: A StorSimple parancsfájl időtúllépés történik, ha több időt mennyiségének klónozhatja parancsprogram (jelenleg 120 perc) / Azure webhely helyreállítási határértéknél.

- Idő szinkronizálási hiba: A StorSimple parancsfájlok hibák ki közli, hogy a biztonsági másolatok sikertelen volt-e annak ellenére, hogy a biztonsági mentés sikeres a portálon. A lehetséges okai előfordulhat, hogy a StorSimple készülék idő valószínűleg nincs szinkronban a pontos idő az időzónát.
 
    > [AZURE.IMPORTANT] Szinkronizálja a készülék az időt az aktuális idő az időzónát.

- Berendezés feladatátvevő hiba: A StorSimple parancsfájl előfordulhat, hogy nem, ha van egy készülék feladatátvevő, a helyreállítási terv fut.
    
    > [AZURE.IMPORTANT] Futtassa újra a helyreállítási csomagot, a készülék feladatátvételi befejeződése után.

## <a name="summary"></a>Összefoglalás

Azure webhely helyreállítási használ, létrehozhat virtuális fájlkiszolgálóra teljes automatizált katasztrófa helyreállítási tervet StorSimple tárterület is fájlmegosztások problémákat. A feladatátvételi kezdeményezhet bárhonnan másodpercek megelőzve a zavarok és a Mentés és futó néhány perc alatt az alkalmazás kérése.
