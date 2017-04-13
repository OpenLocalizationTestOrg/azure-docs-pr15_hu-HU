<properties
   pageTitle="Rövid útmutató az Azure biztonság otthon |} Microsoft Azure"
   description="Ez a cikk segít – első lépések gyorsan Azure biztonság otthon létrehozásában biztonsági figyelemmel kísérésére és a házirend felügyeleti összetevőket és összekapcsolása, a következő lépésekkel."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Rövid útmutató az Azure biztonság otthon

Ez a cikk segít gyorsan – első lépések Azure biztonság otthon létrehozásában biztonság otthon biztonsági figyelemmel kísérésére és a házirend-kezelés összetevői.

> [AZURE.NOTE] Ez a cikk a szolgáltatás használatával egy példa telepítési vezet be. Ez a cikk nem cikk részletesen ismerteti.

## <a name="prerequisites"></a>Előfeltételek

Első lépések a biztonság otthon, a Microsoft Azure-előfizetést kell rendelkeznie. Ha nincs előfizetésem, jelentkezzen [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).

Biztonság otthon: a szabad réteg automatikusan engedélyezve van-előfizetéséhez, és itt betekintést kap abba, hogy az Azure erőforrások biztonsági állapotát. Biztosít egyszerű biztonsági házirendek kezelése, biztonsági javaslatok és integráció számára biztonsági termékek és szolgáltatások Azure partnertől.

Biztonság otthon az [Azure portál](https://azure.microsoft.com/features/azure-portal/)érheti el. Többet szeretne tudni az Azure-portálra, lásd a [portál dokumentációt](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Adatok gyűjtése

Biztonság otthon adatait gyűjti össze a biztonsági állapotukban értékeléséhez, adja meg a biztonsági javaslatok és veszélyek figyelmezteti a virtuális gépek (VMs). Amikor először nyitja meg a biztonság otthon, adatgyűjtés engedélyezve van az előfizetése összes VMs. Adatgyűjtés ajánlott, de választhatja a biztonság otthon házirend adatgyűjtés kikapcsolásával.

Az alábbi lépéseket ismertetik, hogyan nyithat meg és használja a biztonság otthon összetevői. Ezeket a lépéseket bemutatjuk, hogyan kapcsolhatja ki adatgyűjtés, ha úgy dönt, hogy lemondja.

## <a name="access-security-center"></a>Az Access biztonsági központ

A portálon kövesse az alábbi lépéseket biztonsági központ:

1. A **Microsoft Azure** menüben válassza a **Biztonság otthon**.
![Azure menü][1]

2. Ha első alkalommal érik el a biztonság otthon, az **Üdvözli** a lap nyílik meg. Válassza az Igen **! Indítási Azure biztonság otthon szeretném** nyissa meg a **Biztonság otthon** lap és adatgyűjtés engedélyezni szeretné.
![Üdvözlő képernyője][10]

3. Üdvözli a lap Biztonság otthon indítása vagy a Microsoft Azure menüből válassza a biztonság otthon követően a **Biztonság otthon** lap nyílik meg. Könnyen elérheti a **Biztonság otthon** lap kapcsolatban, válassza ki azt a **PIN-kód lap irányítópult** beállítás (a jobb felső sarokban).
![Irányítópult lehetőség a PIN-kód lap][2]

## <a name="use-security-center"></a>Biztonsági központ használata

Beállíthatja, hogy a Azure előfizetések és erőforrás-csoportok biztonsági házirendeket. Vegyük konfigurálása az előfizetéshez tartozó biztonsági házirendek:

1. A **Biztonság otthon** a lap válassza a **házirend** csempére.
![Eszközbiztonsági házirend beállítása][3]

2. Kattintson a **biztonsági házirend - előfizetést vagy erőforrás csoportonként szabályzat** lap jelöljön ki egy előfizetést.
3. A **Yammer biztonsági házirendje** lap **adatgyűjtés** automatikusan naplógyűjtés, hogy be van kapcsolva. A felügyeleti kiterjesztése a jelenlegi és az új VMs összes az előfizetés már kiépítve. (Választhatja ki a adatgyűjtés értékre állításával **adatgyűjtés** **kikapcsolása**, de ez megakadályozza, hogy a biztonság otthon, biztosítva a biztonsági figyelmeztetések és javaslatok.)
4. Válassza a **Yammer biztonsági házirendje** lap, **Válassza ki a tárhely fiók / régió**. Az egyes régiókra operációs rendszert futtató VMs lehetősége van válassza ki az adott VMs gyűjtött adatokat tároló tárterület-fiókot. Ha nem az egyes régiókra vonatkozó egy tároló fiókot választja, akkor létrejön. Gyűjtött adatokat a többi ügyfél adataitól adatok biztonsági okokból logikailag elszigetelt.

     > [AZURE.NOTE] Azt javasoljuk, hogy adatgyűjtés engedélyezése, és választja ki először egy előfizetés szintre tárterület-fiókkal. Eszközbiztonsági házirendek beállítható, hogy az erőforrás csoportszint és Azure előfizetés szintű, de az adatok gyűjtése és tároló fiók konfigurálása akkor fordul elő, csak az előfizetés szintjén.

5. Válassza a **Yammer biztonsági házirendje** lap **megelőzése házirend**. Ekkor megnyílik a **megelőzése házirend** lap.
![Házirend megelőzése][4]

6. A **házirend megelőzése** lap a bekapcsolása a javaslatok, hogy meg szeretné jeleníteni a Yammer biztonsági házirendje részeként. Példa:

 - **A** **rendszer-frissítések** beállítása ellenőrzi az összes támogatott virtuális gépeken futó operációs rendszer frissítések hiányzó.
 - Az összes támogatott virtuális gépeken futó operációs rendszer beállításaitól, amelyek a virtuális gép támadásokkal szemben tehetik azonosítása **OS biztonsági** beállítást **a** ellenőrzése.

### <a name="view-recommendations"></a>Javaslatok megtekintése

1. Térjen vissza a **Biztonság otthon** lap, és jelölje ki a **javaslatok** mozaikot. Biztonság otthon rendszeres elemzi a biztonsági állapot Azure erőforrásait. Biztonság otthon azonosítja a potenciális biztonsági rés, amikor a **javaslatok** lap javaslatok látható.
![Javaslatok az Azure biztonság otthon][5]

2.  Jelölje ki a **javaslatok** lap ajánlást további információk megjelenítéséhez, illetve művelet végrehajtása a probléma megoldásához.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Az erőforrások egészség és biztonsági állapotának megtekintése

1.  Vissza a **Biztonság otthon** lap. Az **erőforrások biztonsági állapot** csempe virtuális gépeken futó, a hálózat, az adatok és a alkalmazások biztonsági állam jelölőket tartalmazza.
2.  Jelölje ki a **virtuális gépeken futó** tájékozódhat. A **virtuális gépeken futó** lap megnyílik, és megjeleníti a antimalware programok, rendszer frissítések, újraindítása és az VMs az operációs rendszer biztonsági állapot összefoglalását.
![Az erőforrások állapot csempe az Azure biztonság otthon][6]

3.  Jelölje ki a **Virtuális gép javaslatok** további információk megtekintése vagy a művelet végrehajtása szükséges vezérlőket konfigurálása ajánlást.
4.  Jelölje ki a virtuális a **virtuális gépeken futó** további részletes adatainak megjelenítéséhez.

### <a name="view-security-alerts"></a>Biztonsági figyelmeztetések megtekintése

1.  Térjen vissza a **Biztonság otthon** lap, és kattintson a **biztonsági figyelmeztetések** csempére. A **biztonsági figyelmeztetések** lap megnyílik, és az értesítések listáját jeleníti meg. A biztonság otthon elemzése a biztonsági naplókról és a hálózati tevékenység az értesítések hoz létre. A beépített partnermegoldások riasztását számításba veszi.
![Biztonsági figyelmeztetések az Azure biztonság otthon][7]

    > [AZURE.NOTE] Biztonsági figyelmeztetések csak érhetők el, ha engedélyezve van a biztonság otthon: a Standard réteg. A szokásos réteg 90 napos ingyenes próbaverziójának érhető el. Nézze meg a [következő lépések](#next-steps) beszerzése a szabványos réteg olvashat.

2.  Jelölje ki a értesítés további adatok megjelenítéséhez. Ebben a példában jelöljük ki **módosítva rendszer bináris talált**. Ekkor megnyílik a pengéit, amelyekkel további részleteket az értesítésre.
![Biztonsági figyelmeztetés részletei az Azure biztonság otthon][8]

### <a name="view-the-health-of-your-partner-solutions"></a>A partnerek megoldásaival állapotának megtekintése

1. Vissza a **Biztonság otthon** lap. A **partnerek megoldásaival** csempe egy pillantással figyelheti a partnerek megoldásaival integrálódik az Azure előfizetés állapotának teszi lehetővé.
2. Jelölje ki a **partnerek megoldásaival** csempére. A lap megnyílik, és a partnerek megoldásaival csatlakozik a biztonság otthon listáját jeleníti meg.
![A partnermegoldások][9]

3. Jelölje be a partner megoldást. Ebben a példában jelöljük ki az **F5 – WAF** megoldás.  Egy lap nyílik meg, és megmutatja, hogy a partner megoldást és kapcsolódó erőforrásokat a megoldást állapotának. Jelölje ki a **megoldást konzol** meg szeretné nyitni a partner adatkezelési folyamatok ehhez a megoldáshoz.

## <a name="next-steps"></a>Következő lépések
Ez a cikk bevezetett biztonság otthon biztonsági figyelemmel kísérésére és a házirend-kezelés összetevői. Most, hogy ismeri a biztonság otthon, próbálkozzon az alábbi lépéseket:

- Azure előfizetéshez tartozó biztonsági házirendek beállítása. További tudnivalókért olvassa el a [Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md)című témakört.
- Használja a javaslatok biztonsági központban az Azure erőforrások védelme. További tudnivalókért olvassa el a [kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md)című témakört.
- Tekintse át, és az aktuális biztonsági figyelmeztetések kezelése. További információért olvassa el a [kezelése és a biztonsági figyelmeztetések az Azure biztonság otthon megválaszolása](security-center-managing-and-responding-alerts.md)című témakört.
- További tudnivalók a [Speciális veszélyt észlelési szolgáltatások](security-center-detection-capabilities.md) biztonság otthon: a [Standard réteg](security-center-pricing.md) mellékelt. A szokásos réteg 90 napos ingyenes próbaverziójának érhető el.
- Ha biztonság otthon használatával kapcsolatban, olvassa el a az [Azure biztonsági központja – gyakori kérdések](security-center-faq.md)című témakört.

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
