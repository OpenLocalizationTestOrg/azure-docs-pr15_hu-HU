<properties
    pageTitle="Azure Active Directory egyszeri bejelentkezés integrálása szoftver-alkalmazások |}  Microsoft Azure"
    description="Egyszeri bejelentkezés hitelesítési és felhasználói szoftver alkalmazásokat az Azure Active Directory kezelése központi access kiépítési engedélyezése. Megtudhatja, hogy miként integrálása az Azure Active Directory szoftver-alkalmazás között."
    services="active-directory"
      keywords="Azure Active Directory integrálása szoftver alkalmazások"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Azure Active Directory egyszeri bejelentkezés integrálása szoftver alkalmazások  

> [AZURE.SELECTOR]
- [Azure portál](active-directory-enterprise-apps-manage-sso.md)
- [Azure klasszikus portál](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Első lépésként az egyszeri bejelentkezés-alkalmazáshoz, hogy Ön éppen való betöltésük a szervezet beállításához fogja használni, akkor egy meglévő könyvtár az Azure Active Directory (Azure Active Directory). Microsoft Azure, Office 365-ben vagy Windows Intune keresztül kaphat az Azure Active Directory címtárhoz is használhatja. Ha két vagy több ilyen, olvassa el [az Azure Active directory felügyelete](active-directory-administer.md) rendeltetésének meghatározása.

## <a name="authentication"></a>Hitelesítés

Alkalmazások, amely támogatja a SAML 2.0-s Webszolgáltatás-Összevonás, vagy a protokollok OpenID csatlakozás aláíró tanúsítványok megbízhatósági kapcsolatok létesítése az Azure Active Directory használ. További információt talál [az összevont egyszeri bejelentkezési tanúsítványok kezelése](active-directory-sso-certs.md).

Azure Active Directory csak HTML űrlapalapú bejelentkezés támogató alkalmazások, használja a "jelszó vaulting" bizalmi kapcsolatok létrehozásához. Ez lehetővé teszi, hogy a felhasználók a szervezet automatikusan bejelentkezni a szoftver-alkalmazások által Azure Active Directory, a felhasználói fiók adatait a szoftver alkalmazásból használatával. Azure Active Directory gyűjti össze, és biztonságos módon tárolja a felhasználói fiók adatait, és a kapcsolódó jelszót. További tudnivalókért lásd: [jelszó-alapú egyszeri bejelentkezés](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Engedély

A kiépített fiók lehetővé teszi, hogy azt, hogy az alkalmazás használatát, miután keresztül egyszeri bejelentkezés a hitelesített felhasználó. Felhasználói kiépítési történik manuálisan vagy egyes esetekben hozzáadása és a felhasználói adatok eltávolítása a szoftver alkalmazásból, az Azure Active Directory végrehajtott módosítások alapján. -Összekötőkkel meglévő Azure AD automatikus kialakítási a további tudnivalókért lásd: az [automatikus felhasználói kiépítési és vonja kiépítési szoftver-alkalmazásokhoz](active-directory-saas-app-provisioning.md).

Egyéb Ön is manuálisan felhasználói adatok hozzáadása az alkalmazás, vagy érhetők el a piactér kiépítési megoldásait.

## <a name="access"></a>Az Access

Azure Active Directory többféleképpen testre szabható a felhasználóknak a szervezet alkalmazások telepítéséhez. Nem zárolt bármely adott deployment vagy a hozzáférési megoldás. [A, amely az igényeknek leginkább megfelelő megoldást](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)is használhatja.

## <a name="additional-considerations-for-applications-already-in-use"></a>További megfontolandó szempontok alkalmazások már használatban van

Az új alkalmazást az új fiók létrehozásának folyamata egy másik folyamat beállítása egyszeri bejelentkezési meg a szervezeten belül már használó alkalmazások. Első lépések, beleértve a néhány: hozzárendelése felhasználói identitások az Azure Active Directory azonosítási alkalmazásban, és hogyan felhasználók fog tapasztalni után integrálva van az alkalmazás naplózás ismertetése.

> [AZURE.NOTE] Beállításához egyszeri bejelentkezés egy meglévő alkalmazáshoz kell globális rendszergazdai jogosultságok mindkét Azure Active Directory és a szoftver alkalmazás.

### <a name="mapping-user-accounts"></a>Hozzárendelése felhasználói fiókokhoz

Egy felhasználó személyazonosságára általában rendelkezik egy egyedi azonosítót, amely hasznos lehet a kívánt e-mail címet, vagy az egyszerű felhasználónév (UDN). Hivatkozás (térkép) kell minden felhasználó alkalmazás azonosítója, hogy a megfelelő Azure AD-identitás. Néhány módszert, attól függően, hogy miként ehhez a követelmény az alkalmazás-hitelesítést.

További információt az alkalmazás identitások Azure AD-felhasználókkal hozzárendelése [a SAML jogkivonat kiállított testreszabása jogcímeken](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) és megtekintése [testreszabása attribútum hozzárendeléseinek kiépítése](active-directory-saas-customizing-attribute-mappings.md)

### <a name="understanding-the-users-log-in-experience"></a>A felhasználói élmény a napló ismertetése

Amikor az alkalmazás, amely már használatban van SSO integrálhatná, fontos látja, hogy a felhasználói felület hatással lesz. Felhasználók összes alkalmazáshoz elkezdenek Azure Active Directory hitelesítő használatával jelentkezzen be. Az is előfordulhat, hogy a különböző portál kell használniuk az alkalmazások eléréséhez.

Egyes alkalmazások SSO el tud végezni az alkalmazás bejelentkezési felületén, de más alkalmazások, a felhasználónak kell egy központi portált, például ([Office-alkalmazások](http://myapps.microsoft.com) vagy az [Office 365-ben](http://portal.office.com/myapps)) való bejelentkezéshez folyamatát. További tudnivalók a különböző típusú-ÖS és a felhasználói élmény a [Mi az alkalmazás elérése és az egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

Egy másik értékes erőforrás *Suppressing felhasználói beleegyezés* [Guiding fejlesztők számára](active-directory-applications-guiding-developers-for-lob-applications.md) című témakörben.

## <a name="next-steps"></a>Következő lépések


Szoftver alkalmazásokat, hogy az alkalmazás gyűjteményben találja, az Azure Active Directory számos [szoftver alkalmazások integrálása ismertető oktatóanyagok](active-directory-saas-tutorial-list.md).

Ha az alkalmazás nem szerepel a Alkalmazásgyűjteményébe, [vegye fel az Azure Active Directory alkalmazásgyűjteményébe egyéni alkalmazásként](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)is.

A Azure.com tárban problémákra kezdődő összes sokkal több részlet van [Újdonságok alkalmazás elérése és az egyszeri bejelentkezés az Azure Active Directory címtárral.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Lásd még:

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
