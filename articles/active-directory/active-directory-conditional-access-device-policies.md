<properties
    pageTitle="Az Office 365 szolgáltatásainak feltételes access házirendeket |} Microsoft Azure"
    description="Hogyan eszköz-alapú feltételek részletes tudnivalókat az Office 365-szolgáltatásokkal való hozzáférés szabályozása. Informatikai munkatársak (IWs) szeretné az access az Office 365 szolgáltatásai, például az Exchange és a SharePoint Online munkahelyi és iskolai saját személyes eszközökről, miközben az informatikai rendszergazda azt szeretné, az access kell secure.IT rendszergazdák is kiépítése feltételes hozzáférés biztonságossá tétele a vállalati erőforrások, miközben egy időben IWs engedélyezése a megfelelő eszközök elérése a szolgáltatások házirendeket."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Feltételes access eszköz házirendek az Office 365-szolgáltatások

"Hozzáférés feltételes" kifejezést, még sok feltételt, például többtényezős hitelesített felhasználó hitelesített társított eszköz, a megfelelő eszköz stb. Ez a témakör elsősorban focusses feltételekkel eszköz-alapú Office 365-szolgáltatásokkal való hozzáférés szabályozása a. Informatikai munkatársak (IWs) szeretné az access az Office 365 szolgáltatásai, például az Exchange és a SharePoint Online munkahelyi és iskolai saját személyes eszközökről, miközben az informatikai rendszergazda a biztonságos elérése szeretne. INFORMATIKAI rendszergazdák is kiépítése feltételes hozzáférés biztonságossá tétele a vállalati erőforrások, miközben egy időben IWs engedélyezése a megfelelő eszközök elérése a szolgáltatások házirendeket. Feltételes access házirendek az Office 365-be a Microsoft Intune feltételes access portálról is beállítható.

Azure Active Directory kényszeríti feltételes access házirendek biztonságos módon elérhetők az Office 365-szolgáltatásokkal. Bérlői rendszergazdának egy feltételes hozzáférési házirendet, amely a felhasználó nem férhet hozzá az Office 365 szolgáltatás nem kompatibilis eszközön akadályozza hozhat létre. A felhasználónak meg kell felelnie a vállalat házirendeket előtt hozzáférést is a szolgáltatás. Másik megoldás a rendszergazda is létrehozhat egy házirendet, amely csak a saját eszközök az Office 365 szolgáltatásainak eléréséhez szükséges regisztrálni a felhasználónak. Házirendek is alkalmazható egy a szervezet összes felhasználójának vagy néhány cél csoportokra korlátozott, további cél csoportok felvenni időbeli enhanced.

Egy előfeltétel házirendeket kényszerítése a felhasználóknak, hogy azok eszközök regisztrálása az Azure Active Directory eszköz regisztrációs szolgáltatás szolgál. Többtényezős hitelesítés (MFA) eszközök regisztrálása az Azure Active Directory eszköz regisztrációs szolgáltatás engedélyezése választhatja. Azure Active Directory eszköz regisztrációs szolgáltatás MFA ajánlott. Ha MFA engedélyezve van, azok eszközök regisztrálása az Azure Active Directory eszköz regisztrációs szolgáltatással felhasználók második tényező hitelesítéshez sor kerül.

##<a name="how-does-conditional-access-policy-work"></a>Hogyan működik a feltételes hozzáférési házirend?

Amikor a felhasználó hozzáférést kér Office 365 szolgáltatásba a támogatott eszköz platformot, az Azure Active Directory hitelesíti a felhasználó és eszköz, ahonnan a felhasználó elindítja a kérelem; és a hozzáférési a szolgáltatás csak akkor, amikor a felhasználó megfelel-e a házirend-szolgáltatás beállítása. Az igényelt eszköze nem rendelkező felhasználók útmutatást javító igénylése és a vállalati Office 365-szolgáltatások eléréséhez kompatibilis válnak. Az iOS és Android rendszerű eszközökön a felhasználó számára kötelező a vállalati portál alkalmazás segítségével eszköz beléptetéséhez. Amikor egy felhasználó az oktatók eszköz igényléséhez, az eszköz Azure Active Directory regisztrált, és: Ebben a mobileszköz-kezelés és a megfelelőség. Ügyfelek kell használnia az Azure Active Directory eszköz regisztrációs szolgáltatás Microsoft Intune együtt engedélyezése a mobileszköz-kezelés az Office 365 szolgáltatással. Eszköz beléptetése felhasználók az Office 365 access Services szolgáltatásban előzetesen szükséges esetén eszköz házirendek akkor lépnek érvénybe.

Amikor a felhasználó sikeresen igényléséhez oktatók eszköz, az eszköz lesz megbízható. Azure Active Directory biztosít Single-Sign-On való hozzáférés vállalati alkalmazások és kényszeríti feltételes hozzáférési házirendet, ha szeretne engedélyt adni egy szolgáltatás nem csak az első alkalommal a felhasználó hozzáférést, de valahányszor a felhasználó megújítása hozzáférést kér kér. A felhasználó nem férhet hozzá a szolgáltatásokhoz, bejelentkezési hitelesítő adatok módosulnak, eszköz elvesztett vagy ellopják és a házirend nem éri el megújítása kérelem időben.

## <a name="deployment-considerations"></a>Telepítése során megfontolandó kérdések:
Azure Active Directory eszköz regisztrációs szolgáltatás eszköz regisztráció kell használnia.

Ha felhasználók hitelesítését helyszíni, az Active Directory összevonási szolgáltatások (AD FS) (1,0 és a fenti) szükség. Többtényezős hitelesítés munkahelye Bekapcsolódás sikertelen lesz, amikor a identitásszolgáltató nem képes a többtényezős hitelesítés. Ha például az AD FS 2.0-s verziója nem többtényezős hitelesítést internetezésre alkalmas. Bérlői rendszergazdai gondoskodnia kell arról, hogy a helyszíni Active Directory összevonási szolgáltatások alkalmas MFA, és egy érvényes MFA módszer engedélyezve van, MFA engedélyezése az Azure Active Directory eszköz regisztrációs szolgáltatása előtt. A Windows Server 2012 R2 rendszer AD FS például a MFA funkciókat tartalmaz. Engedélyeznie kell egy további érvényes authentication (MFA) módszer a AD FS-kiszolgáló engedélyezése MFA Azure Active Directory eszköz regisztrációs szolgáltatása előtt. Az Active Directory összevonási szolgáltatások támogatott MFA módszerekkel kapcsolatos további tudnivalókért lásd: további hitelesítési módszerek konfigurálása az Active Directory összevonási szolgáltatások.

## <a name="frequently-asked-questions-faq"></a>Gyakori kérdések

Kérdés: Mi az, hogy az alapértelmezett való felelősség kizárását házirendet, az eszközön nem támogatott platformok?

A: jelenleg feltételes hozzáférési vannak szelektív kényszeríti az iOS és az Android-eszközök felhasználóinak. Alkalmazás a többi platformon futó verziók eszköz, alapértelmezés szerint ki legyenek, a feltételes hozzáférési házirendet, az iOS és az Android-eszközön nem érinti. Bérlői rendszergazdai, azonban dönthet a globális házirendet, amely a felhasználók nem támogatott platformokon való hozzáférés letiltása felülbírálása.
Ha ki szeretné terjeszteni a feltételes hozzáférési házirendet a más platformokhoz készült, beleértve a Windows felhasználók az ütemterv van telepítve.

Probléma: amikor az Office 365-ös szolgáltatásokhoz feltételes hozzáférési házirend kiterjesztik böngészőalapú alkalmazásokat (például az Outlook Web App, böngészőalapú SharePoint).

A: jelenleg Office 365-ben services feltételes eléréséhez az eszközön gazdag alkalmazások korlátozott. Ha ki szeretné terjeszteni a feltételes hozzáférési házirendet a felhasználóknak a szolgáltatások elérése a böngészőből az ütemterv van telepítve.
