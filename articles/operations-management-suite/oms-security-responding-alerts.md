<properties
   pageTitle="Figyelés és a biztonsági figyelmeztetések válaszol a műveletek Management csomagja biztonsági és naplózási megoldás |} Microsoft Azure"
   description="A dokumentum adja meg a kockázatot üzletiintelligencia-beállítást elérhető a MOBILE biztonsági és naplózási figyelésére és megválaszolása a biztonsági figyelmeztetések segítségével."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Figyelés és a biztonsági figyelmeztetések a műveletek Management csomagja biztonsági és naplózási megoldás megválaszolása

Ehhez a dokumentumhoz, mellyel a kockázatot intelligencia beállítás az MOBILE és a naplózási figyelésére és a biztonsági figyelmeztetések válaszolni.

## <a name="what-is-oms"></a>Mi az MOBILE?

A Microsoft műveletek Management csomagja (MOBILE), a Microsoft cloud-alapú, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra management levelezőprogramból. MOBILE kapcsolatos további tudnivalókért olvassa el a cikk [Műveletek Management csomagja](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Veszélyt intelligenciával kapcsolatos funkciók

Egy vállalati környezetben, ahol a felhasználók széles férhet hozzá a hálózathoz és csatlakozhat különféle eszközökön vállalati adatok elengedhetetlen aktívan figyelemmel az erőforrások és gyorsan reagálni védelmi események is. Az adott biztonsági életciklus szempontjából fontos, mert néhány cybersecurity nem vehet fel veszélyek értesítések vagy a gyanús tevékenységek hagyományos biztonsági technikai vezérlők azonosítania kell magát. 

A **Kockázatot intelligencia** beállítás az MOBILE és a naplózási használatával az informatikai rendszergazdák biztonsági kockázatok ellen a környezetben, például azonosítása, azonosítása, ha egy adott számítógépre egy [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection)része. Számítógépek egy botnet található csomópontok válhat támadók törvénytelenül telepítésekor kártevő ezen a számítógépen parancsot, és a vezérlő titokban csatlakozik. Azt is megállapítható föld kommunikációs csatornák, például [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents)érkező potenciális veszélyek. 

Annak érdekében, hogy a kockázatot intelligencia készítéséhez MOBILE és a naplózási használja a Microsoft belül több forrásból származó adatok. MOBILE és a naplózási fog kihasználhatja a potenciális kockázatok ellen a környezet azonosításához.

A kockázatot üzletiintelligencia-ablak három fő beállítások áll:
- A kimenő rosszindulatú forgalom kiszolgálók
- Észlelt veszélyek típusai
- Üzletiintelligencia-leképezés veszélyt

> [AZURE.NOTE] Ezek a beállítások áttekintése olvassa el a [tevékenységek kezelése csomagja biztonsági és naplózási megoldás az első lépések](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Biztonsági figyelmeztetések megválaszolása

A lépéseket a [biztonsági az esemény válasz](https://technet.microsoft.com/library/cc512623.aspx) folyamat egyik azonosítja a biztonságos rendszer súlyosságát. Ez a fázis kell hajtsa végre az alábbi műveleteket:

- A homonimaszerű webcímmel jellegét meghatározása
- A homonimaszerű webcímmel pont származási meghatározása
- Határozza meg a szándék a támadások. Az adott információkat szerezheti be a szervezetnél kifejezetten irányított támadások volt, illetve lett véletlen?
- Az, hogy melyik van kerülhetett
- A fájlok van nyitottak, és meghatározzák, hogy a magánjellegű azokat a fájlokat a azonosítása

**Veszélyt intelligenciával kapcsolatos** adatokat MOBILE biztonsági és naplózási megoldás segítség az alábbi műveleteket is élvezheti. A **Kockázatot üzletiintelligencia** -beállításokat érhet el az alábbi lépésekkel:

1. A **Microsoft műveletek Management Suite** fő irányítópulton kattintson a **Biztonság és naplózási** csempére.

    ![Biztonság és naplózás](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. A **Biztonság és naplózási** irányítópulton a jobb oldalon **Veszély üzletiintelligencia** kiválasztható alább látható módon jelennek meg:

    ![Intel veszélyt](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

A következő három csempék képet ad az aktuális veszélyek áttekintése. A **kimenő rosszindulatú forgalom Server** fogja tudni azonosíthatja, ha a bármely olyan számítógépen, figyeli (belüli vagy kívüli a hálózat), amely küld rosszindulatú forgalom az internethez. 

A **Detected veszélyt típusok** csempét a kockázatok, amelyek aktuális "a helyettesítő" összefoglalását mutatja, a csempére kattintva jelenik meg többet szeretne tudni az alábbi veszélyek diavetítésként az alábbi:

![Észlelt veszélyt típusai](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Rá kattintva kibonthatja a minden veszélyt további információt. Az alábbi példában látható Botnet olvashat bővebben:

![többet szeretne tudni az veszélyt](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Ez a szakasz elejére leírtak ezt az információt csak nagyon hasznos lehet egy esemény válasz esetet során. Azt is fontos a törvényszéki vizsgálat alatt hol kell keresse meg a homonimaszerű webcímmel, melyik rendszer jutott és az ütemterv forrását. A jelentés, például a homonimaszerű webcímmel néhány fontosabb részleteket könnyedén megállapítható: a forrás a támadások, a helyi IP jutott, és az aktuális munkamenete a kapcsolat állapotát. 

Az **üzletiintelligencia-leképezés veszélyt** nyújt segítséget az aktuális helyek körül a földgömb, amelyek a forgalom rosszindulatú azonosítása. Narancssárga (bejövő) vannak, és a piros (kimenő) nyilak a listatartományt, amely azonosítja a forgalom irányának, ha az ezen nyilak valamelyikére kattint, akkor jelennek meg veszélyt és a forgalom irányát alább látható módon:

![intel térkép veszélyt](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Megjelenik a bemutató használatát ezt a lehetőséget az esemény során a Microsoft Ignite kézbesítve úgy, hogy nézi a bemutató [Mitigate adatközponthoz biztonsági veszélyek interaktív vizsgálat műveletek Management csomagot használ,](https://myignite.microsoft.com/videos/5000) a válasz folyamat.

## <a name="see-also"></a>Lásd még:

A dokumentumban a MOBILE és a naplózási megoldás a **Kockázatot üzletiintelligencia** parancs használata biztonsági szóló jelzés kezelésével megtanulta azt. MOBILE biztonsággal kapcsolatos további információért olvassa el a következő cikkeket:

- [Tevékenységek kezelése csomagja (MOBILE) – áttekintés](operations-management-suite-overview.md)
- [Tevékenységek kezelése csomagja biztonsági és naplózási megoldás első lépések](oms-security-getting-started.md)
- [Tevékenységek kezelése csomagja biztonsági és a naplózási megoldás erőforrások ellenőrzése](oms-security-monitoring-resources.md)
