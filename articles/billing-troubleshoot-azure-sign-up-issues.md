<properties
    pageTitle="Azure bejelentkezési elhárításáról problémák beállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként néhány gyakori Azure bejelentkezési hibáinak elhárítása problémák fel."
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="felixwu"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="cjiang"/>

# <a name="i-cant-sign-up-for-azure"></a>Nem tudok bejelentkezni az Azure

Ha nem tudja regisztrál az Azure, több dolgot ellenőrizheti, hogy a probléma elhárításához.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Nincs SMS-EK és a hívásokat feliratkozás fiók ellenőrzése során 

- Győződjön meg arról, hogy a telefonszám fogadhat SMS.
- Dupla jelölőnégyzet a telefonszám be, beleértve a legördülő menü kijelölt ország kódot.
- Győződjön meg arról, hogy a telefonon kaphatnak a szöveges üzenetek (SMS), ha "Szöveges üzenet küldése", vagy telefonhívások Ha úgy dönt, hogy a "Általi felhívást kiváltó" alternatív.
- Mobiltelefon használata esetén ügyeljen arra, hogy jó phone kapcsolat.
- Csak a legfeljebb négy perccel, hogy a üzenetkezelő rendszer előadása a szöveg kódot, ha úgy dönt, hogy a "Küldés szöveges üzenet."
- A szöveges üzenet megjelenésekor szúrja be a kódot a szövegmezőbe, és kattintson a ellenőrzése gombra, a folytatáshoz.

### <a name="suggestions"></a>Javaslatok

- Szöveges üzenetek (SMS) nem jelenik meg a telefonon, ha a "általi felhívást kiváltó" alternatív ellenőrzési módszer használata
- Egy másik telefonszám használja, ha a telefon ellenőrző lépésben nem sikerül módszerekkel SMS és a "Általi felhívást kiváltó".
- VoIP-hez telefonszám a telefon ellenőrzési folyamat nem használhatók.

>[AZURE.NOTE] Módosíthatja a használni kívánt telefonszám később frissíti a [profiladatait](billing-how-to-change-azure-account-profile.md).

## <a name="credit-card-declined-or-not-accepted"></a>Hitelkártyával csökkent, vagy nem fogadta

Győződjön meg arról, hogy a fizetési módot, a regisztrációs használja az Azure aktiválásra vagy kifizetések támogatott.

- Virtuális és előre kifizetett jóváírási/terhelési értesítés kártyák nem használhatók.
- Elfogadott jóváírási/terhelési értesítés kártya szolgáltatók fiók ország attól függően változnak.

### <a name="suggestion"></a>Javaslatok

Vagy követel kártya használata bejelentkezéssel kapcsolatos problémák okait [a bankkártya vagy bankkártya van mondták Azure kukac mentése](billing-credit-card-fails-during-azure-sign-up.md)című témakör tartalmaz.

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Nem aktiválható az MSDN, BizSpark, BizSparkPlus és MPN Azure előny csomag

Ha a kiválasztott terv használatára jogosult, ellenőrizze a kedvezménye program csatornán keresztül:

- AZ MSDN WEBHELYEN
    - Ellenőrizze a jogosultsági állapotát az [MSDN-fióklapon](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
    - Ha az állapot nem tudja ellenőrizni, lépjen kapcsolatba az [MSDN előfizetések ügyfél szolgáltatás központok](https://msdn.microsoft.com/subscriptions/contactus.aspx)
- MPN
    - Jelentkezzen be a [MPN portálra](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) , és ellenőrizze a jogosultsági állapot. Ha rendelkezik a megfelelő [Felhő Platform szakértelem](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), akkor előfordulhat, hogy vehetők igénybe további előnyökkel jár.
    - Ha az állapot nem tudja ellenőrizni, forduljon a [MPN támogatja](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).
- Bizpark
    - Jelentkezzen be a [BizSpark portálra](https://www.microsoft.com/bizspark/default.aspx#start-two) , és ellenőrizze a jogosultsági állapot BizSpark és BizSpark plusz.
    - Ha az állapot nem tudja ellenőrizni, ügyfélszolgálatát Bizspark egy e-mailt küld [Forduljon BizSpark csoport](mailto:bizspark@microsoft.com?subject=BizSpark%20Support&body=Thank%20you%20for%20contacting%20BizSpark.%20Please%20provide%20as%20much%20of%20the%20following%20information%20as%20possible,%20as%20it%20will%20help%20expedite%20our%20response%20to%20you.%0aContact%20name:%0aStartup%20name:%0aMicrosoft%20Account/Live%20ID:%0aSpecific%20description%20of%20issue%20experienced%20or%20question:%0a%0aThank%20you,%0a%0aThe%20BizSpark%20Team)

### <a name="suggestion"></a>Javaslatok

Ha egy új kedvezménye előfizetés aktiválásához hiba bejelentkezési során használ, győződjön meg arról, hogy befejeződött-e az előfizetés beállítása az [Azure az előfizetések lapon](http://account.windowsazure.com/Subscriptions). Eltarthat néhány percet, amíg az előfizetés kattintva jelenítse meg az aktív. Ha az előfizetés aktiválva van, e-mailben kap. Ha az előfizetés állapotának függőben marad négy percnél hosszabb, [Azure ügyfélszolgálat](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) segítségét.

## <a name="cant-activate-new-azure-in-open-subscription"></a>Nem aktiválható az új megnyitása az Azure-előfizetés

Érvényes OSA kulcs kell rendelkeznie, amely legalább egy megnyitott az Azure jogkivonathoz társított, hogy új megnyitása az Azure-előfizetés aktiválása.

### <a name="suggestion"></a>Javaslatok

Ha nem rendelkezik egy OSA, lépjen kapcsolatba [A Microsoft Pinpoint](http://pinpoint.microsoft.com/)egy Microsoft Partners szerepel.

## <a name="cant-activate-azure-free-trial"></a>Nem aktiválható az Azure ingyenes próbaverziója

Használta már a egy múltbeli Azure-előfizetést? A használati feltételek Azure szerződés csak egy felhasználónál, amely az Azure új ingyenes próbaverzió aktiválási korlátozza. Ha van más típusú előfizetés Azure, ingyenes próbaverziót nem aktiválhatók.

### <a name="suggestion"></a>Javaslatok

-  Ha aktiválta az elmúlt egy Azure-előfizetést, és az ingyenes próbaverzió aktiválás sikertelen, fontolja meg az kirovó előfizetés. 
-  Ellenőrizze, hogy, hogy egy kedvezménye ajánlatot használatára jogosult. További tudnivalók a [Microsoft Azure ajánlat részletei lapon](https://azure.microsoft.com/support/legal/offer-details/). Előny csomag adott előfeltételekről szükség.

## <a name="need-help-contact-support"></a>Segítségre van szüksége? Forduljon az ügyfélszolgálathoz. 

Ha továbbra is segítségre van szüksége, [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a probléma megoldódott gyorsan. 
