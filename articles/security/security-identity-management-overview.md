<properties
   pageTitle="Azure identitás kezelése biztonsági áttekintése |} Microsoft Azure"
   description=" A Microsoft identitás- és a hozzáférés kezelése megoldások – Súgó informatikai access-alkalmazások és az erőforrások végig a vállalati adatközponthoz és védelme a felhőben, az engedélyezéséről további érvényességi például többtényezős hitelesítés és feltételes hozzáférési szinteket. Ez a cikk áttekintést nyújt az alapvető Azure biztonsági funkciók, amelyek segítségével az Identitáskezelés. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Azure identitás kezelése biztonsági – áttekintés

A Microsoft identitás- és a hozzáférés kezelése megoldások – Súgó informatikai access-alkalmazások és az erőforrások végig a vállalati adatközponthoz és védelme a felhőben, az engedélyezéséről további érvényességi például többtényezős hitelesítés és feltételes hozzáférési szinteket. A gyanús tevékenységek figyelése Speciális adatvédelme jelentések, a naplózás és a riasztási segít csökkentésében potenciális biztonsági problémákról. [Azure Active Directory prémium](../active-directory/active-directory-editions.md) (szoftver) alkalmazások egyszeri bejelentkezés a felhőben ezer biztosít, és web Apps alkalmazások hozzáférést a helyszíni futtatása.

Biztonsági előnyeit az Azure Active Directory (AD) tartalmaz, az azt jelenti, hogy:

- Létrehozása és kezelése egy egyetlen identitás minden felhasználó számára a hibrid vállalati, a felhasználók, csoportok és eszközök szinkronizálja megőrzési keresztül
- Egyszeri bejelentkezés hozzáférést biztosít az alkalmazásokat, például előre integrált szoftver alkalmazások ezer
- Alkalmazást az access biztonsági kapcsolhatja be érvényesítési szabályok-alapú többtényezős hitelesítés mindkét a helyszíni és a felhő alkalmazások
- A helyszíni webalkalmazások Azure Active Directory-alkalmazás proxyn keresztül biztonságos távoli hozzáférés kiépítése

Ez a cikk az a célja, hogy az Azure biztonsági funkciók, amelyek segítségével az Identitáskezelés az alapvető áttekintést nyújtanak. Azt is, amely képet ad szolgáltatások részleteit, így további projektvezetési cikkeket.  

A cikk az alábbi core Azure identitás szolgáltatásairól koncentrál:

- Egyszeri bejelentkezés
- Fordított proxykiszolgáló
- Többtényezős hitelesítés
- Biztonsági felügyeletet igényel, az értesítéseket és gépi tanulási-alapú jelentések
- Fogyasztói identitás- és a hozzáférés kezelése
- Regisztráció eszköz
- Kiemelt Identitáskezelés
- Identitás-védelem
- Hibrid Identitáskezelés

## <a name="single-sign-on"></a>Egyszeri bejelentkezés

Egyszeri bejelentkezés (SSO) azt jelenti, nem tudnak hozzáférni az alkalmazások és információforrások, amelyek kell tennie az üzleti, jelentkezzen be a csak egyszer az egyetlen felhasználói fiók használatával. Miután bejelentkezett, az összes kötelező hitelesítés nélkül van szüksége az alkalmazások elérheti (például adja meg a jelszót) másodszori.

Sok szervezet egy szolgáltatási (szoftver) alkalmazások, például Office 365-ben, a mező és a Salesforce végfelhasználói is elérhető, szoftver támaszkodik. Régebben informatikai szakemberek szükséges külön-külön létrehozása és frissítése a felhasználói fiókokat az egyes szoftver alkalmazásban, és a felhasználók ne feledje, hogy minden szoftver alkalmazáshoz jelszó kellett.

Azure Active Directory átnyúlik a helyszíni Active Directory a felhőben, engedélyezése a felhasználóknak, hogy azok elsődleges szervezeti fiókja segítségével nem csak jelentkezzen be a tartományhoz tartozó eszközök és a vállalati erőforrások, de minden szoftver-alkalmazások és webes a feladat is szükséges.

Nemcsak felhasználó nem rendelkezik a felhasználóneveket és jelszavakat, ha több feltételcsoportnak kezelése, az access alkalmazás is automatikusan kiépített vagy vonja kiépített alapuló szervezeti csoportok és állapotuk alkalmazottként. Azure AD a biztonság és irányítási vezérlőelemek elérése, melyekkel központi szoftver számára a felhasználók hozzáférésének kezelése vezet be.

tudj meg többet:

- [Egyszeri bejelentkezés áttekintése](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Azure Active Directory egyszeri bejelentkezés integrálása szoftver alkalmazások](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Fordított proxykiszolgáló

Azure Active Directory szolgáltatásalkalmazás-Proxy lehetővé teszi közzé a helyszíni alkalmazások, például a [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) webhelyek, [Az Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)és [az IIS](http://www.iis.net/)-alapú alkalmazások a magánhálózaton belül és a biztonságos hozzáférést biztosít a hálózaton kívüli felhasználók. Szolgáltatásalkalmazás-Proxy távelérési és az egyszeri bejelentkezés (SSO) biztosít a helyszíni webalkalmazások szoftver alkalmazások, amely támogatja az Azure Active Directory ezer számos különböző típusú. Az alkalmazottak be tud jelentkezni az alkalmazások az otthoni saját eszközön, és a hitelesítési a felhőalapú proxyn keresztül.

tudj meg többet:

- [Azure Active Directory szolgáltatásalkalmazás-Proxy engedélyezése](../active-directory/active-directory-application-proxy-enable.md)
- [Azure Active Directory szolgáltatásalkalmazás-Proxy alkalmazások közzététele](../active-directory/active-directory-application-proxy-publish.md)
- [Single-sign-on a szolgáltatásalkalmazás-Proxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes access használata](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Többtényezős hitelesítés
Azure többtényezős hitelesítés (MFA), amelyek egynél több ellenőrzési módszer van szükség, és hozzáadja a kritikus második réteg biztonsági felhasználói bejelentkezések, és a tranzakciók hitelesítési módszer. MFA segít védelmét hozzáférés az adatokhoz, és az alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben. Via ellenőrzési beállítások adattartomány erős hitelesítés kézbesíti – telefonhívás, szöveges üzenet vagy mobilalkalmazásban értesítési vagy ellenőrzési kódot és 3rd fél OAuth tokenek.

tudj meg többet:

- [Többtényezős hitelesítés](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Mi az Azure többtényezős hitelesítést?](../multi-factor-authentication/multi-factor-authentication.md)
- [Hogyan működik a Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Biztonsági felügyeletet igényel, az értesítéseket és gépi tanulási-alapú jelentések

Biztonsági figyelése és értesítések és gépi tanulási-alapú jelentések, amely nem következetes access azonosításához segíthetnek a vállalkozás védelme. Azure Active Directory access és a használati jelentések segítségével betekintést kap abba, hogy a integritás és a szervezet címtár biztonsága nyereség. Az adatokkal a címtár-rendszergazdai jobban megállapítható hol lehetséges biztonsági kockázatot elhelyezkednie előfordulhat, hogy azok megfelelően tervezi, hogy e kockázatok csökkentésében.

Az Azure klasszikus portálon jelentések kategóriába az alábbi módon:

- Rendellenességet jelentések – tartalmaznia kell anomalous talált események bejelentkezés. Célunk ellenőrizze az ilyen tevékenység tudomást és lehetővé teszi, győződjön meg arról, hogy az esemény gyanús meghatározása láthatja.
- Integrált alkalmazások jelentések – adja meg az összefüggéseket be, hogy hogyan használják a szervezetben a felhő alkalmazások. Azure Active Directory-integráció a felhőben alkalmazások ezer kínál.
- Hibajelentések – külső alkalmazásokban fiókok kiépítésének esetleg előforduló hibák jelzésére.
- Felhasználó-specifikus jelentések – eszköz/bejelentkezési az adott felhasználó tevékenység adatainak megjelenítése
- Tevékenység naplók – a belül a az elmúlt 24 óra, a utóbbi 7 napból vagy a 30 napnál, valamint a csoport tevékenység módosítások és a jelszó alaphelyzetbe állítása és nyilvántartási tevékenység felvételét ellenőrzött összes eseményének tartalmazzák.

tudj meg többet:

- [A hozzáférési és használati jelentések megtekintése](../active-directory/active-directory-view-access-usage-reports.md)
- [Első lépések – az Azure Active Directory jelentése](../active-directory/active-directory-reporting-getting-started.md)
- [Azure Active Directory jelentéskészítés útmutató](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Fogyasztói identitás- és a hozzáférés kezelése

Azure Active Directory B2C egy könnyen hozzáférhető, globális identitás kezelése szolgáltatás fogyasztói elérésű-alkalmazásokhoz, méretezze át a szeretne több száz identitások milliónyi. Mobil keresztül is integrált és webes platformokon. A fogyasztói bejelentkezhet keresztül testre szabható élményt összes alkalmazás a meglévő közösségi fiókjuk vagy: hozzon létre új hitelesítő adatokat.

Az elmúlt alkalmazások fejlesztői számára, akik jelentkezhetnek, és bejelentkezés az alkalmazásokba fogyasztói szeretett volna volna írt a saját kód. És azok ugyanúgy használható a helyszíni adatbázisok vagy rendszerek felhasználóneveket és jelszavakat tárolja. Azure Active Directory B2C felajánlja a szervezet egy biztonságos, szabványok platform és egy nagy bővíthető házirendeket segítségével alkalmazásait fogyasztói Identitáskezelés integrálni szeretné a hatékonyabb módszerre.

Azure Active Directory B2C használata esetén a fogyasztói regisztrálhatnak-alkalmazás létrehozása új hitelesítő adatok (e-mail címét és jelszavát, vagy a felhasználónév és jelszó) vagy a meglévő közösségi fiókjuk (Facebook, a Google, Amazon, LinkedIn) segítségével.

tudj meg többet:

- [Mi az Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C előzetes verzió: Jelentkezzen be, és jelentkezzen be az alkalmazások fogyasztói](../active-directory-b2c/active-directory-b2c-overview.md)
- [Azure Active Directory B2C előnézete: Típusú alkalmazások](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Regisztráció eszköz

Azure Active Directory-eszköz regisztrációs az alapja eszköz-alapú [feltételes hozzáférési](../active-directory/active-directory-conditional-access-on-premises-setup.md) környezetek kialakításához. Olyan eszköz van regisztrálva, Azure Active Directory eszköz regisztrációs biztosít az identitással hitelesíteni az eszközt, amikor a felhasználó bejelentkezik a használt eszköz. A hitelesített eszközt, és az eszköz tulajdonságainak meg lehessen őrizni a felhőben, és a helyszíni telepített alkalmazások feltételes access házirendek majd használható.

Ha például az Intune mobileszköz-kezelés (MDM) megoldást együtt, az Azure Active Directory eszköz tulajdonságainak frissülnek, az eszközről további információt. Így hozhat létre, amely megfelel a követelményeknek, biztonság és megfelelőség az eszközökről access hivatkozási feltételes hozzáférési szabályokat.

tudj meg többet:

- [Első lépések az Azure Active Directory eszköz regisztráció](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [A helyszíni feltételes elérésének Azure Active Directory eszköz regisztrációs beállítása](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Az automatikus eszköz regisztrációs az Azure Active Directory a Windows-tartományhoz csatlakoztatott eszközök](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Kiemelt Identitáskezelés
Azure Active Directory (AD) jogosultsággal rendelkező Identitáskezelés lehetővé teszi, hogy kezeléséhez, szabályozni, és figyelheti a Yammerhez identitások és erőforrások az Azure Active Directory, valamint az egyéb Microsoft online szolgáltatások, például az Office 365-ös vagy Microsoft Intune eléréséhez.

Előfordul, hogy a felhasználóknak kell az Azure vagy az Office 365 erőforrások vagy más szoftver alkalmazások jogosultsággal rendelkező tevékenység végzését. Ez általában azt jelenti szervezetek betekintést állandó Yammerhez az Azure Active Directory. Ez a felhőben tárolt erőforrások növekvő biztonsági kockázatot jelentenek azért, mert a szervezetek elég nem lehet figyelni a mi azoknak a felhasználóknak mivel foglalkoznak a rendszergazdai engedélyekkel. Ezenkívül hozzáférési jogosultsággal rendelkező felhasználói fiókot sérül meg, ha, hogy egy szerződésszegés hatással lehetnek a felhőben védelméért. Azure Active Directory jogosultsággal rendelkező Identitáskezelés segít a kockázatot feloldásához.

Azure Active Directory jogosultsággal rendelkező Identitáskezelés teszi lehetővé:

- Mely felhasználók találhatók Azure AD-rendszergazdák
- Igény szerinti, engedélyezze a "csak az idő" a Microsoft Online Services rendszergazdai hozzáférése például az Office 365-ben és az Intune
- Kapcsolatfelvétel észleléséről készült jelentések megtekinté rendszergazdai hozzáférés előzmények és a módosítások rendszergazda hozzárendelések
- Hozzáférési jogosultsággal rendelkező szerepkörbe kapcsolatos értesítéseket kapjon

tudj meg többet:

- [Azure Active Directory jogosultsággal rendelkező Identitáskezelés](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Azure Active Directory jogosultsággal rendelkező Identitáskezelés szerepkörök](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure Active Directory Yammerhez Identitáskezelés: Hogyan lehet hozzáadni vagy eltávolítani egy felhasználói szerepkör](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identitás-védelem
Azure Active Directory-identitás védelme biztonsági szolgáltatás, amely a kockázatok események és a szervezet identitások érintő potenciális biztonsági összevont nézetben. Identitás védelem használja a meglévő Azure Active Directory rendellenességet észlelés (Azure AD-Anomalous tevékenység jelentések keresztül érhető el), és bemutatja az új kockázati esemény adattípusa rendellenességeinek képes észlelni a lapra.

tudj meg többet:

- [Azure Active Directory identitás-védelem](../active-directory/active-directory-identityprotection.md)
- [Csatorna 9: Azure Active Directory és a Megjelenítés identitás: identitás védelem előzetes verzió](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hibrid Identitáskezelés

A Microsoft megközelítése identitás átnyúlik a helyszíni és a felhő hitelesítés és az összes erőforrás, függetlenül a hely engedély egyetlen felhasználói azonosítót létrehozása.

tudj meg többet:

- [Hibrid identitás tanulmány](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Az Active Directory csapatának blogja](https://blogs.technet.microsoft.com/ad/)
