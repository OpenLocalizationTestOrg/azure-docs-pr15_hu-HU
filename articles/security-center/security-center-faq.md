<properties
   pageTitle="Gyakori kérdések a Azure biztonság otthon |} Microsoft Azure"
   description="Ez a cikk megválaszolja Azure biztonsági központban kérdésekkel."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure biztonság otthon gyakori kérdések

Ez a cikk megválaszolja Azure biztonság otthon, a szolgáltatás, amellyel megelőzésére, feltárására és veszélyek megválaszolása a könnyebb láthatóság érdekében és szabályozni kell a Microsoft Azure erőforrásait biztonsági kérdések.

## <a name="general-questions"></a>Általános kérdések

### <a name="what-is-azure-security-center"></a>Mi az Azure biztonság otthon?
Azure biztonság otthon segítségével megakadályozhatja, hogy észleli és veszélyek megválaszolása a könnyebb láthatóság érdekében és szabályozni kell a biztonsági Azure erőforrásait. Integrált adatvédelem figyelése és a Csoportházirend kezelése a előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

### <a name="how-do-i-get-azure-security-center"></a>Hogyan szerezhető be Azure biztonság otthon?
Azure biztonság otthon engedélyezve van a Microsoft Azure-előfizetéssel, és az [Azure portál](https://azure.microsoft.com/features/azure-portal/)elérhető. ([Bejelentkezés a portálra](https://portal.azure.com), válassza a **Tallózás gombra**, és görgessen a **Biztonság otthon**).  

## <a name="billing"></a>A számlázás

### <a name="how-does-billing-work-for-azure-security-center"></a>Honnan tudja az Azure biztonság otthon számlázási munkát?
A két rétegek felajánlott biztonság otthon: szabad és normál.

A szabad réteg lehetővé teszi, hogy a biztonsági házirendek beállítása és a biztonsági figyelmeztetések, események és javaslatok, amely végigvezeti Önt a folyamaton, a szükséges vezérlőket konfigurálása kapni. A szabad réteg segítségével is figyelheti a Azure erőforrások és Azure-előfizetése integrálódik partnermegoldások biztonsági állapotát.

A szokásos réteg biztosít a réteg ingyenes szolgáltatások plusz speciális észlelési: üzletiintelligencia, viselkedési elemzés, összeomlik elemzés és rendellenességet észlelési veszélyt. A szokásos réteg 90 napos ingyenes próbaverziójának érhető el. Frissíteni, jelölje ki a [Yammer biztonsági házirendje](security-center-policies.md#setting-security-policies-for-subscriptions)árak réteg. [Biztonság otthon árak](security-center-pricing.md) talál további információt.

## <a name="data-collection"></a>Adatok gyűjtése

A virtuális gépeken futó mérje fel, hogy a biztonsági állapotukban, adja meg a biztonsági javaslatok és veszélyek figyelmezteti adatait gyűjti össze a biztonság otthon. Amikor először nyitja meg a biztonság otthon, a adatgyűjtés engedélyezve van, az előfizetése összes virtuális gépeken. Adatgyűjtés ajánlott, de Ön is letiltásra [adatgyűjtés letiltása](#how-do-i-disable-data-collection) a biztonság otthon házirend.

### <a name="how-do-i-disable-data-collection"></a>Hogyan tilthatom le a adatgyűjtés?

**Adatgyűjtés** letilthatja a biztonsági házirendek: az előfizetés bármikor. ([Jelentkezzen be az Azure-portálra](https://portal.azure.com), válassza a **Tallózás gombra**, válassza a **Biztonság otthon**és válassza a **házirend**.)  Amikor kijelöl egy előfizetés, egy új lap megnyílik, és **adatgyűjtés** kikapcsolása lehetőséget nyújt. Jelölje ki a felső menüszalag ügynökök eltávolítása a meglévő virtuális gépeken futó a **ügynökök törlése** lehetőséget.

> [AZURE.NOTE] Eszközbiztonsági házirendek beállítható, hogy az erőforrás csoportszint és Azure előfizetés szintű, de ki kell választania-előfizetést adatgyűjtést kikapcsolása.

### <a name="how-do-i-enable-data-collection"></a>Hogyan engedélyezhetem az adatok gyűjtése?
Adatgyűjtés az Azure előfizetés(ek) a biztonsági házirendben engedélyezheti. Ahhoz, hogy az adatok gyűjtése, a [bejelentkezési az Azure-portálra](https://portal.azure.com), válassza a **Tallózás gombra**, válassza a **Biztonság otthon**, és válassza a **házirend**. **Adatgyűjtés** állítsa **meg** és állítsa be a tárterület-fiókok, amelyre adatokat szeretne gyűjteni (lásd: a kérdés "[az adatok tároló?](#where-is-my-data-stored)"). Ha engedélyezve van a **adatgyűjtés** , azt automatikusan gyűjti össze a biztonsági konfiguráció és esemény információk összes támogatott virtuális gépeken futó az előfizetés.

> [AZURE.NOTE] Eszközbiztonsági házirendek beállítható, hogy az erőforrás csoportszint és Azure előfizetés szintű, de adatgyűjtés konfigurálása csak az előfizetés szinten fordul elő.

### <a name="what-happens-when-data-collection-is-enabled"></a>Mi történik, ha engedélyezve van a adatgyűjtés?
Adatgyűjtés az Azure figyelése ügynök és az Azure biztonsági figyelése bővítmény keresztül érhető el. Az Azure biztonsági figyelése bővítmény keres a különböző biztonsági vonatkozó beállításokat, és elküldi [Eseményvezérelt Tracing Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (esemény-nyomkövetés) nyomkövetést be. Ezeken kívül az operációs rendszer létrehozza a eseménynaplójának tételeket.  Az Azure figyelése ügynök felolvassa eseménynaplójának bejegyzései, és esemény-nyomkövetés követi nyomon, és másolja át az őket tároló analysis fiókjára.  Ez az a tárterület-fiókot, a biztonsági házirendben beállított. A tárterület-fiókkal kapcsolatos további tudnivalókért lásd: a kérdés "[az adatok tároló?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>A figyelés ügynök vagy a biztonsági figyelése kiterjesztésű hatással a kiszolgálókat teljesítményét?
A ügynök kiterjesztése a névleges mennyiségű rendszer-erőforrásokat fogyaszt és kell rendelkeznie a kis hatást a teljesítményre.

### <a name="where-is-my-data-stored"></a>Az adatok tároló?
Az egyes régiókra virtuális gépeken futó operációs rendszert futtató lehetősége van válassza ki az adott virtuális gépeken futó gyűjtött adatokat tároló tárterület-fiókot. Ezzel megkönnyíti az meg szeretné tartani az adatokat az azonos földrajzi terület adatvédelmi és adatok felségterületéhez céljából. A tárhely fiók előfizetéshez válassza a biztonsági házirend. ([Jelentkezzen be az Azure-portálra](https://portal.azure.com), válassza a **Tallózás gombra**, válassza a **Biztonság otthon**és válassza a **házirend**.) Amikor egy előfizetés kattint, egy új lap nyílik meg. Jelölje be a **választ tároló fiókok** terület kiválasztása.

> [AZURE.NOTE] Eszközbiztonsági házirendek beállítható, hogy az erőforrás csoportszint és Azure előfizetés szintű, de csak az előfizetés szinten történik a tárterület-fiók terület kijelölése.

Azure tárterület és tárhely fiókok kapcsolatos további információért lásd: [Tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/) és [kapcsolatos Azure tárterület-fiókok](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Azure biztonsági központ használatával

### <a name="what-is-a-security-policy"></a>Mi az biztonsági házirendek?
Biztonsági házirendek meghatároz a vezérlők, amelyek a megadott előfizetés vagy erőforráscsoport erőforrások ajánlott. Az Azure biztonság otthon a Azure előfizetések és erőforrás-csoportok szerint a vállalat biztonsági követelményeknek, és milyen típusú alkalmazásokat vagy védelmi szintjének az adatokat az egyes előfizetések házirendek definiálhatók.

Például a fejlesztés és tesztelése erőforrásoknak lehetnek különböző biztonsági követelményeknek, mint a termelési alkalmazásokhoz. Hasonlóképpen szabályozott adatok, például az adat (személyes azonosításra) alkalmazások biztonsági magasabb szintű lehetnek szükségesek. A biztonsági házirendek Azure biztonság otthon engedélyezve van a javaslatok biztonsági és a figyeléshez fog meghajtó. Eszközbiztonsági házirendek kapcsolatos további információért lásd: a [biztonsági rendszerállapot figyelése a Azure biztonság otthon](security-center-monitoring.md).

> [AZURE.NOTE] Abban az esetben, ha egy ütközést előfizetés szintű házirend- és erőforrás szintű csoportházirend között az erőforrás-csoport szintű házirend elsőbbséget szemben.

### <a name="who-can-modify-a-security-policy"></a>A biztonsági házirend módosítására jogosult?
Az egyes előfizetés vagy erőforráscsoport biztonsági házirendek beállítása. Biztonsági házirendek: az előfizetés vagy erőforrás csoport szint módosításához-tulajdonos vagy a közös munka az adott előfizetés kell lennie.

Biztonsági házirendek beállítása című témakörben talál [Azure biztonság otthon biztonsági házirendek beállítást](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Mi az biztonsági ajánlást?
Azure biztonság otthon elemzi a biztonsági állapot Azure erőforrásait. Ha megadott potenciális biztonsági rés, javaslatok jönnek létre. A javaslatok végigvezeti Önt a folyamaton, a szükséges vezérlő konfigurálása. Példák:

- Az azonosításához és távolítsa el a kártékony szoftverek antimalware kiépítése
- [Hálózati biztonsági csoportokat](../virtual-network/virtual-networks-nsg.md) és a vezérlőelem-alapú forgalmat virtuális gépeken futó szabályok beállítása
- A webes alkalmazás tűzfal segíti a webalkalmazások kiválasztásával támadások elleni védelmét a kiépítési
- Nyekhttp://www.Office.com/redir/xt103979639 hiányzó rendszer frissítések
- Operációs rendszer beállításokat, amelyek nem felelnek meg a javasolt alaptervek megcímezheti

Csak az engedélyezett biztonsági házirendek javaslatok Itt jelennek meg.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Hogyan jelennek meg az Azure erőforrások aktuális biztonsági állapotát?
Egy **erőforrások állapot** csempe a **Biztonság otthon** lap jeleníti meg az általános biztonsági testtartását a környezet virtuális gépeken futó webalkalmazások és más erőforrások szerinti bontásban. Az egyes erőforrások tartozik egy jelző megjelenítő, ha a semmilyen potenciális biztonsági rés azonosították. Az erőforrások állapot csempére az erőforrások jeleníti meg, és hol figyelmet szükség, és a problémák.

### <a name="what-triggers-a-security-alert"></a>Mi váltja egy biztonsági riasztás?
Azure biztonság otthon automatikusan összegyűjti, elemzi, és az Azure erőforrások, a hálózati és a partnerek megoldásaival például antimalware és a tűzfalak naplóadatok biztosítók. Ha veszélyek észlel, a biztonsági figyelmeztetés jön létre. Az észlelési többek között:

- Biztonságos virtuális gépeken futó ismert rosszindulatú IP-címek kommunikáció
- Speciális kártevőt észlelt, használja a Windows-hibák jelentése
- Ellen támadások elleni virtuális gépeken futó
- Beépített biztonsági partnermegoldások például kártevők elleni vagy a webes alkalmazás tűzfalak a biztonsági figyelmeztetések

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Mi a különbség a talált és a Microsoft Security Response Center és Azure biztonság otthon értesítési szintjének beállításához veszélyek között?
A Microsoft biztonsági válasz központ (Kialakításában) elvégzi a választó biztonsági Azure hálózati és infrastruktúra figyelemmel kísérését, és veszélyt intelligencia és visszaélés panaszok kap külső féltől származó. Ha Kialakításában, hogy az ügyfél adatait egy jogellenes vagy jogosulatlan fél által elérhető-e, illetve, hogy Azure használatát az ügyfélnek nem felel meg a adatokkal elfogadható használja az tudomást, a biztonsági az esemény kezelőjének jelzi az ügyfélnek. Értesítés oka általában az e-mailben küld a megadott Azure biztonság otthon vagy az Azure előfizetés tulajdonosa, ha nincs megadva biztonsági partner biztonsági partnereket.

Biztonság otthon egy Azure szolgáltatás, amely folyamatosan Azure ügyfélkörnyezet figyeli automatikus észlelése az esetlegesen ártalmas tevékenység széles köre analytics vonatkozik. Ezek az észlelési az biztonsági figyelmeztetések a biztonsági központ irányítópult a következőképpen jelenik meg.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Engedélyek kezelésének módja az Azure biztonság otthon?
Azure biztonság otthon szerepköralapú hozzáférés-támogatja. Szerepköralapú hozzáférés-szerepalapú Azure-ban kapcsolatos további információért olvassa el az [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)című témakört.

Amikor a felhasználó megnyitja a biztonság otthon, csak a javaslatok, és kapcsolódó erőforrásokat a felhasználó hozzáfér a riasztások is látható lesz. Ez azt jelenti, hogy a felhasználók csak fogják látni az erőforrásokhoz, ahol a felhasználónak van rendelve a tulajdonos, munkatársi vagy olvasó szerepe az előfizetést, vagy az erőforrás-csoportot, amelyhez az erőforrás tartozik kapcsolódó elemet.

Ha módosítani szeretné:

- **Biztonsági házirendek szerkesztése**, kell lennie egy tulajdonosa, vagy a közös munka az előfizetés.
- **Alkalmazás ajánlást**, a-tulajdonos vagy a közös munka az előfizetés kell lennie.
- **Betekintést kap abba, hogy a biztonsági állapotát az előfizetések összes van**, kell lennie egy tulajdonos, a közös munka vagy az olvasót (informatikai rendszergazda, biztonsági csoport) minden előfizetés.
- **Van betekintést kap abba, hogy az erőforrások biztonsági állapotát**, egy erőforrás csoport tulajdonosának, munkatársi vagy olvasó (DevOps) kell lennie.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure erőforrások vannak felügyeli Azure biztonság otthon?
Azure biztonság otthon figyeli Azure alábbi forrásokat:

- Virtuális gépeken futó (VMs) (beleértve a [Cloud Services](../cloud-services/cloud-services-choose-me.md))
- Azure virtuális hálózatok
- Azure SQL-szolgáltatás
- A partnermegoldások integrálódik az Azure előfizetés, például a webes alkalmazás tűzfalat VMs és [Alkalmazás szolgáltatási környezetben](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuális gépeken futó

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Virtuális gépeken futó típusú lesz támogatott?
Biztonsági állapot ellenőrzése és javaslatok a virtuális gépeken futó (VMs) mind a [hagyományos és erőforrás-kezelő telepítési modellek](../azure-classic-rm.md)alapján létre érhetők el.

A Windows VMs támogatott:

- A Windows Server 2008 R2 rendszerben
- A Windows Server 2012
- A Windows Server 2012 R2

Támogatott Linux VMs:

- Ubuntu verziók 12.04, 14.04, 16.04
- Debian verziók 7, 8
- Verziók 6 centOS. \*, 7.*
- Piros kalap vállalati Linux (RHEL) verziójú 6. \*, 7.*
- SUSE Linux vállalati kiszolgáló (SLES) 11-es verzió. \*, 12.*
- Az Oracle Linux 6 verziók. \*, 7.*

Egy felhőalapú szolgáltatásba futó VMs szintén támogatottak. Csak felhőalapú szolgáltatások webes és dolgozó szerepkörök operációs rendszert futtató gyártási helyek is ellenőrizni. Többet szeretne tudni a felhőbeli szolgáltatástól, hogy a [Cloud Services áttekintése](../cloud-services/cloud-services-choose-me.md)című témakörben találhat.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Miért nem Azure biztonság otthon ismeri fel a antimalware megoldás az Azure virtuális fut?

Azure biztonság otthon csak akkor betekintést kap abba, antimalware keresztül Azure bővítmények telepítve van. Ha például nem észleli antimalware előre telepítve van a megadott képet vagy ha saját folyamatok (például konfigurációs rendszerek) használata a virtuális gépeken antimalware telepítve volt biztonság otthon.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Miért kapok az üzenet "Hiányzik a beolvasás-adatok karakterláncot" a virtuális?

Eltarthat egy kis időt (általában kevesebbet óránként) beolvasás adatok után adatgyűjtés engedélyezve van-e Azure biztonság otthon feltöltéséhez. Ellenőrzés nem fog feltöltése az VMs leállítva állapotba kerül.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Miért kapok az üzenet "Virtuális ügynök az hiányzó?"

A virtuális Agent kell telepíteni kell VMs adatgyűjtés engedélyezése. A virtuális Agent alapértelmezés szerint a a Microsoft Azure piactéren lévő telepített VMs telepítve van. A virtuális Agent telepítése más VMs tudnivalókért tekintse meg az közlemény [virtuális ügynök és bővítmények elemet](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
