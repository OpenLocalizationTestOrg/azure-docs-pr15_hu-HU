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

## <a name="discovery"></a>Feltárás

| A biztonsági mentés | Részletek | Megoldás: |
| -------- | -------- | -------|
| Feltárás | Fedezze fel az új elemek - észlelt a Microsoft Azure biztonsági mentése és belső hiba sikertelen volt. Várjon néhány percet, és próbálkozzon újra a műveletet. | 15 perc után el újra a feltáráshoz használt folyamatot.
| Feltárás | Nem sikerült felderíteni az új elemek – egy másik feltárás művelet már folyamatban van. Kis türelmet, amíg az aktuális feltárás művelet befejeződik. | Nincs lehetőség |

## <a name="register"></a>Regisztráció
| A biztonsági mentés | Részletek | Megoldás: |
| -------- | -------- | -------|
| Regisztráció | A virtuális géphez csatlakoztatott adatok lemez száma meghaladta a vonatkozó -, kérjük, bizonyos adatok lemezt a virtuális gépen leválasztása, és ismételje meg a műveletet. Azure biztonsági másolat támogatja az Azure virtuális gép biztonsági másolatának csatolt legfeljebb 16 adatok lemez | Nincs lehetőség |
| Regisztráció | Microsoft Azure biztonsági másolat belső hibába ütközött - Várjon néhány percet, és majd próbálkozzon újra a műveletet. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support. | Ez a hiba miatt a következő nem támogatott konfiguráció virtuális gép közül a prémium LRS elérheti. <br> Prémium tároló VMs készíthető használatával helyreállítási szolgáltatások tárolóból elemre. [tudj meg többet](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Regisztráció | Nem sikerült regisztrálni telepítése ügynök művelet időtúllépése | Annak ellenőrzése, ha a virtuális gép az operációs rendszer verziója támogatott. |
| Regisztráció | Nem sikerült - parancs végrehajtása egy másik művelet már folyamatban van az elemre. Kis türelmet, amíg az előző művelet végrehajtása | Nincs lehetőség |
| Regisztráció | Nem támogatott prémium tárterület-on tárolt virtuális merevlemezeken problémákat virtuális gépeken futó biztonsági mentése | Nincs lehetőség |
| Regisztráció | Virtuális gép ügynök nem szerepel a virtuális gépen -, telepítse a szükséges virtuális ügynök előzetesen szükséges, és indítsa újra a műveletet. | [További információ:](#vm-agent) virtuális ügynök telepítésről, és hogyan kell a virtuális ügynök telepítés ellenőrzése. |

## <a name="backup"></a>Biztonsági másolat

| A biztonsági mentés | Részletek | Megoldás: |
| -------- | -------- | -------|
| Biztonsági másolat | Nem lehet kommunikálni a virtuális agent pillanatkép státuszt. Pillanatkép virtuális sub tevékenység időtúllépési. -Kérjük, tekintse át a hibaelhárítási útmutatója megoldásához. | Ezt a hibát elő, ha a virtuális Agent probléma, vagy valamilyen módon a hálózati hozzáférés az Azure infrastruktúra zárolva van. További tudnivalók a [virtuális pillanatkép felfelé hibakeresési problémák](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Ha a virtuális agent nem okoz kapcsolatos problémák megoldásához, majd indítsa újra a virtuális. Időnként helytelen virtuális állapotba problémákat okozhatják, és indítsa újra a virtuális visszaállítja az ebben a "rossz állapotban" |
| Biztonsági másolat | Biztonsági másolat belső hiba miatt sikertelen volt – a művelet néhány perc múlva próbálkozzon újra. Ha a probléma továbbra is fennáll, lépjen kapcsolatba a Microsoft Support | Ellenőrizze, ha van-e egy ideiglenes (tranziens) hiba a virtuális tároló elérése. Ellenőrizze az [Azure állapot](https://azure.microsoft.com/en-us/status/) van-e a tartományban lévő számítási/tároló/hálózati kapcsolatos bármilyen a folyamatban lévő problémát. Ellenőrizze ismét a biztonsági másolat bejegyzés probléma szünteti meg. |
| Biztonsági másolat | A művelet nem hajtható végre, virtuális már nem létezik. | Biztonsági másolat nem hajtható végre, mivel a biztonsági másolat konfigurált virtuális törölve lett. További másolatok leállításához Ugrás védett elemek megtekintése, jelölje be a védett elemekhez, és kattintson a Dokumentumvédelem kikapcsolása gombra. Adatok segítségével megőrizheti a biztonsági megőrzi az adatok lehetőség választásával. Később folytathatja védelem a virtuális számítógéphez, kattintson a konfigurálás védelem regisztrált elemek nézetből|
| Biztonsági másolat | Nem sikerült telepítenie az Azure helyreállítási szolgáltatások a a kijelölt elem – virtuális ügynök kiterjesztés Azure helyreállítási szolgáltatások bővítmény előzetesen szükséges. Telepítse az Azure virtuális ügynök, majd indítsa újra a regisztrációs művelet | <ol> <li>Annak ellenőrzése, ha a virtuális agent megfelelően telepítve. <li>Győződjön meg arról, hogy a virtuális config a jelölő megfelelően van-e beállítva.</ol> [További információ:](#validating-vm-agent-installation) virtuális ügynök telepítésről, és hogyan kell a virtuális ügynök telepítés ellenőrzése. |
| Biztonsági másolat | Nem sikerült - parancs végrehajtása egy másik művelet jelenleg folyamatban van az elemre. Kis türelmet, amíg az előző művelet befejeződik, majd próbálkozzon újra | Egy meglévő biztonsági másolat vagy a virtuális visszaállítási feladat fut, és az új feladat nem indítható el a meglévő feladat futtatása közben. |
| Biztonsági másolat | Bővítmény telepítése nem sikerült a "COM + nem tudott felvegye a Microsoft Distributed tranzakciókoordinátorral | Ez általában azt jelenti, hogy nem fut a COM + szolgáltatás. Forduljon segítségért a Microsofthoz a problémák orvoslására: Ez a probléma. |
| Biztonsági másolat | Pillanatkép művelet nem sikerült a VSS művelet "meghajtó zárolt a BitLocker titkosítással. A meghajtó Vezérlőpultról kell zárolásának feloldása | Az összes meghajtón lévő a virtuális BitLocker kikapcsolása, és tekintse meg az a VSS probléma megoldódott |
| Biztonsági másolat | Nem támogatott prémium tárterület-on tárolt virtuális merevlemezeken problémákat virtuális gépeken futó biztonsági mentése | Nincs lehetőség |
| Biztonsági másolat | Azure virtuális gép nem található. | Ez történik, ha az elsődleges virtuális törlődik, de a biztonsági másolat házirend keresse meg a biztonsági másolat virtuális továbbra is. Ez a hiba kijavításához: <ol><li>Hozza létre a virtuális készülék ugyanazt a nevet és egy erőforrás csoportnévre [felhőalapú szolgáltatás neve], <br>(VAGY) <li> Tiltsa le a védelmet a virtuális, hogy a későbbi biztonsági másolatok nem első indított. </ol> |
| Biztonsági másolat | Virtuális gép ügynök nem szerepel a virtuális gépen -, telepítse a szükséges előzetesen szükséges virtuális ügynök, és indítsa újra a műveletet. | [További információ:](#vm-agent) virtuális ügynök telepítésről, és hogyan kell a virtuális ügynök telepítés ellenőrzése. |

## <a name="jobs"></a>Feladatok
| Művelet | Részletek | Megoldás: |
| -------- | -------- | -------|
| Feladat megszakítása | A lemondás nem támogatott a projekt típusa – kis türelmet, amíg a feladat befejezése után. | Nincs lehetőség |
| Feladat megszakítása | A feladat nem törölhető állapotban-as-kis türelmet, amíg a feladat befejezése után. <br>VAGY<br> A kijelölt feladat nem törölhető állapotban-as-várja meg a feladat befejezéséhez.| A projekt minden valószínűség szerint, szinte befejeződött; kis türelmet, amíg a feladat befejezése után |
| Feladat megszakítása | A feladat nem vonható vissza, mert nincs folyamatban - lemondás csak a feladatokat, amely a folyamatban lévő támogatott. Kérjük, a kísérlet a egy folyamatban lévő megszüntetése feladatot. | Ez történik, átmeneti állapot miatt. Várja meg a percet, és újra a Mégse műveletet |
| Feladat megszakítása | A feladat megszakítása - Kis türelmet, amíg a feladat befejezése sikertelen volt. | Nincs lehetőség |


## <a name="restore"></a>Visszaállítása
| Művelet | Részletek | Megoldás: |
| -------- | -------- | -------|
| Visszaállítása | Nem sikerült visszaállítani felhő belső hiba | <ol><li>DNS-beállítások van beállítva, amelyhez visszaállítani kívánt felhőszolgáltatásba. Érdemes <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-tárolóhely "Gyártási" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ha címmel, ez azt jelenti, hogy a DNS-beállítások vannak-e beállítva.<br> <li>Fenntartott IP, amelyre visszaállítani kívánt felhőszolgáltatásba van beállítva, és felhőszolgáltatásában már meglévő VMs leállítva állapotban vannak.<br>Ellenőrizheti, hogy egy felhőalapú szolgáltatásba IP lefoglalt alábbi powershell-parancsmagok használatával:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-"Gyártás" $tárolóhely DEP ReservedIPName <br><li>Ha vissza szeretne állítani egy virtuális számítógépre, az alábbi speciális hálózati beállításokat ugyanazon felhőszolgáltatásba próbálja. <br>-Virtuális gépeken futó betöltés terheléselosztó konfigurációja (belső és külső)<br>-Virtuális gépeken futó a több fenntartott IP-címei<br>-Virtuális gépeken futó több együtt<br>Kérjük, olvassa el az [Visszaállítás előtt megfontolandó kérdések](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) VMs különleges hálózati konfigurációjának, vagy jelöljön ki egy új felhőszolgáltatásba a felhasználói felület</ol> |
| Visszaállítása | A kijelölt tartománynév már foglalt - adjon meg egy másik DNS nevet, és próbálkozzon újra. | A tartománynév hivatkozik a felhőalapú szolgáltatás neve (általában végződésű. cloudapp.net). Ez a szükséges egyedinek kell lennie. Ha ezt a hibát, meg kell egy másik virtuális nevet válassza a visszaállítás során. <br><br> Ez a hiba csak azokra a felhasználókra az Azure portál jelenik meg. A visszaállítás Powershellen keresztül sikeres lesz, mert csak visszaállítja a lemezt, de nem hoz létre a virtuális. A hiba lesz projektvezetők, létrehozásakor a virtuális kifejezetten úgy, hogy a lemez visszaállítási művelet után. |
| Visszaállítása | A megadott virtuális hálózati konfigurálásról nem helyes - adjon meg egy másik virtuális hálózat konfigurálása és próbálkozzon újra. | Nincs lehetőség |
| Visszaállítása | A megadott felhőalapú szolgáltatást használ egy fenntartott IP, amely nem felel meg a visszaállítandó virtuális gép konfigurációja - adjon meg egy másik felhőalapú szolgáltatásba, amely nem használ fenntartott IP, vagy válasszon másik helyreállítási pontot szeretné visszaállítani. | Nincs lehetőség |
| Visszaállítása | Felhőbeli szolgáltatástól kapott beviteli végpontjait száma korlátozott – valamelyik másik felhőszolgáltatásában megadásával, illetve meglévő zárólap használatával, ismételje meg a műveletet. | Nincs lehetőség |
| Visszaállítása | Biztonsági másolat tárolóból elemre, és a cél tárterület-fiókot a két különböző régiók – győződjön meg arról, hogy a visszaállítási művelet megadott tárterület-fiók a biztonsági másolat tárolóra azonos Azure régióban. | Nincs lehetőség |
| Visszaállítása | Tárterület-fiók megadott a visszaállítási művelet nem támogatott - csak Basic/Standard tároló fiókok helyileg felesleges vagy geo felesleges replikációs beállításai támogatottak. Jelöljön ki egy támogatott tárterület-fiókkal | Nincs lehetőség |
| Visszaállítása | Nem érhető el a tárhely fiók-visszaállítási művelet megadott típusú - győződjön meg arról, hogy online állapotban-e a visszaállítási művelet megadott tárterület-fiók | Ez akkor fordulhat Azure-tárolóban lévő, vagy egy üzemszünetek miatt tranziens hiba miatt. Válasszon egy másik tárterület-fiókot. |
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

- Telepítse a legújabb [Linux ügynök](https://github.com/Azure/WALinuxAgent) github.
- [A virtuális tulajdonság frissítéséhez](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) jelzi, hogy telepítve van-e a agent.


### <a name="updating-the-vm-agent"></a>A virtuális Agent frissítése
A Windows VMs:

- Frissítés a virtuális Agent akkor egyszerűen, a [virtuális ügynök bináris](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)újratelepítése. Azonban kell ellenőrizze, hogy nincs biztonsági mentést fut a virtuális Agent frissítése közben.

A Linux VMs:

- A megjelenő útmutatást követve [Linux virtuális ügynök frissítése](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Virtuális ügynök telepítési érvényesítése
Útmutató a Windows VMs virtuális Agent verziójának ellenőrzése:

1. Jelentkezzen be az Azure virtuális géphez, és keresse meg azt a mappát, *C:\WindowsAzure\Packages*. Keresse meg a bemutató WaAppAgent.exe fájlt.
2. Kattintson a jobb gombbal a fájlra, válassza a **Tulajdonságok parancsot**, és válassza a **Részletek** fülre. A termék verziója írjuk 2.6.1198.718 vagy újabb





