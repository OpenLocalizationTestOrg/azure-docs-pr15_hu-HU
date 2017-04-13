<properties
   pageTitle="Azure Active Directory integrálása az alkalmazások az első lépések útmutató |}  Microsoft Azure"
   description="Ez a cikk lépések útmutató letöltése az Azure Active Directory (AD) integrálása helyszíni alkalmazások, és a felhő alkalmazások."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integrálása első alkalmazások használatának megkezdését segítő útmutató
## <a name="overview"></a>– Áttekintés
Ez a témakör az alkalmazásokat az Azure Active Directory (AD) egyesítő tudnivalókat foglaltuk össze ad szolgál. Részletesebb témakör rövid összefoglalása az alábbi szakaszok tartalmazzák, így azonosíthatja, hogy mely részei az keresztüli lépések útmutató szempontjából fontos Önnek.  Kövesse a hivatkozásokat egy mélyebb merülési az egyes témák.

## <a name="before-you-begin-take-inventory"></a>Mielőtt elkezdené, hogy készlet
Az alkalmazások integrálása az Azure Active Directory ugorhat előtt célszerű fontos tudni, hogy hol és ugrás.  Segít a Azure AD-alkalmazás integration projekt megfontolni a következő kérdések szolgálnak.

### <a name="application-inventory"></a>Alkalmazás leltár
- Hol találhatók az alkalmazásokat? Ki a tulajdonosa őket?
- Milyen típusú hitelesítési végezze el kell az alkalmazások?
- Ki az alkalmazások hozzáférésre van szüksége?
- Szeretne egy új alkalmazás telepítéséhez?
  - Lesz, házon belüli összeállítása és üzembe Azure számítási példányán?
  - Használ, amely érhető el az Azure alkalmazás gyűjteményben?

### <a name="user-and-group-inventory"></a>Felhasználók és csoportok leltár
- Hol találhatók a felhasználói fiók?
 - A helyszíni Active Directory
 - Azure Active Directory
 - A saját különálló alkalmazásként adatbázis belül
 - Unsanctioned alkalmazásokban
 - A fentiek mindegyike
- Milyen engedélyek és szerepkör-hozzárendelések az egyéni felhasználók jelenleg a számítógépemen? Szükség van, tekintse át a hozzáférésüket, és biztos abban, hogy a felhasználói hozzáférés és szerepkör-hozzárendelések is megfelelő most?
- Vannak csoportok már folyamatban a helyszíni Active Directory?
 - Hogyan vannak rendezve csoportot?
 - A csoport tagjai akik?
 - Milyen engedélyek/szerepkör-hozzárendelések a csoportok jelenleg a számítógépemen?
- Szükség van üríteni a felhasználó vagy csoport adatbázisok integrálása előtt?  (Ez a kérdés igazán fontos. Szemét a szemét ki.)

### <a name="access-management-inventory"></a>Access-kezelés leltár
- Hogyan tegye meg jelenleg felhasználói hozzáférés kezelése a alkalmazások? Nem, amely kell módosítása  Tekintette más módokon való hozzáférés kezelése, például a [RBAC](role-based-access-control-configure.md) például?
- Ki mit hozzáférésre van szüksége?

Esetleg nem megvan a válasz mindenkinek a fenti kérdések látható, de ez nem probléma.  Ez az útmutató segítségével e kérdésekre választ, és néhány tájékoztatni döntések.

## <a name="prerequisites"></a>Előfeltételek
- Egy Azure-előfizetést, és az Azure Active directory.  Ha még nem rendelkezik az Azure előfizetéssel, Azure megpróbálhatja 30 napig ingyenes. [Próbálja ki!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Azure Active Directory alkalmazás integrációja
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Keresés unsanctioned Cloud App feltárás cloud-alkalmazások
Fent említett alkalmazások által a szervezet eddig még nem lett kezelt lehet.  A folyamat részeként unsanctioned felhő alkalmazások kereséséhez. Olvassa el a [Cloud App feltárás unsanctioned cloud-alkalmazások megkeresése](active-directory-cloudappdiscovery-whatis.md)című témakört.

### <a name="authentication-types"></a>Hitelesítés típusa
Az alkalmazás minden más hitelesítési követelményeket lehetnek. Azure Active Directory aláíró tanúsítványok is használható a SAML 2.0-s, Webszolgáltatás-összevonás OpenID csatlakozás protokollok törtek vagy jelszó egyszeri bejelentkezés használó alkalmazások. További információt az alkalmazás hitelesítéstípusok Azure Active Directory való használatra [Tanúsítványok kezelése az összevont Single Sign-On az Azure Active Directory](active-directory-sso-certs.md) és a [jelszó alapján egyszeri bejelentkezési](active-directory-appssoaccess-whatis.md)témakörben talál.

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Azure Active Directory-alkalmazás Proxy SSO engedélyezése
Microsoft Azure Active Directory szolgáltatásalkalmazás-Proxy akkor megadhatja alkalmazások található a magánhálózaton biztonságosan, bárhol és bármilyen eszközön való hozzáférést. A frissítés telepítése után egy alkalmazás proxy összekötő a környezetben, akkor egyszerűen beállítható az Azure Active Directory.

### <a name="integrating-applications-with-azure-ad"></a>Alkalmazások integrálása az Azure Active Directory
Az alábbi cikkekből megtudhatja, hogy milyen különböző módokon alkalmazások integrálása az Azure Active Directory és néhány útmutatást.

- [Annak megállapítása, mely az Active Directory használata](active-directory-administer.md)
- [Alkalmazások használata a Azure alkalmazás gyűjtemény](active-directory-appssoaccess-whatis.md)
- [Szoftver alkalmazások oktatóanyagok lista integrálásának](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Alkalmazások való hozzáférés kezelése
Az alábbi cikkek ismertetik, hogyan után az Azure Active Directory-összekötőkkel Azure Active Directory és Azure Active Directory integrált alkalmazások hozzáférést kezelheti.

- [Alkalmazások használata az Azure Active Directory való hozzáférés kezelése](active-directory-managing-access-to-apps.md)
- [Azure AD-összekötőkkel automatizálása](active-directory-saas-app-provisioning.md)
- [Az alkalmazás a felhasználók hozzárendelése](active-directory-applications-guiding-developers-assigning-users.md)
- [Az alkalmazás csoportok hozzárendelése](active-directory-applications-guiding-developers-assigning-groups.md)
- [Megosztott partnerek](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Egyéni alkalmazások integrálása
Ha az éppen írt egy új alkalmazást, és segítse a fejlesztők feljebb helyezése a power Azure Active Directory szeretné, olvassa el a [Guiding fejlesztők számára](active-directory-applications-guiding-developers-for-lob-applications.md).

Azure alkalmazás gyűjteménybe az egyéni alkalmazás hozzáadása, című témakörben olvashat ["Saját alkalmazás hozása" Azure Active Directory önkiszolgáló SAML-beállításokkal](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Lásd még:

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
