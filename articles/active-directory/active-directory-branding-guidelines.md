<properties
   pageTitle="Útmutatások védjegyzése alkalmazások |} Microsoft Azure"
   description="Erőforrások Fejlesztőeszközök készült Azure Active Directory átfogó útmutató"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Alkalmazások esetleges szabályai


Ez a témakör azt ismerteti, hogy az esetleges irányelveket, akkor használja, ha az Azure Active Directory (Azure Active Directory) alkalmazások fejlesztéséhez. Következő irányelvek segítséget nyújt az ügyfelekkel közvetlen, amikor kívánja használni a munkahelyi vagy iskolai fiókjukkal, kezeli az Azure Active Directory, vagy a személyes számla az előfizetést, és bejelentkezés az alkalmazásba.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Személyes fiókok és összehasonlítása a munkahelyi vagy iskolai fiókjából Microsoft

A Microsoft kezeli kétféle felhasználói fiókok:

- **Személyes fiókok** (korábbi nevén Windows Live ID ismert). Ezekben a fiókokban jelenítik meg *az egyes* felhasználók és a Microsoft közötti kapcsolatra, és szolgálnak fogyasztói eszközök és szolgáltatások elérése a Microsoft. Ezekben a fiókokban személyes használatra szolgálnak.

- **Munkahelyi vagy iskolai fiók.** Ezekben a fiókokban kezelhetők a Microsoft Azure Active Directory használó szervezetekben nevében. E fiók használatával jelentkezzen be az Office 365-ben és a más üzleti szolgáltatások a Microsoft.

A Microsoft munkahelyi vagy iskolai fiók általában kiosztotta a felhasználóknak (alkalmazottak, diákok, szövetségi alkalmazottak) a szervezetek (munkahelyi, iskolai, kormányzati Ügynökség). Ezekben a fiókokban lezárt fájlrendszer közvetlenül a felhőben, az Azure Active Directory platform, vagy egy helyszíni címtárból, például a Windows Server Active Directory Azure Active Directory szinkronizált. A Microsoft a munkahelyi vagy iskolai fiókja *kezelésükben* , de a partnerek tulajdonában lévő és a szervezet által vezérelt.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Az alkalmazás az Azure Active Directory-fiókok hivatkozó

A Microsoft nem jelenítik meg az Azure Active Directory-arculatának neveket és sem a végfelhasználók meg kell.

- Amikor a felhasználók van jelentkezve, a szervezet neve és a lehető embléma kell használnia. Ez célszerűbb általános kifejezéseket, például: "a szervezet" történő használata helyett.

- Amikor a felhasználó nincs bejelentkezve, tekintse át a fiókok, mint "munkahelyi vagy iskolai fiókjából", és használja a Microsoft embléma bajlódnia, hogy ezek a fiókok kezelése a Microsoft által. Ne használja a kifejezéseket, például: "a vállalati fiók", "Vállalati verziós fiókban" vagy "a vállalati fiók", amely hoz létre felhasználói fejetlenséget.

## <a name="user-account-pictogram"></a>Felhasználói fiók ábrát
Az alábbi irányelveket korábbi verziójában ajánlott egy "kék jelvény" ábrát használatával. Felhasználói és fejlesztői visszajelzései alapján, most javasoljuk, a Microsoft embléma használata helyett. Ez segít a felhasználók megtudhatják, hogy a számlát azok az Office 365-ös vagy más, Microsoft business szolgáltatások használatával jelentkezzen be az alkalmazás felhasználhatja.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Fizet elő, és bejelentkezés az Azure Active Directory

Az alkalmazás terjeszthet előfizetési és a bejelentkezési külön elérési útját, és a következő szakaszokban vizuális útmutató a mindkét esetek.

**Ha az alkalmazás támogatja a felhasználó bejelentkezési felfelé (pl. ingyenes próba- vagy freemium modellbe)**: jelenítheti meg a **Bejelentkezés** gomb, amely lehetővé teszi a felhasználóknak az alkalmazást, az illető munkahelyi vagy személyes fiókjukat elérését. Azure Active Directory jóváhagyását adja kérdés jelenik meg az első alkalommal hozzáférésük van az alkalmazás.

**Ha az alkalmazás csak a rendszergazdák hozzájárul is, jogosultság szükséges, vagy ha az alkalmazás csak a szervezeti licencelési**: kell elválasztani a felügyeleti beszerzése a felhasználónak bejelentkezés. A **"Ez az alkalmazás beolvasása" gomb** úgy irányítja át a rendszergazdák jelentkezzen be, és kérje meg, hogy adja meg a felhasználó hozzájárul ahhoz nevében a szervezet felhasználói. További előnye megjelenítése a felhasználóknak hozzájárulása megjelenő utasításokat követve az alkalmazás, azt.

## <a name="visual-guidance-for-app-acquisition"></a>Vizuális útmutató a alkalmazás beszerzése

A "az alkalmazás letöltése" hivatkozás átirányítást kell beállítania a felhasználó az Azure Active Directory hozzáférés (engedélyezése) lapon, az alkalmazás, hogy a Microsoft által üzemeltetett szervezetük adatokhoz való hozzáférés engedélyezése a szervezet rendszergazdája engedélyezi. [Alkalmazások integrálása az Azure Active Directory címtárral](active-directory-integrating-applications.md) című témakörben olvashat a következőképpen igényelhet hozzáférést tárgyalja.

Miután rendszergazdák hozzájárul az alkalmazást, választhatnak fel szeretne venni a felhasználói Office 365 alkalmazás megnyitó számára (a rácsos ikon és [https://portal.office.com/myapps](https://portal.office.com/myapps)elérhető). Szeretne meghirdetése ezt a lehetőséget, ha használhat kifejezéseket, például "Ez az alkalmazás hozzáadása a szervezet" és jelennek meg a gomb megjelenítése:

![Alkalmazás-típusok és -esetek](./media/active-directory-branding-guidelines/add-to-my-org.png)

Jó helyen jár azt javasoljuk, hogy helyett gombok használna magyarázó szöveget írni. Példa:
> *Ha már használja az Office 365- vagy más üzleti szolgáltatás a Microsoft, egyszerűen adhat a szervezet adatainak < your_app_name > elérését. Ez lehetővé teszi a felhasználóknak, hogy a meglévő erőforrás-fiókjuk < your_app_name > elérni.*


## <a name="visual-guidance-for-sign-in"></a>Vizuális útmutató a bejelentkezési
Az alkalmazás megjelenjen egy bejelentkezési az, hogy a felhasználók átirányítja az bejelentkezési végpontot, amely megfelel a protokoll használatával integrálása az Azure Active Directory gombra. A következő szakasz részletesen e button kell néz.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Ábrát, és a "Microsoft jelentkezzen"
A társítási a Microsoft embléma és Azure Active Directory egyik ablaktábláról a másikra más identitásszolgáltatók egyedileg képviselő "Sign in with Microsoft" adatokra támogathatnak az alkalmazást. Nincs elegendő hely a "Sign in with Microsoft", érdemes ok "Bejelentkezési" lerövidítéséhez azt.

![Alkalmazás-típusok és -esetek](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Alkalmazás-típusok és -esetek](./media/active-directory-branding-guidelines/sign-in-light.png)

Az egyes gombok sötét színsémát is használhatja.

![Alkalmazás-típusok és -esetek](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Alkalmazás-típusok és -esetek](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Javaslatok és intelmet védjegyzés

**Végezze el** segítségével "munkahelyi vagy iskolai fiók" a "Sign in with Microsoft" gombra a további magyarázatot mindig ismerik fel, hogy azt használni tudja a végfelhasználók érdekében. **Nem** használja az egyéb kifejezéseket, például "a vállalati fiók", "Vállalati verziós fiókban" vagy "a vállalati fiók."

**Nem** használja az "Office 365" vagy "Azure azonosítója". Az Office 365 egyben a Microsoft Azure Active Directory nem használó hitelesítéshez kínáló ügyfél nevét.

A Microsoft embléma **nem** változtatja meg.

A vagy az Azure Active Directory márka végfelhasználóknak **nem** jelenítik meg. Ok azonban fejlesztők, az informatikai szakemberek és a rendszergazdák ezeket a kifejezéseket használatára.

## <a name="navigation-dos-and-donts"></a>Navigációs tennivalók és intelmet

**Végezze el** a felhasználók jelentkezzen ki, és kattintson egy másik fiókkal lehetőséget biztosít. A legtöbben a Twitteren Microsoft/Facebook/Google egyetlen személyes fiókkal rendelkezik, miközben a személyek gyakran társítva egynél több szervezet. Több aláírt felhasználók támogatása hamarosan.
