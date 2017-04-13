<properties
   pageTitle="Az Azure biztonság otthon partnermegoldások kezelése |} Microsoft Azure"
   description="A dokumentum végigvezeti hogyan Azure biztonság otthon lehetővé teszi, hogy az Azure előfizetés integrálódik a partnerek megoldásaival állapotának áttekintése monitort."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>A partnermegoldások az Azure biztonság otthon figyelése

A dokumentum végigvezeti a partnerek megoldásaival az Azure biztonság otthon állapotának figyelése.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást. Ez a cikk részletesen ismerteti nem.

## <a name="monitoring-partner-solutions"></a>A partnermegoldások figyelése

A **partnerek megoldásaival** csempe a **Biztonság otthon** lap lehetővé teszi, hogy a monitor az Azure előfizetés integrálódik a partnerek megoldásaival állapotának áttekintése.
![A partnermegoldások csempe][1]

A **partnerek megoldásaival** csempét a partnerek megoldásaival és azok hatókörét összefoglalása állapotban számát jeleníti meg.

Partner megoldást **állapot** lehet:

- A védett (zöld) – nem állapota probléma van.
- Sérült (piros) – egy azonnali figyelmet igénylő állapot probléma.
- Leállítva (narancs színnel) jelentéskészítés – a megoldást leállt a jelentési állapotát.
- Ismeretlen védelem (narancs színnel) – a megoldást állapotának állapota ismeretlen miatt sikertelen folyamat: új erőforrás felvétele a meglévő megoldást adott időben.
- Nem jelentett (szürke) – a megoldást nem jelentett semmit, még egy megoldás állapota nem jelentett lehet, ha azt közvetlenül csatlakozik, és továbbra is üzembe helyezése.

Nincs megoldások integrálódik az előfizetés esetén a csempe fog adja meg, hogy nincsenek-e nincs megoldás. A **partnerek megoldásaival** csempe kiválasztása lehetővé teszi a **javaslatok** lap biztonsági partnermegoldások telepítéséhez nyissa meg.
![Nincs partnermegoldások][2]

A partnerek megoldásaival állapotának megtekintése:

1. Jelölje ki a **partnerek megoldásaival** csempére. Egy lap megnyitja a partnerek megoldásaival csatlakozik a biztonság otthon listájának megjelenítéséhez.
![A partnermegoldások][3]

2. Jelölje be a partner megoldást. Ebben a példában lehetővé teszi, jelölje ki az **F5 – WAF2** megoldást.  A lap megnyílik a megjeleníti a partner megoldást és a megoldás állapotának kapcsolódó erőforrásokat. Jelölje ki a **megoldást konzol** meg szeretné nyitni a partner adatkezelési folyamatok ehhez a megoldáshoz.
![Partner megoldás részletei][4]

3. Térjen vissza az **F5 – WAF2** lap, és válassza a **hivatkozás alkalmazás**. Ekkor megnyílik az **Alkalmazások hivatkozásra** a lap. Az alábbi források csatlakoztathatja a partner megoldás.
![Partner megoldás hivatkozás források][5]

## <a name="see-also"></a>Lásd még:
A dokumentumban, bevezetett biztonság otthon a **Partnerek megoldásaival** mozaikot. Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Biztonsági ajánlások az Azure biztonság otthon kezelése](security-center-recommendations.md) – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – gyakori kérdések a szolgáltatással a keresés.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
