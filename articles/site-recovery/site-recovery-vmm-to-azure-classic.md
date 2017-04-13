<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket a replikáció az Azure |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan való replikáció a Hyper-V virtuális gépeken futó a Hyper-V állomásokon System Center VMM felhőket az Azure található."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Párhuzamos a Hyper-V virtuális gépeken futó VMM felhőket az Azure-ban

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-azure.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasszikus portál](site-recovery-vmm-to-azure-classic.md)
- [A PowerShell - klasszikus](site-recovery-deploy-with-powershell.md)



A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md).

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan webhely helyreállítási való replikáció a Hyper-V kiszolgálók VMM magánjellegű felhőket az Azure található, a Hyper-V virtuális gépeken futó telepítéséhez.

A cikk az alkalmazási példát előfeltételei tartalmaz, és megtudhatja, hogy miként állíthat be egy webhely helyreállítási tárolóból elemre, a forrás VMM kiszolgálóra telepített Azure-webhely helyreállítási szolgáltató juthat, a kiszolgáló regisztrálása a tárolóból elemre, Azure tárterület-fiók felvétele, telepíteni az Azure helyreállítási szolgáltatások ügynököt a Hyper-V kiszolgálók, VMM felhő, amely az összes védett virtuális gépeken futó fog vonatkozni védelem beállításainak konfigurálása , majd engedélyezze az adott virtuális gépeken futó védelmét. Győződjön meg arról, hogy minden működik-e meg a várt módon való áttérés tesztelésével befejezéshez.

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="architecture"></a>Architektúra

![Architektúra](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Az Azure webhely helyreállítási szolgáltató telepítve van a VMM webhely helyreállítási a telepítés során, és a VMM kiszolgáló regisztrálva van a webhely helyreállítási tárolóból elemre. A szolgáltató kommunikál webhely helyreállítási replikációs üzletifolyamat-tervező kezelheti.
- Az Azure helyreállítási szolgáltatások ügynök telepítve van a Hyper-V kiszolgálók webhely helyreállítási a telepítés során. Adatok replikációs Azure tárolóhoz kezeli.


## <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

Az alábbiakban amire szüksége lesz Azure-ban.

**Előfeltételek** | **Részletek**
--- | ---
**Azure-fiók**| Akkor, ha a [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**Azure tárhely** | Azure tároló fiók replikált adatok tárolására lesz szüksége. Azure tároló replikált adatok vannak tárolva, és Azure VMs is be, amikor feladatátadás. <br/><br/>Egy [szabványos geo felesleges tárterület-fiók](../storage/storage-redundancy.md#geo-redundant-storage)szükséges. A fiók kell a webhely helyreállítás szolgáltatással ugyanabban a régióban, és társítjuk az azonos előfizetés. Figyelje meg, hogy a replikáció prémium verzió tároló használata jelenleg nem támogatott, és ne használható.<br/><br/>[Információ](../storage/storage-introduction.md) Azure tárhely.
**Azure hálózati** | Szüksége van egy Azure virtuális hálózat, amely az Azure VMs fog csatlakozni feladatátadás. A webhely helyreállítási tárolóra azonos régió az Azure virtuális hálózati kell lennie.

## <a name="on-premises-prerequisites"></a>A helyszíni vonatkozó követelmények

Amire szüksége lesz az alábbiakban a helyszíni.

**Előfeltételek** | **Részletek**
--- | ---
**VMM** | Meg kell legalább egy VMM kiszolgáló fizikai vagy virtuális egyedülálló kiszolgálót, vagy egy virtuális fürt rendszerbe. <br/><br/>A VMM kiszolgáló futnia kell System Center 2012 R2 göngyölt legújabb frissítéseket.<br/><br/>Meg kell legalább egy felhőalapú VMM kiszolgálóján konfigurált.<br/><br/>A forrás felhő, amely a védelemmel ellátni kívánt egy vagy több VMM host csoportok kell tartalmaznia.<br/><br/>További tudnivalók a VMM felhőket beállítása [Útmutató: személyes felhőket létrehozása System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) a Kiss Mayer blogjában.
**A Hyper-V** | Egy vagy több a Hyper-V kiszolgálók, vagy a VMM felhőben fürt lesz szüksége. A kiszolgáló kell lennie, és egy vagy több VMs. <br/><br/>A Hyper-V server futnia kell legalább **Windows Server 2012 R2** a Hyper-V szerepkör vagy a **Microsoft a Hyper-V Server 2012 R2** és telepítve van a legújabb frissítéseket.<br/><br/>Bármely a Hyper-V kiszolgáló védelemmel ellátni kívánt VMs tartalmazó egy VMM felhőben kell lennie.<br/><br/>Ha futtatja a Hyper-V fürt feljegyzésben, hogy fürt ügynök nem automatikusan létrejön egy statikus IP cím alapú fürt esetén. Kézi konfigurálás fürt közvetítő kell. [Tudjon meg többet](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) a Aidan Finn blogbejegyzést.
**Védett gépek** | Védelemmel ellátni kívánt VMs meg kell feleljen az [Azure követelményeknek](site-recovery-best-practices.md#azure-virtual-machine-requirements).


## <a name="network-mapping-prerequisites"></a>Hálózati megfeleltetés vonatkozó követelmények
Ha védelme virtuális gépeken futó Azure hálózati megfeleltetés mértékben a forrás VMM kiszolgálón virtuális hálózatok között, és Azure hálózatok ahhoz, hogy a következő a cél:

- Melyik feladatátvevő ugyanazon a hálózaton csatlakozhat egymással, mely helyreállítási terv függetlenül azok minden gép.
- A hálózati átjáró állítva, a célhelyen Azure hálózati, ha más helyszíni virtuális gépeken futó csatlakoztathatja virtuális gépeken futó.
- Ha nem adja meg hálózati megfeleltetés csak virtuális gépeken futó, amely fölé a helyreállítási ugyanarra a csomagra sikertelen lesz kapcsolhatók Azure való áttérés után.

Ha azt szeretné, a hálózati hozzárendelést üzembe szüksége lesz az alábbiakat:

- A virtuális gépeken futó VMM a forráskiszolgálóval a védelemmel ellátni kívánt kell csatlakoztatni egy virtuális hálózathoz. A hálózat kell kötni logikai a felhőben társított hálózathoz.
- Az Azure hálózat, amelyhez a replikált virtuális gépeken futó feladatátvétel után csatlakozhat. A hálózat feladatátvevő idején választania. A hálózat azonos régió az Azure webhely helyreállítási előfizetés kell lennie.

Felkészülés a hálózati leképezése az alábbi képlettel történik:

1. [Olvassa el](site-recovery-network-mapping.md) a hálózati leképezési követelményeket.
2. Felkészülés a virtuális hálózatok VMM:

    - [Logikai hálózatok beállítása](https://technet.microsoft.com/library/jj721568.aspx).
    - A [virtuális hálózatok beállítása](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Lépés: 1: Hozzon létre egy webhely helyreállítási tárolóból elemre.

1. Jelentkezzen be az [Adatkezelési portál](https://portal.azure.com) a rögzíteni kívánt VMM kiszolgálóról.
2. Kattintson a **Data Services** > **helyreállítási szolgáltatások** > **webhely helyreállítási tárolóból elemre**.
3. Kattintson a **létrehozhat új** > **gyors létrehozásához**.
4. A **név**mezőbe írja be egy rövid nevet, amely azonosítja a tárolóból elemre.
5. **Régió**jelölje ki a földrajzi régióban esetében a tárolóból elemre. Jelölje be a támogatott régiók [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/)adatokat földrajzi elérhetősége témakörben olvashat.
6. Kattintson a **Létrehozás tárolóból elemre**.

    ![Új tárolóból elemre](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Jelölje be az állapotsoron a tárolóra sikeresen létrehozott megerősítéséhez. A tárolóból elemre a fő helyreállítási-szolgáltatások lapon megjelenik **aktív** .

## <a name="step-2-generate-a-vault-registration-key"></a>Lépés: 2: Tárolóra regisztrációs kulcs generálása

Regisztráció kulcsot létrehozni a tárolóból elemre. Miután töltse le az Azure webhely helyreállítási szolgáltató, és telepítse a VMM kiszolgáló, a kulcsot kell használni a VMM kiszolgáló regisztrálása a tárolóból elemre.

1. A **Helyreállítási-szolgáltatások** lapon kattintson a tárolóból elemre kattintva nyissa meg az első lépések lap. Rövid útmutató az ikonnal bármikor is megnyitható.

    ![Rövid útmutató az első ikonra](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. A legördülő listában jelölje ki **egy helyszíni VMM webhely és a Microsoft Azure között**.
3. **Felkészülés VMM kiszolgálók**kattintson a **regisztráció kulcs generálása** fájl. A fő fájl automatikusan létrejön és érvényes-5 nap után jön létre. Ha nem elérni az Azure-portálra a VMM kiszolgálóról kell a fájl másolása a kiszolgálóra.

    ![Regisztráció billentyűt](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Lépés 3: Telepítse az Azure webhely helyreállítási szolgáltató

1. **Első**lépések > **előkészítése VMM kiszolgálók**, **Töltse le a Microsoft Azure webhely helyreállítási Provider VMM kiszolgálókon telepítéshez** , a szolgáltató telepítőfájlt legújabb verziójának beszerzése gombra.
2. Futtassa a forráskiszolgálóval VMM meg a fájlt.

    >[AZURE.NOTE] Ha VMM fürt telepíti, és a Provider for telepíti az első alkalommal telepítse az active csomópontot, és a VMM kiszolgáló regisztrálása a tárolóból elemre a telepítés befejezéséhez. Telepítse a szolgáltató a csomóponton. Figyelje meg, hogy ha frissít a szolgáltató kell frissíteni az összes csomópont, mert azok minden futnia kell a megfelelő szolgáltató verziójú.

3. A telepítő tartalmaz egy prerequirements jelölőnégyzetet, és a szolgáltató telepítés elkezdéséhez VMM szolgáltatás leállítása engedélyt kér. A VMM szolgáltatás automatikusan újraindul telepítés befejezése után. Ha telepíti a kérni fogja a fürt szerepkör leállítása VMM fürthöz.

4. A **Microsoft Update** választhatja a frissítések keresése. Ezzel a beállítással engedélyezve van a szolgáltató frissítések telepíti a Microsoft Update irányelvek szerint.

    ![Microsoft-frissítések](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  A telepítés helyének a szolgáltató beállítása ** <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual gépi Manager\bin**. Kattintson a **telepítés**gombra.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. A szolgáltató telepítése után kattintson a **regisztráció** kiszolgáló regisztrálása a tárolóból elemre.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. **Név tárolóból elemre**ellenőrizze a tárolóból elemre a kiszolgáló nyilvántartásba nevét. Kattintson a *Tovább*gombra.

    ![Kiszolgáló regisztráció](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. **Internetkapcsolat esetén** adja meg, hogyan a szolgáltató a VMM kiszolgálón futó csatlakozik az internethez. Kattintson a **Csatlakozás a meglévő proxybeállításokhoz** a kiszolgálón beállított alapértelmezett internetes kapcsolat beállításait.

    ![Internetes beállításai](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Ha egy egyéni proxy használni kívánt állítson be azt a szolgáltatót telepítése előtt. Egyéni proxybeállításokat konfigurálásakor teszten ellenőrizni a proxy kapcsolat fog futni.
    - Ha egyéni proxykiszolgálót használ, vagy az alapértelmezett proxy adja meg a proxy adatait, beleértve a proxycím és port kell hitelesítést igényel.
    - Következő URL-címek a VMM kiszolgáló és a Hyper-v hosts elérhető legyen
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Engedélyezi az IP-címek [Azure adatközpont IP-tartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és HTTPS (443) protokoll ismertetett. Azt szeretné, hogy fehér-lista IP-címtartományai az Azure-terület, amelyet használni szeretne használata és a nyugati US.
    - Ha egy VMM RunAs fiók (DRAProxyAccount) használatával a megadott proxyhoz tartozó automatikusan létrejön egy egyéni proxy. A proxykiszolgáló beállítása, hogy ezt a fiókot sikeresen hitelesíthet A VMM RunAs fiókbeállításokat a VMM konzolban módosítható. Ehhez nyissa meg a **beállításokat** a munkaterület, bontsa ki a **biztonsági**, kattintson a **Futtatás fiókok**, és módosítsa a jelszót az DRAProxyAccount. Indítsa újra a VMM szolgáltatást, így ez a beállítás érvénybe kell.


8. **Regisztrációs billentyűt**jelölje ki az Azure webhely helyreállítási letöltött, és másolja a VMM kiszolgáló billentyűt.


10.  A titkosítási beállítást csak akkor használja, ha Ön éppen esetében, amelyek a Hyper-V VMs VMM felhőket az Azure. Ha Ön éppen esetében, amelyek egy másodlagos webhely nem használatos.

11.  A **kiszolgáló nevét**adja meg a tárolóra VMM kiszolgálójába azonosítása egy rövid nevet. A fürt konfiguráció adja meg a VMM fürt szerepkör nevét.
12.  **Szinkronizálás felhőalapú metaadat-alapú** jelölje ki azt meg szeretné-e a VMM kiszolgálón minden felhő metaadatait szinkronizálni a tárolóból elemre. Ez a művelet csak egyszer fordulhat elő, minden egyes kiszolgálón szükséges. Ha nem szeretné az összes felhőket szinkronizálni, ezeket nem jelöli be ezt a beállítást, és egyenként az a felhő tulajdonságok a VMM konzolban minden Felhő szinkronizálása.

13.  Kattintson a **Tovább gombra** a folyamat befejezéséhez. Regisztráció után a VMM kiszolgálóról metaadatok által Azure webhely helyreállítási keresi. A kiszolgáló a **VMM kiszolgálók** lapján a tárolóból elemre a **kiszolgálók** lapján megjelenik.

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Regisztráció után a VMM kiszolgálóról metaadatok által Azure webhely helyreállítási keresi. A kiszolgáló a **VMM kiszolgálók** lapján a tárolóból elemre a **kiszolgálók** lapján megjelenik.

### <a name="command-line-installation"></a>Parancssori telepítése

Az Azure webhely helyreállítási szolgáltató is lehet telepíteni a következő parancsot használja. Ez a módszer a szolgáltató telepítése a kiszolgáló Core a Windows Server 2012 R2 használható.

1. Töltse le a szolgáltató telepítési fájl és a regisztrációs kulcs mappára. Példa: C:\ASR.
2. A rendszer központ virtuális gép kezelő szolgáltatás leállítása
3. A rendszergazda jogú parancssort bontsa ki a szolgáltató telepítő, ezek a parancsok:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Telepítse a a szolgáltató az alábbi képlettel történik:

        C:\ASR> setupdr.exe /i

5. A szolgáltató regisztrálásához az alábbi képlettel történik:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Ha paramétereket a következők:

 - **/Credentials** : kötelező paraméter, amely a bejegyzés fő fájlt a helye  
 - **/FriendlyName** : a kiszolgáló nevének a Hyper-V host az Azure webhely helyreállítási portálon megjelenő paramétert kötelező megadni.
 - **/EncryptionEnabled** : megadhatja, hogy a titkosítási a virtuális gépeken futó Azure-ban (a többi titkosítás) választható paramétert. A Fájlnév **.pfx** kiterjesztésű kell rendelkeznie.
 - **/proxyAddress** : választható paraméter, amely megadja a címét a proxykiszolgáló.
 - **/proxyport** : választható paraméter, amely megadja a port a proxykiszolgáló.
 - **/proxyUsername** : választható paraméter, amely megadja a proxy felhasználó nevét.
 - **/proxyPassword** : választható paraméter, amely megadja a proxy jelszót.  


## <a name="step-4-create-an-azure-storage-account"></a>Lépés: 4: Azure tároló fiók létrehozása

1. Ha nincs Azure tároló fiók kattintson a fiók létrehozása **az Azure tárterület-fiók hozzáadása** .
2. Hozzon létre egy fiókot geo replikációs engedélyezve van. A Azure webhely helyreállítás szolgáltatással ugyanabban a régióban kell, és társítjuk az azonos előfizetést.

    ![Tárterület-fiók](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Tárterület-fiókok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott üzembe helyezése a webhely helyreállítási használt tárterület-fiókokat.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Lépés 5: Telepítse az Azure helyreállítási szolgáltatások ügynök

Telepítése az Azure helyreállítási szolgáltatások ügynök kiszolgálón egyes a Hyper-V VMM felhőbeli.

1. Kattintson a **Quick Start** > **Azure webhely helyreállítási szolgáltatások ügynök letöltése és a telepítés állomásokon** juthat a ügynök telepítőfájlt legújabb verzióját.

    ![Ügynököt a helyreállítási szolgáltatások](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Futtassa a telepítőfájlt a Hyper-V host-kiszolgálókon.
3. **Előfeltételek ellenőrzése** lapon kattintson a **Tovább**gombra. Bármely hiányzó előfeltételek automatikusan települ.

    ![Előfeltételek helyreállítási szolgáltatások Agent](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. A **Telepítési beállítások** lapon adja meg, amelyre a agent telepítése, és válassza ki a telepítendő biztonsági metaadatok gyorsítótár helyét. Kattintson a **telepítés**.
5. Telepítés befejezése után kattintson a **Bezárás** gombra a varázsló.

    ![Regisztráció MARS Agent](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Parancssori telepítése

Ezzel a paranccsal a parancssorból is telepítheti a Microsoft Azure helyreállítási szolgáltatások Agent:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Lépés a 6: Állítsa be a felhőben adatvédelmi beállítások

Miután regisztrálta a VMM kiszolgáló, beállíthatja a felhőben Dokumentumvédelmi beállításokat. A szolgáltató telepítésekor, így minden felhő VMM kiszolgálói fognak megjelenni a tárolóból elemre a <b>Védett elemek</b> lapján engedélyezte a beállítást, **felhőalapú adatok szinkronizálása a tárolóból elemre** .

![Közzétett felhő](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Az első lépések lapon kattintson a **VMM felhőket védelmének beállítása**gombra.
2. Kattintson a **Védett elemek** fülre kattintson a felhő szeretne beállítani, és válassza a **beállítás** lapon.
3. **A TARGET** **Azure**kijelölése
4. **Tárterület-fiók** jelölje ki az Azure tárterület-fiókot replikációs-et.
5. Állítsa **titkosítása tárolt adatok** **ki**. Ez a beállítás meghatározza, hogy adatokat kell titkosítható replikált a helyszíni webhely és Azure között.
6. A **gyakoriság másolása** az alapértelmezett hagyja. Ezt az értéket adja meg, milyen gyakran adatokat kell szinkronizálni a forrás- és célwebhelyek hely között.
7. **Megtartása helyreállítási pontok**hagyja ki az alapértelmezett. Alapértelmezett értéke nulla csak a legújabb helyreállítási pont elsődleges virtuális géphez replika host kiszolgálón tárolja.
8. Az **alkalmazás egységes pillanatképek gyakoriság**hagyja meg az alapértelmezett. Ezt az értéket Itt adhatja meg, hogy milyen gyakran történjen pillanatfelvételt. Pillanatképek kötet szolgáltatás (Árnyékmásolata) használja annak érdekében, hogy alkalmazásokat konzisztens állapotát, ha a pillanatkép származik.  Ha egy érték, hogy az kisebb, mint konfigurálnia további helyreállítási pontok száma.
9. A **replikáció elindítása**adja meg, ha a kezdeti replikációs Azure adatok kell kezdődnie. Az időzóna a Hyper-V kiszolgálón fogja használni. Azt javasoljuk, hogy a essen során a kezdeti replikáció ütemezése.

    ![Felhőalapú replikációs beállításai](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

A beállítások mentése után a feladat jön létre, és a **feladatok** lap figyelhető. Minden a Hyper-V kiszolgálók VMM forrás felhőbeli konfigurálja a rendszer a replikáció.

A mentés után a felhőben beállítások a **beállítás** lapon módosítható. A célhely vagy cél tároló fiók módosítani kell a felhőben konfiguráció eltávolítása, és majd újrakonfigurálni az a felhő. Figyelje meg, hogy ha tárterület-fiók módosítása a változás csak érvényesül a virtuális gépeken futó engedélyezett védelem után a tárterület-fiók módosították-e. Meglévő virtuális gépeken futó áttelepítése nem történik az új tárterület-fiókjába.

## <a name="step-7-configure-network-mapping"></a>7 lépés: A hálózati hozzárendelést konfigurálása
Első lépések a hálózati hozzárendelést ellenőrizze, hogy virtuális gépeken futó a forrás VMM kiszolgálón virtuális hálózathoz csatlakozik. Ezenkívül hozzon létre egy vagy több Azure virtuális hálózatok. Figyelje meg, hogy egyetlen hálózat Azure virtuális hálózatok összevonása csatolható.

1. Első lépések lapon kattintson a **hálózatok**gombra.
2. A **hálózatok** lapon **Forráshely**, jelölje be a forráskiszolgálóval VMM. Válassza a Azure **célhelyét** .
3. Az **adatforrás** hálózatok a VMM kiszolgáló társított virtuális hálózatok listájának jelennek meg. **Cél** hálózatokon az Azure hálózatok az előfizetéshez tartozó jelennek meg.
4. Jelölje be a forrás virtuális hálózat, és a **térkép**gombra.
5. **Jelölje be a cél hálózati** lapján jelölje be a cél Azure hálózathoz használni kívánt.
6. Kattintson a jelölőnégyzet be van jelölve a hozzárendelés befejezéséhez.

    ![Felhőalapú replikációs beállításai](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

A beállítások mentése után egy feladat elindul, hogy nyomon kövesse a hozzárendelés, és a feladatok lap figyelhető. Bármelyik meglévő replika virtuális gépeken futó, amelyek megfelelnek a forrás virtuális hálózathoz kapcsolódik, a cél Azure hálózatok. Új virtuális gépeken futó, amelyeket a forrás virtuális hálózat csatlakoztatott fog csatlakoznia az Azure csatlakoztatott hálózati replikáció után. Ha módosítja egy már meglévő hozzárendelése egy új hálózattal, replika virtuális gépeken futó is csatlakoznak az új beállítások használatával.

Megjegyzendő, hogy a célhely hálózatnak több alhálózat és az egy adott alhálózat nevével megegyező alhálózat, amelyen a forrás virtuális gép található, majd a replika virtuális gép fog csatlakozni célalkalmazás alhálózat feladatátvétel után. Ha nincs cél alhálózat egyező nevű, a virtuális gép csatlakozik az első alhálózat a hálózaton.

> [AZURE.NOTE] Webhely helyreállítási pontról hálózatok [hálózatok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott.

## <a name="step-8-enable-protection-for-virtual-machines"></a>8 lépés: A virtuális gépeken futó védelem bekapcsolása

Miután kiszolgálók, a felhő és a hálózatok helyesen van beállítva, engedélyezheti védelmének virtuális gépeken futó a felhőben. Vegye figyelembe az alábbiakat:

- Virtuális gépeken futó meg kell felelnie [Azure követelményeknek](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Védelem bekapcsolása az operációs rendszer és az operációs rendszer lemez tulajdonságainak megadása kell a virtuális gépen. Virtuális gép sablon használatával VMM virtuális gép létrehozásakor beállíthatja, hogy a tulajdonságot. Is beállíthatja, hogy a meglévő virtuális gépeken futó tulajdonságokból virtuális gép tulajdonságainak **Általános** és **Hardverkonfiguráció** lapokon. Ha VMM nem állítja be a tulajdonságok is beállíthatja azt az Azure webhely helyreállítási portálon.

    ![Virtuális gép létrehozása](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Virtuális gép tulajdonságainak módosítása](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Védelem a felhőben, amelyben a virtuális gép tartózkodik, a **virtuális gépeken futó** lapon kattintson a **védelem** > **hozzáadása virtuális gépeken futó**.
2. A felhőben virtuális gépeken futó a listából jelölje ki a védelemmel ellátni kívánt.

    ![Virtuális gép védelem bekapcsolása](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    A **Védelem bekapcsolása** művelet a **feladatok** lap, beleértve a kezdeti replikáció nyomon kövesse. A **Védelem véglegesítése** feladat futtatása után a virtuális gép készen áll a feladatátvételi. Védelem engedélyezett és a virtuális gépeken futó replikált után is Azure-ban meg őket.


    ![Virtuális gép védelem feladat](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Ellenőrizze a virtuális gép tulajdonságait, és szükség szerint módosítsa.

    ![Virtuális gépeken futó ellenőrzése](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Virtuális gép tulajdonságainak **konfigurálása** lapon a következő hálózat tulajdonságai módosíthatók.





- **A cél virtuális gépen hálózati adaptereken száma** - hálózati adaptereken számát adja meg, ha a cél virtuális gép méretének határozza meg. Jelölje be a [virtuális gép méret specs](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) virtuális gép méretének által támogatott adaptereken számát. Ha virtuális géphez méretének módosítása, majd mentse a beállításokat, a hálózati kártya számát **konfigurálása** lapon a következő alkalommal megnyitásakor változik. A cél virtuális gépeken futó hálózati adaptereken értéke a lehető legkevesebb hálózati adaptereken virtuális-gépen, és a virtuális gép választja, az alábbi képlettel történik méretének által támogatott hálózati adaptereken maximális száma:

    - A forrás számítógép hálózati adaptereken értéke kisebb vagy egyenlő adaptereken engedélyezett a cél gépi méretét a száma, majd a cél van adaptereken ugyanannyi forrásaként.
    - Ha a forrás virtuális gép kártya száma meghaladja a megengedett célalkalmazás méretét, majd a cél maximális használt.
    - Például egy adatforrás számítógépre két hálózati adaptereken van, és a négy támogatja a cél gépi méretét, a cél gép van két adaptereken. Ha a forrás számítógépben két adaptert, de a támogatott cél mérete csak akkor támogatja a egy a cél gép csak egy kártya lesz.    

- **A cél virtuális gép hálózati** – a hálózat, amelyhez a virtuális gép csatlakozik a hálózat forrás virtuális gép hálózati hozzárendelést határozza meg. Ha a forrás virtuális gép egynél több hálózati kártya és a cél különböző hálózatokhoz forrás hálózatok vannak megfeleltetve, majd kell a cél hálózatok közül választhat.
- **Az összes hálózati adapteren alhálózat** – minden egyes hálózati kártya választhatja ki a alhálózat, amelyhez a sikertelen virtuális gép fölé kíván csatlakozni.
- **Cél IP-cím** – Ha a hálózati kártya forrás virtuális gép van konfigurálva a statikus IP-címet, majd a cél virtuális gép megadhatja az IP-címét. Ez a funkció használata után feladatátvevő megőrzi az virtuális gép forrás IP-címét. Ha nincs IP-cím szerepel majd bármely elérhető IP-cím kapja a hálózati kártya feladatátvevő idején. Ha a cél IP-cím van megadva, de már használja egy másik virtuális gép Azure-ban futó feladatátvevő meghiúsul.  

    ![Hálózati tulajdonságainak módosítása](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Nem támogatott a Linux virtuális gépeken futó statikus IP-címet.

## <a name="test-the-deployment"></a>A telepítés tesztelése

A központi telepítés tesztelése futtassa a próba feladatátvevő egyetlen virtuális géphez, vagy több virtuális gépeken futó álló helyreállítási terv létrehozása, és futtassa a terv próba feladatátvevő.  

Teszt feladatátvevő keltő szűrő megőrzi a feladatátvétel és helyreállítási mechanizmusa elszigetelt hálózat. Megjegyzés:

- A távoli asztali változatában a feladatátvétel után Azure virtuális gép csatlakozni, engedélyezze a távoli asztali kapcsolat a virtuális gépen a próba feladatátvételi futtatása előtt.
- Feladatátvétel után kell megadnia a nyilvános IP-címet a távoli asztali változatában Azure virtuális gép csatlakozni. Ha meg szeretne ehhez, győződjön meg róla, nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.

>[AZURE.NOTE] Azure feladatátvevő tehát tenni a legjobb teljesítmény elérése érdekében beszerzéséhez győződjön meg arról, hogy telepítve van az Azure ügynök a védett gépen. Ez a segíti a gyorsabb betöltése, és lehetővé teszi a diagnosztikai problémák esetén. Lehet, hogy Linux ügynök megtalálható [Itt](https://github.com/Azure/WALinuxAgent) - és Windows ügynök megtalálható [Itt](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

1. A **Helyreállítási csomagok** lapon adja hozzá az új csomag. Adjon meg egy nevet, **VMM** az **adatforrás típusa**listában, és a **forrás**, a cél forráskiszolgáló VMM Azure.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. **Jelölje ki a virtuális gépeken futó** lapon jelölje ki a virtuális gépeken futó hozzáadása a helyreállítási terv. A virtuális gépeken futó hozzá lesznek adva a helyreállítási terv alapértelmezett csoporthoz – 1 csoportot. Legfeljebb 100 virtuális gépeken futó egyetlen helyreállítási tervben teszteltük.

- Ha azt szeretné, ellenőrizze a virtuális gép tulajdonságait, mielőtt fel azokat a csomagot, kattintson a Tulajdonságok lapon, ahol az a felhő a virtuális gépre van található. A virtuális gép tulajdonságait a VMM konzolban is állíthatja.
- Az összes megjelenített virtuális gépeken futó engedélyezve van a védelmet. A lista mindkét engedélyezett protection és az első replikációs befejeződött, és azokat, amelyek érhetők el a védelmet a kezdeti függőben lévő replikációs virtuális gépeken futó tartalmazza. Csak az első ismétlésekkel befejeződött virtuális gépek hiba történhet a fölé a helyreállítási terv részeként.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

A helyreállítási terv létrehozását követően ez jelenik meg a **Helyreállítási tervek** fülre. Műveletek automatizálása feladatátvételkor a helyreállítási terv felveheti [Azure automatizálási runbooks](site-recovery-runbook-automation.md) is.

### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

Kétféleképpen Azure próba feladatátvevő futtatásához.

- **Az Azure-hálózat nélkül tesztelni**– ilyen típusú vizsgálati feladatátvevő ellenőrzi, hogy a virtuális gép tervezés megfelelően Azure-ban. A virtuális gép nem kell minden Azure hálózathoz csatlakoztatva feladatátvétel után.
- **Az Azure hálózattal tesztelni**– ilyen típusú feladatátvevő ellenőrzései, hogy a teljes replikációs környezet akár a várt módon származik, és hogy nem sikerült a virtuális gépeken futó fölé a megadott cél Azure hálózati kell csatlakoztatni. Alhálózat kezeléshez vizsgálathoz feladatátvevő a alhálózat a próba virtuális gép fog kell kiegészítéseként a alhálózat replika virtuális gép alapján. Ez különbözik való replikáció normál, amikor a alhálózat replika virtuális gép az alhálózat a forrás virtuális gép alapul.

Ha a használni kívánt engedélyezve van egy Azure cél hálózat megadása nélkülire védelmet Azure virtuális géphez próba feladatátvevő nem kell semmit előkészítése. Az Azure hálózati cél próba feladatátvevő futtatásához kell, hogy van-e elszigetelt új Azure hálózat létrehozása az Azure gyártási hálózatról (új hálózat Azure-ban létrehozott alapértelmezett viselkedés). Nézze meg a [próba feladatátvevő futtatása](site-recovery-failover.md#run-a-test-failover) további információt.


Is kell a várt módon működnek a replikált virtuális gépen infrastruktúra beállítása. Például egy virtuális gép tartományvezérlőnek és a DNS-ÉT az Azure-webhely helyreállítás Azure replikálhatók, és hozhat létre a próba hálózat tesztelése feladatátvevő használatával. Tekintse meg a [feladatátvevő megfontolások az active directory tesztelése](site-recovery-active-directory.md#considerations-for-test-failover) szakaszban további információt.

A próba feladatátvevő tegye a következő futtatása:

1. A **Helyreállítási tervek** lapon jelölje ki azt a csomagot, és kattintson a **Vizsgálat feladatátvevő**.
2. A **Próba feladatátvevő erősítse meg** lapon válassza a **nincs** vagy egy adott Azure hálózathoz.  Figyelje meg, hogy ha válassza a nincs a próba feladatátvételi ellenőrzi, hogy a virtuális gép replikált megfelelően Azure, de nem ellenőrzi a replikáció hálózati konfigurálásról.

    ![Nincs hálózat](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Adatok titkosítása engedélyezve van a felhőben, ha a **Titkosítási kulcs** jelölje ki a kibocsátott tanúsítványt, a szolgáltató a VMM kiszolgálón, a telepítés során a vezérlőt, amellyel felhő titkosítás engedélyezéséhez bekapcsolt.
4. A **feladatok** lap nyomon követhető feladatátvevő előrehaladását. A virtuális gép próba replika az Azure-portálon is kell meg. Ha access virtuális gépeken futó felfelé beállítása a helyszíni hálózatról a virtuális géphez távoli asztali kapcsolaton kezdeményezhet.
5. A feladatátvételi eléri a **teljes tesztelés** fázis, kattintson a **Teljes tesztelje** a próba feladatátvételi befejezéshez. Felhatolás lefelé a **feladat** lap feladatátvevő folyamatban és az állapot nyomon követésére, és végezze el a szükséges műveleteket.
6. Feladatátvétel után is láthatja a virtuális gép próba replika az Azure-portálon. Ha access virtuális gépeken futó felfelé beállítása a helyszíni hálózatról a virtuális géphez távoli asztali kapcsolaton kezdeményezhet. Tegye a következőket:

    1. Győződjön meg arról, hogy a virtuális gépeken futó sikeres indítása érdekében.
    2. A távoli asztali változatában a feladatátvétel után Azure virtuális gép csatlakozni, engedélyezze a távoli asztali kapcsolat a virtuális gépen a próba feladatátvételi futtatása előtt. Is kell RDP végpont hozzáadása a virtuális gépen. Az [Azure automatizálási Runbooks](site-recovery-runbook-automation.md) megtennie is élvezheti.
    3. Után feladatátvevő Ha egy nyilvános IP-cím használatával csatlakozhat a távoli asztal használatával Azure virtuális gép győződjön meg róla nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.

7.  A tesztelés befejezése után tegye a következőket:
    - Kattintson **a vizsgálat feladatátvételi befejeződött**. Az automatikus kapcsolja ki és törölje a próba virtuális gépeken futó tesztkörnyezetben karbantartása.
    - Kattintson a **jegyzetek** rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.

>

## <a name="next-steps"></a>Következő lépések

Tudjon meg többet a [helyreállítási csomag beállításának](site-recovery-create-recovery-plans.md) és [feladatátvételi](site-recovery-failover.md).
