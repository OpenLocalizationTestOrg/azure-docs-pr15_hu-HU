<properties
    pageTitle="Előfeltételek API jelentéskészítés az Azure AD eléréséhez. | Microsoft Azure"
    description="További tudnivalók az Azure AD-jelentése API eléréséhez az Előfeltételek:"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Az Azure AD-API jelentéskészítés eléréséhez vonatkozó követelmények 

Az [API-khoz jelentéskészítés Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) a többi-alapú API-khoz keresztül programozott hozzáférés szükséges. Számos különböző programozási nyelven és eszközök ezek API-khoz is felhívhatja.

A jelentés API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) használ a webes API-hoz való hozzáférés engedélyezése. 

Felkészülés az access a jelentéskészítési API-hoz, a következőket kell tennie:

1. Az alkalmazás a Azure AD-bérlő létrehozása 

2. Az alkalmazás megfelelő engedélyeket az Azure Active Directory-adatok eléréséhez

3. A címtár konfigurációs beállítások összegyűjtése

A kérdésekre, problémák vagy visszajelzést, kérjük, forduljon [AAD: segítséget](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Azure AD-alkalmazás létrehozása

Konfigurálása a címtárában az Azure AD-jelentése API eléréséhez, meg kell jelentkezzen be az Azure klasszikus portálon, kijelölt egy Azure előfizetés is a tagja a globális rendszergazdai címtár szerepkör az Azure Active Directory-ös bérlői rendszergazdai fiókkal.

> [AZURE.IMPORTANT] Lehet, hogy a hitelesítő adatok területén jelennek meg a "rendszergazdai" engedélyekkel futtatott alkalmazások nagyon nagy teljesítményű, így mindenképp az alkalmazás azonosítója/titkos hitelesítő adatok biztonsága.


1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Az active directory** listában jelölje ki a címtárban.

3. A felső sávon kattintson az **alkalmazások**elemre.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Az alsó sávján kattintson a **Hozzáadás**gombra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/03.png) 

5. A a **milyen feladatot szeretne tenni?** párbeszédpanelen kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**gombra. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/04.png) 


6. A **mondja el nekünk az alkalmazásról** párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/05.png) 

    egy. Az **neve** mezőbe írjon be egy nevet (például: API-alkalmazás jelentése).

    b. Jelölje be a **webes alkalmazás és/vagy a webes API-val**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.


7. Az **alkalmazás tulajdonságai** párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/06.png) 

    egy. Az **bejelentkezési URL** mezőbe írja be a `https://localhost`.

    b. Az **Alkalmazás azonosítója URI** mezőbe írja be a ```https://localhost/ReportingApiApp```.

    c billentyűkombinációt. Kattintson a **kész**gombra.



## <a name="grant-your-application-permission-to-use-the-api"></a>Alkalmazás engedélyt adhat az API

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Az active directory** listában jelölje ki a címtárban.

3. A felső sávon kattintson az **alkalmazások**elemre.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png)


3. Az alkalmazások listájában jelölje ki az újonnan létrehozott alkalmazást.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)

4. A felső sávon kattintson a **Konfigurálás**gombra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)


5. **Engedélyek egyéb alkalmazások** csoportban az **Azure Active Directory** erőforrás kattintson a **Szolgáltatásalkalmazás jogosultságainak** legördülő listára, és válassza a **címtár-adatok olvasása**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/09.png)


5. Az alsó sávján kattintson a **Mentés**gombra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>A címtár konfigurációs beállítások összegyűjtése

Ez a szakasz megtudhatja, hogy miként juthat az alábbi beállítások a címtárában:

- Tartománynév
- Ügyfél-azonosító
- Ügyfél titkos kulcs

Hívások átirányítása a jelentéskészítési API konfigurálásakor szüksége ezeket az értékeket. 


### <a name="get-your-domain-name"></a>A tartománynév beszerzése

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Az active directory** listában jelölje ki a címtárban.

3. A felső sávon kattintson a **tartományok**hivatkozásra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/11.png) 

4. A **Domain Name** oszlop másolja a vágólapra a tartománynév.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Az alkalmazás ügyfél-azonosító beszerzése

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Az active directory** listában jelölje ki a címtárban.

3. A felső sávon kattintson az **alkalmazások**elemre.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Az alkalmazások listájában jelölje ki az újonnan létrehozott alkalmazást.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)

4. A felső sávon kattintson a **Konfigurálás**gombra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)

4. Másolja a vágólapra a **ügyfél-azonosító**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Az alkalmazás ügyfél titkos beszerzése

Úgy juthat az alkalmazás ügyfél titkos, szüksége egy új kulcs létrehozása és mentése az új kulcs, mert nem lehetséges a továbbiakban újabb beolvasásához ezt az értéket, az érték mentése.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Az active directory** listában jelölje ki a címtárban.

3. A felső sávon kattintson az **alkalmazások**elemre.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Az alkalmazások listájában jelölje ki az újonnan létrehozott alkalmazást.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)

4. A felső sávon kattintson a **Konfigurálás**gombra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)

5. A **hívóbetűk** csoportban hajtsa végre az alábbi lépéseket: 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/14.png)

    egy. Az időtartam listából válassza ki az időtartamot

    b. Az alsó sávján kattintson a **Mentés**gombra.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)

    c billentyűkombinációt. Másolja a kulcs értékét.

## <a name="next-steps"></a>Következő lépések

- Szeretné elérni az adatokat az API jelentési programozott módon Azure AD-ból? Nézze meg az [első lépések az Azure Active Directory jelentéskészítés API](active-directory-reporting-api-getting-started.md).

- Ha szeretne többet megtudni az Azure Active Directory-jelentése, című az [Azure Active Directory jelentési útmutató](active-directory-reporting-guide.md).  
