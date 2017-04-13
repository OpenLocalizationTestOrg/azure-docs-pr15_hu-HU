<properties
    pageTitle="Azure Active Directory B2C: Jogkivonat, munkamenet és egyszeri bejelentkezés beállítása |} Microsoft Azure"
    description="Jogkivonat, a munkamenet és az egyszeri bejelentkezéses konfigurációs az Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Jogkivonat, munkamenet és egyszeri bejelentkezéses konfigurálása

Ez a funkció is biztosít külön vezérlő [Csoportházirend alap](active-directory-b2c-reference-policies.md), a:
 
1. Az Azure Active Directory (Azure Active Directory) B2C által kibocsátott biztonsági tokenek élettartama.
2. Azure Active Directory B2C kezeli a webes alkalmazás munkamenetek élettartama.
3. Egyszeri bejelentkezés (SSO) viselkedés alkalmazások és a házirendek az B2C bérlői webhelyen keresztül.

Használhatja ezt a szolgáltatást a B2C bérlői webhelyen az alábbi képlettel történik:

1. Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
2. Kattintson a **Sign in házirendek**. *Megjegyzés: a funkció használható házirend típus, nem csak a* *Bejelentkezési házirendek***.
3. Kattintással nyissa meg a házirend. Ha például kattintson a **B2C_1_SiIn**.
4. Kattintson a lap tetején a **szerkesztése** elemre.
5. **Jogkivonat, a munkamenet és az egyszeri bejelentkezéses config**gombra.
6. Végezze el a kívánt módosításokat. További tudnivalók: további szakaszok elérhető tulajdonságok.
7. Kattintson az **OK gombra**.
8. A lap tetején kattintson a **Mentés** gombra.

![Jogkivonat, a munkamenet és az egyszeri bejelentkezéses config képernyőképe](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Jogkivonat élettartama konfigurálása

Azure Active Directory B2C [OAuth 2.0-s engedélyezési protokoll](active-directory-b2c-reference-protocols.md) támogatja a védett erőforrások biztonságos hozzáférés engedélyezése. A támogatás végrehajtásához az Azure Active Directory B2C különböző [biztonsági tokenek](active-directory-b2c-reference-tokens.md)bocsát ki. A tulajdonságok is használhatja az Azure Active Directory B2C által kibocsátott biztonsági tokenek élettartama kezelése a következők:

- **Az Access & ID jogkivonat élettartama (perc)**: az 2.0-s OAuth bearer jogkivonathoz élettartama védett erőforrás eléréséhez használt. Azure Active Directory B2C a csak azonosító tokenek hibák megoldása. Amikor hozzáadunk őket támogatása ezt az értéket hozzáférési jogkivonat, valamint szeretné alkalmazni.
   - Alapértelmezett = 60 perc.
   - A minimum (a végpontokat is beleértve) = 5 percig tart.
   - (A végpontokat is beleértve) legfeljebb 1440 perc =.
- **Frissítés az élettartam (nap)**: az maximális időszakra, amelyik elé frissítés jogkivonat szerezheti be egy új access vagy az azonosító token használható (és ha szükséges, egy új frissítés jogkivonat, ha megadták az alkalmazás a `offline_access` hatókör).
   - Alapértelmezett = 14 nappal.
   - A minimum (a végpontokat is beleértve) = 1 nap.
   - A maximális (a végpontokat is beleértve) = 90 napon.
- **Frissítés jogkivonat csúszó ablak élettartam (nap)**: ezt az időtartamot elteltével a felhasználónak meg kell-e újból hitelesíteni, függetlenül a legutóbbi érvényességi jogkivonat szerezte be az alkalmazás frissítése. Azt is csak biztosítható, ha a kapcsoló **Bounded**van beállítva. Nagyobb vagy egyenlő lesz szüksége van a **frissítés jogkivonat élettartama (nap)** értéket. A Váltás a **Unbounded**van beállítva, ha egy adott értéket nem adhat meg.
   - Alapértelmezett 90 napon =.
   - A minimum (a végpontokat is beleértve) = 1 nap.
   - A maximális (a végpontokat is beleértve) = 365 nap.

Használati eset, amelyekkel ezen tulajdonságokat használva néhány a következők:

- A mobil alkalmazásba bejelentkezve kapcsolatának határozatlan ideig, felhasználójának engedélyezni mindaddig, amíg az illető kikkel az alkalmazás folyamatosan aktív. Ehhez megadásával a **frissítés jogkivonat csúsztatható ablak leírási idő (nap)** **Unbounded** váltani a bejelentkezési házirend.
- Az ágazatra biztonság és a megfelelőségi követelmények felel meg a megfelelő hozzáférési jogkivonat élettartama megadásával.

## <a name="session-configuration"></a>Munkamenet konfigurálása

Azure Active Directory B2C [OpenID csatlakozás hitelesítési protokoll](active-directory-b2c-reference-oidc.md) támogatja a biztonságos bejelentkezés webalkalmazások engedélyezése. A tulajdonságok kezelése a webes alkalmazás munkamenetek is használhatja a következők:

- **Webalkalmazás munkamenet élettartama (perc)**: Azure Active Directory B2C munkamenet cookie-k sikeres hitelesítés, a felhasználó böngésző-on tárolt élettartama.
   - Alapértelmezett = 1440 perc.
   - (A végpontokat is beleértve) legalább 15 percet =.
   - (A végpontokat is beleértve) legfeljebb 1440 perc =.
- **Web app időtúllépés**: Ezzel a kapcsolóval **abszolút**értéke, a felhasználónak meg kell-e újra a **munkamenetet élettartama (perc) webalkalmazás** letelik által meghatározott időtartam után hitelesítést végezni. Ha ezzel a kapcsolóval **működés közbeni** (az alapértelmezett beállítás) van beállítva, a felhasználók továbbra is bejelentkezve mindaddig, amíg a felhasználó folyamatosan aktív, a webalkalmazásban.

Használati eset, amelyekkel ezen tulajdonságokat használva néhány a következők:

- Követelményeknek az ágazatra biztonság és megfelelőség beállításával a megfelelő web alkalmazás munkamenet élettartama.
- Kényszerített újbóli hitelesítést során a webalkalmazás magas szintű biztonsági része a felhasználó interakció egy idő beállítása után. 

## <a name="single-sign-on-sso-configuration"></a>Egyszeri bejelentkezés (SSO) konfigurálása

Ha több alkalmazás és a házirendek az B2C bérlői webhelyemen, felhasználói kommunikációt át őket az **egyszeri bejelentkezéses konfigurációs** tulajdonság használatával kezelheti. A tulajdonság a következő beállítások egyikére:

- **Bérlői**: Ez az alapértelmezett beállítása. Ezzel a beállítással lehetővé teszi, hogy több alkalmazás és a házirendek az adott felhasználó munkamenetben megosztása a B2C bérlői webhelyén. Például a bejelentkezés az alkalmazásba, miután Contoso vásárlás, akkor hatékonyabbnak is is zökkenőmentesen jelentkezzen be egy másik egy, a Contoso gyógyszerészeti, elérése azt követően.
- **Alkalmazás**: Ez lehetővé teszi, hogy a felhasználói munkamenet kizárólag az alkalmazáshoz, más alkalmazások független karbantartása. Például ha a felhasználót, hogy jelentkezzen be a Contoso gyógyszerészeti (a hitelesítő adatait), akkor is, ha az illető kikkel van már bejelentkezett a Contoso vásárlás, egy másik alkalmazásban az azonos B2C bérlői. 
- **Házirend**: Ez lehetővé teszi, hogy egy felhasználó munkamenet kizárólag a házirendet, az alkalmazások használat független karbantartása. Például ha a felhasználó már bejelentkezve és több tényező authentication (MFA) lépés befejezése, akkor hatékonyabbnak kaphatnak access több alkalmazások nagyobb biztonság részeinek mindaddig, amíg a munkamenet tartozik a házirend nem jár le.
- **Letiltott**: Ez kényszeríti a felhasználót, hogy a szabály minden végrehajtás révén a teljes felhasználói utazás futtatni. Például ez lehetővé teszi a több felhasználó számára, hogy az alkalmazás (a megosztott asztal forgatókönyvben) regisztráció, még akkor is, miközben egy felhasználó marad bejelentkezve teljes ideje alatt.
