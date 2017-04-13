<properties
    pageTitle="Azure virtuális gép biztonsági másolat elhárítása |} Microsoft Azure"
    description="Biztonsági mentési és visszaállítási az Azure virtuális gépeken futó – problémamegoldás"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure virtuális gép biztonsági másolat – problémamegoldás

> [AZURE.SELECTOR]
- [Helyreállítási szolgáltatások tárolóból elemre](backup-azure-vms-troubleshoot.md)
- [Biztonsági másolat tárolóból elemre](backup-azure-vms-troubleshoot-classic.md)

Közben az alábbi táblázatban felsorolt biztonsági mentéssel Azure információkkal hibák elhárításához.

## <a name="backup"></a>Biztonsági másolat

| A biztonsági mentés | Részletek | Megoldás: |
| -------- | -------- | -------|
|Biztonsági másolat|    A művelet nem hajtható végre, virtuális már nem létezik. -Állítsa le a virtuális gép védelme biztonsági adatok törlése nélkül. További részleteket a http://go.microsoft.com/fwlink/?LinkId=808124   |Ez történik, ha az elsődleges virtuális törlődik, de a biztonsági másolat házirend keresse meg a biztonsági másolat virtuális továbbra is. Ez a hiba kijavításához: <ol><li> Hozza létre a virtuális készülék ugyanazt a nevet és egy erőforrás csoportnévre [felhőalapú szolgáltatás neve],<br>(VAGY)</li><li> Állítsa le a védelme virtuális gép vagy a biztonsági adatok törlése nélkül. [További részletek](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Biztonsági másolat|Nem lehet kommunikálni a virtuális agent pillanatkép állapot. -Győződjön meg arról, hogy virtuális csatlakozik az internethez. Frissítse a virtuális agent http://go.microsoft.com/fwlink/?LinkId=800034 a hibaelhárítási guide említett |Ezt a hibát elő, ha a virtuális Agent probléma, vagy valamilyen módon a hálózati hozzáférés az Azure infrastruktúra zárolva van. További információ a virtuális felfelé hibakeresési pillanatkép problémák megismerése<br> Ha a virtuális agent nem okoz kapcsolatos problémák megoldásához, majd indítsa újra a virtuális. Időnként helytelen virtuális állapotba problémákat okozhatják, és indítsa újra a virtuális visszaállítja az ebben a "rossz állapotban"|
|Biztonsági másolat|    Helyreállítási szolgáltatások bővítmény művelet sikertelen volt. -Ellenőrizze, hogy virtuális gép agent legújabb megtalálható a virtuális gépen és ügynök szolgáltatást futtató. Ismételje meg a biztonsági mentést, és ha nem sikerül, forduljon a Microsoft támogatási.|   Ez a hiba esetén virtuális ügynök elavult elő. Olvassa el a virtuális agent frissítése az alábbi "A virtuális ügynök frissítése" szakaszt.|
|Biztonsági másolat |Virtuális gép nem létezik. -Kérjük, győződjön meg róla, hogy a virtuális gép, vagy válassza ki a virtuális másik gépre. | Ez történik, ha az elsődleges virtuális törlődik, de a biztonsági másolat házirend keresse meg a biztonsági másolat virtuális továbbra is. Ez a hiba kijavításához: <ol><li> Hozza létre a virtuális készülék ugyanazt a nevet és egy erőforrás csoportnevet [felhőalapú szolgáltatás neve],<br>(VAGY)<br></li><li>Állítsa le a virtuális gép védelme biztonsági adatok törlése nélkül. [További részletek](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Biztonsági másolat |Nem sikerült parancs végrehajtása. – Egy másik művelet jelenleg folyamatban van az elemre. Kis türelmet, amíg az előző művelet befejeződik, majd próbálkozzon újra |Egy meglévő biztonsági másolat vagy a virtuális visszaállítási feladat fut, és az új feladat nem indítható el a meglévő feladat futtatása közben.|
| Biztonsági másolat | VHD másolása a időtúllépési - biztonsági tárolóból elemre próbálkozzon ismét néhány perc alatt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support. | Ez történik, ha túl sok adatot másolni. Ellenőrizze a kisebb, mint 16 adatok lemez esetén. |
| Biztonsági másolat | Biztonsági másolat belső hiba miatt sikertelen volt – a művelet néhány perc múlva próbálkozzon újra. Ha a probléma továbbra is fennáll, lépjen kapcsolatba a Microsoft Support | Ez a hibaüzenet 2 okok miatt: <ol><li> Van egy tranziens probléma virtuális tároló elérése. Ellenőrizze az [Azure állapot](https://azure.microsoft.com/en-us/status/) van-e a tartományban lévő számítási/tároló/hálózati kapcsolatos bármilyen a folyamatban lévő problémát. Ellenőrizze ismét a biztonsági másolat bejegyzés probléma szünteti meg. <li>Az eredeti virtuális törli, és nem tud tehát tenni biztonsági másolatot. Az adatok biztonsági másolatának megőrzése a törölt virtuális, de a biztonsági másolat hibák megszüntetése, Lapvédelem feloldása a virtuális, és válassza ki a vezérlőt, amellyel tartani az adatokat. Ezzel leállítja a biztonsági mentés ütemezése, és az ismétlődő hibaüzenetek jelennek meg. |
| Biztonsági másolat | Nem sikerült telepítenie az Azure helyreállítási szolgáltatások a a kijelölt elem – virtuális ügynök kiterjesztés Azure helyreállítási szolgáltatások bővítmény előzetesen szükséges. Telepítse az Azure virtuális ügynök, majd indítsa újra a regisztrációs művelet | <ol> <li>Annak ellenőrzése, ha a virtuális agent megfelelően telepítve. <li>Győződjön meg arról, hogy a virtuális config a jelölő megfelelően van-e beállítva.</ol> [További információ:](#validating-vm-agent-installation) virtuális ügynök telepítésről, és hogyan kell a virtuális ügynök telepítés ellenőrzése. |
| Biztonsági másolat | Bővítmény telepítése nem sikerült a "COM + nem tudott felvegye a Microsoft Distributed tranzakciókoordinátorral | Ez általában azt jelenti, hogy nem fut a COM + szolgáltatás. Forduljon segítségért a Microsofthoz a problémák orvoslására: Ez a probléma. |
| Biztonsági másolat | Pillanatkép művelet nem sikerült a VSS művelet "meghajtó zárolt a BitLocker titkosítással. A meghajtó Vezérlőpultról kell zárolásának feloldása | Az összes meghajtón lévő a virtuális BitLocker kikapcsolása, és tekintse meg az a VSS probléma megoldódott |
| Biztonsági másolat | Nem támogatott prémium tárterület-on tárolt virtuális merevlemezeken problémákat virtuális gépeken futó biztonsági mentése | Nincs lehetőség |
| Biztonsági másolat | Azure virtuális gép nem található. | Ez történik, ha az elsődleges virtuális törlődik, de a biztonsági másolat házirend keresse meg a biztonsági másolat virtuális továbbra is. Ez a hiba kijavításához: <ol><li>Hozza létre a virtuális készülék ugyanazt a nevet és egy erőforrás csoportnévre [felhőalapú szolgáltatás neve], <br>(VAGY) <li> Tiltsa le a védelmet a virtuális, így a biztonsági másolat feladatok nem hozható létre </ol> |
| Biztonsági másolat | Virtuális gép ügynök nem szerepel a virtuális gépen -, telepítse a szükséges előzetesen szükséges virtuális ügynök, és indítsa újra a műveletet. | [További információ:](#vm-agent) virtuális ügynök telepítésről, és hogyan kell a virtuális ügynök telepítés ellenőrzése. |

## <a name="jobs"></a>Feladatok

| Művelet | Részletek | Megoldás: |
| -------- | -------- | -------|
| Feladat megszakítása | A lemondás nem támogatott a projekt típusa – kis türelmet, amíg a feladat befejezése után. | Nincs lehetőség |
| Feladat megszakítása | A feladat nem törölhető állapotban-as-kis türelmet, amíg a feladat befejezése után. <br>VAGY<br> A kijelölt feladat nem törölhető állapotban-as-várja meg a feladat befejezéséhez.| A projekt minden valószínűség szerint szinte befejeződött; kis türelmet, amíg a feladat befejezése után |
| Feladat megszakítása | A feladat nem vonható vissza, mert nincs folyamatban - lemondás csak a feladatokat, amely a folyamatban lévő támogatott. Kérjük, a kísérlet a egy folyamatban lévő megszüntetése feladatot. | Ez történik, átmeneti állapot miatt. Várja meg a percet, és újra a Mégse műveletet |
| Feladat megszakítása | A feladat megszakítása - Kis türelmet, amíg a feladat befejezése sikertelen volt. | Nincs lehetőség |


## <a name="restore"></a>Visszaállítása
| Művelet | Részletek | Megoldás: |
| -------- | -------- | -------|
| Visszaállítása | Nem sikerült visszaállítani felhő belső hiba | <ol><li>DNS-beállítások van beállítva, amelyhez visszaállítani kívánt felhőszolgáltatásba. Érdemes <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-tárolóhely "Gyártási" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ha címmel, ez azt jelenti, hogy a DNS-beállítások vannak-e beállítva.<br> <li>Fenntartott IP, amelyre visszaállítani kívánt felhőszolgáltatásba van beállítva, és felhőszolgáltatásában már meglévő VMs leállítva állapotban vannak.<br>Ellenőrizheti, hogy egy felhőalapú szolgáltatásba IP lefoglalt alábbi powershell-parancsmagok használatával:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-"Gyártási" $tárolóhely DEP ReservedIPName <br><li>Ha vissza szeretne állítani egy virtuális számítógépre, az alábbi speciális hálózati beállításokat ugyanazon felhőszolgáltatásba próbálja. <br>-Virtuális gépeken futó betöltés terheléselosztó konfigurációja (belső és külső)<br>-Virtuális gépeken futó a több fenntartott IP-címei<br>-Virtuális gépeken futó több együtt<br>Jelöljön ki egy új felhőszolgáltatásba a felhasználói felület vagy kérjük, olvassa el az [Visszaállítás előtt megfontolandó kérdések](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) VMs különleges hálózati konfigurációjának</ol> |
| Visszaállítása | A kijelölt DNS névnek - adjon meg egy másik DNS nevet, és próbálkozzon újra. | A tartománynév hivatkozik a felhőalapú szolgáltatás neve (általában végződésű. cloudapp.net). Ez a szükséges egyedinek kell lennie. Ha ezt a hibát, meg kell egy másik virtuális nevet válassza a visszaállítás során. <br><br> Megjegyzés: Ez a hiba csak azokra a felhasználókra az Azure Portal által megjelenített. A visszaállítás Powershellen keresztül fog sikerülni, mert csak visszaállítja a lemez és a virtuális nem jön létre. A hiba lesz projektvezetők, létrehozásakor a virtuális kifejezetten úgy, hogy a lemez visszaállítási művelet után. |
| Visszaállítása | A megadott virtuális hálózati konfigurálásról nem helyes - adjon meg egy másik virtuális hálózat konfigurálása és próbálkozzon újra. | Nincs lehetőség |
| Visszaállítása | A megadott felhőalapú szolgáltatást használ egy fenntartott IP, amely nem felel meg a visszaállítandó virtuális gép konfigurációja - adjon meg egy másik felhőalapú szolgáltatásba, amely nem használ fenntartott IP, vagy válasszon másik helyreállítási pontot szeretné visszaállítani. | Nincs lehetőség |
| Visszaállítása | Felhőbeli szolgáltatástól kapott beviteli végpontjait száma korlátozott – valamelyik másik felhőszolgáltatásában megadásával, illetve meglévő zárólap használatával, ismételje meg a műveletet. | Nincs lehetőség |
| Visszaállítása | Biztonsági másolat tárolóból elemre, és a cél tárterület-fiókot a két különböző régiók – győződjön meg arról, hogy a visszaállítási művelet megadott tárterület-fiók a biztonsági másolat tárolóra azonos Azure régióban. | Nincs lehetőség |
| Visszaállítása | Tárterület-fiók megadott a visszaállítási művelet nem támogatott - csak Basic/Standard tároló fiókok helyileg felesleges vagy geo felesleges replikációs beállításai támogatottak. Jelöljön ki egy támogatott tárterület-fiókkal | Nincs lehetőség |
| Visszaállítása | Nem érhető el a tárhely fiók-visszaállítási művelet megadott típusú - győződjön meg arról, hogy online állapotban-e a visszaállítási művelet megadott tárterület-fiók | Ez akkor fordulhat Azure-tárolóban lévő vagy egy üzemszünetek miatt tranziens hiba miatt. Válasszon egy másik tárterület-fiókot. |
| Visszaállítása | Erőforráscsoport kvóta elérte - töröljön néhány erőforrás csoportok Azure portálról vagy Azure ügyfélszolgálatát a korlátok növeléséhez. | Nincs lehetőség |
| Visszaállítása | A kijelölt alhálózathoz nem létezik - jelöljön ki egy meglévő alhálózat | Nincs lehetőség |


## <a name="policy"></a>Házirend
| Művelet | Részletek | Megoldás: |
| -------- | -------- | -------|
| Házirend létrehozása | A házirend - létrehozása sikertelen volt a adatmegőrzési lehetőségek házirend beállítása folytatásához csökkentse. | Nincs lehetőség |


## <a name="vm-agent"></a>Virtuális Agent

### <a name="setting-up-the-vm-agent"></a>A virtuális ügynökök beállítása
Általában a virtuális Agent már szerepel a Azure gyűjteményből létrehozott VMs. Virtuális gépeken futó helyszíni adatközpontokkal az áttelepítendő azonban nem áll a virtuális Agent. Az ilyen VMs a virtuális Agent kell explicit módon telepíthető. További információ [a virtuális ügynök egy meglévő virtuális telepítése](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

A Windows VMs:

- Töltse le és telepítse az [MSI ügynök](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). A telepítés elvégzéséhez rendszergazdai jogosultságokkal kell.
- [A virtuális tulajdonság frissítéséhez](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) jelzi, hogy telepítve van-e a agent.

A Linux VMs:

- Telepítse a legújabb [Linux agent](https://github.com/Azure/WALinuxAgent) github.
- [A virtuális tulajdonság frissítéséhez](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) jelzi, hogy telepítve van-e a agent.


### <a name="updating-the-vm-agent"></a>A virtuális Agent frissítése
A Windows VMs:

- Frissítés a virtuális Agent akkor egyszerűen, a [virtuális Agent bináris](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)újratelepítése. Azonban kell ellenőrizze, hogy nincs biztonsági mentést fut a virtuális Agent frissítése közben.

A Linux VMs:

- A megjelenő útmutatást követve [Linux virtuális Agent frissítése](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Virtuális Agent telepítési érvényesítése
Útmutató a Windows VMs virtuális Agent verziójának ellenőrzése:

1. Jelentkezzen be az Azure virtuális géphez, és keresse meg azt a mappát, *C:\WindowsAzure\Packages*. Keresse meg a bemutató WaAppAgent.exe fájlt.
2. Kattintson a jobb gombbal a fájlra, válassza a **Tulajdonságok parancsot**, és válassza a **Részletek** fülre. A termék verziója írjuk 2.6.1198.718 vagy újabb

## <a name="troubleshoot-vm-snapshot-issues"></a>Virtuális pillanatkép kapcsolatos problémák megoldása
Virtuális biztonsági másolat támaszkodik mögöttes tároló Pillanatkép parancs kiadását. Nem rendelkező pillanatkép feladat-végrehajtás tároló vagy késleltetés a hozzáférést a biztonsági másolat is nem sikerül. A következő pillanatkép feladat sikertelensége okozhatják.

1. Hálózati hozzáférés tárolóhoz zárolva van NSG használatával<br>
   Részletesebben a [hálózati hozzáférés engedélyezése](backup-azure-vms-prepare.md#2-network-connectivity) a tárterület vagy WhiteListing az IP-címei vagy proxykiszolgáló keresztül.
2.  Beállította az Sql Server mentéssel VMs okozhatják pillanatkép tevékenység késleltetése <br>
    Virtuális alapértelmezés szerint a problémák VSS teljes biztonsági mentése a Windows VMs biztonsági másolatot készíteni. VMs, amely futtatja az Sql-kiszolgálók és az Sql Server biztonsági másolat be van állítva, a késleltetés okozhat a pillanatkép végrehajtása. Ha biztonsági hibákat tapasztal pillanatkép hibák miatt állítsa be következő beállításkulcsot.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  Virtuális állapot helytelenül jelenteni, mivel a virtuális RDP leállítás.  <br>
    A virtuális gép RDP kikapcsolja, ha ellenőrizze ismét a portálon, hogy virtuális állapotot megfelelően. Ha nem, állítsa le a virtuális "Kikapcsolás" beállítás használata virtuális irányítópult portálon.
4.  Ha több mint négy virtuális megosztás azonos cloud szolgáltatás, állítsa be a előkészítése a biztonsági másolat időpontok így nem legfeljebb négy virtuális biztonsági másolatok elindított egyszerre több biztonsági házirendek. Próbálja meg a terjeszteni időpontok közötti házirendek egymástól óránként biztonsági indítása. 
5.  Virtuális fut a magas Processzor/memóriában.<br>
    Ha a virtuális gépen fut, a nagy Processzor usage(>90%) vagy memória pillanatkép tevékenység aszinkron, késleltetett pedig a program végül időkorlátot. Próbálja ki az igény szerinti biztonsági másolatot az ilyen helyzetekben.

<br>

## <a name="networking"></a>Hálózati
Az összes bővítmény, például a biztonsági másolat bővítmény használata nyilvános internetkapcsolat hozzáférésre van szükségük. Nem a nyilvános internet-hozzáféréssel rendelkező cikkét is maga többféle módon:

- A bővítmény telepítése sikertelen lehet
- Hiba történhet a biztonsági mentés (például a lemez pillanatkép)
- Hiba történhet a biztonsági mentés állapotának megjelenítése

Nyilvános internetcímeket megoldása van szükség van már csuklós [Itt](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Jelölje be a DNS-beállításokat a VNET számára, és biztosítani, hogy az Azure URL-címe hozzárendelhető kell.

Miután a névfeloldás megfelelően befejeződött, a hozzáférést az Azure IP-címei is biztosítható kell. Tiltásának feloldása az Azure infrastruktúra való hozzáférést, hajtsa végre az alábbi lépések egyikét:

1. WhiteList az Azure adatközpont IP-tartományok szükségesek.
    - A lista beszerzése az [Azure adatközpont IP-címei](https://www.microsoft.com/download/details.aspx?id=41653) whitelisted lesz.
    - A [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) parancsmaggal IP-címei feloldása. Ezzel a parancsmaggal a Azure virtuális belül futnak egy jogú PowerShell-ablak (Futtatás rendszergazdaként).
    - Szabályok felvétele a NSG való (ha van egy helyen) az IP-címei való hozzáférés engedélyezése.
2. HTTP forgalmat mozgásvonal létrehozása
    - Ha bizonyos hálózati korlátozások helyen (a hálózat biztonsági csoport, például) üzembe a forgalmat a HTTP-proxy kiszolgálón. Található lépéseket követve telepítse a HTTP-Proxy server [Itt](backup-azure-vms-prepare.md#2-network-connectivity).
    - Szabályok felvétele a NSG való (ha van egy helyen) a HTTP-Proxy az INTERNETEN hozzáférést.

>[AZURE.NOTE] DHCP belül a vendégként való bekapcsolódáshoz IaaS virtuális biztonsági másolat használata engedélyezve kell lennie.  Ha egy statikus magánjellegű IP-Címeket, platformon keresztül kell beállítani. Balra engedélyezni kell a virtuális belül a DHCP lehetőséget.
Megtekintheti [Egy statikus belső magánjellegű IP-cím](../virtual-network/virtual-networks-reserved-private-ip.md)beállításáról további információt.
