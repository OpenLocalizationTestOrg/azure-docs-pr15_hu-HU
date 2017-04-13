<properties
    pageTitle="Azure Active Directory-alkalmazások kezelése |} Microsoft Azure"
    description="Ez a cikk az Azure Active Directory integrálása a helyszíni, a felhő és a szoftver alkalmazások előnyei."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Azure Active Directory-alkalmazások kezelése

Túl a tényleges munkafolyamat vagy a tartalom vállalkozások van az összes alkalmazás két alapvető követelményei:

1. A hatékonyság növelése, kérelmeket kell egyszerű felderítése és hozzáférés

2. Biztonság és irányítási engedélyezéséhez a szervezetének szükséges vezérlők, és a felhasználók és ténylegesen éri el minden alkalmazás betekintése

Felhőalapú alkalmazások földrajzi Ez leginkább érhető el ",*hogy KIK kereshetnek MILYEN műveleteket*" vezérlőre azonosító használatával.

A számítások kifejezések:

- *Kik* nevezik *identitás* - felhasználók és csoportok kezelése

- *Milyen* nevezik *Hozzáférés-kezelés* – védett erőforrásokhoz való hozzáférés kezelése

Mindkét összetevő közös tudjuk *identitás- és az Access-kezelés (IAM)*, amely határozzák meg a [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) csoport, mint "*, amely lehetővé teszi, hogy a megfelelő személyek elérése a megfelelő erőforrások jobb oldali biztonsági tantárgy a megfelelő okokból időpontok*".

Rendben, mi is a probléma? Ha IAM *nem felügyelt* egy integrált megoldáskészítő egy helyen:

- Identitás rendszergazdák egyenként létrehozása és frissítése a felhasználói fiókokat az összes alkalmazás külön-külön kell egy felesleges és időigényes tevékenységet.

- Felhasználók kell memorize több hitelesítő adatok eléréséhez az alkalmazások azokat meg kell használnia. Emiatt felhasználók általában jegyezze fel a jelszavát, vagy más jelszó kezelése megoldást, amely bemutatja a más adatok biztonsági kockázatot használja.

- Felesleges, időigényes tevékenységek Rövidítse le a felhasználók és a rendszergazdák dolgozik, amely az üzleti alsó sor növelése üzleti tevékenységek.

Igen mi általában megakadályozza, hogy az integrált IAM megoldások elfogadásakor szervezetek?

- A műszaki megoldások szoftver platformokon rendszerbe, és a saját alkalmazások minden szervezet által módosítani kell alapulnak.

- A nagyobb, mint a szervezet informatikai integrálhatja meglévő IAM megoldásokkal díjazott gyakran elfogadása a felhőben alkalmazások van.

- Biztonság és a figyeléshez szerszámok szükség további testreszabásokat és integráció átfogó E2E esetek eléréséhez.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory integrált alkalmazások

Azure Active Directory a Microsoft átfogó identitás (IDaaS) szolgáltatás megoldásként, amely:

- Lehetővé teszi, hogy egy felhőalapú szolgáltatásba, IAM 

- Itt központi access-egyszeri bejelentkezés (SSO) és a jelentési kezelése 

- Támogatja az integrált [alkalmazások ezer](https://azure.microsoft.com/marketplace/active-directory/) az alkalmazás a gyűjteményben, köztük Salesforce, a Google Apps, a mezőbe, a Concur és más hozzáférés-kezelés. 


Az Azure Active Directory összes alkalmazás közzététele a partnerek számára, és ügyfelek (üzleti vagy fogyasztói) azonos identitással rendelkezik, és kezelési funkciók elérése.<br> Ez lehetővé teszi, hogy jelentősen csökkenti a működési költségeket.

Mi történik, ha szeretne olyan alkalmazás, amely még nem szerepel a alkalmazás gyűjtemény megvalósítása? Most egy kicsit több időt vesz igénybe, mint az egyszeri bejelentkezés beállítása a alkalmazás gyűjteményből alkalmazások, miközben Azure Active Directory biztosít, amely megkönnyíti a konfigurációs varázslóval.

Azure Active Directory értékét a "csak" felhő alkalmazások túli kerül. Is vele a helyszíni alkalmazásokkal, mert a biztonságos távoli hozzáférés. A biztonságos távelérési kiküszöbölheti a VPN adatai és a többi hagyományos távelérési management megvalósítás meg.

Az összes alkalmazás központi hozzáférés-kezelés és az egyszeri bejelentkezés (SSO) megadásával Azure Active Directory ismertetése a legfontosabb adatok biztonsági és termelékenység kapcsolatos problémák megoldása

- Felhasználók hozzá tudnak férni az egy bejelentkezési több időt biztosítva bevétel létrehozásakor vagy üzleti végzett műveletek tevékenységek több alkalmazást.

- Identitás rendszergazdák kezelhetik az access-alkalmazások – egy helyen.

A felhasználó és a vállalat előnye egyértelmű. Nézzük előnyökkel jár részletesebben az identitás-rendszergazda, és a szervezet.

## <a name="integrated-application-benefits"></a>Integrált alkalmazások legfontosabb előny

Az egyszeri bejelentkezési folyamat két lépést foglal magában:

- Hitelesítés, a felhasználó az identitás ellenőrzése során.

- Engedély, a döntési való hozzáférés engedélyezése vagy letiltása az access-házirend egy erőforrás.

Azure AD a alkalmazások kezeléséhez, és az egyszeri bejelentkezés engedélyezése:

- Hitelesítés befejeződött, a felhasználó a helyszíni (például az Active Directory) vagy Azure AD-fiókkal.

- Az Azure Active Directory hozzárendelés és védelme házirend egységes végfelhasználói élmény biztosítva, és úgy hozzárendelés, helyek és MFA feltételek hozzáadása egy alkalmazást, függetlenül a belső képességei végrehajtja a engedélyt.

Fontos, hogy a engedélyezési módja a célalkalmazás végrehajtása tisztában eltér, attól függően, hogy az alkalmazás volt integrálva Azure AD.

- **Szolgáltató által előre integrált alkalmazások** Office 365-ben és az Azure, például ezek közvetlenül készült Azure Active Directory és a teljes identitás- és az access szolgáltatásairól a támaszkodva alkalmazásokat. Ezeket az alkalmazásokat a hozzáférést a címtár-információk és a token kiállítási engedélyezve van.

- **A Microsoft és az egyéni alkalmazások előre integrált alkalmazások** Ezek a független felhő alkalmazások, amely egy belső alkalmazás könyvtárában támaszkodhat és Azure Active Directory függetlenül is működnek. Ezeket az alkalmazásokat az Access-alkalmazás adott hitelesítőadat-alkalmazás fiókhoz csatlakoztatott azáltal érhető el. Attól függően, hogy az alkalmazás funkciók a hitelesítő adatok lehet összevonási jogkivonat vagy -felhasználónevet és egy, az alkalmazás korábban kiépített fiók jelszava.

- **A helyszíni alkalmazások** Alkalmazások közzétett elsősorban a hozzáférést a helyszíni környezetbe applications engedélyezése Azure AD-alkalmazás proxyn keresztül. A helyi könyvtárában, például a Windows Server Active Directory egy központi használja arra, ezeket az alkalmazásokat. Ezeket az alkalmazásokat a hozzáférést a proxy kiváltó szerint engedélyezve van a felhasználó az alkalmazás tartalmát végzi a bejelentkezés kötelező helyszíni síkjai.

Például ha egy felhasználó csatlakozik a szervezeti, szüksége a felhasználó fiók létrehozása az Azure Active Directory az elsődleges bejelentkezési műveletekhez. Ha a felhasználó egy felügyelt alkalmazást, például a Salesforce hozzáférésre van szüksége, is kell-e a felhasználói fiók létrehozása a Salesforce, és csatolja az Azure-fiókjába, hogy egyszeri bejelentkezés használata. Amikor a felhasználó elhagyja a szervezet, célszerű az Azure Active Directory-fiók törlése, és az összes partner fiókok lehetőséget a IAM a tárolja, az alkalmazások, a felhasználónál meg volt a hozzáférést.

## <a name="access-detection"></a>Az Access kimutatására

Modern vállalkozások, az informatikai részlegeknek ismerik gyakran nem használt összes felhő alkalmazást. Cloud App feltárás együtt Azure Active Directory biztosít a megoldást feltárása ezeket az alkalmazásokat.

## <a name="account-management"></a>Fiókok kezelése

Régebben a különféle alkalmazásokat kezelésére a kézi végrehajtás által elvégzett informatikai vagy személyes a szervezet támogatja. Azure Active Directory számlakezelési teljesen automatikus összes szolgáltató integrált alkalmazások és az automatizált felhasználói kiépítése támogatására Microsoft, mind a SAML igény szerinti előre integrált alkalmazások.

## <a name="automated-user-provisioning"></a>Az automatikus felhasználói kiépítése

Egyes alkalmazások létrehozása és eltávolítása (vagy az Inaktiválás) fiókok automatizálási felületek biztosítanak. Ha egy szolgáltatót az ilyen felületet kínál, Azure Active Directory által kapcsolatos károkozásra. Csökkenti a működési költségek, mivel a felügyeleti feladatok automatikusan fordulhat elő, és javítja a környezet biztonsága, mivel a így csökken, ezzel az illetéktelen hozzáférést lehetőségét.

## <a name="access-management"></a>Hozzáférés kezelése

Azure Active Directory használatával kezelheti használó egyéni alkalmazások vagy hozzárendelések alapú szabály elérésére. Akkor is átadhatja hozzáférés kezelése a szervezet a legjobb betekintése biztosítva, és az IT-részleghez terhet csökkentése a megfelelő személyekkel.

## <a name="on-premises-applications"></a>A helyszíni környezetbe applications

A beépített alkalmazásban proxy lehetővé teszi, hogy a felhasználók, így a helyszíni alkalmazások közzététele élmény modern felhő alkalmazás és a előnyét is egységes elérhető Azure AD-figyelés, jelentéskészítés és biztonsági funkciók.

## <a name="reporting-and-monitoring"></a>Jelentések és figyelése

Azure Active Directory biztosít előre integrált jelentése és figyelése funkciók, amelyek lehetővé teszik, hogy az alkalmazáshoz, és azokat ténylegesen használni őket hozzáféréssel rendelkező személyek közül.

## <a name="related-capabilities"></a>Kapcsolódó funkciók

Az Azure Active Directory az alkalmazások részletes hozzáférési és előre integrált MFA biztonságát. További információ a Azure MFA információ [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Első lépések

Első lépésként alkalmazások integrálása az Azure Active Directory nézze meg az [integrálása Azure Active Directory-alkalmazásokkal az első lépések útmutató](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Lásd még:

[Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)
