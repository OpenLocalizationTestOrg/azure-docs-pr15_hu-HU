<properties
   pageTitle="Azure biztonság otthon – bevezetés |} Microsoft Azure"
   description="Tudjon meg többet az Azure biztonság otthon, a főbb funkciói és működéséről."
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
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Azure biztonság otthon – bevezetés

Tudjon meg többet az Azure biztonság otthon, a főbb funkciói és működéséről.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.

## <a name="what-is-azure-security-center"></a>Mi az Azure biztonság otthon?
 Biztonság otthon segítségével megakadályozhatja, hogy észleli és veszélyek megválaszolása a könnyebb láthatóság érdekében és szabályozni kell a biztonsági Azure erőforrásait. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

##  <a name="key-capabilities"></a>Főbb funkciók
 Biztonság otthon kialakításának megelőzése észlelési és válasz funkcióját biztosítja a könnyen használható és hatékony veszélyt Azure. Főbb funkciók a következők:

| | |
|----- |-----|
| Megakadályozhatja, hogy | A biztonsági állapotát az Azure erőforrások figyelése |
| | Megadja az Azure előfizetések és a vállalat biztonsági igényeknek megfelelően alakítható, az erőforrás-csoportok házirendek használt alkalmazások és az adatok a magánjellegű típusai |
| | Használja a házirend-alapú biztonsági javaslatok Útmutató azon a folyamaton, végrehajtási szolgáltatás tulajdonosok szükséges vezérlők |
| | Gyorsan üzembe helyezése a biztonsági szolgáltatásokat és a Microsoft és a partnerek készülékek |
| Feltárása |Automatikusan összegyűjti és biztonsági elemzi az Azure erőforrások, a hálózati és a partnerek megoldásaival például antimalware programok és a tűzfalak |
| | Globális leverages veszélyt intelligenciával kapcsolatos funkciók a Microsoft-termékek és szolgáltatások, a Microsoft digitális bűncselekmények egység (DCU), a Microsoft biztonsági válasz központ (Kialakításában), és külső hírcsatornák |
| | Fejlett analitikai, beleértve a gépi tanulási és viselkedési analysis vonatkozik |
| Visszajelzés | Itt elsőbbségi biztonsági eseményekre és figyelmeztetések |
| | A forrásban homonimaszerű webcímmel és probléma által sújtott erőforrások ajánlatok Hírcsatornájában |
| | Javasol módszert, amellyel leállítása az aktuális homonimaszerű webcímmel és jövőbeli támadások elleni védekezéshez |

## <a name="introductory-walkthrough"></a>Bevezető útmutató
 Biztonság otthon az [Azure portál](https://azure.microsoft.com/features/azure-portal/)érheti el. [Jelentkezzen be a portálon](https://portal.azure.com), válassza a **Tallózás gombra**, és görgessen a **Biztonság otthon** lehetőséget, vagy kattintson a **Biztonság otthon** csempére, amely a portál irányítópult kiemelt.

![Biztonsági csempe az Azure-portálon][1]

Biztonság otthon, a biztonsági házirendek beállítása, figyelése, biztonsági beállításokat, és megtekintheti a biztonsági figyelmeztetések.

### <a name="security-policies"></a>Eszközbiztonsági házirendek

Megadhatja a házirendek az Azure előfizetések és erőforrás-csoportok szerint a vállalat biztonsági követelményeknek. Is szabhatja őket a típusú alkalmazásokat használ, vagy a magánjellegű az adatok minden-előfizetésben. Előfordulhat például, fejlesztéshez vagy teszteléshez használt erőforrások különböző biztonsági követelményeknek, mint a termelési alkalmazásokhoz. Hasonlóképpen szabályozott adatok, például az adat-alkalmazások biztonsági magasabb szintű lehetnek szükségesek.

> [AZURE.NOTE] Ha módosítani szeretné az előfizetést vagy az erőforrás csoport szintjén a biztonsági házirendet, az előfizetés tulajdonosa, vagy azt a munkatárs kell lennie.

Válassza a **Biztonság otthon** lap a **házirend** csempére az előfizetések és erőforrás-csoportok listája.   

![Biztonság otthon lap][2]

Kattintson a **biztonsági házirendje** lap jelöljön ki egy előfizetést, a házirend részletes adatainak megjelenítéséhez.

![Biztonsági házirend lap előfizetés][3]

**Adatok gyűjtése** (lásd fent) lehetővé teszi, hogy a biztonsági házirendek vonatkozó adatok gyűjtése. Itt engedélyezése:

- Napi ellenőrzés az összes támogatott virtuális gépeken futó biztonsági figyelemmel kísérésére és a javaslatok.
- Az elemzés és veszélyt észlelést biztonsági események gyűjteménye.

**Válasszon egy tároló fiók / régió** (lásd fent) lehetővé teszi, az egyes régiókra virtuális gépeken futó operációs rendszert futtató, lehetősége van a tárterület-fiók e virtuális gépeken futó gyűjtött adatokat tároló. Ha nem az egyes régiókra vonatkozó egy tároló fiókot választja, akkor létrejön. Gyűjtött adatokat a többi ügyfél adataitól adatok biztonsági okokból logikailag elszigetelt.

> [AZURE.NOTE] Adatgyűjtés, majd a egy tároló fiók / régió, az előfizetés szinten van beállítva.

Jelölje ki a **megelőzése házirend** (lásd fent) a **megelőzése házirend** lap megnyitásához. **Javaslatok megjelenítése** lehetővé teszi a biztonsági vezérlőelemeket figyelésére és javasoljuk az előfizetés erőforrása biztonsági igényeinek megfelelően.

Ezután jelölje ki a erőforráscsoport házirend részletes adatainak megjelenítéséhez.

![Biztonsági házirend lap erőforráscsoport][4]

**Engedélyöröklés** (lásd fent) meghatározhatja, hogy az erőforráscsoport:

- (Alapértelmezett) örökölt ami azt jelenti, az összes biztonsági házirendek az erőforrás csoport öröklik az előfizetés szintjét.
- Egyedi ami azt jelenti, hogy az erőforráscsoport lesz egy egyéni biztonsági házirend. Meg kell módosítások **javaslatok a Megjelenítés**csoportban.

> [AZURE.NOTE] Ha nincs ütközés előfizetés szintű házirend- és erőforrás szintű csoportházirend között, az erőforrás-csoport szintű házirend elsőbbséget.

### <a name="security-recommendations"></a>Javaslatok biztonsági

 Biztonság otthon elemzi a biztonsági állapotát az Azure erőforrások azonosítása potenciális biztonsági rés. A javaslatok listáját végigvezeti Önt a folyamat a szükséges vezérlőelemeket. Példák:

- Kiépítési antimalware azonosításához és a kártevők eltávolítása
- Hálózati biztonsági csoportokat és a vezérlőelem-alapú forgalmat virtuális gépeken futó szabályok beállítása
- A webalkalmazások érdekében, hogy a célhely támadások elleni védelmét a webes alkalmazás tűzfalak kiépítése
- Nyekhttp://www.Office.com/redir/xt103979639 hiányzó rendszer frissítések
- Operációs rendszer beállításokat, amelyek nem felelnek meg a javasolt alaptervek megcímezheti

Kattintson a **javaslatok** csempére javaslatok listáját. Kattintson a minden ajánlási további információk megtekintése vagy művelet végrehajtása a probléma megoldásához.

![Biztonsági ajánlások az Azure biztonság otthon][5]

### <a name="resource-health"></a>Erőforrás állapota

Az **erőforrás biztonsági állapot** csempe a környezet erőforrástípus, beleértve a virtuális gépeken futó, webalkalmazások és más erőforrások: a teljes biztonsági testtartását jeleníti meg.   

Jelöljön ki egy erőforrás az **erőforrás biztonsági állapot** csempével Továbi tudnivalók és semmilyen potenciális biztonsági rés, amely azonosították listájának megtekintése. (**Virtuális gépeken futó** van kijelölve az alábbi példában.)

![Erőforrások állapot csempe][6]

### <a name="security-alerts"></a>Biztonsági figyelmeztetések

 Biztonság otthon automatikusan összegyűjti, elemzi és egyesíti az Azure erőforrások, a hálózati és a partnerek megoldásaival például antimalware programok és a tűzfalak naplóadatok. Ha veszélyek észlel, a biztonsági figyelmeztetés jön létre. Az észlelési többek között:

- Biztonságos virtuális gépeken futó ismert rosszindulatú IP-címek kommunikáció
- Speciális kártevőt észlelt a Windows-hibák jelentése
- Ellen támadások elleni virtuális gépeken futó
- Biztonsági figyelmeztetések integrált antimalware programok és a tűzfalak

A **biztonsági figyelmeztetések** csempére rangsorolt riasztások listáját jeleníti meg.

![Biztonsági figyelmeztetések][7]

További információt a homonimaszerű webcímmel és néhány javaslat, hogyan ismételt, akkor jelölje ki a értesítés jeleníti meg.

![Biztonsági figyelmeztetés részletei][8]

### <a name="partner-solutions"></a>A partnermegoldások

A **partnerek megoldásaival** csempe lehetővé teszi, hogy a monitor az Azure előfizetés integrálódik a partnerek megoldásaival állapotának áttekintése. Biztonság otthon megoldásokkal érkező riasztások jeleníti meg.

Jelölje ki a **partnerek megoldásaival** csempére. Egy lap megnyitja a csatlakoztatott partnermegoldások listájának megjelenítéséhez.

![A partnermegoldások][9]

## <a name="get-started"></a>Első lépések
Első lépések a biztonság otthon, a Microsoft Azure-előfizetést szüksége van. Biztonság otthon engedélyezve van az Azure-előfizetéséhez. Ha nem rendelkezik az előfizetést, jelentkezzen az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

 Biztonság otthon az [Azure portál](https://azure.microsoft.com/features/azure-portal/)érheti el. A [portál dokumentáció](https://azure.microsoft.com/documentation/services/azure-portal/) további információt találhat.

[Első lépések – Azure biztonság otthon](security-center-get-started.md) gyorsan útmutatók az első lépésekhez végig a biztonsági figyelemmel kísérésére és biztonság otthon házirendkezelő összetevői.

## <a name="see-also"></a>Lásd még:
A dokumentum biztonság otthon, a főbb funkciói és kezdőlépéseket kell megtennie hozták be. További információért olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Biztonsági ajánlások az Azure biztonság otthon kezelése](security-center-recommendations.md) – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Az Azure biztonság otthon partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – gyakori kérdések a szolgáltatással a keresés.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
