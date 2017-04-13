<properties
   pageTitle="Tevékenységek kezelése csomagja biztonság és a naplózási megoldás eredeti |} Microsoft Azure"
   description="A dokumentum megtudhatja, hogyan MOBILE biztonsági és megoldási végezze el a felügyelt számítógépeknek eredeti értékelése megfelelőség és biztonsági célra naplózási."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>A műveletek Management csomagja biztonsági és naplózási megoldás eredeti értékelése

A dokumentum segítségével [Műveletek Management (MOBILE) csomagja biztonsági és naplózási megoldás](operations-management-suite-overview.md) eredeti értékelési funkciók a felügyelt erőforrások biztonságos állapotát eléréséhez használni.

## <a name="what-is-baseline-assessment"></a>Mi az eredeti értékelést?

A Microsoft nemzetközi webhelye, üzleti és kormányzati szervezetek együtt határozza meg a Windows konfiguráció, amely a nagyon biztonságos server-telepítések. Ez a beállítás beállításkulcsainak, a naplózási házirend-beállítások és a biztonsági házirend-beállítások az alábbi beállítások a Microsoft ajánlott értékeit együtt. A szabálykészletet biztonsági eredeti néven. Eredeti értékelési MOBILE és a naplózási lehetőséget is zökkenőmentesen tekintse át a megfelelőség az összes számítógépre. 

Háromféle szabályok van:

- **Beállításjegyzék szabályok**: Ellenőrizze, hogy beállításkulcsainak helyesen vannak-e beállítva.
- **Naplózási házirend szabályok**: a naplózási házirend szabályokból.
- **Biztonsági szabályok**: a felhasználó engedélyeinek a gép szabályokból.

> [AZURE.NOTE] Ez a funkció rövid áttekintése, olvassa el [a biztonsági konfigurációs eredeti felmérése MOBILE biztonság használata](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) .

## <a name="security-baseline-assessment"></a>Biztonsági eredeti értékelése

Ellenőrizheti, hogy az aktuális biztonsági eredeti értékelési MOBILE és a naplózási irányítópultról felügyeli minden számítógépre.  Hajtsa végre az alábbi lépéseket a biztonsági eredeti értékelési irányítópult eléréséhez:

1. A **Microsoft műveletek Management Suite** fő irányítópulton kattintson a **Biztonság és naplózási** csempére.
2. A **Biztonság és naplózási** irányítópulton kattintson az **Eredeti értékelési** **Biztonsági**a tartományok. A **Biztonsági eredeti értékelési** irányítópult jelenik meg, az alábbi képen látható módon:
    
    ![MOBILE és a naplózási eredeti értékelése](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Az irányítópult három fő területen van osztva:

- **Eredeti számítógépek és összehasonlítása**: Ez a szakasz összegzi a használt számítógépek száma és a számítógépen, amely a felmérés át. Is kínál a felső 10 számítógépek és százalékos eredménye a vizsgálathoz.
- **Kötelező szabályok állapota**: Ez a szakasz a szándék elérését segítő ahhoz, hogy a sikertelen szabályok tájékoztatás súlyosságát által van, és nem sikerült szabályok típus szerint. Megjeleníti az első Graph gyorsan azonosíthatók, ha a legtöbb sikertelen szabályok kritikus, vagy nem. A felső 10 sikertelen szabályokat és azok súlyosságát is kínál. A második grafikonon szabályt, amely a felmérés során nem sikerült a típusát. 
- **Hiányzó eredeti értékelési számítógépek**: Ebben a szakaszban a lista az operációs rendszere nem kompatibilis vagy hibák miatt nem használt számítógépen. 

### <a name="accessing-computers-compared-to-baseline"></a>A számítógépek és összehasonlítása az eredeti elérése

Ideális a biztonsági eredeti felmérés kompatibilis lehetnek összes a számítógépet. Jó helyen jár, hogy bizonyos körülmények között ez várhatóan nem történik. A biztonsági kezelési folyamat részeként fontos, ha meg szeretné jeleníteni a néző adják át az összes biztonsági értékelési teszt sikertelen volt a számítógépen. Gyorsan ábrázolása, amely a, **számítógépek érhető el** az **eredeti számítógépek és összehasonlítása** című szakaszában található lehetőség választásával. Meg kell jelennie a napló keresési eredményben, amelyen a számítógépek, ahogy az alábbi képen látható:

![A számítógépen elérhető eredménye](./media/oms-security-baseline/oms-security-baseline-fig2.png)

A keresési eredmény táblázatos formátumban, ahol az első oszlop van a számítógép neve és a második színt a szám szabályok, amelyek nem sikerült jelenik meg. A szabályt, amely nem sikerült típusú adatok beolvasásához, kattintson a számítógépnek a neve mellett a sikertelen szabályok számát. Az alábbi képen látható egy hasonló eredményt kell megjelennie:

![A számítógépen elérhető eredmények részletei](./media/oms-security-baseline/oms-security-baseline-fig3.png)

A találatok között ha a teljes hozzáférés szabályok, a kritikus szabályok, amelyek nem sikerült, a figyelmeztetés szabályokat és a nem sikerült információkra vonatkozó szabályok.

### <a name="accessing-required-rules-status"></a>Szeretne hozzáférni a szükséges szabályokat állapota

A számítógépek, a felmérés átadott százalékos száma vonatkozó információkat, miután érdemes arról, hogy melyik szabályok nem működnek megfelelően a kritikusság további információkhoz juthat. Ez a Megjelenítés segít a fontossági, mely számítógépek kell juttatni először győződjön meg róla, kattintson a Tovább értékelő kompatibilis lesz. Vigye az egérmutatót a diagram **nem sikerült szabályok szerint súlyosságát** csempére **szükséges szabályokat állapot** csoportban található kritikus része fölé, és kattintson rá. Eredménye a következő képernyőn láthatóhoz hasonlóan kell megjelennie:

![Nem sikerült szabályok szerint súlyosságát részletei](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

A napló eredménye eredeti szabályt, amely nem sikerült, ez a szabály leírásának, és a gyakori konfigurációs számbavétel (CCE) Ez a szabály azonosítója típusú láthatók. Következő attribútumok elég a probléma megoldásához a cél számítógép korrekciós műveletet végre kell lennie.

> [AZURE.NOTE] További információt a CCE a [Nemzeti biztonsági adatbázis](https://nvd.nist.gov/cce/index.cfm)elérheti.

### <a name="accessing-computers-missing-baseline-assessment"></a>Hiányzó eredeti értékelési számítógépek elérése

MOBILE Windows Server 2008 R2 felfelé a Windows Server 2012 R2 rendszer a tartományi tag eredeti profil használatát támogatja. A Windows Server 2016 eredeti egyelőre végleges nem, és megjelenik, amint teszi közzé. Minden más MOBILE és a naplózási eredeti értékelési keresztül beolvasott operációs rendszerek az **eredeti értékelési hiányzó számítógépek** csoportban jelenik meg.

## <a name="see-also"></a>Lásd még:

A dokumentumban, miként értesült MOBILE és a naplózási eredeti értékelést. MOBILE biztonsággal kapcsolatos további információért olvassa el a következő cikkeket:

- [Tevékenységek kezelése csomagja (MOBILE) – áttekintés](operations-management-suite-overview.md)
- [Figyelés és a biztonsági figyelmeztetések válaszol a műveletek Management csomagja biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
- [Tevékenységek kezelése csomagja biztonsági és a naplózási megoldás erőforrások ellenőrzése](oms-security-monitoring-resources.md)

