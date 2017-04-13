<properties
   pageTitle="Integrálása az Azure Active Directory |} Microsoft Azure"
   description="Segédvonalhoz előnyei és erőforrások integrálása az Azure Active Directory számára."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integrálása az Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory biztosít a vállalati szintű Identitáskezelés felhő alkalmazások rendelkező szervezetek.  Azure Active Directory-integráció ad a felhasználóknak egy egyszerűsített bejelentkezés módja, és az informatikai házirend felel meg alkalmazás segítségével.

## <a name="how-to-integrate"></a>Hogyan integrálja

Többféleképpen integrálása az Azure Active Directory az alkalmazás.  Kihasználhatja annyi vagy kevés az alkalmazás megfelelő forgatókönyvekben.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Azure AD egy mód, ha az alkalmazás bejelentkezési támogatása

**Csökkentse a súrlódás bejelentkezés, és a támogatási költségek csökkentése.** Jelentkezzen be az alkalmazás használatával Azure Active Directory, a felhasználók ne kelljen egy további nevét és jelszavát, ne feledje, hogy.  Fejlesztői egy kisebb tárolására és védelme jelszó lesz.  Elfelejtett jelszó alaphelyzetbe állítása kezelése nem rendelkező, lehet, hogy egy egyedül jelentős megtakarítási.  Azure Active Directory bejelentkezés hatásköröket az egyes a világ legnépszerűbb felhő alkalmazásairól – többek között az Office 365-ben és a Microsoft Azure.  A száz milliós szervezetek milliónyi felhasználóit, akkor minden bizonnyal a felhasználó már bejelentkezve Azure AD.  További tudnivalók [az Azure Active Directory bejelentkezési hozzáadása támogatása](../active-directory-authentication-scenarios.md).

**Egyszerűsítése jelentkezzen be az alkalmazás.**  Jelentkezzen be az alkalmazás, alatt Azure Active Directory elküldheti felhasználó alapvető információ, hogy előre töltse ki a bejelentkezési képernyőn felfelé, vagy kiküszöbölheti a teljesen.  Felhasználói regisztrálhatnak az alkalmazás a Azure AD-fiókkal, a már jól ismert hozzájárulása használhatóság hasonlít a közösségi média és mobilalkalmazások található keresztül.  Bármelyik felhasználó is regisztráció, és jelentkezzen be egy alkalmazást, amely anélkül, hogy informatikai bevonása integrálva Azure AD.  További tudnivalók az [aláíró felfelé az alkalmazás az Azure Active Directory-fiókkal bejelentkezni](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Tallózással keresse meg a felhasználóknak, kezelheti a felhasználó kiépítési és az alkalmazás való hozzáférés szabályozása

**Keresse meg a felhasználó a címtárban.**  A diagram API segítségével segíti a felhasználókat, kereshet és böngészhet az a szervezet más tagjai, amikor felkérhet másokat a vagy hozzáférést helyett kelljen írja be az e-mail címeket.  Felhasználók megkeresheti egy már jól ismert cím könyv stílus csatolófelület, például a szervezeti hierarchia részleteinek megtekintése.  További tudnivalók a [Graph API](../active-directory-graph-api.md).

**Újra felhasználhat az Active Directory-csoportok és az ügyfél már van-e kezelni a terjesztési listák.**  Azure AD az ügyfél már e-mail terjesztési használatának és hozzáférés kezelése a csoportokat tartalmazza.  A diagram API-t használ, ezek a csoportok létrehozása és kezelése egy külön csoporthalmaz az alkalmazás az ügyfél igénylő helyett felhasználás.  Csoportadatok is lehet küldeni a bejelentkezési tokenek az alkalmazást.  További tudnivalók a [Graph API](../active-directory-graph-api.md).

**Azure Active Directory vezérlőelemre, aki rendelkezik hozzáféréssel a alkalmazást használja.**  Rendszergazdák és a alkalmazás tulajdonosok az Azure Active Directory access oszthatnak alkalmazások adott felhasználókhoz és csoportokhoz.  A diagram API-t használ, olvassa el a ebben a listában, és segítségével kiépítési és vonja kiépítési az erőforrások és a hozzáférési az alkalmazáson belül.

**Azure AD-szerepkörök alapuló hozzáférés-vezérlés.**  Rendszergazdák és a tulajdonosok alkalmazás oszthatnak felhasználók és csoportok szerepkörök, amikor regisztrál az alkalmazás Azure AD meg.  Szerepkör-információ küldi el az alkalmazás a tokenek bejelentkezés, és is olvasható a Graph API.  További tudnivalók az [Azure AD-hitelesítés használatával](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Felhasználói profil, a naptár, e-mailek, névjegyek, fájlok és további való hozzáférés

**Azure AD az a engedélyezési kiszolgáló, az Office 365 és az egyéb Microsoft üzleti szolgáltatásokat.**  Ha való bejelentkezéshez támogatás az aktuális felhasználói fiókok csatolása Azure AD-fiókokkal OAuth 2.0-s verzióját használja, vagy az alkalmazás támogatja az Azure Active Directory, kérheti olvasási és írási hozzáféréssel a felhasználói profil, naptár, e-mailek, névjegyek, fájlok és egyéb információkat.  Zökkenőmentes írhat események felhasználó naptárt, és olvassa el vagy fájlok írja be a saját onedrive-ra.  További tudnivalók [az Office 365 API elérése](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Az alkalmazás az Azure és az Office 365 piactér előléptetése

**Az alkalmazás szervezetek, akik már a Azure Active Directory milliónyi előléptetése**  A felhasználók, akik kereshet és böngészhet az alábbi piactér már használ egy vagy több felhőszolgáltatások hatékonyabbá minősített felhőalapú szolgáltatás ügyfelek.  További tudnivalók az alkalmazást [A Azure piactér](https://azure.microsoft.com/marketplace/partner-program/)támogatása.

**Amikor a felhasználók regisztrálni szeretne az alkalmazáshoz, meg kíván jeleníteni a Azure Active Directory access panel és az Office 365 alkalmazásindítójához.**  Felhasználók fogja tudni gyorsan és egyszerűen visszatérhet az alkalmazást később felhasználói tetszés szerint elmélyedhet javításához.  További tudnivalók az [Azure Active Directory panel eléréséhez](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Biztonságos eszköz-szolgáltatás és a szolgáltatás kommunikáció

**Azure Active Directory használ, az Identitáskezelés szolgáltatások és eszközök csökkenti a kódot kell írni, és lehetővé teszi, hogy informatikai való hozzáférés kezelése.**  Szolgáltatások és eszközök tokenek beolvasása OAuth használatával Azure Active Directory és ezek tokenek webes API-khoz eléréséhez használt.  Azure Active Directory segítségével elkerülheti, összetett hitelesítési kódírás.  Mivel az Azure Active Directory, a szolgáltatások és eszközök identitása tárolja informatikai kulcsok és a visszavonási művelet külön-külön az alkalmazás nem egy helyről kezelheti.

## <a name="benefits-of-integration"></a>Integráció előnyei

Integrálása az Azure Active Directory előnyökkel jár, hogy nincs szükség további kódírás megtalálható.

### <a name="integration-with-enterprise-identity-management"></a>Vállalati Identitáskezelés integrációja

**Az informatikai házirendek betartása alkalmazás segítségével.**  Szervezetek saját vállalati identitáskezelő rendszerben integrálása az Azure Active Directory, így a szervezet egy személy elhagyásakor automatikusan elvesznek az access az alkalmazás nélkül további teendője, erre szolgáló informatikai.  Informatikai kezelhetik ki az alkalmazás érhetik el és annak eldöntésében, milyen hozzáférési szabályok szükséges – például többtényezős hitelesítéshez - kódírás összetett vállalati házirendek ahhoz, hogy a szükség csökkentése.  Azure Active Directory segítségével a rendszergazdák a részletes napló, akik bejelentkezve az alkalmazás, a informatikai nyomon követhető használatát.

**Azure AD az Active Directory, hogy az alkalmazás az Active Directory integrálhatja a felhőben kiterjed.**  Világszerte sok szervezet Active Directory használni a fő bejelentkezési és identitáskezelő rendszer, és csak a saját alkalmazások használata az Active Directory.  Az alkalmazás integrálása az Azure Active Directory együttműködik az Active Directory.

### <a name="advanced-security-features"></a>Speciális biztonsági szolgáltatások

**Többtényezős hitelesítés.**  Azure Active Directory natív többtényezős hitelesítés biztosít.  Rendszergazdák megkövetelheti többtényezős hitelesítés elérni az alkalmazást, így nem kell a támogatási kód, saját maga.  További tudnivalók a [Többtényezős hitelesítés](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Észlelési anomalous bejelentkezés.**  Azure Active Directory feldolgozásával több, mint egy milliárd bejelentkezések nappal, a gyanús tevékenység észleli és értesíteni kell a rendszergazdák lehetséges problémák a gépi tanulási algoritmusok használata közben.  Támogatásával Azure AD-bejelentkezés, az alkalmazás megkapja a védelem előnyeit. További információ [megtekintése az Azure Active Directory access jelentés](../active-directory-view-access-usage-reports.md).

**Feltételes hozzáférést.**  Többtényezős hitelesítés, mellett a rendszergazdák kérheti adott feltételnek teljesülnie előtt felhasználók jelentkezhetnek be az alkalmazás.  Feltételek szerint beállítható, hogy az IP-címtartományokat ügyféleszközök, a megadott csoporttagság és az access használt eszköz állapotát tartalmazza.  További tudnivalók a [feltételes Azure Active Directory-hozzáférést](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Egyszerűen használható fejlesztői

**Üzleti szabványos protokollok.**  A Microsoft kötelességének tekinti, a támogatási ipari szabványoknak.  Azure AD a SAML 2.0-s, OpenID csatlakozás 1.0, OAuth 2.0-s vagy Webszolgáltatás-összevonás 1.2-es hitelesítési protokollok támogatja.  A diagram API OData 4.0 kompatibilis.  Ha már az alkalmazás támogatja a SAML 2.0-s vagy OpenID csatlakozás 1.0 protokollok szövetséges bejelentkezés, felvételét támogatja az Azure Active Directory lehet egyszerű.  További tudnivalók a [támogatott Azure Active Directory authentication protokollok](../active-directory-authentication-protocols.md).

**Nyissa meg a forrás-tárakban.**  A Microsoft teljesen támogatott Megnyitás tárak nyújt a használt nyelvek és platformokon sebesség fejlesztés.  A forráskód licencbe Apache 2.0-s verzióját, és szabadon ágaznak és hozzájárul a projektek.  További tudnivalók az [Azure Active Directory authentication tárak](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Világszerte jelenléti adatok és a magas elérhetősége

**Azure Active Directory telepítve van a világon adatközpont esetén felügyelt és éjjel ellenőrizni.**  Azure AD a Microsoft Azure és az Office 365-identitáskezelő rendszer, és telepítve van a világon 28 adatközpont esetén.  A címtár-adatok van garantált kell replikált az legalább három adatközpontokkal.  Globális terheléselosztókkal felhasználók hozzáférés biztosítása érdekében az adatokat tartalmazó Azure Active Directory legközelebbi példányát, és automatikusan, ha probléma merül fel újra átirányíthatja az egyéb adatközpontokkal kérések.

## <a name="next-steps"></a>Következő lépések

[Első lépések kódírás](../active-directory-developers-guide.md#getting-started).

[Azure AD a bejelentkezési felhasználók](../active-directory-authentication-scenarios.md)
