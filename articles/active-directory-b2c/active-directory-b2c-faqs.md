<properties
    pageTitle="Azure Active Directory B2C: Gyakori kérdések |} Microsoft Azure"
    description="Gyakori kérdések a Azure Active Directory B2C"
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
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: gyakori kérdések

Ezen az oldalon az Azure Active Directory (Azure Active Directory) B2C kapcsolatos gyakori kérdésekre ad választ. Tartsa vissza frissítések keresése.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Van lehetőség Azure Active Directory B2C funkciók a már meglévő, alkalmazott Azure AD-ös bérlői webhelyemhez?

Jelenleg Azure Active Directory B2C funkciók nem kapcsolható be a meglévő Azure AD-bérlő. Azt javasoljuk, hogy a Azure Active Directory B2C funkciók használatával kezelheti a fogyasztói külön bérlő létrehozása.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Van lehetőség Azure Active Directory B2C közösségi login (Facebook és a Google +) megadásához be az Office 365?

Azure Active Directory B2C nem használhatók a Microsoft Office 365-tel. Az általános nem használható a hitelesítés bármely szoftver-alkalmazás (Office 365-ben, Salesforce, kalk.munkanap stb.). Identitás-és az access csak nyújt fogyasztói elérésű webhely és a mobilalkalmazások, és esetében nem alkalmazható alkalmazott vagy partner esetek.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Mik azok az Azure Active Directory B2C helyi fiókok? Miben különböznek ezek a munkahelyi vagy iskolai fiókokat az Azure Active Directory?

Egy Azure AD-bérlői webhelyen, a bérlői webhelyemen (kivéve a Microsoft-fiókkal rendelkező felhasználók) minden felhasználó bejelentkezik be egy e-mail címet az űrlap `<xyz>@<tenant domain>`, ahol `<tenant domain>` egy igazolt tartományt a bérlő vagy a kezdeti `<...>.onmicrosoft.com` tartományt. Az ilyen típusú fiók egy munkahelyi vagy iskolai fiókjával.

Az Azure Active Directory B2C bérlői, a legtöbb alkalmazások szeretné a felhasználó, jelentkezzen be a bármely tetszőleges e-mail címét (például joe@comcast.net, bob@gmail.com, sarah@contoso.com, vagy jim@live.com). Fiók az ilyen típusú helyi fiók. Ma is támogatjuk tetszőleges felhasználónevek (egyszerű karakterláncok) helyi partnerként (például Mintaügyvezető, Péter, Anna vagy jim). Az Azure Active Directory B2C szolgáltatás e két helyi fióktípusok közül választhat.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Melyik közösségi Identitásszolgáltatók támogatják a most? Melyek a későbbi támogatási tervez?

Jelenleg támogatott diagramtípusról Facebook, a Google +, LinkedIn és Amazon. Hozzáadunk más népszerű közösségi Identitásszolgáltatók vevői igények alapján támogatása.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Állítható be egy keresési fogyasztói kapcsolatos további információk összegyűjtése az különböző közösségi Identitásszolgáltatók?

Nem, de ez a funkció a saját ütemterv. Az alapértelmezett hatókörök használható a közösségi Identitásszolgáltatók támogatott csoportja a következők:

- Facebook: e-mailben
- A Google +: e-mailben
- Microsoft-fiókkal: openid levelezési profil
- Amazon: profil
- LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Az alkalmazás muszáj futtatható Azure azt az Azure Active Directory B2C működik?

Nem, akkor üzemeltethet az alkalmazás tetszőleges (a helyszíni vagy felhőalapú). Együttműködhet az Azure Active Directory B2C szüksége van az összes, az azt jelenti, hogy a küldési és fogadási nyilvánosan hozzáférhető végpontok HTTP összehívásokat.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Több Azure Active Directory B2C bérlők-e. Hogyan kezelhetem őket az Azure-portálra?

Egyes Azure Active Directory B2C bérlői van saját B2C szolgáltatások lap az Azure-portálra. Lásd: [Azure Active Directory B2C: az alkalmazás regisztrálása](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) megtudhatja, hogy meg tudja nyitni egy adott bérlői B2C szolgáltatások lap az Azure portálon. Váltás az Azure Active Directory B2C könyvtárak az Azure portálon nem őrzik meg a B2C szolgáltatások lap nyissa meg a legtöbb böngészőben.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Hogyan szabhatom testre ellenőrzés e-mailek (a tartalom és a "a:" mező) Azure Active Directory B2C által küldött?

A [vállalat esetleges szolgáltatás](../active-directory/active-directory-add-company-branding.md) segítségével ellenőrzés e-mailek tartalmának testreszabását. Konkrétan a két elem a levelezés testre szabható:

- **Szalagcím embléma**: jobb alsó látható.
- **Háttérszín**: a képernyő tetején látható.

    ![Képernyőkép: a testre szabott megerősítő e-mailt](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Az e-mail aláírás a B2C bérlő nevét, az B2C bérlői webhely első létrehozásakor megadott tartalmazza. Az alábbi utasításokat követve nevét módosíthatja:

- Az előfizetés-rendszergazdaként jelentkezzen be az az [Azure klasszikus portálon](https://manage.windowsazure.com/) .
- Nyissa meg az B2C bérlői webhelyen.
- Kattintson a **beállítás** lapon.
- A **címtár-tulajdonságok** csoportjában a **név** mezőben módosíthatja.
- A lap alján a **Mentés** gombra.

Jelenleg nincs mód módosítása a "a:" mezőt az e-mailt. Ha érdeklik a ezt a lehetőséget, és a teljes mértékben testreszabása a megerősítő e-mailt törzsében, szavazzon a [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails)funkciót.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Hogyan lehet tudom áttelepíteni a meglévő felhasználóneveket, a jelszavakat és a profilok az adatbázisból az Azure Active Directory B2C?

Az Azure Active Directory Graph API segítségével írja be az áttelepítési eszköz. Lásd: További információ a [Graph API minta](active-directory-b2c-devquickstarts-graph-dotnet.md) . Különböző áttelepítési lehetőségek és eszközök ki az-kész későbbi lesz elérhető.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Milyen jelszóházirend az Azure Active Directory B2C helyi fiókok használják?

A helyi fiókok Azure Active Directory B2C házirendet a házirend alapján Azure AD. Azure Active Directory B2C előfizetési, előfizetési vagy bejelentkezési és a jelszó alaphelyzetbe házirendek használja a "erős" jelszó erőssége, és nem jár le a megadott jelszavakat. Olvassa el az [Azure Active Directory jelszóházirend](https://msdn.microsoft.com/library/azure/jj943764.aspx) további információt.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Azure AD Connect segítségével áttelepítése a helyszíni Active Directoryval, hogy az Azure Active Directory B2C tárolt fogyasztói identitások?

Nem, Azure AD Connect van nem verziójához készült Azure Active Directory B2C. Különböző áttelepítési lehetőségek és eszközök ki az-kész későbbi lesz elérhető.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure Active Directory B2C például Microsoft Dynamics CRM rendszerrel működik?

Jelenleg nem. Ezek a rendszerek integrálása az ütemterv van.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure Active Directory B2C jelent dolgozhat a helyszíni SharePoint 2016 vagy korábbi verzió?

Jelenleg nem. Azure Active Directory B2C nem tartozik, amelyek portálokhoz és az e-kereskedelmi alkalmazásaival épülő SharePoint helyszíni szükséges SAML 1.1 tokenek támogatása. Megjegyzendő, hogy az Azure Active Directory B2C nem értendő a SharePoint külső partner megosztása eset; [Azure Active Directory B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) ehelyett látható.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Azure Active Directory B2C vagy használjam B2B a külső felhasználók kezelése?

További tudnivalók a [külső identitások](../active-directory/active-directory-b2b-compare-external-identities.md) , ha többet szeretne tudni a megfelelő funkciók alkalmazása a külső identitás esetek Ez a cikk.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Milyen jelentéskészítési és naplózási szolgáltatások Azure Active Directory B2C nyújt? Azok a ezeket ugyanúgy Azure Active Directory prémium?

Azure Active Directory B2C nem, nem támogatja a ugyanazok a jelentések másként Azure Active Directory prémium verzióban. Azure Active Directory B2C fog kibocsátásának folyamatát, egyszerű jelentése és hamarosan Képletvizsgálat API-khoz.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>A felhasználói felület Azure Active Directory B2C, melyet lapok is localize? Mely nyelveket támogatja?

Jelenleg Azure Active Directory B2C van optimalizálva angol csak. Minél korábban bevezetésére honosítási szolgáltatások tervezzük.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Használhatom a saját URL-ek a saját előfizetési és a bejelentkezési lapon, Azure Active Directory B2C kombinációja? Például módosíthatom az URL-címet a login.microsoftonline.com login.contoso.com?

Jelenleg nem. Ez a funkció a saját ütemterv. Tartsa szem előtt, hogy a **tartományok** lapon a a bérlő a Azure klasszikus portálon a tartomány hitelesítésével nem tölti le.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hogyan törölhetők a Azure Active Directory B2C bérlői webhelyemhez?

Kövesse ezeket a lépéseket követve az Azure Active Directory B2C bérlői webhelyen törlése:

- Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
- **Alkalmazások**, **Identitásszolgáltatók** és **az összes házirendek** rögzítéséhez nyissa meg, és törölje az egyes azokat a bejegyzéseket.
- Jelentkezzen be az [Azure klasszikus portál](https://manage.windowsazure.com/) az előfizetés-rendszergazdaként. (Ez a ugyanazt a munkahelyi vagy iskolai fiók vagy a Microsoft-fiókot regisztrálni szeretne az Azure használt.)
- Nyissa meg az Active Directory-bővítmény a bal oldalon, majd kattintson az B2C bérlői webhelyen.
- Kattintson a **felhasználók** fülre.
- Válassza a minden felhasználó bekapcsolása (kizárható a felhasználó be van jelentkezve, tehát az előfizetés rendszergazdaként). A lap alján a **Törlés** gombra, és kattintson az **Igen** gombra.
- Kattintson az **alkalmazások** fülre.
- A **Megjelenítés** legördülő mezőben válassza az **alkalmazások, a vállalatom tulajdonában van** , és kattintson a pipára.
- Az alább felsorolt **b2c-bővítmények-app** nevű alkalmazás megjelenik. A lap alján a **Törlés** gombra, és kattintson az **Igen** gombra.
- Nyissa meg az Active Directory-bővítmény újra, majd jelölje ki az B2C bérlői webhelyen.
- Kattintson a **Törlés** az oldal alján. Kövesse a képernyőn a folyamat befejezéséhez.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Azure Active Directory B2C vállalati mobilitás csomagja részeként szerezhetők be?

Nem, Azure Active Directory B2C egy kirovó Azure szolgáltatás pedig nem vállalati mobilitás csomag része.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Hogyan lehet jelenteni az Azure Active Directory B2C kapcsolatos problémákat?

[Támogatási kérelem Azure Active Directory B2C-fájl](active-directory-b2c-support.md)megtekintéséhez.

## <a name="more-information"></a>További információ

Érdemes azt is, tekintse át az aktuális [szolgáltatáselérhetőség, korlátozások, és korlátozások](active-directory-b2c-limitations.md).
