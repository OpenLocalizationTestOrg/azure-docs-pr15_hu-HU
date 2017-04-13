<properties
   pageTitle="Az Azure biztonság otthon védelmi esemény kezelése |} Microsoft Azure"
   description="A dokumentum Azure biztonság otthon funkciók használatával védelmi események kezeléséhez nyújt segítséget."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Az Azure biztonság otthon védelmi esemény kezelése 
Triaging és vizsgálja meg a biztonsági figyelmeztetések még a legtöbb képzett biztonsági elemzők a időigényes lehet, és készült, akkor nehéz lehet akkor is, hogy hogyan kezdjek hozzá. [Analytics](security-center-detection-capabilities.md) csatlakozni az adatok között különböző [biztonsági figyelmeztetések](security-center-managing-and-responding-alerts.md)segítségével biztonsági Center adja meg a egy homonimaszerű webcímmel marketingkampány és az összes kapcsolódó riasztások egyetlen nézetének – gyorsan megismerheti a támadó tartott műveletek, és milyen erőforrások hatással volt.

A dokumentum biztonsági értesítés képességek használandó biztonság otthon, amely segít a biztonsági események kezelése ismerteti.


## <a name="what-is-a-security-incident"></a>Mi az a biztonsági incidens?

A biztonság otthon védelmi esemény az erőforrás összes riasztásainak zárt beállítást a [leállítása egymásba fonódó](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) mintázattal egy összesítést. Események jelennek meg a [Biztonsági figyelmeztetések](security-center-managing-and-responding-alerts.md) csempére és a lap. Esemény megjelenítheti a kapcsolódó riasztások, a listáját, amely lehetővé teszi, hogy minden előfordulást bővebb tájékoztatást kaphat.

## <a name="managing-security-incidents"></a>Védelmi események kezelése

Az aktuális biztonsági események áttekintheti megjeleníti a biztonsági figyelmeztetések csempére. Az Azure portál elérhető, és hajtsa végre az alábbi lépéseket követve többet szeretne tudni az egyes védelmi esemény:

1. A biztonság otthon irányítópulton jelenik meg a **biztonsági figyelmeztetések** csempére.

    ![Biztonsági figyelmeztetések a biztonsági központban csempe](./media/security-center-incident/security-center-incident-fig1.png)

2.  Kattintson a csempe kibontásához, és ha védelmi esemény lép fel, az a biztonsági figyelmeztetések graph alább látható módon fog megjelenni:

    ![Védelmi esemény](./media/security-center-incident/security-center-incident-fig2.png)

3.  Figyelje meg, hogy a biztonsági az esemény leírását és más riasztást összehasonlítása más ikon tartozik. Kattintson rá, ez az esemény további adatainak megjelenítéséhez.

    ![Védelmi esemény](./media/security-center-incident/security-center-incident-fig3.png)

4.  Kattintson az **esemény** lap jelenik meg további részleteket a biztonsági esemény, amelyek tartalmazzák még a teljes körű leírását, annak súlyosságát (Ez ebben az esetben magas), annak aktuális állapotában (ebben az esetben célszerű továbbra is *aktív*, ami azt jelenti, a felhasználó még nem vett művelet *bezárásához* – ezt megteheti a alkalmanként a **biztonsági figyelmeztetések** a lap jobb oldali gombjával) , a megtámadott erőforrás (Ez esetben *VM1*), a remediation lépéseit az eseményt, és az alsó ablaktáblában az értesítések, ez az esemény szereplő van. Ha azt szeretné, a minden riasztás további információkhoz juthat, csak kattintson rá, és egy másik lap nyitja meg, alább látható módon:

    ![Védelmi esemény](./media/security-center-incident/security-center-incident-fig4.png)

Ez a lap az információkat az értesítésre szerint változhatnak. Olvassa el a [kezelése és a biztonsági figyelmeztetések az Azure biztonság otthon válasz](security-center-managing-and-responding-alerts.md) további információt az értesítések kezelése. Ezzel a szolgáltatással kapcsolatos néhány fontos dolgot:

- Új szűrő lehetővé teszi a nézet csak az esemény vagy csak az értesítések testreszabása. 
- Az azonos értesítés az esemény (ha van), valamint egy különálló figyelmeztetés látható legyen részeként tartozhat. 
- Esemény elrejtésével fog nem zárja be a kapcsolódó értesítések.

## <a name="see-also"></a>Lásd még:

A dokumentumban hogyan használható a biztonsági az esemény képességek biztonság otthon megtanulta azt. Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Kezelés és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md)
- [Azure biztonság otthon észlelés](security-center-detection-capabilities.md)
- [Azure biztonság otthon tervezése és a műveletek útmutatója](security-center-planning-and-operations-guide.md)
- [Kezelés és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md)
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md)– keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.
