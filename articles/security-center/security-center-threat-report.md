<properties
   pageTitle="Azure biztonság otthon veszélyt üzletiintelligencia-jelentés |} Microsoft Azure"
   description="A dokumentum Azure biztonsági központ veszélyt intelligens jelentések segítségével vizsgálat alatt keresse meg a biztonsági figyelmeztetés kapcsolatban további információt nyújt segítséget."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Azure biztonság otthon veszélyt üzletiintelligencia-jelentés
A dokumentum elmagyarázza, hogy Azure biztonsági központ veszélyt intelligens jelentéseket is többet megtudhat a biztonsági figyelmeztetés létrehozott veszélyt.

## <a name="what-is-a-threat-intelligence-report"></a>ActiveX-vezérlőket veszélyt üzletiintelligencia-jelentés
Biztonság otthon veszélyt észlelési figyelése, biztonsági információk az Azure erőforrások, a hálózati és a csatlakoztatott partnermegoldások működik. Megvizsgálja ezt az információt, gyakran használatával történik a kockázatok azonosítása, több forrásból származó információkat. Ez az eljárás a biztonság otthon [észlelés](security-center-detection-capabilities.md)része. 

Biztonság otthon veszélyt azonosítja, amikor elindítja a egy [biztonsági figyelmeztetés](security-center-managing-and-responding-alerts.md), amely egy adott rendezvény részleteivel, például javaslatok remediation kapcsolatos részletes információkat tartalmaz. Biztonság otthon tartalmaz, amely arról tájékoztat, hogy az észlelt, beleértve az információt, például a veszélyt veszélyt intelligencia jelentés segítséget nyújt a csoportoknak vizsgálja meg, és veszélyek ismételt az esemény válasz, a: 

- Támadó identitás vagy társítások (Ha ez az információ akkor érhető el)
- Támadó célok
- Aktuális és korábbi homonimaszerű webcímmel kampányok (Ha ez az információ akkor érhető el)
- Támadó taktikák, eszközök és eljárások
- A biztonságos (IoC), például az URL-EK és a fájl társított jelölők
- Victimology, amely az üzleti, földrajzi előfordulási, amely segít a meghatározásában, ha az Azure erőforrások kockázatára
- Kezelési és remediation információk

>[AZURE.NOTE] Mennyi információ bármely adott jelentés változnak; a kártevőt tevékenység és előfordulási részletességi szintjét alapuló.

Biztonság otthon háromféle veszélyt-jelentéseket, amelyek szerint a támadások változhat. A rendelkezésre álló jelentéseket lehet:

- **Jelentés a tevékenységek csoport**: mély dives biztosít a támadók, célkitűzések, és taktikák.
- **Marketingkampány jelentés**: szolgáltatásaival kapcsolatos bizonyos támadások kampányok részleteit. 
- **Veszélyt összefoglaló jelentés**: terjed ki az összes elemet az előző két jelentésekben.

Ilyen típusú információt nagyon hasznos a [választ az esemény](security-center-incident-response.md) során, ahol a folyamatban lévő nyomozásban megérteni a homonimaszerű webcímmel és a támadó összefüggések teendők: a probléma mozgatása előre csökkentésében forrását. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Hogyan érhető el a kockázatot üzletiintelligencia-jelentés?

Az aktuális értesítések áttekintheti megjeleníti a **biztonsági figyelmeztetések** csempére. Nyissa meg az Azure-portált, és hajtsa végre az alábbi lépéseket követve többet szeretne tudni a minden riasztás:

1. A biztonság otthon irányítópulton jelenik meg a **biztonsági figyelmeztetések** csempére.

2. A csempére kattintva nyissa meg a **biztonsági figyelmeztetések** a lap, amely tartalmazza az értesítések olvashat, és kattintson a biztonsági figyelmeztetés kapcsolatos további információkhoz juthat, amelyet az.

    ![Biztonsági figyelmeztetések](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. Ebben az esetben a **gyanús folyamat végrehajtott** lap a részletes adatait jeleníti meg az értesítés az alábbi ábrán látható módon:

    ![Biztonsági értesítés detais](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Mennyi információ minden egyes biztonsági riasztás érhető el a megadott típusnak megfelelő értesítés változhatnak. A **jelentések** mezőben, ha a kockázatot üzletiintelligencia-jelentés mutató hivatkozást. Kattintson rá, és egy másik böngészőablakban fog kinézni a PDF-fájlt.

    ![Tárterület kiválasztása](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Innen töltse le ezt a jelentést és olvasható további információ a biztonsági problémát észlelt a PDF-fájlt, és műveleteket kapott adatok alapján.

## <a name="see-also"></a>Lásd még:

A jelen dokumentum megtanulta azt is bemutatja, hogyan segít Azure biztonsági központ veszélyt intelligens jelentések kapcsolatos biztonsági riasztások vizsgálat alatt. Többet szeretne tudni az Azure biztonság otthon, olvassa el az alábbiakat:

- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md). Keresse meg a szolgáltatás használatával kapcsolatos gyakori kérdésekre.
- [Az Azure biztonság otthon az esemény válasz](security-center-incident-response.md)
- [Azure biztonság otthon észlelés](security-center-detection-capabilities.md)
- [Azure biztonság otthon tervezés és a műveletek útmutató](security-center-planning-and-operations-guide.md). Megtudhatja, hogy miként tervezése és Azure biztonság otthon elfogadása a tervezési szempontok ismertetése.
- [Kezelése és a biztonsági figyelmeztetések az Azure biztonság otthon válaszolni](security-center-managing-and-responding-alerts.md). Megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Az Azure biztonság otthon védelmi esemény kezelése](security-center-incident.md)
- [Azure biztonsági Blog](http://blogs.msdn.com/b/azuresecurity/). Azure biztonság és megfelelőség kapcsolatos blogbejegyzéseket megkeresése
