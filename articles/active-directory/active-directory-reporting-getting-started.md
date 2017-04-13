<properties
   pageTitle="Azure Active Directory jelentés: Első lépések |} Microsoft Azure"
   description="Megjeleníti a különböző rendelkezésre álló jelentések az Azure Active Directory-jelentésekben"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Első lépések – az Azure Active Directory jelentése

## <a name="what-it-is"></a>Mi az

Azure Active Directory (Azure Active Directory) biztonsági, a tevékenység és a naplójelentések a könyvtár tartalmazza. Az alábbiakban listája a jelentések tartalmazza:

### <a name="security-reports"></a>Biztonsági jelentések

- Bejelentkezési bővítmények ismeretlen forrásokból
- Bejelentkezési bővítmények több hiba után
- A több geographies bejelentkezések
- A gyanús tevékenység IP-címek bejelentkezések
- Szabálytalan bejelentkezési tevékenység
- Bejelentkezési bővítmények esetleg fertőzött eszközökről
- Anomalous bejelentkezési tevékenységeket használó felhasználók

### <a name="activity-reports"></a>Tevékenység-jelentések

- Alkalmazás használatát: összefoglalása
- Alkalmazás használatát: részletes
- Alkalmazás irányítópult
- Áttelepítési hibákkal fiók
- Egyéni felhasználói eszközök
- Egyes felhasználói tevékenységek
- Csoportok jelentés a tevékenységek
- Jelszó alaphelyzetbe állítása regisztrációs Tevékenységjelentés
- Az új jelszó kérésének tevékenység

### <a name="audit-reports"></a>Naplózási jelentések

- A címtár-naplójelentés

> [AZURE.TIP] Azure Active Directory-jelentés további dokumentációt tanulmányozza [a hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Működése


### <a name="reporting-pipeline"></a>Jelentéskészítési folyamat

A jelentés folyamat három fő lépésből áll. Valahányszor a felhasználó bejelentkezik, vagy -hitelesítés történik, az alábbiak történnek:

- Első lépésként a felhasználó hitelesítése (sikeres vagy sikertelen), és az eredmény a Azure Active Directory-adatbázisok tárolja.
- Rendszeres időközönként, az összes legutóbbi bejelentkezési modulok feldolgozása. Ezen a ponton a biztonság és algoritmusok anomalous tevékenység keres a legutóbbi bejelentkezési összes bővítmények a gyanús tevékenységet.
- A feldolgozás után a jelentések írt, gyorsítótárazott, és az Azure klasszikus portálon felszolgált.

### <a name="report-generation-times"></a>Időbe telik jelentése

A nagy mennyiségű hitelesítési miatt, és jelentkezzen be az Azure Active Directory-platform által feldolgozott modulok, a legutóbbi bejelentkezési bővítmények feldolgozása, átlagosan óra régi. Ritkán eltarthat feldolgozni a legutóbbi bejelentkezési bővítmények 8 órát.

A legutóbbi feldolgozott bejelentkezés minden jelentés tetején súgószöveg vizsgálata alapján találja.

![Súgó a szöveg minden jelentés tetején](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Azure Active Directory-jelentés további dokumentációt tanulmányozza [a hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Első lépések


### <a name="sign-into-the-azure-classic-portal"></a>Jelentkezzen be az Azure klasszikus portálra

Első lépésként meg kell jelentkezzen be a [Azure klasszikus portál](https://manage.windowsazure.com) be globális vagy a megfelelőségi rendszergazda. Is kell lenniük, az Azure előfizetési szolgáltatás vagy közös rendszergazdája, vagy használja a "Azure AD hozzáférést" Azure előfizetés.

### <a name="navigate-to-reports"></a>Nyissa meg azt a jelentések

Jelentések megtekintéséhez nyissa meg a címtár tetején a jelentések fülre.

Ha első alkalommal tekinti meg a jelentések, kell egy párbeszédpanel jeleníthető meg a jelentések megtekintése előtt elfogadja. Ez a annak érdekében, hogy az elfogadható a rendszergazdák ezeket az adatokat lehet tekinteni, amely egyes országokban személyes információ megtekintése a szervezet számára.

![Párbeszédpanel](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Minden alkalommal feltárása

Nyissa meg az minden alkalommal, hogy az adatok gyűjtése és a bejelentkezési bővítmények feldolgozása. [Az összes jelentés az alábbi listában](active-directory-reporting-guide.md)megtalálhatja.

![Az összes jelentések](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Töltse le a jelentéseket a CSV-ként

Minden alkalommal letölthető CSV (pontosvesszővel tagolt) fájlként. Ezeket a fájlokat, az Excel, a PowerBI vagy a további alkalmazások adatok elemzése külső analysis is használhatja.

Bármelyik jelentés egy CSV-ként letöltéséhez keresse meg a jelentést, és kattintson a "Letöltése" a képernyő alján.

![Letöltési gomb](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Azure Active Directory-jelentés további dokumentációt tanulmányozza [a hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Következő lépések

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>A tevékenység a anomalous bejelentkezési vonatkozó értesítések testreszabása

Nyissa meg a könyvtár "Konfigurálása" lapja.

Görgessen le a "Értesítések" szakaszt.

Engedélyezheti vagy letilthatja a "Elküldése e-mailben értesítést a Anomalous bejelentkezések" szakaszt.

![Az értesítések szakaszban](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integrálása az Azure AD-API jelentése

[Első lépések a jelentési API](active-directory-reporting-api-getting-started.md)című témakör tartalmaz.

### <a name="engage-multi-factor-authentication-on-users"></a>Többtényezős hitelesítés folytatni a felhasználók számára

Jelölje ki a felhasználó jelentés.

A "MFA engedélyezése" gombra a képernyő alján.

![A képernyő alján a többtényezős hitelesítés gomb](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Azure Active Directory-jelentés további dokumentációt tanulmányozza [a hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>tudj meg többet


### <a name="audit-events"></a>A naplózandó események

Megtudhatja, hogy milyen eseményekről naplózza az [Azure Active Directory jelentéskészítés naplózása](active-directory-reporting-audit-events.md)a címtárban.

### <a name="api-integration"></a>API-integráció

Lásd: [az első lépések a jelentési API-val](active-directory-reporting-api-getting-started.md) és a [API hivatkozási dokumentáció](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Az érintéses beszerzése

E-mailek [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) visszajelzést, súgó vagy lehet, hogy kérdéseit.

> [AZURE.TIP] Azure Active Directory-jelentés további dokumentációt tanulmányozza [a hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md).
