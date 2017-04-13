<properties
   pageTitle="A Microsoft Azure piactéren kifizetés jelentési megértéséhez |} Microsoft Azure"
   description="Megtudhatja, hogyan tekintheti át és a Microsoft Azure piactéren kifizetés jelentés ingest."
   services="marketplace-publishing"
   documentationCenter="na"
   authors="v-jeana"
   manager="lakoch"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="v-jeana; hascipio; v-dabosl"/>

# <a name="understand-your-azure-marketplace-payout-reports"></a>A Microsoft Azure piactéren kifizetés jelentéseket ismertetése

## <a name="access-and-view-your-payout-reports"></a>Hozzáférés és a kifizetés jelentések megtekintése

Miközben fejlesztői központ átmenet azt a kifizetés jelentések részét lehetséges érhető el a fejlesztői központ https://dev.windows.com/en-us a közben másokat is találhatók meg a közzétételi portál https://publish.windowsazure.com.

Kifizetés Jelentéskészítés most érhetők el **Fejlesztői központ** bármely piactér ajánlatokról társított modern payouts; Ez jelenleg tartalmazza:
- VMs
- B + C ajánlatok
- A EA kínált adatok és a fejlesztői szolgáltatások

Kifizetés jelentéskészítés továbbra is szerepelni fognak **Közzétételi portál** :
- Adatok és a fejlesztői szolgáltatások felajánlja a webes közvetlen (amely továbbra is használja a kifizetés régebbi rendszer).

Jelentések elérhető 45 nap után a negyedév a Bezárás gombra, és bármelyik visszatérítésről után számítja ki.

### <a name="access-payout-reports-in-dev-center"></a>Az Access kifizetés jelentések a fejlesztői központ

1. Nyissa meg a https://dev.windows.com/en-us fejlesztői központ.
2. Az **Irányítópult**elemre.

    ![LandingPageDashboardHighlight][1]

3. Kattintson a **Kifizetés összegzése**.

    ![DashboardPayoutSummary][2]


## <a name="view-your-payout-reports-in-dev-center"></a>A kifizetés jelentések megtekintése a fejlesztői központ

A kifizetés jelentés negyedévenként rekordok lépett fel, hogy negyedévbe eső összes tranzakciók.

- Fenntartott összeg azt jelzi, hogy bármely kifizetések vannak merülnek fel, kívül a közelgő fizetési ciklus (például az összeg helyezi át a közelgő fizetési a következő hónap).  Ez az összeg általában lesz 0 Ft (kivéve, ha egy ügyfél fizet jól az advance).
- Kattintson a közelgő feladatok fizetési vagy legutóbbi fizetési **Részletek** hivatkozásokra, hogy egy adott payouts megjegyzést.
- Kattintson a **Fizetési kimutatások** alkalmazás/termékenként a bevétel részleteinek megtekintését.
- Kattintson a lásd: az egyes kimutatások **megtekintése** hivatkozásra.

    ![PayoutSummaryUpcomingMostRecentLinksStatement][3]

- Az egyes utasítás alján a **Részletezése során** szűrő használatával több alkalmazások-termék megtekintése, ha vannak ilyenek.

    ![PayoutSummaryPaymentStatementsFilterControl][4]



## <a name="view-your-payout-reports-in-publishing-portal"></a>A kifizetés jelentések megtekintése a közzétételi portál
A kifizetés jelentés negyedévenként rekordok lépett fel, hogy negyedévbe eső összes tranzakciók.

1. Nyissa meg a közzétételi portál https://publish.windowsazure.com elemre.
2. A **Közzétevőknek** csoportban kattintson a **Kifizetés jelentések**elemre.
3. Kattintson a legördülő menü megjelenítése az összes rendelkezésre álló negyedéves kifizetés jelentések.

    ![accessingpayoutreport][5]


### <a name="read-your-payout-reports"></a>A kifizetés jelentések elolvasása

A kifizetés jelentés negyedévenként rekordok lépett fel, hogy negyedévbe eső összes tranzakciók.

- Ha egy adott negyedév valamely tételek keres, jelölje be a kifizetés jelentés adott negyedév a legördülő. Például ha érdeklik a tételeket április június 2015 szeretne, jelölje ki a dátumtartományt a legördülő.
- Ha a keres, amelyek egy adott negyedév kapcsolatosak payouts részleteit, jelölje be a kifizetés jelentés a következő negyedév. Például ha érdeklik a payouts való június 2015 április, ezek az összegek fog megjelenni a későbbi kifizetés jelentés szeptember 2015 július.
![readingpayoutreport][6]

- A pénzügyi összefoglaló panel kategória szerint balances, jóváírások és tartozik jeleníti meg.
- Tételek megjelenítése a tranzakciókat.

## <a name="definitions"></a>Definíciók

**Pénzügyi az Összegzés lapon:**

![financialdefinitions][7]

**Tételek:**

![ledgerdefinitions][8]

## <a name="payout-questions"></a>Kifizetés kérdések

A payouts kapcsolódó kérdése van, forduljon a támogatási csoportunk.

![payoutquestions][9]

1. Nyissa meg a támogatás lapra.
2. Jelölje ki a **Payouts**.
3. Jelölje ki a **kifizetés kapcsolódó lekérdezések**.
4. Kattintson a **kérelem indítása**gombra.

## <a name="next-steps"></a>Következő lépések

Más támogatási lekérdezések jelentkezzen <https://portal.azure.com>a problémát.

[1]: ./media/marketplace-publishing-report-payout/LandingPage-DashboardHighlight.png
[2]: ./media/marketplace-publishing-report-payout/Dashboard-PayoutSummary.png
[3]: ./media/marketplace-publishing-report-payout/PayoutSummary-UpcomingOrMostRecentPaymentLinksSingleStatementLink.png
[4]: ./media/marketplace-publishing-report-payout/PayoutSummary-PaymentStatements-SingleStatement-FilterControl.png
[5]: ./media/marketplace-publishing-report-payout/accessingpayoutreport.png
[6]: ./media/marketplace-publishing-report-payout/readingpayoutreport.png
[7]: ./media/marketplace-publishing-report-payout/financialdefinitions.png
[8]: ./media/marketplace-publishing-report-payout/ledgerdefinitions.png
[9]: ./media/marketplace-publishing-report-payout/payoutquestions.png
