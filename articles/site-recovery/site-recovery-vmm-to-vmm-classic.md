<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket a replikáció másodlagos VMM webhelyre |} Microsoft Azure"
    description="Ez a cikk ismerteti a Hyper-V VMs VMM felhőket a replikáció az Azure-webhely helyreállításhoz másodlagos VMM webhelyre."
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
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>A Hyper-V virtuális gépeken futó VMM felhőket a replikáció másodlagos VMM webhelyre

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-vmm.md)
- [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan való replikáció a Hyper-V kiszolgálók, amely a másodlagos VMM-webhelyre Azure webhely helyreállítási VMM felhőket kezelhetők a Hyper-V virtuális gépeken futó.

Című cikk tartalmaz Előfeltételek, megtudhatja, hogy miként állíthat be egy webhely helyreállítási tárolóból elemre, telepítse az Azure webhely helyreállítási szolgáltató forrás és VMM kiszolgálók cél, a kiszolgáló regisztrálása a tárolóból elemre, VMM felhőket védelem beállításainak konfigurálása és engedélyeznie kell a Hyper-V VMs védelmét. Győződjön meg arról, hogy minden működik-e meg a várt módon való áttérés tesztelésével befejezéshez.

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="architecture"></a>Architektúra

Az alábbi képen a különböző kommunikációs csatornák és az üzletifolyamat-tervezési és replikációs Azure webhely helyreállítási által használt portok

![E2E topológiája](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Előzetes teendők

Győződjön meg arról, hogy az alábbi előfeltételek már a helyükön:

**Előfeltételek** | **Részletek**
--- | ---
**Azure**| A [Microsoft Azure](https://azure.microsoft.com/) -fiókra van szüksége. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**VMM** | Legalább egy VMM server szükséges.<br/><br/>A VMM kiszolgáló legalább futnia kell System Center 2012 SP1 göngyölt legújabb frissítéseket.<br/><br/>Ha egyetlen VMM kiszolgálóval védelmének beállítása, a kiszolgálón beállított legalább két felhőket szüksége.<br/><br/>Ha meg szeretné üzembe védelem két VMM kiszolgálóval, minden kiszolgáló rendelkeznie kell legalább egy, a kiszolgálón beállított elsődleges VMM védelemmel ellátni kívánt felhőben, és a kiszolgálón beállított másodlagos VMM védelme és helyreállítási használni kívánt egy felhőalapú<br/><br/>Az összes VMM felhőket rendelkeznie kell a Hyper-V képesség profil beállítása.<br/><br/>A forrás felhő, amely a védelemmel ellátni kívánt egy vagy több VMM host csoportok kell tartalmaznia.<br/><br/>További tudnivalók a VMM felhőket beállítása [Útmutató: személyes felhőket létrehozása System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) a Kiss Mayer blogjában.
**A Hyper-V** | Egy vagy több a Hyper-V kiszolgálók az elsődleges és másodlagos VMM host csoport, és egy vagy több virtuális gépeken futó egyes a Hyper-V kiszolgálón szükséges.<br/><br/>A host- és célwebhelyek a Hyper-V kiszolgálók legalább futnia kell a Windows Server 2012 a Hyper-V szerepkörrel és telepítve van a legújabb frissítéseket.<br/><br/>Bármely a Hyper-V kiszolgáló védelemmel ellátni kívánt VMs tartalmazó egy VMM felhőben kell lennie.<br/><br/>A Hyper-V fürt futtat, ne feledje, hogy fürt ügynök statikus IP cím alapú fürt esetén automatikusan létrehozott nem. Meg kell fürt közvetítő manuális beállítását. [Tudjon meg többet](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) a Aidan Finn blogbejegyzést.
**Hálózati hozzárendelést** | Győződjön meg arról, hogy replikált virtuális gépeken futó optimálisan kerülnek a másodlagos a Hyper-V kiszolgálók feladatátvétel után, és hogy csatlakozni tudnak a megfelelő virtuális hálózatok hálózati hozzárendelését is beállíthatja. Ha hálózati hozzárendelést nem adja meg, replika VMs nem csatlakozik olyan hálózat feladatátvétel után.<br/><br/>Beállítani a hálózati hozzárendelést a telepítés során, ellenőrizze, hogy a virtuális gépeken futó a Hyper-V forrás kiszolgálón VMM virtuális hálózathoz csatlakozik. A hálózat kapcsolni kell logikai hálózathoz társított a felhőben. < br /<br/>A cél felhő a helyreállítási használt másodlagos VMM kiszolgálón van, hogy a megfelelő virtuális hálózatnak, és azt be kell kötni a cél felhő társított hálózathoz megfelelő logikai.<br/><br/>[Tudjon meg többet](site-recovery-network-mapping.md) is megtudhat a hálózati hozzárendelést.
**Tárterület-hozzárendelés** | Alapértelmezés szerint egy adatforrás a Hyper-V kiszolgálón virtuális géphez, bizonyos a cél a Hyper-V host kiszolgálóhoz replikált adatokat tárolja az alapértelmezett helyre a Hyper-V kezelője a Hyper-V célállomás jelölt. Replikált adatokat tároló pontosabban beállíthatja a tárterület-hozzárendelés<br/><br/> Tárterület-megfeleltetés konfigurálásához szeretne tároló besorolása elemet, kattintson a forrás beállítása és VMM kiszolgálók Megcélozhat a telepítés megkezdése előtt. [Tudjon meg többet](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Lépés: 1: Hozzon létre egy webhely helyreállítási tárolóból elemre.

1. Jelentkezzen be az [Adatkezelési portál](https://portal.azure.com) a rögzíteni kívánt VMM kiszolgálóról.

2. Bontsa ki a **Data Services** > **Helyreállítási szolgáltatások** , és kattintson a **Webhely helyreállítási tárolóból elemre**.

3. Kattintson a **létrehozhat új** > **gyors létrehozásához**.

4. A **név**mezőbe írja be egy rövid nevet, amely azonosítja a tárolóból elemre.

5. **Területen** jelölje ki a földrajzi régióban esetében a tárolóból elemre. Jelölje be a támogatott régiók [Azure webhely helyreállítási árak](http://go.microsoft.com/fwlink/?LinkId=389880)adatokat földrajzi elérhetősége témakörben olvashat.

6. Kattintson a **Létrehozás tárolóból elemre**.

    ![Hozzon létre a tárolóból elemre](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Az állapotsor ellenőrizze, hogy a tárolóra hozták létre. A tárolóból elemre a fő helyreállítási-szolgáltatások lapon megjelenik **aktív** .

## <a name="step-2-generate-a-vault-registration-key"></a>Lépés: 2: Tárolóra regisztrációs kulcs generálása

Regisztráció kulcsot létrehozni a tárolóból elemre. Miután töltse le az Azure webhely helyreállítási szolgáltató, és telepítse a VMM kiszolgáló, a kulcsot kell használni a VMM kiszolgáló regisztrálása a tárolóból elemre.

1. A **Helyreállítási-szolgáltatások** lapon kattintson a tárolóból elemre kattintva nyissa meg az első lépések lap. Rövid útmutató az ikonnal bármikor is megnyitható.

    ![Rövid útmutató az első ikonra](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. A legördülő listában válassza a **két helyszíni VMM webhelyek között**.
3. **Felkészülés VMM kiszolgálók**kattintson a **Létrehozás regisztrációs kulcs fájl**. A fő fájl automatikusan létrejön és érvényes-5 nap után jön létre. Ha nem elérni az Azure-portálra a VMM kiszolgálóról szeretne másolni ezt a fájlt a kiszolgálóra.

    ![Regisztráció billentyűt](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Lépés 3: Telepítse az Azure webhely helyreállítási szolgáltató

4. Kattintson az **Első lépések** lapon **előkészítése VMM kiszolgálók**, **Töltse le a Microsoft Azure webhely helyreállítási Provider VMM kiszolgálókon telepítéshez** , a szolgáltató telepítőfájlt legújabb verziójának beszerzése.

2. Futtassa a forráskiszolgálóval VMM meg a fájlt.

    >[AZURE.NOTE] Ha VMM fürt telepíti, és a Provider for telepíti az első alkalommal telepítse az active csomópontot, és a VMM kiszolgáló regisztrálása a tárolóból elemre a telepítés befejezéséhez. Telepítse a szolgáltató a csomóponton. Fontos tudni, hogy ha frissít a szolgáltató, frissítheti az összes csomópont, mert azok minden futnia kell a megfelelő szolgáltató verziójú.

3. A telepítő tartalmaz néhány **Előtti követelmények ellenőrzése** és kérések jogosultsági szolgáltató telepítés elkezdéséhez VMM szolgáltatás leállítása. A VMM szolgáltatás automatikusan újraindul telepítés befejezése után. Ha telepíti a kérni fogja a fürt szerepkör leállítása VMM fürthöz.

4. A **Microsoft Update** választhatja a frissítések keresése. Ezzel a beállítással engedélyezve van a szolgáltató frissítések telepíti a Microsoft Update irányelvek szerint.

    ![Microsoft-frissítések](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. A telepítés helyének beállítása ** <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual gépi Manager\bin**. Kattintson a telepítés gombra a szolgáltató telepítés elindításához.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. A szolgáltató telepítése után kattintson a **regisztráció** kiszolgáló regisztrálása a tárolóból elemre.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
9. **Név tárolóból elemre**ellenőrizze a tárolóból elemre a kiszolgáló nyilvántartásba nevét. Kattintson a *Tovább*gombra.

    ![Kiszolgáló regisztráció](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. **Internetkapcsolat esetén** adja meg, hogyan a szolgáltató a VMM kiszolgálón futó csatlakozik az internethez. Kattintson a **Csatlakozás a meglévő proxybeállításokhoz** a kiszolgálón beállított alapértelmezett internetes kapcsolat beállításait.

    ![Internetes beállításai](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Ha egy egyéni proxy használni kívánt állítson be azt a szolgáltatót telepítése előtt. Egyéni proxybeállításokat konfigurálásakor teszten ellenőrizni a proxy kapcsolat fog futni.
    - Ha egyéni proxykiszolgálót használ, vagy az alapértelmezett proxy meg kell adnia a proxy részleteket, például a proxycím és port hitelesítést igényel.
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

13.  Kattintson a **Tovább gombra** a folyamat befejezéséhez. Regisztráció után a VMM kiszolgálóról metaadatok által Azure webhely helyreállítási keresi. Megjelenik a kiszolgáló **VMM kiszolgálók** > **kiszolgálók** a tárolóból elemre.

    ![Kiszolgálók](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Parancssori telepítése

Az Azure webhely helyreállítási szolgáltató a parancssorból is lehet telepíteni. Ez a módszer a szolgáltató telepítése a kiszolgáló CORE a Windows Server 2012 R2 használható.

1. Töltse le a szolgáltató telepítési fájl és a regisztrációs kulcs mappára. Ha például C:\ASR.
2. A System Center virtuális gép kezelő szolgáltatás leállítása
3. Bontsa ki a szolgáltató telepítő ezeket a parancsokat a **rendszergazda** jogosultságokkal rendelkező parancssorból futtatásával:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Telepítse a szolgáltató futtatásával:

        C:\ASR> setupdr.exe /i

5. A szolgáltató regisztrálásához futtatásával:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Hol találhatók a Paraméterek:

 - **/Credentials**: kötelező paraméter, amely a bejegyzés fő fájlt a helye  
 - **/FriendlyName**: a kiszolgáló nevének a Hyper-V host az Azure webhely helyreállítási portálon megjelenő paramétert kötelező megadni.
 - **/EncryptionEnabled**: választható paraméter, amely csak használja az Azure forgatókönyv a VMM titkosítása a virtuális gépeken futó a többi Azure-ban, ha szükséges. Győződjön meg arról, hogy a megadott **.pfx** kiterjesztésű fájl nevét.
 - **/proxyAddress**: választható paraméter, amely megadja a címét a proxykiszolgáló.
 - **/proxyport**: választható paraméter, amely megadja a port a proxykiszolgáló.
 - **/proxyUsername**: választható paraméter, amely a Proxy felhasználónév (Ha a proxy-hitelesítést igényel).
 - **/proxyPassword**: választható paraméter, amely a proxykiszolgáló használata hitelesítése (Ha a proxy-hitelesítést igényel) jelszava.  

## <a name="step-4-configure-cloud-protection-settings"></a>Lépés: 4: Állítsa be a felhőben adatvédelmi beállítások

Miután regisztrált VMM kiszolgálók, beállíthatja felhő Dokumentumvédelmi beállításokat. Ha engedélyezte a beállítást, **felhőalapú adatok szinkronizálása a tárolóból elemre** a szolgáltató telepítésekor VMM kiszolgálói tehát minden felhő megjelennek az **Elemek védett** fülre és a tárolóból elemre. Ha Ön nem tudja szinkronizálni egy adott felhő Azure webhely helyreállítási **Általános** lapján a VMM konzolban felhő tulajdonságok lapon.

![Közzétett felhő](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Az első lépések lapon kattintson a **VMM felhőket védelmének beállítása**gombra.
2. A **VMM felhőket** lapon jelölje be a használni kívánt állítsa be, és válassza a **beállítás** lapon a felhőben.
3. Jelölje ki a **cél**, **VMM**.
4. **Célt**jelölje ki a helyszíni VMM kiszolgáló, amely a felhőben helyreállítás használni kívánt kezeli.
4. **Cél felhőben**jelölje be a cél felhő virtuális gépeken futó a forrás felhőben feladatátvételének használni kívánt. Megjegyzés:

    - Azt javasoljuk, hogy, jelölje be a cél felhő, amely megfelel-e helyreállítási a virtuális gépeken futó fogja védeni.
    - Felhő tartozhat csak egyetlen felhő két – egy elsődleges vagy a Céllista felhő.

5. **Másolja a gyakoriság**, adja meg, hogy milyen gyakran kell szinkronizálja az adatokat 5he forrás- és célwebhelyek helyek között. Ne feledje, hogy ez a beállítás csak vonatkozó a Hyper-V host fut a Windows Server 2012 R2. A többi kiszolgáló öt perc az alapértelmezett beállítás szolgál.
6. **További helyreállítási pontok**adja meg, hogy további helyreállítási pontok létrehozásához. Az alapértelmezett nulla érték azt jelzi, hogy csak a legújabb helyreállítási pont elsődleges virtuális géphez replika host kiszolgálón tárolja. Figyelje meg, hogy segítségével, így több helyreállítási pontok megköveteli a további tárterületet, de a pillanatképek minden helyreállítási ponton tárolt. Alapértelmezés szerint helyreállítási pontok létrehozott óránként, hogy az egyes helyreállítási pont egy óra értékű adatokat tartalmazza. A helyreállítási pont érték, amely a virtuális gép a VMM konzolban az hozzárendeli nem lehet kisebb, mint az érték, amely az Azure webhely helyreállítási konzolban rendelhet.
7. Az **alkalmazás egységes pillanatképek gyakoriság**megadhatja, hogy milyen gyakran alkalmazás egységes pillanatfelvételt. A Hyper-V használja kétféle pillanatképek –, ahol egy teljes virtuális gépen növekményes pillanatképét szabványos pillanatkép, és az alkalmazás adatok belül a virtuális gép pont és az idő pillanatfelvételt-alkalmazás egységes pillanatfelvételek. Alkalmazás egységes pillanatképek kötet szolgáltatás (Árnyékmásolata) használja annak érdekében, hogy alkalmazásokat konzisztens állapotban, amikor a pillanatfelvétel származik. Figyelje meg, hogy ha bekapcsolja az alkalmazás egységes pillanatképek, befolyásolja a forrás virtuális gépeken futó alkalmazások teljesítményét. Győződjön meg arról, hogy beállított értéke kisebb, mint konfigurálnia további helyreállítási pontok száma.

    ![Adatvédelmi beállítások konfigurálása](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. **Adatok átvitele tömörítés**adja meg, hogy van-e replikált átvitt adatok tömörítve.
9. **Hitelesítés**adja meg, hogyan hitelesíti forgalom az elsődleges és helyreállítási a Hyper-V kiszolgálók között. Jelölje ki a HTTPS, hacsak nem működő Kerberos környezetben konfigurálva van. Azure webhely helyreállítási automatikusan beállítja a tanúsítványok a HTTPS-hitelesítést. Nincs kézi beállítás nem szükséges. Kerberos választásakor Kerberos-jegy host-kiszolgálók kölcsönös hitelesítéshez használható. Alapértelmezés szerint port 8083 és 8084 (a tanúsítványok) nyitja meg a Windows tűzfal be a Hyper-V kiszolgálók. Figyelje meg, hogy ez a beállítás csak akkor jelentősége a Windows Server 2012 R2 rendszeren futó a Hyper-V kiszolgálók.
10. A **Port**módosítsa a portszámot, amelyen a forrás- és célwebhelyek állomásokon meghallgatása replikációs. Például a beállítás előfordulhat, hogy módosítsa, ha szolgáltatás minősége (QoS) a hálózati sávszélesség-replikációs szabályozási alkalmazni szeretné. Ellenőrizze, hogy a portot más alkalmazás nem használja, és nyissa meg a tűzfal beállításai.
11. **Replikációs módszer**adja meg a kezdeti replikáció célalkalmazás helyekre forrásból származó adatok kezelésének módját, normál replikációs indítása előtt:

    - **Hálózaton keresztül**– adatok másolása a hálózaton keresztül időigényes lehet és erőforrás-igényes. Azt javasoljuk, hogy ezt a beállítást használja, ha a felhőben virtuális gépeken futó viszonylag kis virtuális merevlemez tartalmazza, és az elsődleges webhely széles sávszélességű kapcsolaton keresztül csatlakozik a másodlagos webhelyet. Megadhatja, hogy a példány kell azonnal elindul, vagy jelölje ki a kívánt időtartamot. Ha hálózati replikációs használja, azt javasoljuk, ütemezése a során essen.
    - **Kapcsolat nélküli**– ezzel a módszerrel, adja meg, hogy a kezdeti replikáció fog történni külső médiaformátumokkal. Ez akkor hasznos, ha el szeretné kerülni a hálózati teljesítmény, illetve távoli földrajzi helyek csökkenés. Ezt a módszert, adja meg az exportálási hely a forrás felhőben, és a cél felhő importálása pontjára. Ha engedélyezi a védelem virtuális géphez, a virtuális merevlemez a megadott export helyre másolja. Küldje el a célwebhely, és másolja a vágólapra az importálás helyre. A rendszer az importált adatok másolása a replika virtuális gépeken futó.

12. Jelölje ki a **replika virtuális gép törlése** megadhatja, hogy a replika virtuális gép törölni kell ha abbahagyja a virtuális gép védelme a **védelmet a virtuális gép törlése** lehetőség választásával virtuális gépeken futó lapján az a felhő tulajdonságok. Ezzel a beállítással, védelem letiltásakor a virtuális gép Azure webhely helyreállítási törlődik, a webhely helyreállítási beállításait a virtuális gép törlődnek a VMM konzol és a replika törlődik.

    ![Adatvédelmi beállítások konfigurálása](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

A beállítások mentése után a feladat jön létre, és a **feladatok** lap figyelhető. Minden a Hyper-V kiszolgálók VMM forrás felhőbeli konfigurálja a rendszer a replikáció. A **beállítás** lapon módosíthatók a felhőben beállításai. Ha meg szeretné változtatni a cél helyre vagy a Céllista felhő távolítsa el a felhőben konfiguráció, és az majd átkonfigurálása a felhőben.

### <a name="prepare-for-offline-initial-replication"></a>Kapcsolat nélküli kezdeti replikációs előkészítése

Végezze el az alábbi műveletek offline kezdeti replikációs Felkészülés kell:

- A forrás kiszolgálón egy elérési útját, amelyből az adatok exportálása vitelekor fogja adja meg. Teljes hozzáférés hozzárendelése NTFS és megosztása az engedélyek a VMM szolgáltatás, kattintson az Exportálás elérési útját. A cél kiszolgálón fogja adja meg a elérési útját, amelyből az adatok importálása akkor fordul elő. Rendelje hozzá az Importálás elérési út a ugyanolyan engedélyekkel.
- Az importálás és exportálás elérési út meg van osztva, rendelje hozzá a rendszergazda, kiemelt felhasználó, nyomtatás operátor vagy Server operátor csoporttagság az VMM szolgáltatásfiókhoz a távoli számítógépen, amelyen a megosztott található.
- Hosts, hozzáadásához kattintson az importálás és exportálás elérési út bármelyik Run As fiókok esetén, ha olvasási és hozzárendelése írási engedély VMM Futtatás mint fiókok.
- Az importálás és exportálás megosztások nem kell elhelyezni bármely olyan számítógépen, használja a Hyper-V kiszolgáló, mert visszacsatolási konfigurációs nem támogatja a Hyper-V.
- Az Active Directory a minden a Hyper-V kiszolgáló, amely tartalmazza a virtuális gépeken futó szeretné megvédeni, engedélyezése és beállítása korlátozott delegálás meg a távoli számítógépek, amelyen az importálás és exportálás elérési utak találhatók, az alábbi képlettel történik:
    1. Nyissa meg a tartományvezérlőnek **Active Directory-felhasználók és a számítógépen**.
    2. Kattintson a konzolfáján **tartománynév** > **számítógépek**.
    3. Kattintson a jobb gombbal a Hyper-V kiszolgáló állomásneve > **Tulajdonságok**.
    4. Kattintson a **Meghatalmazás** lap T**Rozsdás delegálás megadott szolgáltatások csak ezen a számítógépen**.
    5. Kattintson a **bármely hitelesítési protokoll használatával**.
    6. **Hozzáadás** > **felhasználók és a számítógépen**.
    7. Írja be a nevét a számítógéphez, amelyen az exportálási útvonal > **OK gombra**. Az elérhető szolgáltatások listájából, tartsa lenyomva a CTRL billentyűt, és kattintson a **cifs** > **az OK gombra**. Ismételje meg a számítógéphez, amelyen az Importálás elérési útja nevét. Ismételje meg ezt a Hyper-V kiszolgálók további szükséges.

## <a name="step-5-configure-network-mapping"></a>Lépés 5: A hálózati hozzárendelést beállítása
1. Első lépések lapon kattintson a **hálózatok**gombra.
2. Kattintson a VMM forráskiszolgáló, amelyhez képest hálózatok csatlakoztatása, és válassza a cél VMM kiszolgáló, amely a hálózatok kell megfeleltetni. A forrás hálózatok listáját és a kapcsolódó cél hálózatok jelennek meg. Üres érték, amely nem hozzárendelve hálózatok jelennek meg.
3. Jelölje ki a hálózati **Forrás hálózati** > **térképen**. A szolgáltatás a virtuális hálózatok célalkalmazás kiszolgálói észleli és jeleníti meg őket. Kattintson az adatok ikonra megtekintése a alhálózat minden hálózat forrás- és célwebhelyek hálózati neve mellett.

    ![Hálózati hozzárendelést konfigurálása](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. A párbeszédpanelen jelölje ki azt a virtuális hálózatok a cél VMM kiszolgálóról.

    ![Jelölje be a cél hálózaton](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Amikor kijelöl egy célalkalmazás hálózaton, használja a source network a védett felhő jelennek meg. Rendelkezésre álló cél hálózatok rendelt az a felhő védelem használt is megjelennek. Azt javasoljuk, válassza a cél hálózaton használja-védelem felhőket ábrázoló számára elérhető. Vagy is nyissa meg a VMM kiszolgáló és a megfelelő a virtuális hálózat, válassza a kívánt logikai hálózat hozzáadása a felhőben tulajdonságainak módosítása.
6. Kattintson a jelölőnégyzet be van jelölve a hozzárendelés befejezéséhez. A feladat nyomon kövesse a hozzárendelés elindul. Megtekintheti a **feladatok** lap.


## <a name="step-6-configure-storage-mapping"></a>Lépés 6: A tárterület-megfeleltetés beállítása
Alapértelmezés szerint egy adatforrás a Hyper-V kiszolgálón virtuális géphez, bizonyos célalkalmazás a Hyper-V host-kiszolgálón, replikált adatokat tárolja az alapértelmezett helyre a Hyper-V kezelője a Hyper-V célállomás jelölt. Replikált adatokat tároló pontosabban adhatja tárterület-hozzárendelések az alábbi képlettel történik:



1. A forrás- és célwebhelyek VMM kiszolgálókon tároló sorosztály megadása [Tudjon meg többet](https://technet.microsoft.com/library/gg610685.aspx). Besorolása elemet kell lennie a forrás- és célwebhelyek felhőket a Hyper-V kiszolgálók elérhető. Osztályozás nem kell a tároló azonos típusú van. Forrás osztályozás, amely tartalmazza a kis-és Középvállalatok megosztások CSVs tartalmazó cél osztályozási például képezhető.
2. Miután sorosztály vannak még hozzárendelések hozhat létre. Ehhez az **Első lépések** oldalon > **térkép tároló**.
3. Kattintson a **tárolás** lap > **térkép tároló besorolása elemet**.
4. A **tároló sorosztály hozzárendelése** lapon jelölje ki a besorolása elemet, kattintson a forrás, és cél VMM kiszolgálók. A beállítások mentéséhez.

    ![Jelölje be a cél hálózaton](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>7 lépés: A virtuális gép védelem bekapcsolása
Miután kiszolgálók, a felhő és a hálózatok helyesen van beállítva, engedélyezheti védelmének virtuális gépeken futó a felhőben.

1. A felhőben, amelyben a virtuális gép található **virtuális gépeken futó** lapon kattintson a **védelem** > **hozzáadása virtuális gépeken futó**.
2. A felhőben virtuális gépeken futó a listából jelölje ki a védelemmel ellátni kívánt.

    ![Virtuális gép védelem bekapcsolása](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. A védelem bekapcsolása művelet a **feladatok** lap, beleértve a kezdeti replikáció nyomon kövesse. A védelem véglegesítése feladat futtatása után a virtuális gép készen áll a feladatátvételi. Védelem engedélyezett és a virtuális gépeken futó replikált után is Azure-ban meg őket.

    ![Virtuális gép védelem feladat](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] A védelmet a virtuális gépeken futó a VMM konzolban. **Védelem bekapcsolása** kattintson az eszköztáron a virtuális gép tulajdonságok **Azure webhely helyreállítás** lapon.

### <a name="on-board-existing-virtual-machines"></a>Fedélzeti meglévő virtuális gépeken futó

Ha van meglévő virtuális gépeken futó VMM az, hogy a Hyper-V replikával replikálja kell beépített őket Azure webhely helyreállítási védelme az alábbi képlettel történik:

1. Ellenőrizze, hogy az elsődleges és másodlagos felhőket rendelkezik. Győződjön meg arról, hogy a meglévő virtuális gép a Hyper-V kiszolgálójához elsődleges felhőbeli és is, hogy a Hyper-V kiszolgáló szolgáltatója a replika virtuális gép található, a másodlagos felhőben. Győződjön meg arról, hogy beállított adatvédelmi beállításokat, az a felhő. A beállítások meg kell egyezniük már konfigurálva vannak a Hyper-V kópia. Egyéb esetben virtuális gép replikációs előfordulhat, hogy nem működik megfelelően.
2. Az elsődleges virtuális gép védelem majd engedélyezze. Azure webhely helyreállítási és VMM biztosítja, hogy az azonos replika host és virtuális gép lép fel, és Azure webhely helyreállítási fog használni a leképezést, és a felhő konfigurálása során konfigurált beállításokkal replikációs visszaállítása.


## <a name="test-your-deployment"></a>A telepítés tesztelése

A telepítés, futtathatja a próba feladatátvevő egyetlen virtuális géphez, vagy több virtuális gépeken futó álló helyreállítási terv létrehozása és futtatása a terv próba feladatátvevő ellenőrzéséhez.  Teszt feladatátvevő keltő szűrő megőrzi a feladatátvétel és helyreállítási mechanizmusa elszigetelt hálózat.

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

1. A **Helyreállítási tervek** lapon kattintson a **Helyreállítási terv létrehozása**gombra.
2. Adja meg a helyreállítási tervet, és a forrás- és célwebhelyek VMM kiszolgáló nevét. A forráskiszolgálóval virtuális gépeken futó feladatátvétel és helyreállítási engedélyezett kell rendelkeznie. Jelölje be **A Hyper-V** csak a Hyper-V replikációs konfigurálták felhőket megtekintéséhez.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. **Jelölje ki a virtuális gép**jelölje ki a replikáció csoportokat. A replikáció csoporthoz társított virtuális gépeken futó lesz jelölve, és a helyreállítási terv adott hozzá. A virtuális gépeken futó hozzá lesznek adva a helyreállítási terv alapértelmezett csoporthoz – 1 csoportot. További csoportok is hozzáadhat, ha szükséges. Megjegyzendő, hogy miután replikációs virtuális gépeken futó fog használatba veszik a helyreállítási terv csoportok sorrendjének megfelelően.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

A helyreállítás terv létrejött, a listában a **Helyreállítási tervek** lapon megjelenik.

###<a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

1. A **Helyreállítási tervek** lapon jelölje ki azt a csomagot, és kattintson a **Vizsgálat feladatátvevő**.
2. A **Próba feladatátvevő erősítse meg** lapon válassza a **nincs**lehetőséget. Nem látható, hogy ezt a beállítást választja replika virtuális gépeken futó fölé a sikertelen engedélyezve van olyan hálózat csatlakozik. Ez lesz tesztelése, hogy a virtuális gép vártnak átvétele, de nem vizsgálja meg a replikáció hálózati környezetben. Nézze meg a [próba feladatátvevő futtatása](site-recovery-failover.md#run-a-test-failover) másik hálózati beállítások használatáról olvashat bővebben.
3. A próba virtuális gépen, amelyen a kópia virtuális gép létezik a host azonos állomáson jön létre. A replika virtuális gép található azonos felhőbeli lett hozzáadva.

### <a name="run-a-recovery-plan"></a>Futtassa a helyreállítási terv
Replikáció után a replika virtuális gép előfordulhat, hogy nincs IP-címet, amely megegyezik az elsődleges virtuális gép IP-címét. Virtuális gépeken futó frissíti a DNS-kiszolgáló, amely használnak után induljanak el. Frissítse a DNS-kiszolgáló ahhoz, hogy egy időben frissítés parancsfájl is hozzáadhat.

#### <a name="script-to-retrieve-the-ip-address"></a>Az IP-cím beolvasásához parancsfájl
Futtassa a mintaparancsfájl beolvasásához IP-címét.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>A DNS frissítése parancsfájl
Futtassa a mintaparancsfájl DNS, adja meg az IP-cím használata az előző mintaparancsfájl lekért frissítéséhez.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Webhely-helyreállítás adatvédelmi információkat

Ez a témakör a Microsoft Azure webhely helyreállítási ("") szolgáltatást a további adatvédelmi információkat. A Microsoft Azure-szolgáltatások adatvédelmi nyilatkozat megtekintéséhez, lásd: a [Microsoft Azure adatvédelmi nyilatkozata](http://go.microsoft.com/fwlink/?LinkId=324899)

**Szolgáltatás: regisztráció**

- **Mit jelent**: kiszolgáló regisztrálja, hogy virtuális gépeken futó védhetők szolgáltatásban
- **Adatok gyűjtése**: után regisztrálása a szolgáltatás által gyűjtött, folyamatok, és továbbítja az adatkezelési tanúsítvánnyal kapcsolatos információk megadásához a szolgáltatás nevét a VMM kiszolgáló, és a virtuális gép felhőket ábrázoló használata a VMM kiszolgálón vészhelyreállítás jelölt VMM kiszolgálóról.
- Az **információk felhasználása**:
    - Adatkezelési tanúsítvány – használatos azonosításához és az access a szolgáltatás regisztrált VMM kiszolgálója hitelesítést végezni. A szolgáltatás biztonságos jogkivonat, amely csak a regisztrált VMM kiszolgáló férhetnek hozzá a tanúsítvány nyilvános kulcs részét használja. A kiszolgáló van szüksége a token a szolgáltatások eléréséhez szükséges.
    - Kiszolgáló nevének a VMM – a VMM kiszolgálónév szükség azonosításához és kommunikálni a megfelelő VMM-kiszolgálóval, amelyen a felhő találhatók.
    - Nevek a VMM kiszolgálóról cloud – a felhőben neve szükség, a szolgáltatás felhő párosítás/unpairing a szolgáltatás leírása alább használata esetén. Ha úgy dönt, hogy a felhőben egy elsődleges adatok központból az helyreállítási adatközpontban egy másik felhőben való csatlakoztatásához, a neveket a helyreállítási adatok központból felhőket ábrázoló jelennek meg.

- **Választási lehetőségek**: ezt az információt a regisztrációs folyamat alapvető részét képezi, mert Ön és a szolgáltatás azonosítja a VMM kiszolgálót, amelyhez hozzá szeretne megadni a Azure webhely helyreállítási védelmét, valamint hogy meghatározza azokat a megfelelő regisztrált VMM kiszolgáló segít. Ha nem szeretné a szolgáltatás elküldi a ezt az információt, ne használja ezt a szolgáltatást. Ha a kiszolgáló regisztrálása, és majd később szeretné azt unregister, ezt az (Ez az Azure-portálra) szolgáltatás portálról a VMM kiszolgálóadatok törlésével teheti meg.

**Szolgáltatás: Azure webhely helyreállítási védelem bekapcsolása**

- **Mit jelent**: az Azure webhely helyreállítási szolgáltatót telepíteni a kiszolgálón VMM a továbbítás kidolgozása a szolgáltatással. A szolgáltató dinamikus csatolású függvénytár (DLL) is a VMM folyamatban. A szolgáltató telepítése után a "Adatközponthoz helyreállítási" funkció VMM felügyeleti konzolban kap engedélyezett. Minden új vagy meglévő virtuális gépeken futó a felhőben engedélyezheti a virtuális gép védelme érdekében "adatközponthoz helyreállítási" nevű tulajdonságot. Amikor megadja ezt a tulajdonságot, a szolgáltató küld a nevét, és a virtuális gép Azonosítóját a szolgáltatást. Virtuális védelme a Windows Server 2012-ben vagy Windows Server 2012 R2 Hyper-V replikációs technológiával engedélyezve van. A virtuális gép adatokat kapja replikált egyik számítógépről a Hyper-V között (általában egy másik "helyreállítási" adatközpont található).

- **Adatok gyűjtése**: A szolgáltatás által gyűjtött, folyamatok és továbbítja a virtuális gép, amelyben szerepel a név, azonosító, virtuális hálózati és a felhő neve metaadatait, amelyhez tartozik.

- **Információk felhasználása**: A szolgáltatás a fenti információk segítségével az virtuális gép információkat a szolgáltatás portálon feltöltése.

- **Választási lehetőségek**: Ez az alapvető része a szolgáltatást, és nem kapcsolható ki. Ha nem szeretné, hogy a szolgáltatásnak küldött, ezt az információt, nem engedélyezi az Azure webhely helyreállítási védelmének bármely virtuális gépeken futó. Figyelje meg, hogy a szolgáltatás a szolgáltató által küldött összes adat a HTTPS küldi.

**Szolgáltatás: Helyreállítási terv**

- **Mit jelent**: Ez a funkció segítséget nyújt az "helyreállítási" adatközpont üzletifolyamat-tervező csomagjával össze. Megadhatja, hogy a sorrendben, amelyben a virtuális gépeken futó vagy egy virtuális gépeken futó csoportja el kell indítani a helyreállítási webhelyén. Megadhatja, hogy minden automatikus parancsfájlok futtatása vagy minden kézi végrehajtandó műveletet, a virtuális gépeken helyreállítási időpontjában lesz. (A következő szakaszban szereplő) feladatátvevő általában induljanak összehangolt helyreállítási helyreállítási terv szintjén.

- **Adatok gyűjtése**: A szolgáltatás által gyűjtött, folyamatok és a helyreállítási tervet, beleértve a virtuális gép metaadatok és automatizálási parancsfájlokat és a kézi művelet jegyzetek metaadat-alapú metaadatait továbbítja.

- **Információk felhasználása**: A fentebb ismertetett metaadatok a portálon helyreállítási terv szolgál.

- **Választási lehetőségek**: Ez az alapvető része a szolgáltatást, és nem kapcsolható ki. Ha nem szeretné, hogy a szolgáltatásnak küldött, ezt az információt, nem hozhat létre helyreállítási tervek a szolgáltatásban.

**Szolgáltatás: Hálózati hozzárendelést**

- **Mit jelent**: Ezzel a funkcióval hálózati adatait az elsődleges adatközpont helyreállítási adatközpontja térképen. A virtuális gépeken futó helyreállítási a helyszínen van vissza, ha a hozzárendelés segíti őket a hálózati kapcsolat létrehozásáról.

- **Adatok gyűjtése**: a hálózati megfeleltetés szolgáltatás részét képező a szolgáltatás által gyűjtött, folyamatok és a metaadatokat az egyes webhely (elsődleges és adatközponthoz) logikai hálózatok továbbítja.

- **Információk felhasználása**: A szolgáltatás használja a metaadatok a portálon feltöltése hol képezhető a hálózat adatait.

- **Választási lehetőségek**: Ez az alapvető része a szolgáltatást, és nem kapcsolható ki. Ha nem szeretné, hogy a szolgáltatásnak küldött, ezt az információt, nem a funkcióval hálózati megfeleltetés.

**Szolgáltatás: Feladatátvevő - tervezett, a tervezett, teszt**

- **Mit jelent**: Ez a funkció egy virtuális gép segítségével egy felügyelt adatközpont VMM egy másik VMM felügyelt adatközpontja. A szolgáltatás a portálon a felhasználó által a feladatátvevő művelet induljanak. Lehetséges okai feladatátvevő tervezett esemény tartalmazza (például egy természetes disaster0; Ha a tervezett esemény (Ha például adatközponthoz terheléselosztás); próba feladatátvevő (például egy helyreállítási terv részletes).

A szolgáltató VMM kiszolgálói a szolgáltatásból az esemény értesítést kap, és egy feladatátvevő művelet végrehajtása a Hyper-V állomáson VMM felületeken keresztül. A Windows Server 2012-ben vagy Windows Server 2012 R2 Hyper-V replikációs technológia tényleges feladatátvevő a virtuális gép egyik számítógépről a Hyper-V között (általában egy másik "helyreállítási" adatközpont operációs rendszerrel) kezeli. A feladatátvételi befejeződése után a szolgáltatót telepíteni a kiszolgálón, az "helyreállítási" adatközpont VMM adatokat küld a a sikeres a szolgáltatást.

- **Adatok gyűjtése**: A szolgáltatás állapotát az feladatátvevő művelet információkat a szolgáltatás portálon feltöltése a fenti információk használja.

- **Információk felhasználása**: A szolgáltatás által a fenti információk az alábbi képlettel történik:

    - Adatkezelési tanúsítvány – használatos azonosításához és az access a szolgáltatás regisztrált VMM kiszolgálója hitelesítést végezni. A szolgáltatás biztonságos jogkivonat, amely csak a regisztrált VMM kiszolgáló férhetnek hozzá a tanúsítvány nyilvános kulcs részét használja. A kiszolgáló van szüksége a token a szolgáltatások eléréséhez szükséges.
    - Kiszolgáló nevének a VMM – a VMM kiszolgálónév szükség azonosításához és kommunikálni a megfelelő VMM-kiszolgálóval, amelyen a felhő találhatók.
    - Nevek a VMM kiszolgálóról cloud – a felhőben neve szükség, a szolgáltatás felhő párosítás/unpairing a szolgáltatás leírása alább használata esetén. Ha úgy dönt, hogy a felhőben egy elsődleges adatok központból az helyreállítási adatközpontban egy másik felhőben való csatlakoztatásához, a neveket a helyreállítási adatok központból felhőket ábrázoló jelennek meg.

- **Választási lehetőségek**: Ez az alapvető része a szolgáltatást, és nem kapcsolható ki. Ha nem szeretné, hogy a szolgáltatásnak küldött, ezt az információt, ne használja ezt a szolgáltatást.

## <a name="next-steps"></a>Következő lépések

Után már futtatása próba áttérni ellenőrizze, hogy a környezetben van a várt módon működnek, feladatátadás különböző típusú [ismertetése](site-recovery-failover.md) .
