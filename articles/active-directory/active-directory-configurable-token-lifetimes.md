<properties
   pageTitle="Az Azure Active Directory konfigurálható jogkivonat élettartama |} Microsoft Azure"
   description="Ez a funkció rendszergazdák és a felhasználók által használt adja meg a tokenek Azure Active Directory által kibocsátott élettartamának."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/06/2016"
   ms.author="billmath"/>


# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Konfigurálható jogkivonat élettartama az Azure Active Directory (nyilvános előzetes verzió)

>[AZURE.NOTE]
>Ez a lehetőség jelenleg nyilvános előzetes verzióban.  Meg kell hajlandó visszavált, vagy távolítsa el a módosításokat.  Ezt a szolgáltatást, mindenki számára a nyilvános villámnézetben próbálja meg megnyitni azt, azonban van szükség bizonyos szempontokat előfordulhat, hogy miután általában elérhető az [Azure Active Directory prémium verzióra szóló előfizetés](active-directory-get-started-premium.md) .


## <a name="introduction"></a>– Bevezetés
Ez a funkció rendszergazdák és a felhasználók által használt adja meg a tokenek Azure Active Directory által kibocsátott élettartamának. Jogkivonat élettartama beállítható az összes alkalmazás egy bérlői webhelyen, több bérlői alkalmazáshoz, vagy az egy adott szolgáltatás egyszerű egy bérlői webhelyen.

Azure Active Directory, a csoportházirend-objektum egyes alkalmazások vagy a bérlő összes alkalmazások kényszerített szabályok csoportját képviseli.  Különböző típusú házirend egyedi szerkezete tartalmazó tulajdonságok, majd alkalmazott objektumok, amelyhez hozzá van rendelve.

A kijelölt házirend alapértelmezés szerint egy bérlői webhelyen. Ezzel a házirenddel majd akkor lépnek érvénybe, a minden alkalmazás, amely belül mindaddig, amíg ki nem magasabb prioritású házirend bírálni bérlői. Egyes alkalmazások is rendelhet hozzá házirendeket. Az elsőbbségi sorrend házirend-típus szerint változik.

Élettartam házirendek a frissítés tokenek, a hozzáférési jogkivonat, a munkamenet tokenek és az azonosító tokenek beállíthatók.


### <a name="access-tokens"></a>Hozzáférési jogkivonat
Egy hozzáférési jogkivonat ügyfél védett erőforrás eléréséhez használja. Egy hozzáférési jogkivonat csak akkor használható, a felhasználónak, az ügyfél és az erőforrás adott kombinációi. Hozzáférési jogkivonat nem kell vonni, addig, amíg a lejárati érvényesek. A kártékony szereplő, amely egy hozzáférési jogkivonat kapott felhasználhatja mértékének élettartama.  Beállító hozzáférési jogkivonat élettartam rendszer teljesítményének javítása és növekvő, hogy az ügyfél access megőrzi a felhasználói fiók letiltása után idő mennyiségét között.  Továbbfejlesztett rendszerteljesítmény ügyfél kell szerez egy friss jogkivonat hányszor csökkentésével érhető el. 

### <a name="refresh-tokens"></a>Tokenek frissítése
Amikor egy ügyfél megszerzi a védett erőforrás eléréséhez egy hozzáférési jogkivonat, kap egy frissítés token és egy hozzáférési jogkivonat is. A frissítés jogkivonat beszerzése a token párban új access/frissítés az aktuális jogkivonat lejártakor. Frissítés tokenek kötődnek felhasználói és az ügyfél kombinációi. Is vonni, és azok érvényessége be van jelölve, minden alkalommal szolgálnak.

Fontos, hogy a bizalmas és nyilvános-ügyfelek közötti különbséget. Bizalmas ügyfelek olyan alkalmazásokat, amelyek képesek biztonságos tárolása az ügyfél jelszavát, amelyek lehetővé teszik, amellyel igazolhatja, hogy kérések származó az ügyfélalkalmazás és nincs kártevő szereplő. Ezek a folyamatok biztosítani, esetén a frissítés, ezek a folyamatok kiadott tokenek magasabb, és nem módosíthatók a házirenddel alapértelmezett élettartamának.

A környezet, az alkalmazások futó korlátai miatt nyilvános ügyfelek nem tudnak biztonságos tárolása az ügyfél jelszót. Megakadályozhatja, hogy frissítés tokenek régebbi, mint egy adott időszak nyilvános ügyfelektől érkező új access/frissítés jogkivonat két (frissítése jogkivonat Max inaktiválása idő) házirendek beállítható erőforrásait.  Ezenkívül házirendek beállítása túl, amelyek a frissítés tokenek már nem fogadta a partner (frissítése jogkivonat Max életkor) időbeli használható.  Frissítés beállító jogkivonat élettartamának szabályozhatja, hogy mikor és hogyan gyakran helyett meg automatikusan újra hitelesített folyamatban, egy nyilvános ügyfélalkalmazás használata esetén a hitelesítő adatok újbóli szükség-e a felhasználó teszi lehetővé.


### <a name="id-tokens"></a>Azonosító tokenek
Azonosító tokenek át a webhelyek és a natív ügyfelek és a felhasználói profil információt tartalmaznak. Bizonyos kombinációi felhasználó, és az ügyfél-azonosító token kötött. Azonosító tokenek addig, amíg a lejárati érvényes számítanak.  Általában a webalkalmazás megegyezik egy felhasználó adatait a munkamenet élettartama az alkalmazás a azonosító jogkivonat élettartama ki a felhasználó számára.  Beállító azonosító élettartamot lehetővé teszi, hogy milyen gyakran a webalkalmazás fog jár le az alkalmazás munkamenet és a felhasználó újra hitelesítse az Azure Active Directory (csendes vagy interaktív).

### <a name="single-sign-on-session-token"></a>Egyszeri bejelentkezéses munkamenetben jogkivonat
Ha egy felhasználó az Azure Active Directory hitelesíti, bejelentkezéses munkamenetben a felhasználó a böngésző és az Azure Active Directory jön létre.  Az egyetlen bejelentkezéses munkamenetben jogkivonat, a cookie-k formájában ebben a munkamenetben jelöli. Fontos tudni, hogy az egyszeri bejelentkezés munkamenet jogkivonathoz nem kapcsolódik egy adott erőforrás/ügyfélalkalmazás. Egyszeri bejelentkezés munkamenet tokenek vissza kell vonni, és azok érvényessége be van jelölve, minden alkalommal szolgálnak.

Vannak olyan egyszeri bejelentkezés munkamenet tokenek kétféle. Állandó munkamenet tokenek tárolja, állandó cookie-k használata a böngészőben és a munkamenet állandó tokenek vannak tárolva mint munkamenet cookie-k (ezek elvész a böngésző bezárásakor).

Nem állandó munkamenet tokenek 24 óra élettartamának lennie, mivel állandó tokenek 180 nap élettartamának. Az egyszeri bejelentkezés munkamenet jogkivonat belül az érvényességi bármikor érvényességi bővített másik 24 óra vagy nap 180. Ha az egyszeri bejelentkezés munkamenet nem jogkivonat belül az érvényességi célszerű járt le, és már nem fogadja el a rendszer. 

Házirendek beállítása ideje az első munkamenet jogkivonat kiadásának túl, amelyek a munkamenet tokenek már nem (munkamenet jogkivonat Max életkor) elfogadása után használhatók.  Munkamenet beállító jogkivonat élettartamának szabályozhatja, hogy mikor és hogyan gyakran a felhasználó szükséges írja be újra a hitelesítő adatok helyett webalkalmazás használata esetén az csendes hitelesített teszi lehetővé.

### <a name="token-lifetime-policy-properties"></a>Élettartam házirend tulajdonságai
Élettartam házirend egy olyan csoportházirend-objektum, amely tartalmazza az élettartam szabályok típusú.  A Tulajdonságok házirend használatával szabályozhatja a megadott jogkivonat élettartama.  Nincs házirend értéke, a rendszer az alapértelmezett élettartamának kényszeríti.


### <a name="configurable-token-lifetime-properties"></a>Állítható be az élettartam tulajdonságai
A tulajdonság|Házirend tulajdonság karakterlánc|Milyen hatással van|Alapértelmezett|Minimális|Maximum|
----- | ----- | ----- |----- | ----- | ----- |
Hozzáférési jogkivonat élettartam|  AccessTokenLifetime|Hozzáférési jogkivonat, azonosító tokenek, SAML2 tokenek|1 óra értéket|10 perc|1 nap|
Jogkivonat maximális inaktiválása idő frissítése|    MaxInactiveTime |Tokenek frissítése |14 nappal|10 perc|    90 napon|
Egyetlen varianciaanalízis frissítés jogkivonat Max kora|    MaxAgeSingleFactor  |(Az összes felhasználó) tokenek frissítése |90 napon|10 perc |Amíg visszavont *|
Többtényezős frissítés jogkivonat Max kora| MaxAgeMultiFactor|  (Az összes felhasználó) tokenek frissítése| 90 napon|10 perc|Amíg visszavont *|
Egyetlen varianciaanalízis munkamenet jogkivonat Max kora |MaxAgeSessionSingleFactor **    |Munkamenet tokenek (állandó és nem állandó)| Amíg visszavonva   |10 perc |Amíg visszavont *|
Többtényezős munkamenet jogkivonat Max kora| MaxAgeSessionMultiFactor x|    Munkamenet tokenek (állandó és nem állandó)| Amíg visszavonva|  10 perc| Amíg visszavont *



- * 365 nap hossza a maximális explicit, amely a következő attribútumok be lehet állítani.
- ** Ha nincs beállítva MaxAgeSessionSingleFactor ezt az értéket MaxAgeSingleFactor értéket veszi fel. Ha egyik sem a paraméter értéke, a tulajdonság veszi fel az alapértelmezett érték (addig, amíg visszavont).
- Ha nincs beállítva MaxAgeSessionMultiFactor ezt az értéket MaxAgeMultiFactor értéket veszi fel. Ha egyik sem a paraméter értéke, a tulajdonság veszi fel az alapértelmezett érték (addig, amíg visszavont).

### <a name="exceptions"></a>A kivételek
A tulajdonság|Milyen hatással van|Alapértelmezett|
----- | ----- | ----- |
Jogkivonat Max (szövetséges felhasználók elegendő visszavonási adatokkal) inaktív idő frissítése|(Nem elegendő visszavonási információit a szövetséges felhasználók tulajdonos) tokenek frissítése|12 óra|
Jogkivonat maximális inaktiválása idő (bizalmas ügyfelek) frissítése| Frissítse a tokenek (tulajdonos bizalmas ügyfelek számára)|90 napon|
Jogkivonat Max kora (tulajdonos bizalmas ügyfélalkalmazások) frissítése |   Frissítse a tokenek (tulajdonos bizalmas ügyfelek számára) |Amíg visszavonva

### <a name="priority-and-evaluation-of-policies"></a>Prioritás és házirendek értékelése

Jogkivonat élettartam házirendek hozhatja létre és egyes alkalmazások, a bérlők és a szolgáltatás rendszerbiztonsági rendelve. Ez azt jelenti, hogy az adott alkalmazás alkalmazása több házirendek lehetséges. A jogkivonat élettartam-házirendet, amely akkor lép érvénybe, a következőképpen szabályok:


- Ha explicit módon van hozzárendelve egy házirendet a szolgáltatás egyszerű, szabályon. 
- A szolgáltatás egyszerű nincs házirend kifejezetten hozzá van rendelve, ha kifejezetten hozzárendelve a szülő-bérlő a szolgáltatás fő házirend szabályon. 
- Ha nincs házirend kifejezetten hozzárendelve a fő szolgáltatás vagy a bérlő, a házirendet, az alkalmazás rendelt szabályon. 
- Ha nincs házirend a szolgáltatás egyszerű, a bérlő vagy objektum van rendelve, az alapértelmezett értékeket szabályon (lásd a fenti táblázatot).

Az Azure Active Directory alkalmazás és a fő objektumok szolgáltatás közötti kapcsolatra kapcsolatos további tudnivalókért olvassa el az [alkalmazás és a szolgáltatás fő objektumok az Azure Active Directory](active-directory-application-objects.md)című témakört.

Egy token érvényességi kiértékeli használatos időben. A házirend használatban van alkalmazásáról a legnagyobb prioritású lép érvénybe.


>[AZURE.NOTE]
>Példa
>
>A felhasználó szeretne 2 webalkalmazások, a és b elérése 
>
>
>- Mindkét alkalmazások vannak szülő ugyanahhoz a bérlőhöz. 
>- Jogkivonat élettartamának házirendekhez, ha 1 a munkamenet jogkivonat Max korát 8 órát a szülő-bérlő alapértelmezett értéke.
>- Webalkalmazás A normál használata webalkalmazás, és nincs kapcsolatban a talált házirendeket. 
>- Webalkalmazás B szolgál kiemelten fontos folyamatok és a szolgáltatás egyszerű jogkivonat élettartam házirend 2 munkamenet jogkivonat Max kor 30 percig kapcsolódik.
>
>A 12:00 PM a felhasználó nyit meg egy új böngésző-munkamenetet, és megpróbál webalkalmazás válaszokhoz a felhasználónak van-e irányítva a Azure AD, és meg kell adnia bejelentkezési hozzáférni. A böngészőben a munkamenet jogkivonat a cookie-k Ez esik. A felhasználó a rendszerünk átirányítja vissza egy azonosító jogkivonat, amely lehetővé teszi, hogy az alkalmazás elérheti őket az alkalmazás A webes.
>
>A 12:15 du a felhasználó próbálja webalkalmazás b eléréséhez Azure Active Directory, amely a munkamenet cookie észleli átirányítja a böngészőben. Webes alkalmazás B szolgáltatás egyszerű 1 házirend van csatolva, de része is a szülő-ös bérlői webhelye alapértelmezett házirend 2. Házirend 2 óta házirendek szolgáltatást rendszerbiztonsági csatolva van bérlői az alapértelmezett házirendek-nál magasabb prioritást lép érvénybe. A munkamenet jogkivonat kiadásának az utolsó 30 percen belüli így érvényes számít. A felhasználó a rendszerünk átirányítja vissza B webalkalmazás-azonosító jogkivonat-hozzáférés engedélyezése a.
>
>1:00 du.: a felhasználó megpróbál Navigálás – a webes alkalmazás lehetőséget. A felhasználót a rendszer átirányítja Azure AD. A webes alkalmazás bármely házirendek nem kapcsolódik, de mivel ez az alapértelmezett házirend 1 bérlői webhelyre, ez a szabály lép érvénybe. A munkamenet, cookie-k lép fel, hogy az utolsó 8 órát és a felhasználó belül kiadásának van csendes átirányítva A webalkalmazás egy új azonosító token hitelesíti őt.
>
>A felhasználó azonnal próbál webalkalmazás b elérése A felhasználót a rendszer átirányítja Azure AD. Miközben, 2 szabály lép érvénybe. A token 30 perce már bocsátotta, mint a majd felhasználótól újra megadnia a hitelesítő adatait, és egy teljesen új munkamenet és azonosító token kiállított. A felhasználó hozzáférhet webalkalmazás b

## <a name="configurable-policy-properties-in-depth"></a>Konfigurálható házirend tulajdonságai: részletes

### <a name="access-token-lifetime"></a>Hozzáférési jogkivonat élettartam

**Karakterlánc:** AccessTokenLifetime

**Érinti:** Hozzáférési jogkivonat, azonosító tokenek

**Összefoglalása:** Ez a házirend szabályozza, hogy mennyi ideig access és az erőforrás-azonosító tokenek minősülnek érvényes. A hozzáférési jogkivonat leírási idő csökkentése csökkenti a a kockázatot egy access vagy használja a kártékony szereplő hosszabb ideig (ahogy azok nem visszavont) azonosító jogkivonat, de is negatív hatással van a teljesítmény, mint a tokenek kell gyakrabban kell cserélni.

### <a name="refresh-token-max-inactive-time"></a>Jogkivonat max inaktiválása idő frissítése

**Karakterlánc:** MaxInactiveTime

**Érinti:** Tokenek frissítése

**Összefoglalása:** Ez a házirend szabályozza, hogy hogyan régi frissítés jogkivonat lehet, mielőtt egy ügyfél már nem használhatja azt egy új access/frissítés jogkivonat pár beolvasásához, ez az erőforrás elérésekor. Egy új frissítés token általában egy frissítés jogkivonat ad vissza, mivel az ügyfél kell nem ért összes erőforrást, a jelenlegi frissítés token használatával a megadott ideig a házirend megakadályozná access előtt. 

Ezzel a házirenddel hatására a felhasználók, akik nem aktív az ügyfél egy új frissítés token beolvasásához újból hitelesíteni a. 

Fontos tudni, hogy a frissítése jogkivonat Max inaktiválása idő egyetlen varianciaanalízis jogkivonat Max kor és a többtényezős frissítése jogkivonat Max életkor alacsonyabb értékeket kell beállítani.

### <a name="single-factor-refresh-token-max-age"></a>Egyetlen varianciaanalízis frissítés jogkivonat max kora

**Karakterlánc:** MaxAgeSingleFactor

**Érinti:** Tokenek frissítése

**Összefoglalása:** Mennyi ideig a házirend-vezérlők a felhasználók továbbra is használhatja frissítés tokenek szeretne lépni új access/frissítés jogkivonat párban után azok a hitelesített legutóbb sikeresen csak egyetlen tényező. Miután egy felhasználó hitelesíti és kap egy új frissítés jogkivonat, használhatja a frissítés jogkivonat folyamat, (mindaddig, amíg ki nem visszavonták a jelenlegi frissítés jogkivonat, és se maradjon még nem használt hosszabb, mint a inaktiválása idő) lesz a megadott ideig. Ezen a ponton felhasználók mindenképpen lesz egy új frissítés token fogadásához újból hitelesíteni. 

A max kor csökkentése felhasználók hitelesítését gyakrabban alkalmazást kényszerítheti. Számít, hogy egyetlen varianciaanalízis hitelesítési kevésbé biztonságos, mint egy többtényezős hitelesítés, mivel javasoljuk, hogy a házirend a többtényezős frissítése jogkivonat Max kor házirend-nél kisebb vagy egyenlő értékre van állítva.

### <a name="multi-factor-refresh-token-max-age"></a>Többtényezős frissítés jogkivonat max kora

**Karakterlánc:** MaxAgeMultiFactor

**Érinti:** Tokenek frissítése

**Összefoglalása:** Mennyi ideig a házirend-vezérlők a felhasználók továbbra is használhatja frissítés tokenek szeretne lépni új access/frissítés jogkivonat párban után azok a hitelesített legutóbb sikeresen több tényezőket. Miután egy felhasználó hitelesíti és kap egy új frissítés jogkivonat, használhatja a frissítés jogkivonat folyamat, (mindaddig, amíg ki nem visszavonták a jelenlegi frissítés jogkivonat, és se maradjon még nem használt hosszabb, mint a inaktiválása idő) lesz a megadott ideig. Ezen a ponton felhasználók mindenképpen lesz egy új frissítés token fogadásához újból hitelesíteni. 

A max kor csökkentése felhasználók hitelesítését gyakrabban alkalmazást kényszerítheti. Számít, hogy egyetlen varianciaanalízis hitelesítési kevésbé biztonságos, mint egy többtényezős hitelesítés, mivel javasoljuk, hogy a házirend értéke az egyetlen varianciaanalízis frissítése jogkivonat Max kor házirend egyenlő vagy annál nagyobb értékénél.

### <a name="single-factor-session-token-max-age"></a>Egyetlen varianciaanalízis munkamenet jogkivonat max kora

**Karakterlánc:** MaxAgeSessionSingleFactor

**Érinti:** Munkamenet tokenek (állandó és nem állandó)

**Összefoglalása:** Mennyi ideig a házirend-vezérlők felhasználó munkamenet tokenek használata új azonosító és a munkamenet tokenek azok hitelesítése sikerült csak egyetlen tényezővel utolsó után továbbra is. Miután egy felhasználó hitelesíti és kap egy új munkamenet jogkivonat, használhatja a megadott ideig a munkamenet jogkivonat folyamat (mindaddig, amíg ki nem visszavont vagy lejárt az aktuális munkamenete jogkivonathoz) lesznek. Ezen a ponton felhasználók mindenképpen lesz egy új munkamenet token fogadásához újból hitelesíteni. 

A max kor csökkentése felhasználók hitelesítését gyakrabban alkalmazást kényszerítheti. Számít, hogy egyetlen varianciaanalízis hitelesítési kevésbé biztonságos, mint egy többtényezős hitelesítés, mivel javasoljuk, hogy a házirend értéke egy többtényezős munkamenet jogkivonat Max kor házirend-nél kisebb vagy egyenlő értéket.

### <a name="multi-factor-session-token-max-age"></a>Többtényezős munkamenet jogkivonat max kora

**Karakterlánc:** MaxAgeSessionMultiFactor

**Érinti:** Munkamenet tokenek (állandó és nem állandó)

**Összefoglalása:** Mennyi ideig a házirend-vezérlők felhasználó munkamenet tokenek használata új azonosító és a munkamenet tokenek azok hitelesítése sikerült több tényezők az utolsó után továbbra is. Miután egy felhasználó hitelesíti és kap egy új munkamenet jogkivonat, használhatja a megadott ideig a munkamenet jogkivonat folyamat (mindaddig, amíg ki nem visszavont vagy lejárt az aktuális munkamenete jogkivonathoz) lesznek. Ezen a ponton felhasználók mindenképpen lesz egy új munkamenet token fogadásához újból hitelesíteni. 

A max kor csökkentése felhasználók hitelesítését gyakrabban alkalmazást kényszerítheti. Számít, hogy egyetlen varianciaanalízis hitelesítési kevésbé biztonságos, mint egy többtényezős hitelesítés, mivel javasoljuk, hogy a házirend értéke az egyetlen varianciaanalízis munkamenet jogkivonat Max kor házirend egyenlő vagy annál nagyobb értékénél.

## <a name="sample-token-lifetime-policies"></a>Példa az élettartam házirendek

Az összes típusú új forgatókönyveket lehetséges az Azure Active Directory nem hozhat létre és kezelhet az alkalmazások, szolgáltatás alapelvei és a teljes bérlői jogkivonat élettartama közzététele.  Néhány házirendjének esetei, amellyel új szabályairól bevezetése végigvezetik megyünk:


- Jogkivonat élettartama
- Jogkivonat Max inaktiválása idő
- Jogkivonat Max kora

Módszeren végigvezetjük néhány olyan esetek, például:


- A bérlő alapértelmezett házirendek kezelése
- A webes bejelentkezés házirend létrehozása
- Házirend-elérje a webes API-t natív alkalmazások létrehozása
- Egy speciális házirendek kezelése 

### <a name="prerequisites"></a>Előfeltételek
A minta forgatókönyvekben azt fogja kell létrehozása, módosítása, csatolása és alkalmazások, szolgáltatás alapelvei és a teljes bérlői házirendeket törlésével.  Ha még kezdő az Azure Active Directory, a kivétel [Ez a cikk](active-directory-howto-tenant.md) segítséget nyújt az első lépések a következő mintát a folytatás előtt.  


1. A kezdéshez töltse le a legújabb [Azure Active Directory PowerShell-parancsmag előzetes](https://www.powershellgallery.com/packages/AzureADPreview). 
2.  Ha befejezte az Azure Active Directory PowerShell-parancsmagok, futtassa a csatlakozás parancs jelentkezhet be az Azure Active Directory-rendszergazdai fiókjával. Kövesse az alábbi lépéseket minden indításakor új munkamenet kell.
        
        Connect-AzureAD -Confirm

3.  Lásd: a bérlői webhelyén létrehozott összes házirendek a következő parancs futtatásával.  Ez a parancs az alábbi helyzetekben a legtöbb műveletek után kell használni.  Azt is segít a házirendek **Objektumazonosító** képet kaphat. 
        
        Get-AzureADPolicy

### <a name="sample-managing-a-tenants-default-policy"></a>Példa: Egy bérlői alapértelmezett házirendek kezelése

Ez a példa azt egy házirendet, amely lehetővé teszi a felhasználóknak, hogy jelentkezzen be a teljes bérlői webhelyen keresztül ritkábban hoz létre. 

Ehhez az egyetlen varianciaanalízis frissítése tokenek végig a bérlő alkalmazott hozzunk létre egy jogkivonat élettartam házirend. Ezzel a házirenddel minden alkalmazás, a bérlői webhelyén, és minden szolgáltatás, amely még nem rendelkezik a hozzá beállított házirend fő fog vonatkozni. 

1.  **Élettartam házirend létrehozása.** 

Állítsa a egyetlen varianciaanalízis frissítése jogkivonat azt nem járjon le, amíg az access visszavonták "addig, amíg visszavont" jelentését.  Az alábbi házirend-definícióhoz, mit azt fog szervezni:
        
        @("{
          `"TokenLifetimePolicy`":
              {
                 `"Version`":1, 
                 `"MaxAgeSingleFactor`":`"until-revoked`"
              }
        }")

Futtassa a következő parancsot a házirend létrehozásához. 

        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1, `"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName TenantDefaultPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
        
Az új házirend és annak objektumazonosító első, a következő parancsot.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **a házirend módosítása**

Úgy döntött, hogy az első házirend nem igazán olyan szigorú a szolgáltatást igényel, és azt szeretné, hogy a egyetlen varianciaanalízis frissítése tokenek 2 nap múlva lejár webhelyekre. A következő parancsot. 
        
    Set-AzureADPolicy -ObjectId <ObjectID FROM GET COMMAND> -DisplayName TenantDefaultPolicyUpdatedScenario -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"2.00:00:00`"}}")
        
&nbsp;&nbsp;3. a **ezzel elkészült!** 

### <a name="sample-creating-a-policy-for-web-sign-in"></a>Példa: A webes bejelentkezés házirend létrehozása

Ez a példa azt hoz létre egy házirendet, amely esetén kell azonosítania a felhasználók gyakrabban a webalkalmazásba. Ezzel a házirenddel az Access/azonosító tokenek és a Max kora többtényezős munkamenet jogkivonat élettartama állítja be a szolgáltatás egyszerű a web App.

1.  **Élettartam házirend létrehozása.**

Webes bejelentkezés házirendet – 2 órán hozzáférési/azonosítóját az élettartam és a Max varianciaanalízis egyetlen munkamenethez jogkivonat kor állítja.

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"AccessTokenLifetime`":`"02:00:00`",`"MaxAgeSessionSingleFactor`":`"02:00:00`"}}") -DisplayName WebPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy

Az új házirend és annak objektumazonosító első, a következő parancsot.

    Get-AzureADPolicy
&nbsp;&nbsp;2. a **a házirend hozzárendelése a szolgáltatás egyszerű.**

Hivatkozás: Ez a szolgáltatás egyszerű új házirend megyünk.  A fő szolgáltatás a **objektumazonosító** elérése oly módon is szüksége lesz. A [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) lekérdezése a vagy nyissa meg a [Diagram Explorer eszközre](https://graphexplorer.cloudapp.net/) , és jelentkezzen be az Azure Active Directory-fiókját a bérlő szolgáltatás rendszerbiztonsági megjelenítéséhez. 

Ha befejezte a **objektumazonosító**, futtassa a következő parancsot.
        
    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
&nbsp;&nbsp;3. a **ezzel elkészült!** 

 

### <a name="sample-creating-a-policy-for-native-apps-calling-a-web-api"></a>Példa: A webes API hívása natív alkalmazások házirend létrehozása

>[AZURE.NOTE]
>Házirendek csatolása alkalmazások jelenleg le van tiltva.  A munkafüzetek engedélyezéséről a hamarosan dolgozunk.  Ez a lap frissül, amint a funkció akkor érhető el.

Ez a példa azt hoz létre egy házirendet, amely szükségessé teszi a felhasználó hitelesítő kisebb és fog hosszabbíthatja Ez lehet inaktív anélkül, hogy újra hitelesítő idő mennyiségét. A házirend fog vonatkozni a webes API-hoz, így, amikor a natív alkalmazás kér erőforrásként ezzel a házirenddel fog vonatkozni.

1.  **Hozzon létre egy jogkivonat élettartamának házirendet.** 

Ez a parancs a webes API szigorú házirend hoz létre. 
        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"30.00:00:00`",`"MaxAgeMultiFactor`":`"until-revoked`",`"MaxAgeSingleFactor`":`"180.00:00:00`"}}") -DisplayName WebApiDefaultPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy
         
Az új házirend és annak objektumazonosító első, a következő parancsot.

    Get-AzureADPolicy

&nbsp;&nbsp;2. **a webes API a házirend hozzárendelése**.

Az új házirend-alkalmazáshoz csatolás megyünk.  Az alkalmazás a **objektumazonosító** elérése oly módon is szüksége lesz. Keresse meg az alkalmazás **objektumazonosító** legjobb módja használni az [Azure-portálon](https://portal.azure.com/). 

Ha befejezte a **objektumazonosító**, futtassa a következő parancsot.

    Add-AzureADApplicationPolicy -ObjectId <ObjectID of the App> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. a **ezzel elkészült!** 

### <a name="sample-managing-an-advanced-policy"></a>Példa: Egy speciális házirendjének kezelése 

Ez a példa azt mutatja be a prioritás rendszer működéséről, és hogyan kezelheti a több objektumok hozzárendelve több házirendek néhány házirendek hoz létre. Fog betekintést néhány fentiekben házirendek prioritás, és segítséget is bonyolultabb esetek kezelése. 

1.  **Élettartam házirend létrehozása**

Az eddigi meglehetősen egyszerű. Már létrehozott egy bérlői alapértelmezett házirendet, amely egyetlen varianciaanalízis frissítése az élettartam állítja be a 30 napra. 

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"30.00:00:00`"}}") -DisplayName ComplexPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
Keresse meg az új házirendet, és el nem objektumazonosító, futtassa a következő parancsot.
 
    Get-AzureADPolicy

&nbsp;&nbsp;2. **a szolgáltatás egyszerű házirend hozzárendelése**

A házirend most a teljes bérlői van.  Tegyük fel, hogy azt meg szeretné őrizni a 30 napos házirend az egy adott szolgáltatás egyszerű, de a felső határa "addig, amíg visszavont" kell bérlői alapértelmezett házirend módosítása. 

Első lépésként megyünk hivatkozás: Ez a szolgáltatás egyszerű új házirend.  A fő szolgáltatás a **objektumazonosító** elérése oly módon is szüksége lesz. A [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) lekérdezése a vagy nyissa meg a [Diagram Explorer eszközre](https://graphexplorer.cloudapp.net/) , és jelentkezzen be az Azure Active Directory-fiókját a bérlő szolgáltatás rendszerbiztonsági megtekintéséhez. 

Ha befejezte a **objektumazonosító**, futtassa a következő parancsot.

    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **beállítása a IsTenantDefault jelölőre való hamis használatáról a következő parancsot**. 

    Set-AzureADPolicy -ObjectId <ObjectId of Policy> -DisplayName ComplexPolicyScenario -IsTenantDefault $false
&nbsp;&nbsp;4. **Új bérlő alapértelmezett házirend létrehozása**

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName ComplexPolicyScenarioTwo -IsTenantDefault $true -Type TokenLifetimePolicy

&nbsp;&nbsp;5. **ezzel elkészült!** 

Ekkor az eredeti házirend kapcsolódik a szolgáltatás egyszerű és az új házirendet a bérlői alapértelmezett házirend értéke.  Fontos, hogy ne feledje, hogy, hogy a szolgáltatás rendszerbiztonsági alkalmazott házirendek elsőbbséget élveznek bérlői alapértelmezett házirendeket. 


## <a name="cmdlet-reference"></a>Parancsmagjai – referencia

### <a name="manage-policies"></a>Házirendek kezelése
A következő parancsmagok házirendek kezeléséhez használható.</br></br>



#### <a name="new-azureadpolicy"></a>Új AzureADPolicy
Létrehoz egy új házirendet.

    New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsTenantDefault <boolean> -Type <Policy Type> 

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Meghatározása| A szabály a szabályokat tartalmazó stringified JSON tömbje.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-DisplayName|A házirend nevére a karakterlánc|-DisplayName MyTokenPolicy
-IsTenantDefault|Ha igaz állítja be a házirendet, mint az adott bérlői alapértelmezett házirend, hamis, ha nem módosítja|-IsTenantDefault $true
-Típus|A házirend-jogkivonat élettartama mindig típusú "TokenLifetimePolicy" használata|-TokenLifetimePolicy típus
-AlternativeIdentifier [választható]|Alternatív azonosító állítja a házirend.|-AlternativeIdentifier myAltId
</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy         
Kap AzureAD házirendek vagy a megadott házirend. 
        
    Get-AzureADPolicy 

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító [választható]|Az objektum a szeretné, hogy a szabály azonosítója. |-Objektumazonosító &lt;objektumazonosító házirend&gt; 
</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject         
Minden alkalmazás és a kapcsolódó házirend szolgáltatás rendszerbiztonsági módja
        
    Get-AzureADPolicyAppliedObject -ObjectId <object id of policy> 

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum a szeretné, hogy a szabály azonosítója.|-Objektumazonosító &lt;objektumazonosító házirend&gt;
</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Meglévő házirend frissítése
        
    Set-AzureADPolicy -ObjectId <object id of policy> -DisplayName <string> 
 
Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum a szeretné, hogy a szabály azonosítója.|-Objektumazonosító &lt;objektumazonosító házirend&gt;
-DisplayName|A házirend nevére a karakterlánc|-DisplayName MyTokenPolicy
-Definíciós [választható]|A szabály a szabályokat tartalmazó stringified JSON tömbje.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-IsTenantDefault [választható]|Ha igaz állítja be a házirendet, mint az adott bérlői alapértelmezett házirend, hamis, ha nem módosítja|-IsTenantDefault $true
-Típus [választható]|A házirend-jogkivonat élettartama mindig típusú "TokenLifetimePolicy" használata|-TokenLifetimePolicy típus
-AlternativeIdentifier [választható]|Alternatív azonosító állítja a házirend.|-AlternativeIdentifier myAltId
</br></br>

#### <a name="remove-azureadpolicy"></a>Eltávolítás-AzureADPolicy         
A megadott házirend törlése

     Remove-AzureADPolicy -ObjectId <object id of policy>

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum a szeretné, hogy a szabály azonosítója.|-Objektumazonosító &lt;objektumazonosító házirend&gt;
</br></br>

### <a name="application-policies"></a>Alkalmazás-házirendek
A következő parancsmagok használható az alkalmazás-házirendek.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>AzureADApplicationPolicy hozzáadása         
Az alkalmazás a megadott házirend hivatkozások

    Add-AzureADApplicationPolicy -ObjectId <object id of application> -RefObjectId <object id of policy>

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum az alkalmazás azonosítója.|-Objektumazonosító &lt;objektumazonosító alkalmazás&gt; 
-RefObjectId|Az objektum a szabály azonosítója. |-RefObjectId &lt;objektumazonosító házirend&gt;
</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy        
Az alkalmazás házirendje módja

    Get-AzureADApplicationPolicy -ObjectId <object id of application>

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum az alkalmazás azonosítója.|-Objektumazonosító &lt;objektumazonosító alkalmazás&gt; 
</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Eltávolítás-AzureADApplicationPolicy        
Eltávolítja a házirend-alkalmazás

    Remove-AzureADApplicationPolicy -ObjectId <object id of application> -PolicyId <object id of policy>

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum az alkalmazás azonosítója.|-Objektumazonosító &lt;objektumazonosító alkalmazás&gt; 
-Irányelv azonosítója| A házirend objektumazonosító.|-Irányelv azonosítója &lt;objektumazonosító házirend&gt;
</br></br>

### <a name="service-principal-policies"></a>Szolgáltatás fő házirendek
A következő parancsmagok szolgáltatás fő házirendjeihez tartozó használható.</br></br>

#### <a name="add-azureadserviceprincipalpolicy"></a>AzureADServicePrincipalPolicy hozzáadása         
A megadott házirend csatol egy egyszerű

    Add-AzureADServicePrincipalPolicy -ObjectId <object id of service principal> -RefObjectId <object id of policy>

Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum az alkalmazás azonosítója.|-Objektumazonosító &lt;objektumazonosító alkalmazás&gt; 
-RefObjectId|Az objektum a szabály azonosítója. |-RefObjectId &lt;objektumazonosító házirend&gt;
</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy        
Bármely csatolt a megadott szolgáltatás egyszerű házirend módja

    Get-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>
 
Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum az alkalmazás azonosítója.|-Objektumazonosító &lt;objektumazonosító alkalmazás&gt; 
</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Eltávolítás-AzureADServicePrincipalPolicy         
A házirend eltávolítja a megadott szolgáltatás fő

    Remove-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>  -PolicyId <object id of policy>
 
Paraméterek|Leírás|Példa|
-----| ----- |-----|
-Objektumazonosító|Az objektum az alkalmazás azonosítója.|-Objektumazonosító &lt;objektumazonosító alkalmazás&gt; 
-Irányelv azonosítója| A házirend objektumazonosító.|-Irányelv azonosítója &lt;objektumazonosító házirend&gt;
