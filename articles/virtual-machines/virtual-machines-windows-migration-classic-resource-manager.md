<properties
    pageTitle="Áttelepítési platform támogatott IaaS forrásai a klasszikus az Azure erőforrás-kezelő |} Microsoft Azure"
    description="Az alábbiakban ismertetjük az erőforrások platform támogatott áttelepítést a klasszikus az Azure erőforrás-kezelő"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Áttelepítési platform támogatott IaaS forrásai a klasszikus az Azure erőforrás-kezelő

Ez a cikk azt írja le hogyan azt is engedélyezése erőforrásként egy szolgáltatást (IaaS) klasszikus az erőforrás-kezelő telepítési levő infrastruktúra áttelepítési. Erről további információ az [erőforrás-kezelő Azure funkcióit és előnyeit](../azure-resource-manager/resource-group-overview.md). Csatlakozás erőforrások virtuális hálózati hely közötti átjárók használatával az előfizetés párhuzamosan két telepítési modellekből részletesen azt. 

## <a name="goal-for-migration"></a>Az áttelepítés cél

Erőforrás-kezelő lehetővé teszi, hogy a sablonok között összetett alkalmazások üzembe helyezése, konfigurálja a virtuális gépeken futó virtuális bővítmények használatával, és ezzel a paranccsal beépíti a hozzáférés-kezelés és a címkézési. Erőforrás-kezelő Azure virtuális gépeken futó elérhetőség készletben méretezhető, a párhuzamos telepítésének tartalmazza. Az új telepítési modell is tartalmaz életciklus-kezelése számítási, hálózati és tároló egymástól függetlenül. Végezetül van a fókusz a biztonság engedélyezése a virtuális gépeken futó virtuális hálózatban végrehajtásának alapértelmezés szerint.

A klasszikus telepítési modellből szinte minden szolgáltatások számítási, a hálózati és a tárolás területen Azure erőforrás-kezelő támogatott. Az új funkciók az Azure erőforrás-kezelő összekapcsolhatók, áttelepítheti meglévő telepítések a klasszikus telepítési modellből.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Az automatizálási és az áttelepítés után szerszámok módosítása

Az erőforrások áttérés a klasszikus telepítési modell az erőforrás-kezelő telepítési modellhez részeként, frissítenie kell a meglévő automatizálási vagy szerszámok annak érdekében, hogy továbbra is működnek az áttelepítés után.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Az áttelepítés IaaS erőforrások klasszikus az erőforrás-kezelő jelentése

Ahhoz, hogy részletesen megjelenítsen a részletek, a különbség az adatok síkból és kezelési síkból műveletek IaaS erőforrások között nézzük meg.

- *Kezelő sík* ismerteti a a kezelő sík vagy az erőforrások módosításához az API mappába érkező hívások. Például a műveletek, például egy virtuális létrehozása, egy virtuális újraindítása és az új alhálózat virtuális hálózat frissítése a futó erőforrások kezelésére. Nem közvetlenül befolyásolják példányok csatlakozik.
- *Adatok sík* (alkalmazás) az alkalmazás, magát a futtatókörnyezet ismerteti, és jár, hogy ne az Azure API példányok interakció. A webhely elérése vagy adatok futó SQL Server-példányt vagy MongoDB kiszolgáló adatait szeretné tekinteni a adatok sík vagy alkalmazás kapcsolati. Blob másolása egy tárterület-fiókból, és egy nyilvános IP-címet RDP vagy SSH be a virtuális gép elérése is adatok síkban. Ezek a műveletek nyomon számítási, a hálózathasználatra és a tárhely végig az alkalmazást.

>[AZURE.NOTE] Áttelepítési bizonyos esetekben az Azure platform leállítja felszabadítja és újraindul a virtuális gépeken futó. Ez a egy rövid adatok síkból legrövidebb leállás vonz.

## <a name="supported-scopes-of-migration"></a>Az áttelepítés támogatott hatókörök

Vannak olyan három áttelepítési hatókörök városrész elsősorban számítási, hálózati és tárolását. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>A virtuális gépeken futó (nem a virtuális hálózat) áttelepítését

Az erőforrás-kezelő telepítési modell biztonsági jön létre, mint az alkalmazások alapértelmezés szerint. Az összes VMs kell egy virtuális hálózaton, az erőforrás-kezelő modell. Az Azure platform újraindul (`Stop`, `Deallocate`, és `Start`) a VMs az áttelepítés részeként. A virtuális hálózatok két lehetőség közül választhat:

- Hozzon létre egy új virtuális hálózatot, és a virtuális gép áttelepítéséhez az új virtuális hálózatba a platform kérhet.
- A virtuális gép hálózatba egy meglévő virtuális az erőforrás-kezelő telepítheti át.

>[AZURE.NOTE] A áttelepítési hatókör management síkból műveletek és az adatok síkból műveleteket is előfordulhat, hogy nem engedélyezett az áttelepítés során ideje.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>A virtuális gépeken futó (a virtuális hálózat) áttelepítését

A legtöbb virtuális konfigurációk esetén csak a metaadatok áttelepíti a hagyományos és erőforrás-kezelő telepítési modellek között. Az alapul szolgáló VMs ugyanabba a hálózatba, és az azonos adathordozós azonos hardveren futnak. Az adatkezelési síkból műveletek nem engedélyezhető az áttelepítés során bizonyos ideig. Az adatok sík azonban továbbra is, a munkát. Ez azt jelenti, hogy a fölött (klasszikus) VMs futó alkalmazások nem merülnek fel legrövidebb leállás az áttelepítés során.

Az alábbi beállításokat jelenleg nem támogatott. Ha támogatása a jövőben az egyes VMs ebben a konfigurációban előfordulhat, hogy merülnek fel legrövidebb leállás (Ugrás a Leállítás, keresztül felszabadítása, majd indítsa újra a virtuális műveletek).

-   Ha egynél több elérhetőségének beállítása a egy egy felhőalapú szolgáltatásba.
-   Van egy vagy több elérhetőségének beállítása és a VMs, amelyek nem szerepelnek az elérhetőség egy felhőalapú szolgáltatás beállítása.

>[AZURE.NOTE] A áttelepítési hatókör a kezelő sík előfordulhat, hogy nem engedélyezett az áttelepítés során ideje. Az egyes beállítások ismertetett módon, adatok síkból legrövidebb leállás fordul elő.

### <a name="storage-accounts-migration"></a>Tárterület fiókok áttelepítése

Ahhoz, hogy az áttelepítési gördülékeny, erőforrás-kezelő VMs telepítheti klasszikus tárterület-fiókjában. Ezt a lehetőséget a számítási és a hálózati erőforrások is, és függetlenül tároló fiókok kell áttelepíteni. Miután a virtuális gépeken futó és virtuális hálózati áttelepítés kell áttelepíteni a tárterület-fiókok elvégzéséhez az áttelepítési folyamatot. 

>[AZURE.NOTE] Az erőforrás-kezelő telepítési modell, amely a klasszikus képek és a lemez nem tartozik. Amikor a tárterület-fiók áttelepített, klasszikus képek és lemezt nem jelennek meg az erőforrás-kezelő egymást fedő, de biztonsági VHD marad a tárterület-fiókjában. 

## <a name="unsupported-features-and-configurations"></a>Nem támogatott funkciók és beállítások

Hogy jelenleg nem támogatja egyes funkciói és beállításokat. Az alábbi szakaszok ismertetik a beállítására vonatkozó javaslatok őket.

### <a name="unsupported-features"></a>Nem támogatott szolgáltatások

Az alábbi funkciók jelenleg nem támogatott. Tetszés szerint távolítsa el ezeket a beállításokat, a VMs áttelepítése és ismételt engedélyezése a beállításokat, az erőforrás-kezelő telepítési modell.

Erőforrás-szolgáltató | A szolgáltatás
---------- | ------------
Számítási | Társítás nélküli virtuális gép lemezt.
Számítási | Virtuális gép képek.
Hálózati | Végpont hozzáférés-vezérlési listák.
Hálózati | Virtuális hálózati átjárók (webhelyre, Azure készült ExpressRoute, alkalmazás átjárót, mutasson a webhely).
Hálózati | Virtuális hálózatok VNet Peering használatával. (VNet áttelepítése ARM, majd peer) További tudnivalók a [VNet Peering] (... /Virtual-Network/Virtual-Network-peering-overview.MD).
Hálózati | Adatforgalom Manager profilokat.

### <a name="unsupported-configurations"></a>Nem támogatott beállításokat

Az alábbi beállításokat jelenleg nem támogatott.

Szolgáltatás | Konfiguráció | Javaslat
---------- | ------------ | ------------
Erőforrás-kezelő | Szerepkör alapján Access vezérlőelem (RBAC) klasszikus erőforrások | Az erőforrások URI az áttelepítés után módosul, mert javasolt, hogy a RBAC házirend-frissítések, fordulhat elő, az áttelepítés után kell használni.
Számítási | A virtuális társított több alhálózat | Frissítse a alhálózat konfiguráció csak alhálózat hivatkozni szeretne.
Számítási | Virtuális gépeken futó, amely egy virtuális hálózati tartoznak, de nincs egy explicit alhálózat rendelt | A virtuális tetszés szerint törölheti.
Számítási | Értesítések, Automatikus méretezéssel házirendek rendelkező virtuális gépeken futó | Az áttelepítés megy keresztül, és ezeket a beállításokat megszakadnak. A környezet felmérése előtt a művelet az áttelepítés előtt erősen ajánlott. Másik lehetőségként a riasztási beállítások átkonfigurálása, áttelepítés befejeződése után.
Számítási | XML-virtuális bővítmények (BGInfo 1.*, Visual Studio Debugger, webhely üzembe és távoli hibakeresés) | Nem támogatott. Javasoljuk, hogy ezek a bővítmények áttelepítési továbbra is a virtuális gép eltávolítása vagy őket a program eltávolítja a automatikusan során az áttelepítési folyamatot.
Számítási | Indítási diagnosztika prémium adathordozós | Áttelepítés a továbblépés előtt tiltsa le a VMs indítási diagnosztika szolgáltatást. Az áttelepítés befejeződése után újra engedélyezheti az erőforrás-kezelő egymást fedő indítási diagnosztika. Emellett az a és a soros naplók használt BLOB törölni kell így már nem az előfizetést terhelő e BLOB.
Számítási | Webes/dolgozói szerepkörök tartalmazó felhőszolgáltatások | Ez jelenleg nem támogatott.
Hálózati | Virtuális gépeken futó és webes/dolgozó szerepkörök tartalmazó virtuális hálózatok |  Ez jelenleg nem támogatott.
Azure alkalmazás szolgáltatás | Alkalmazás környezetek tartalmazó virtuális hálózatok | Ez jelenleg nem támogatott.
Azure hdinsight szolgáltatáshoz | Virtuális hálózatok, amelyeknél a HDInsight-szolgáltatások | Ez jelenleg nem támogatott.
A Microsoft Dynamics életciklus-szolgáltatások | Virtuális gépeken futó Dynamics életciklus-szolgáltatások által kezelt tartalmazó virtuális hálózatok | Ez jelenleg nem támogatott.
Számítási | Azure biztonság otthon bővítmények egy VNET, amely tartalmaz egy virtuális Magánhálózati átjáró vagy ER átjáró a helyszíni DNS-kiszolgálóval együtt | Azure biztonság otthon bővítmények automatikusan telepíti a virtuális gépeken figyelheti a biztonság és értesítések előléptetése. Ezek a bővítmények általában első települ automatikusan, ha az Azure biztonság otthon házirend engedélyezve van az előfizetést. Átjáró áttelepítési jelenleg nem támogatott, és az átjáró kell folytatása elvégzése az áttelepítés előtt meg kell hagyni, az interneten keresztüli elérését virtuális tárterület-fiók nem vesznek el, az átjáró törlésekor. Az áttelepítés nem folytatódik, amikor ez történik, mint a vendégként való bekapcsolódáshoz ügynök állapot blob nem töltik. Azure biztonság otthon házirend az előfizetéshez tartozó 3 óráig áttelepítési folytatása letiltása ajánlott.

## <a name="the-migration-experience"></a>Az áttelepítési felületen

Előzetes teendők az áttelepítés felületen, az alábbi ajánlott:

- Biztosíthatja, hogy az erőforrásokat, az áttelepítendő nem nem támogatott szolgáltatások vagy a beállításokat. Általában a platform észleli ezeket a problémákat, és hibát jelez.
- Ha VMs, amelyek nem egy virtuális hálózaton van, azok leáll és felszabadítása a előkészítése során. Ha nem szeretné a nyilvános IP-cím. elvesznek, akkor vizsgálja meg a lefoglalására IP-címét az előkészítés művelet elindítása előtt. Jó helyen jár Ha a VMs egy virtuális hálózaton, ezeket nem leállt és felszabadítása.
- Tervezze meg az áttelepítés során nem-munkaidő az áttelepítés során fordulhat váratlan hibák igazodik.
- Töltse le a VMs szoftver jelenlegi konfigurációjának megfelelően, így azok könnyebben adatérvényesítéshez az előkészítés lépés befejezése után PowerShell, a parancssori kezelőfelületről parancsok vagy a REST API-khoz használatával.
- Frissítse az automatizálási/operationalization parancsfájlokat az erőforrás-kezelő telepítési modell kezelheti, az áttelepítés megkezdése előtt. Másik lehetőségként tehet GET műveletek, ha az erőforrások az elkészített állapotban.
- Kiértékelésének eredménye RBAC házirendek, és az áttelepítés befejeződése után megtervezése a klasszikus IaaS erőforrások van beállítva.

Munkafolyamat áttelepítése a a következőképpen van

![Képernyőkép, melyen a munkafolyamat áttelepítése](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Az alábbi szakaszokban ismertetett az összes művelet idempotent. A probléma nem nem támogatott funkciók vagy konfigurációs hiba esetén javasoljuk, hogy meg újra az előkészítés, megszakítása vagy művelet végrehajtása. Az Azure platform próbálja meg ismét a műveletet.

### <a name="validate"></a>Ellenőrzése

Az érvényesítési művelet első lépése a az áttelepítési folyamatot. Ezt a lépést az a célja, hogy a háttérben fut az áttelepítési az erőforrásokhoz tartozó adatok elemzése és adnak a sikeres és sikertelen, ha az erőforrások áttelepítési képesek.

Jelölje ki a virtuális hálózat vagy a szolgáltatott szolgáltatást (Ha még nem egy virtuális hálózati), hogy az áttelepítési érvényesíteni szeretné.

* Ha az erőforrás nem alkalmas az áttelepítés, az Azure platform felsorolja a miért nem támogatott az áttelepítés az okok miatt.

### <a name="prepare"></a>Előkészítése

Az előkészítés művelet, a második lépés az áttelepítési folyamatot. Ebben a lépésben célja, hasonlóan a IaaS erőforrások klasszikus erőforrás-kezelő erőforrások az átalakítás, és ossza meg, ezt egymás mellett a képi megjelenítését.

Jelölje ki a virtuális hálózat vagy a szolgáltatott szolgáltatást (Ha még nem egy virtuális hálózati), hogy szeretne-e előkészítése az áttelepítési.

* Ha az erőforrás nem alkalmas az áttelepítés, az Azure platform leállítja az áttelepítési folyamatot, és megjeleníti az az oka, miért nem sikerült az előkészítés művelet.
* Az áttelepítés alkalmas az erőforrás esetén a az Azure platform első zárolások lefelé a felügyeleti síkból műveleteket áttelepítési forrásokra. Ha például Ön nem egy virtuális az áttelepítési adatok lemezen adni.

Az Azure platform megkezdi az áttelepítés metaadatok klasszikus az erőforrás-kezelő áttelepítése erőforrás.

Az előkészítés művelet befejezése után meg, hogy az erőforrások mindkét klasszikus megjelenítése és erőforrás-kezelő. A klasszikus telepítési modell minden felhőszolgáltatásba, az Azure platform hoz létre egy erőforrás csoport neve, amelyen a mintázatot `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtuális gépeken futó, amelyek nem szerepelnek a klasszikus virtuális hálózati áttelepítés a fázis deallocated vannak leállt.

### <a name="check-manual-or-scripted"></a>Jelölje be a (kézi vagy parancsfájl)

Az ellenőrzés lépésben másik lehetőségként használhatja a beállításokat, amelyek korábban letöltött ellenőrzése, hogy az áttelepítés megjelenése megfelelő. Másik lehetőségként jelentkezhet be a portál és a helyszíni be tulajdonságokat és erőforrások ellenőrzéséhez, hogy a metaadat-alapú áttelepítési jó formátumban.

Ha egy virtuális hálózat, a legtöbb konfigurálása virtuális gépeken futó nem újraindítása. Az adott VMs-alkalmazásokkal ellenőrizheti, hogy az alkalmazás már továbbra is működik.

A figyelés és automatizálási és műveleti parancsfájlok megjelenítéséhez, ha a VMs a várt módon működnek, és a frissített parancsfájlok megfelelően működjön tesztelheti. Csak GET-műveletek támogatottak, ha az erőforrások az elkészített állapotban.

Nincs beállítása időkeret, amelyik elé kell az áttelepítés végrehajtása nem. Minél több időt, ez az állapot a kívánt módon is igénybe vehet. Jó helyen jár a kezelő sík zárolva ezek az erőforrások mindaddig, amíg a megszakítása vagy végrehajtása.

Ha látható kapcsolatos problémák megoldásához, mindig elveti az áttelepítés és visszatérhet a klasszikus telepítési modell. Után vissza, az Azure platform a források kezelése síkból műveleteket nyílik meg, hogy ezek a klasszikus telepítési modell VMs szokásos műveletek folytathatja.

### <a name="abort"></a>Megszakítása

Megszakítás (nem kötelező) a módosításokat a klasszikus telepítési modellhez visszavált, és az áttelepítés vége használható.

>[AZURE.NOTE] Ez a művelet nem hajtható végre, amelyen van a jóváhagyás művelet után.  

### <a name="commit"></a>Jóváhagyás

Miután végzett az érvényesítési, az áttelepítés is véglegesítése után. Erőforrások klasszikus többé nem látható, és csak az erőforrás-kezelő telepítési modell érhető el. Csak az új portálon kezelheti az áttelepített erőforrásokat.

>[AZURE.NOTE] Ez a művelet idempotent. Ha nem sikerül, ajánlott, ismételje meg a műveletet. Ha meghiúsító továbbra is, hozzon létre egy támogatási jegyek, vagy fórum közzéteheti a [virtuális fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)ClassicIaaSMigration címkével ellátott.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

**A áttelepítési terv befolyásolja a meglévő szolgáltatások és Azure virtuális gépeken futó alkalmazások?**

nem. A (klasszikus) VMs általános elérhetősége teljesen támogatott szolgáltatások. Bontsa ki a a Microsoft Azure helyigénye használja az alábbi forrásokat is.

**Mi történik a VMs, ha lehet a közeljövőben áttelepítése a jövőben sem tervezek?**

Azt nem elavulttá, a meglévő klasszikus API-k és az erőforrás típusát. Szeretnénk, hogy az áttelepítési egyszerű, figyelembe véve a elérhető az erőforrás-kezelő telepítési modell speciális funkciókhoz. Azt javasoljuk, hogy tekintse át [az fejlesztések részét](virtual-machines-windows-compare-deployment-models.md) IaaS az erőforrás-kezelő részét képező.

**Mi a áttelepítési terv jelent a meglévő szerszámok?**

Az erőforrás-kezelő telepítési modell a szerszámok frissítése az egyik legfontosabb változásairól az áttelepítési tervezésre számlája beállított.

**Mennyi ideig a kezelés síkból legrövidebb leállás lesznek?**

A hozzárendelt erőforrások, amely áttelepítését a rendszer függ. A telepítési (a VMs néhány tízesre) kisebb a teljes áttelepítési kisebb, mint egy óra kell vennie. Nagyméretű telepítési (VMs több száz) az áttelepítés néhány óráig is tarthat.

**Vissza a az áttelepítés erőforrások vannak lekötött erőforrás-kezelő után is vetítődnek?**

Az áttelepítési megszakítható mindaddig, amíg az erőforrások készített állapotban van. A visszaállítás után az erőforrások sikeresen áttelepítette keresztül a jóváhagyás művelet nem támogatott.

**Is lehet állítsa vissza az áttelepítési a jóváhagyás művelet sikertelen, ha?**

Áttelepítési nem állítható le, ha a jóváhagyás művelet sikertelen lesz. Az összes áttelepítési műveletek, többek között a jóváhagyás művelet idempotent. Ezért azt javasoljuk, hogy a rövid idő múlva próbálkozzon újra. Továbbra is arcra hibát, ha a támogatási jegyek létrehozása, vagy fórum közzéteheti a [virtuális fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)az ClassicIaaSMigration címkével ellátott.

**Van egy másik express útvonal áramkör megvásárlására, az erőforrás-kezelő IaaS használata esetén?**

nem. A legutóbb engedélyezett [az erőforrás-kezelő telepítési adatmodellhez klasszikus készült ExpressRoute áramkörök áthelyezése](../expressroute/expressroute-move.md). Nem kell vásárolni egy új készült ExpressRoute áramkör, ha már van egy.

**Mi történik, ha a klasszikus IaaS erőforrások szerepköralapú hozzáférés-vezérlés házirendek volna beállítása?**

Áttelepítés során az erőforrások átalakíthatja a klasszikus az erőforrás-kezelő. Tehát azt javasoljuk, hogy a RBAC házirend-frissítések, fordulhat elő, az áttelepítés után kell használni.

**Mi történik, ha használok Azure webhely helyreállítás vagy az Azure biztonsági másolat ma?**

A biztonsági másolat, lásd: [engedélyezett virtuális gép áttelepítendő lehet biztonsági másolatot készített a klasszikus VMs a biztonsági másolat tárolóból elemre. Most már lehet át szeretne térni a VMs a Klasszikus módú erőforrás-kezelő módra. Hogyan lehet biztonsági másolatot készíteni őket a helyreállítási szolgáltatások tárolóból elemre?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Az előfizetés vagy a források megjelenítéséhez, ha azokat is alkalmas az áttelepítés is ellenőrzése?**

igen. Az áttelepítési platform támogatott választógombot az első előkészítése az áttelepítési lépésként ellenőrzéséhez, hogy az erőforrások alkalmas az áttelepítés. Abban az esetben, ha nem sikerül az érvényesítés művelet, üzeneteket fogadni az áttelepítés nem fejeződhet be okok miatt.

**Mi történik, ha a lehet közben fellépő esetleges kvóta hiba IaaS erőforrások Felkészülés az áttelepítés?**

Azt javasoljuk, hogy elveti az áttelepítési, és jelentkezzen be a hol telepít át a VMs régióban kvóták növeléséhez támogatási kérelmet. Miután a kvóta kérelem jóváhagyása, indítsa el újra a az áttelepítési lépések végrehajtása.

**Hogyan lehet jelenteni az problémát?**

Tartalmakat tehet közzé a problémák és áttelepítési kapcsolatos kérdésekre a [virtuális fórumát](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), a kulcsszó ClassicIaaSMigration. Azt javasoljuk, hogy a felmerülő kérdésekre könyvelési e fórumon. Ha a támogatási szerződés, Ön Üdvözöljük, valamint támogatási jegy jelentkezzen.

**Mi történik, ha az áttelepítés során átugrott a platform erőforrásnevek nem szeretném?**

Explicit módon adja meg a klasszikus telepítési modell nevek összes erőforrás tartja meg az áttelepítés során. Egyes esetekben az új erőforrások jönnek létre. Példa: a hálózati kapcsolat minden virtuális jön létre. Jelenleg nem támogatott diagramtípusról segítségével szabályozhatja az áttelepítés során létre új erőforrásokról nevét. Jelentkezzen be az [Azure Visszajelzési fórum](http://feedback.azure.com)ehhez a szolgáltatáshoz a szavazatát.

* *Egy arról *"virtuális közöl az általános ügynök állapot szerint nem áll készen. A virtuális emiatt nem telepíthetők át. Győződjön meg arról, hogy a virtuális Agent jelez készként összesített ügynök állapot"* vagy *"virtuális tartalmaz állapotú nem jelentve a virtuális a bővítmény. Ezért a virtuális nem telepíthetők."***

Ez az üzenet érkezik, amikor a virtuális nem rendelkezik az internethez kimenő kapcsolattal. A virtuális ügynök használja a kimenő kapcsolódási ügynök állapotát öt percenként frissítése a Azure tárterület-fiók elérésére.


## <a name="next-steps"></a>Következő lépések
Most, hogy megismeri klasszikus IaaS erőforrások erőforrás-kezelő az áttelepítést, indítsa el erőforrások áttelepítése.

- [Műszaki mély merülési a platform támogatott áttelepítési a klasszikus az Azure erőforrás-kezelő](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [IaaS erőforrások áttelepítése klasszikus az Azure erőforrás-kezelő a PowerShell használatával](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [IaaS erőforrások áttelepítése klasszikus az Azure erőforrás-kezelő CLI használatával](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klasszikus virtuális gép az Azure erőforrás-kezelő klónozhatja közösségi PowerShell-parancsfájlokat használatával](virtual-machines-windows-migration-scripts.md)
