<properties
    pageTitle="Logika alkalmazások összekötők áttekintése |} Microsoft Azure"
    description="Egy logikai alkalmazásban használható összekötők – áttekintés"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Egy logikai alkalmazásban összekötői segítségével

Összekötők eseményeket, az adatok és a műveletek gyors hozzáférést biztosít a szolgáltatások, a protokollok és platformon keresztül.  A teljes listát az összekötők logika alkalmazások támogató is [Itt kell](apis-list.md).  Összekötők az eseményindító vagy művelet összefüggés-alkalmazásban is alkalmazható, és szükség lehet a beállított *kapcsolatot* használó (például: a Twitteren fiók eléréséhez vagy közzé az Ön nevében engedélyezése).

## <a name="basics"></a>Alapjai

Összekötők olyan szolgáltatott szolgáltatások, a logika alkalmazás más szolgáltatások, például a Dynamics, Azure, Salesforce, [és további](apis-list.md)integrálása részeként érheti el.  Ezeket rendszerbe és Microsoft kezeli, így skála, átviteli és biztonsági áll a integrációs munkafolyamatok készíthet.  Összekötő vehet egy logikai alkalmazást, keresést, és válassza a összekötő művelet vagy a kiváltó ok mező **megjelenítése Microsoft felügyelt API-k**csoportban.

![A kiváltó ok mező kijelölése a művelet menü][1]

Mindegyik összekötő művelet vagy a kiváltó ok mező tulajdonságainak beállítása a csoportját lesz.  Kattintson az információ gombok, ha többet szeretne megtudni a művelet, vagy hivatkozás a dokumentáció [További](apis-list.md).

Ha azt szeretné, ha integrálni a szolgáltatás vagy API-val, amely még egy összekötőre, is összefüggés-alkalmazások és az [egyéni összekötő](../app-service-logic/app-service-logic-create-api-app.md) bővítése, illetve csak hívása közvetlenül a szolgáltatást a HTTP például protokollon keresztül nem.

## <a name="triggers"></a>Eseményindítók

Egyes összekötők van eseményindító, ami azt jelenti, ezt az összekötőt az esemény fog egy logikai alkalmazás fire fázisban és adatokat az eseményindító részeként.  A kiváltó ok mező értéke mindig az első lépés a logika alkalmazásban.  Népszerű indítók műveletek, például a következők:
 
 * Ismétlődés - óránként futtatása
 * Ha a HTTP-kérelem érkezik
 * Amikor egy elemet egy várólista hozzáadva
 * Amikor e-mailben érkezik
 
Bizonyos indítók fog fire azonnali esemény történik a logika alkalmazásba értesítést keresztül, és mások szükségük van konfigurálva a logika app milyen gyakran jelölje be a szolgáltatást (legfeljebb minden 15 másodperc) esemény ismétlődési gyakoriságát.  

Esemény érkezik, a Futtatás logika alkalmazás fire lesz, és megkezdi a munkafolyamat műveletei.  Is fogja tudni semmilyen adatot hozzáférhet az eseményindító egész a munkafolyamatot (például az "meg egy új tweetbe" eseményindító továbbítja az tweetbe be a Futtatás).

## <a name="actions"></a>Műveletek

A legtöbb összekötők van egy vagy több, a munkafolyamat részeként végrehajtható műveletek.  Műveletek bármely lépések, amelyek akkor aktiválódnak, a Futtatás elindult az eseményindító után.  Hozzáadása egy művelet kattintson az **Új lépést** gomb és a Keresés az összekötő használni kívánt.  Miután kijelölt (és után a bármely szükség lehet [kapcsolatok](#connections) konfigurálása) jelenik meg a művelet kártya beállíthatja.  Adatok az előző lépést a kimeneti értékeket a tokenek bármelyik kattintva válassza ki, vagy írja be a bármilyen egyéb konfigurációs, szükség szerint.

![Egy összekötő művelet konfigurálása][2]

## <a name="connections"></a>Kapcsolatok

A legtöbb összekötők csak a *kapcsolat* beállítása, az összekötő használata előtt.  *Kapcsolat* a beállítás bármely login vagy a kapcsolat az összekötő eléréséhez szükséges.  Összekötők, amely az OAuth használja, a kapcsolat létrehozása az azt jelenti, hogy (például az Office 365-ben, Salesforce vagy GitHub) a szolgáltatást, ahová titkosított és biztonságos módon Azure titkos tárban tárolt a a hozzáférési jogkivonat-bA bejelentkezve.  Más (például az FTP- és SQL) összekötők beállításokat, például a kiszolgáló címét, felhasználónevét és jelszavát tartalmazó kapcsolat szükséges.  Ezek a kapcsolat konfigurációs részletei is titkosított és tárolja.  Kapcsolat elérheti a szolgáltatást, feltéve, hogy a szolgáltatás lehetővé teszi, hogy lesz.  Az Azure Active Directory OAuth kapcsolatot (például az Office 365-ben és a Dynamics) azt is, a hozzáférési jogkivonat végtelen időre szóló frissítéséhez.  Más szolgáltatások korlátai elhelyezése előfordulhat, hogy mennyi ideig ábrázolásakor jogkivonat frissítése nélkül.  Az általános bizonyos műveletek, például a jelszó módosítása az összes hozzáférési jogkivonat érvényteleníti.  

Kapcsolatok megnézheti és Azure-ban kattintson a **Tallózás gombra** , és válassza a **API kapcsolatok**kezeli.  A kapcsolatok API erőforrás megtekintése, szerkesztése, frissítése vagy ismételt engedélyezése minden kapcsolatot hozott létre.

## <a name="next-steps"></a>Következő lépések

- [Az első összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [További tudnivalók a gyakori felhasználások és logika alkalmazások példák](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Első lépések a nagyvállalati integrációs eseményindítók és műveletek](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png