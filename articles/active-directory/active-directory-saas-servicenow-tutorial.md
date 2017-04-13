<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ServiceNow és a ServiceNow Express |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és ServiceNow és ServiceNow Express között."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Oktatóprogram: Azure Active Directory-integráció a ServiceNow és ServiceNow Express.


Ebből az oktatóanyagból megismerheti, hogyan ServiceNow és ServiceNow Express integrálása az Azure Active Directory (Azure Active Directory).

ServiceNow és ServiceNow Express integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel ServiceNow és ServiceNow Express megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on ServiceNow és ServiceNow Express (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció ServiceNow és ServiceNow Express van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- ServiceNow, példány vagy ServiceNow, Calgary verzió bérlő vagy újabb
- ServiceNow Express, ServiceNow Express, Helsinki verzió egy példányának vagy újabb
- ServiceNow bérlői rendelkeznie kell a [Több szolgáltató egyszeri bejelentkezési a bővítmény](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) engedélyezve van. Ez lehet tenni a <https://hi.service-now.com/> szolgáltatási kérelem benyújtása 


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. ServiceNow hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés ServiceNow vagy ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a ServiceNow vagy ServiceNow Express, meg kell ServiceNow hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből. 

**A gyűjteményből ServiceNow felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **ServiceNow**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. Az eredmény ablaktáblában jelölje ki a **ServiceNow**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a szakaszban beállíthatja és ServiceNow vagy ServiceNow Express Azure Active Directory egyszeri bejelentkezés a próba "Britta Simon" nevű próba felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a ServiceNow megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó ServiceNow hivatkozás viszonya kell létrehozni.
A hivatkozás kapcsolat jön létre által az Azure Active Directory ServiceNow **felhasználónév** értékként a **felhasználó neve** értéket rendeli. Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való ServiceNow tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés az ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
3. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy ServiceNow tesztelése](#creating-a-servicenow-test-user)** - van-e egy megfelelője a Britta Simon ServiceNow, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
6. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

> [AZURE.NOTE] Ha be szeretné állítani a ServiceNow lépés: 2 elhagyja. Hasonlóképpen ha be szeretné állítani a ServiceNow Express kihagyása lépés: 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Azure Active Directory Single Sign-On ServiceNow konfigurálása

1.  Az Azure Active Directory klasszikus portálon **ServiceNow** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az ServiceNow felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás beállításainak megadása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Állítsa be az app URL-címe")

    egy. az **URL a bejelentkezési ServiceNow** mezőbe írja be a bejelentkezés az ServiceNow alkalmazás, a mintázat követően a felhasználók által használt URL-címet: `https://<instance-name>.service-now.com`.

    b. Az **azonosító** mezőbe írja be a való bejelentkezés az ServiceNow alkalmazás, a mintázat követően a felhasználók által használt URL-címe: `https://<instance-name>.service-now.com`.

    c billentyűkombinációt. Kattintson a **Tovább** gombra

4.  Ahhoz, hogy automatikusan konfigurálja a SAML-alapú hitelesítés ServiceNow Azure Active Directory, írja be a ServiceNow példány nevét, a rendszergazdai felhasználónevével és a rendszergazdai jelszavát az **egyszeri bejelentkezés beállítása automatikus** űrlap, majd kattintson a *konfigurálása*. Látható, hogy a rendszergazdai felhasználónevével megadott kell rendelkeznie az **security_admin** szerepkört ServiceNow ahhoz, hogy ez a munkát. Ellenkező esetben Azure Active Directory tartományként a SAML identitásszolgáltató ServiceNow kézi beállításához, **manuálisan állítsa be az alkalmazás az egyszeri bejelentkezési**, kattintson, majd kattintson a **Tovább** gombra, és hajtsa végre az alábbi lépéseket.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Állítsa be az app URL-címe")



5.  A **konfigurálása az egyszeri bejelentkezés a ServiceNow** lapon kattintson a **Letöltés tanúsítvány**, mentse a tanúsítvány fájlt a számítógépen helyben.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Beállítás az egyszeri bejelentkezés")

1. Bejelentkezés az ServiceNow alkalmazás rendszergazdaként.
2. A _több szolgáltató egyszeri bejelentkezéses Installer-integráció -_ bővítmény aktiválása a következő lépéseket követve:

    egy. A navigációs ablakban, a bal oldalon **a rendszer meghatározása** szakasz, és válassza a **bővítmények**.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "A bővítmény aktiválása")
    
    b. Keresse meg a _integráció - több szolgáltató egyszeri bejelentkezéses telepítőt_.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "A bővítmény aktiválása")

    c billentyűkombinációt. Jelölje ki a bővítményt. Rigth kattintson, és válassza az **Aktiválás és frissítés**.
    
    d. Kattintson az **Aktiválás** gombra.

2. A bal oldali navigációs ablakban kattintson a **Tulajdonságok**parancsra.  

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Állítsa be az app URL-címe")


3. A **Több szolgáltató SSO tulajdonságai** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Állítsa be az app URL-címe")

    egy. **Több szolgáltató SSO engedélyezése**jelölje ki a **Igen**.

    b. **A hibakeresési naplózás használ a több szolgáltató SSO integráció engedélyezése**jelölje ki a **Igen**.

    c billentyűkombinációt. **A mező a felhasználó... táblázat, amely** mezőbe írja be a **felhasználónév**.

    d. Kattintson a **Mentés**gombra.



1. A bal oldali navigációs ablakban kattintson a **tanúsítványok x509**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Beállítás az egyszeri bejelentkezés")


1. A **Tanúsítványok X.509** párbeszédpanelen kattintson az **Új**gombra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Beállítás az egyszeri bejelentkezés")


1. A **Tanúsítványok X.509** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Beállítás az egyszeri bejelentkezés")

    egy. Kattintson az **Új**gombra.

    b. Az **neve** mezőbe írja be a konfigurációban nevét (például: **TestSAML2.0**).

    c billentyűkombinációt. Válassza az **aktív**.

    d. Jelölje ki a **formátumot**, **PEM**.

    e. **Típusa**csoportban jelölje ki a **Megbízhatónak áruházból tanúsítvány**.
    
    f. Nyissa meg a kódolt Base64 tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **PEM tanúsítvány** mezőben lévő értéket szeretné.

    g. Kattintson a **frissítés**gombra.


1. Kattintson a navigációs ablak bal oldalán, a **Identitásszolgáltatók**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Beállítás az egyszeri bejelentkezés")

1. A **Identitásszolgáltatók** párbeszédpanelen kattintson az **Új**gombra:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Beállítás az egyszeri bejelentkezés")


1. **Identitásszolgáltatók** párbeszédpanelen kattintson **SAML2 Update1?**:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Beállítás az egyszeri bejelentkezés")


1. A SAML2 Update1 tulajdonságai párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Beállítás az egyszeri bejelentkezés")


    egy. az **neve** mezőbe írja be a konfigurációban nevét (például: **a SAML 2.0**).

    b. A **Felhasználói mező** szövegdoboz, írja be **e-mail** vagy **user_id**, attól függően, amely mező segítségével a felhasználók a ServiceNow környezetben egyedileg azonosító. 
    
    > [AZURE.NOTE] Configue Azure Active Directory elhelyezése az Azure Active Directory felhasználói azonosító (tőke neve) vagy az e-mail címet, a SAML-jogkivonat az egyedi azonosító nyissa meg a lehetőségek közül választhat a **ServiceNow > attribútumok > egyszeri bejelentkezés** az Azure klasszikus portált, és a kívánt mező hozzárendelése a **nameidentifier** attribútum szakaszát. A kijelölt attribútum az Azure Active Directory (például egyszerű felhasználónév) tárolt érték meg kell egyeznie a beírt mező (például user_id) ServiceNow tárolt érték

    c billentyűkombinációt. Az Azure Active Directory-klasszikus portálon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **Identitás szolgáltató URL-címe** mezőben lévő értéket.

    d. Az Azure Active Directory-klasszikus portálon a **Hitelesítési kérése URL-címe** értéket másolja és illessze be a **identitásszolgáltató használatához AuthnRequest** mezőben lévő értéket.

    e. Az Azure Active Directory-klasszikus portálon másolja az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket, és illessze be a **identitásszolgáltató használatához SingleLogoutRequest** mezőben lévő értéket.

    f. Az **ServiceNow kezdőlap** mezőbe írja be az URL-CÍMÉT az ServiceNow példány kezdőlapját.

    > [AZURE.NOTE] A ServiceNow példány kezdőlap a **ServieNow bérlői webhely URL-cím** és a **/navpage.do** összefűzése (pl.:`https://fabrikam.service-now.com/navpage.do`).
 

    g. Az a **entitás azonosítója / kibocsátó** mezőbe írja be a ServiceNow bérlői webhely URL-CÍMÉT.

    h. **A célközönség URL-cím** mezőbe írja be a ServiceNow bérlői webhely URL-CÍMÉT. 

    lehet. Az **A IDP SingleLogoutRequest kötelező Protocol (protokoll)** mezőbe írja be a **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítás**.

    j. Az NameID házirend mezőbe írja be a **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: meghatározatlan**.

    k. **Hozzon létre egy AuthnContextClass**kattintva szüntesse meg.

    l. Írja be a **AuthnContextClassRef módszer** `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Csak akkor szükséges, ha Ön a felhőben csak szervezeti. Ha használja az helyszíni környezetbe ADFS vagy MFA-hitelesítés majd nem konfigurálja a ezt az értéket. 

    m =. Az **Óra ferdeség** mezőbe írja be a **60**.

    az n billentyűt. **Egyszeri bejelentkezés a parancsfájl**jelölje be a **MultiSSO_SAML2_Update1**.

    o. Mint **x509 tanúsítvány**, jelölje ki a tanúsítványt az előző lépésben létrehozott.

    p. Kattintson a **Küldés**gombra. 



6. Az Azure Active Directory klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Beállítás az egyszeri bejelentkezés")

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.
 
    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Beállítás az egyszeri bejelentkezés")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Azure Active Directory Single Sign-On ServiceNow Express konfigurálása

1.  Az Azure Active Directory klasszikus portálon **ServiceNow** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az ServiceNow felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás beállításainak megadása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Állítsa be az app URL-címe")

    egy. az **URL a bejelentkezési ServiceNow** mezőbe írja be a bejelentkezés az ServiceNow alkalmazás, a mintázat követően a felhasználók által használt URL-címet: `https://<instance-name>.service-now.com`.

    b. Az **Kibocsátó URL-cím** mezőbe írja be a való bejelentkezés az ServiceNow alkalmazás, a mintázat követően a felhasználók által használt URL-cím `https://<instance-name>.service-now.com`.

    c billentyűkombinációt. Kattintson a **Tovább** gombra

4.  Kattintson a **kézi megadása az alkalmazás az egyszeri bejelentkezési**, majd kattintson a **Tovább** gombra, és hajtsa végre az alábbi lépéseket.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Állítsa be az app URL-címe")

5.  A **Configure egyszeri bejelentkezés ServiceNow a** lapon kattintson a **tanúsítvány letöltése**, mentse a tanúsítvány fájlt a számítógépen helyben, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Beállítás az egyszeri bejelentkezés")

6. Bejelentkezés az ServiceNow Express alkalmazás rendszergazdaként.

7. A bal oldali navigációs ablakban kattintson **Az egyszeri bejelentkezés**.  

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Állítsa be az app URL-címe")


8. Az **Egyszeri bejelentkezés** párbeszédpanelen kattintson a jobb felső sarokban a konfigurációs ikonjára, és adja meg a következő tulajdonságokat:

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Állítsa be az app URL-címe")

    egy. **Több szolgáltató SSO engedélyezése** váltása jobbra.

    b. Váltás a **hibakeresési naplózás az több szolgáltató SSO integráció engedélyezése** jobbra.

    c billentyűkombinációt. **A mező a felhasználó... táblázat, amely** mezőbe írja be a **felhasználónév**.


9. **Egyszeri bejelentkezés** párbeszédpanelen kattintson az **Új tanúsítvány hozzáadása**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Beállítás az egyszeri bejelentkezés")



10. A **Tanúsítványok X.509** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Beállítás az egyszeri bejelentkezés")

    egy. Az **neve** mezőbe írja be a konfigurációban nevét (például: **TestSAML2.0**).

    b. Válassza az **aktív**.

    c billentyűkombinációt. Jelölje ki a **formátumot**, **PEM**.

    d. **Típusa**csoportban jelölje ki a **Megbízhatónak áruházból tanúsítvány**.

    e. A letöltött tanúsítvány kódolt Base64 fájl létrehozása
   
    > [AZURE.NOTE] További részletekért megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálja](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Nyissa meg a kódolt Base64 tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **PEM tanúsítvány** mezőben lévő értéket szeretné.

    g. Kattintson a **frissítés**gombra.


11. **Egyszeri bejelentkezés** párbeszédpanelen kattintson az **Új IdP hozzáadása**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Beállítás az egyszeri bejelentkezés")


12. Az **Új identitásszolgáltató hozzáadása** párbeszédpanelen **Identitásszolgáltató konfigurálása**csoportban hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Beállítás az egyszeri bejelentkezés")


    egy. Az **neve** mezőbe írja be a konfigurációban nevét (például: **a SAML 2.0**).

    b. Az Azure Active Directory-klasszikus portálon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **Identitás szolgáltató URL-címe** mezőben lévő értéket.

    c billentyűkombinációt. Az Azure Active Directory-klasszikus portálon a **Hitelesítési kérése URL-címe** értéket másolja és illessze be a **identitásszolgáltató használatához AuthnRequest** mezőben lévő értéket.

    d. Az Azure Active Directory-klasszikus portálon másolja az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket, és illessze be a **identitásszolgáltató használatához SingleLogoutRequest** mezőben lévő értéket.

    e. **Identitás szolgáltató tanúsítvány**jelölje ki a tanúsítványt az előző lépésben létrehozott.


13. Kattintson a **Speciális beállítások**gombra, és **További identitás szolgáltató tulajdonságai**csoportban hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Beállítás az egyszeri bejelentkezés")

    egy. Az **A IDP SingleLogoutRequest kötelező Protocol (protokoll)** mezőbe írja be a **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítás**.

    b. Az **NameID házirend** mezőbe írja be a **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: meghatározatlan**.    

    c billentyűkombinációt. Írja be a **AuthnContextClassRef módszer** **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. **Hozzon létre egy AuthnContextClass**kattintva szüntesse meg.

14. **További szolgáltató tulajdonságai**csoportban hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Beállítás az egyszeri bejelentkezés")

    egy. Az **ServiceNow kezdőlap** mezőbe írja be az URL-CÍMÉT az ServiceNow példány kezdőlapját.

    > [AZURE.NOTE] A ServiceNow példány kezdőlap a **ServieNow bérlői webhely URL-cím** és a **/navpage.do** összefűzése (pl.: `https://fabrikam.service-now.com/navpage.do`).

    b. Az a **entitás azonosítója / kibocsátó** mezőbe írja be a ServiceNow bérlői webhely URL-CÍMÉT.

    c billentyűkombinációt. **A célközönség URI** mezőbe írja be a ServiceNow bérlői webhely URL-CÍMÉT. 

    d. Az **Óra ferdeség** mezőbe írja be a **60**.

    e. A **Felhasználói mező** szövegdoboz, írja be **e-mail** vagy **user_id**, attól függően, amely mező segítségével a felhasználók a ServiceNow környezetben egyedileg azonosító.
    
    > [AZURE.NOTE] Configue Azure Active Directory elhelyezése az Azure Active Directory felhasználói azonosító (tőke neve) vagy az e-mail címet, a SAML-jogkivonat az egyedi azonosító nyissa meg a lehetőségek közül választhat a **ServiceNow > attribútumok > egyszeri bejelentkezés** az Azure klasszikus portált, és a kívánt mező hozzárendelése a **nameidentifier** attribútum szakaszát. A kijelölt attribútum az Azure Active Directory (például egyszerű felhasználónév) tárolt érték meg kell egyeznie a beírt mező (például user_id) ServiceNow tárolt érték

    f. Kattintson a **Mentés**gombra. 


15. Az Azure Active Directory klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Beállítás az egyszeri bejelentkezés")

16. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.
 
    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ez a szakasz célja, hogyan engedélyezheti a felhasználó Active Directory felhasználói fiókok ServiceNow kiépítési tagolása.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1. Azure-kezelés klasszikus portálon **ServiceNow** alkalmazás integrációs lapon kattintson a **konfigurálás felhasználói kiépítési**. 

    ![Felhasználói kiépítése] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Felhasználói kiépítése")


2. **Adja meg ahhoz, hogy a felhasználó automatikus kiépítési ServiceNow hitelesítő adatait** lapon adja meg a következő beállítások: felhasználói kiépítési konfigurálása 

     egy. Az **ServiceNow példányának neve** mezőbe írja be a ServiceNow-példány nevét.

     b. Az **ServiceNow rendszergazdai felhasználónevével** mezőbe írja be a nevét a ServiceNow rendszergazdai fiókjával.

     c billentyűkombinációt. Az **ServiceNow rendszergazdai jelszó** mezőbe írja be a fiókhoz tartozó jelszót.

     d. Kattintson **az érvényesítés** a beállításainak ellenőrzése.

     e. Kattintson a **Tovább** gombra kattintva nyissa meg a **következő lépésekkel** lapot.

     f. Ha azt szeretné, ez az alkalmazás minden felhasználó hozhatók létre, válassza a "az**összes felhasználói fiókok ennek az alkalmazásnak a címtárban automatikusan kiépítése**". 

    ![Következő lépések] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Következő lépések")

     g. A **következő lépések** lapon kattintson a **kész** menti a konfigurációt.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   


### <a name="creating-a-servicenow-test-user"></a>ServiceNow próba felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű ServiceNow. Ebben a részben hozzon létre egy felhasználót Britta Simon nevű ServiceNow. Ha nem tudja, hogy a felhasználók hozzáadását ServiceNow vagy ServiceNow Express fiókja, ServiceNow támogatási csoport munkatársaitól kaphat.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít ServiceNow használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése ServiceNow, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **ServiceNow**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Amikor az Access panel a ServiceNow mozaik gombra kattint, meg kell első automatikusan aláírt-on az ServiceNow alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
