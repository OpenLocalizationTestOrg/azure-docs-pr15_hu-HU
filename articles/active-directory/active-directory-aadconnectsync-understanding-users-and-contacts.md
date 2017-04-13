<properties
    pageTitle="Azure AD Connect szinkronizálása: ismertetése felhasználók és a partnerek |} Microsoft Azure"
    description="Felhasználók és a partnerek Azure AD Connect szinkronizálás ismerteti."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect szinkronizálása: ismertetése felhasználók és a partnerek

Több oka különböző miért azt szeretné, hogy több Active Directory-erdők, és nincsenek számos különböző telepítési topológiák. Gyakori modellek után egy egyesülés & WIA-fiók-erőforrás-példányhoz és a globális Címlistában sync'ed erdők tartalmazzák. De még akkor sem ha tiszta modellek, hibrid modellek közös is. Az alapértelmezett beállítások Azure AD Connect szinkronizálás nem tegyük fel, egy adott modell, de attól függően, hogy hogyan felhasználói egyező lehetőséget választotta a telepítési útmutatója, különböző beállításokat kell tartani.

Ez a témakör azt lépjen keresztül az alapértelmezett beállítások az egyes topológiák viselkedését. Megyünk beállítása révén, és tekintse meg a konfigurációt a szinkronizálás szabályok szerkesztő használható.

A konfiguráció feltételezi, hogy néhány általános szabályok vonatkoznak:

- Függetlenül attól, hogy importálása a forrás aktív könyvtárak sorrendjét a végeredmény mindig azonosnak kell lennie.
- Az aktív fiókot a bejelentkezési adatokat, beleértve a **userPrincipalName** és **sourceAnchor**mindig járul.
- Letiltott fiók járul userPrincipalName és sourceAnchor, kivéve, ha egy csatolt postaládához, akkor, ha nincs aktív fiók találhatók.
- Egy fiókot egy csatolt postaládát soha nem lesz a userPrincipalName és sourceAnchor. Feltételezzük, hogy az aktív fiókot később találhatók.
- Egy partner objektum előfordulhat, hogy kell építenie Azure Active Directory partnerként vagy felhasználóként. Nem igazán tudja, hogy amíg az összes adatforrás Active Directory-erdők dolgozott.

## <a name="contacts"></a>Kapcsolattartók

A névjegyek, amely egy másik erdő felhasználó, akkor közös egy egyesülés & WIA után hol a GALSync megoldást az összekötő két vagy több Exchange erdők. A kapcsolattartó objektumot mindig a metaverse a Posta attribútum használatával történő csatlakozás a összekötő tér át. Ha már van egy kapcsolattartó vagy felhasználói objektum mail ugyanazt a címet, az objektumokat egymáshoz. Ez a szabály konfigurációja **az Active Directory – partner Bekapcsolódás a**. Is van nevű szabály **az Active Directory – gyakori partner a** -attribútum illusztrálja a metaverse az attribútum **sourceObjectType** az állandó **partner**. Ez a szabály elsőbbséget nagyon kicsi, ha bármelyik felhasználó objektum csatlakozik, az azonos metaverse objektumra, majd a szabály **a az Active Directory – felhasználói közös** attribútum értéket felhasználó hozzájárul. Ez a szabály az attribútum fog rendelkezni az ügyfél, ha már csatlakozott a felhasználó nem és érték a felhasználó ha legalább egy felhasználónak talált.

Az Azure Active Directory-objektum kiépítési, a **kifelé AAD – csatlakozás lépjen kapcsolatba a** kimenő szabály hoz létre egy kapcsolattartó-objektum, **lépjen kapcsolatba**a metaverse attribútum **sourceObjectType** értéke. Ha az attribútum **felhasználónak**van beállítva, majd a szabály **kifelé a AAD – felhasználói Bekapcsolódás** hoz létre a user objektumban helyette.
Akkor lehet, hogy egy objektum van előléptetett névjegykártyából felhasználó számára további forrás aktív könyvtárak importálása és szinkronizálása vannak.

Ha például GALSync topológiában azt kifejezésre keresve kapcsolattartói objektumok mindenki számára a második erdő azt importálni az első erdő. Ez lesz szakaszban a AAD Connector új kapcsolattartói objektumok. Azt később importálása, és a második erdő szinkronizálni, azt a valós felhasználók megtalálja, és a meglévő metaverse objektumokat Bekapcsolódás őket. Azt fogja a kapcsolattartó AAD objektum törlése, majd hozzon létre egy új felhasználói objektum ehelyett.

Ha van egy topológia hol felhasználók és a partnerek jeleníti meg, mindenképpen jelölje ki a megfelelő felhasználók a levelezési attribútum a telepítési útmutató. Ha egy másik lehetőséget választja, majd lesz egy sorrendje függő beállítást. A kapcsolattartói objektumok mindig a levelezési attribútum fog csatlakozhat, de a felhasználói objektumok csatlakozás a Posta attribútum csak lesz, ha ezt a lehetőséget választotta, a telepítési útmutató. Akkor sikerült majd a végeredmény az azonos e-mail attribútumot tartalmazó metaverse két különböző objektumok, ha a kapcsolattartó objektumot importálta, mielőtt a user objektumban. Azure AD az exportálás során az hibaüzenetet fog elő. Ez a jelenség szándékosan van így, és azt jelzi, hibás adatok vagy az, hogy a topológia megfelelően nem azonosítható a telepítés során.

## <a name="disabled-accounts"></a>Letiltott fiók

Letiltott fiókok és Azure Active Directory szinkronizálódnak. Letiltott fiók megegyeznek a jelenítik meg az Exchange, például üléstermek erőforrásokat. A kivétel, a csatolt postaláda; rendelkező felhasználók korábban említett-fiók felvétele az Azure Active Directory soha nem ezek fog rendelkezni.

Azon a feltételezésen esetén, hogy a letiltott felhasználói fiókok megtalálható, azt nem találja meg egy másik aktív fiókot később és, majd az objektum már kiépítve Azure ad a userPrincipalName és sourceAnchor található. Abban az esetben, ha egy másik aktív fiókot metaverse ugyanazon objektumhoz fog csatlakozni, majd a userPrincipalName és sourceAnchor lesz.

## <a name="changing-sourceanchor"></a>SourceAnchor módosítása

Ha egy objektum exportálva az Azure Active Directory és a nem engedélyezett a sourceAnchor eltűnt módosítása. Az objektum exportálásakor be van állítva a metaverse attribútum **cloudSourceAnchor** a az Azure Active Directory elfogadta **sourceAnchor** értéket. Ha **sourceAnchor** változik, és nem egyezik meg **cloudSourceAnchor**, a **kifelé AAD – felhasználói Bekapcsolódás a** szabály fog throw hiba **sourceAnchor attribútum megváltozott**. Ebben az esetben a konfigurációs vagy az adatok ki kell javítani, az azonos sourceAnchor megtalálható a metaverse újra előtt az objektum szinkronizálható újra.

## <a name="additional-resources"></a>További források

* [Azure Active Directory csatlakozás szinkronizálása: A szinkronizálás beállításainak testreszabása](active-directory-aadconnectsync-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
