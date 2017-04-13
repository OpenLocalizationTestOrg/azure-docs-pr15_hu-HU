<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a kalk.munkanap |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a kalk.munkanap az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Oktatóprogram: Azure Active Directory-integráció a kalk.munkanap
  
Ebben az oktatóanyagban célja Azure és a kalk.munkanap integrációja megjelenítése. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A kalk.munkanap bérlői webhelyre
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  A kalk.munkanap az alkalmazás integráció engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználói kiépítési konfigurálása

![Eset] (./media/active-directory-saas-workday-tutorial/IC782919.png "Eset")

##<a name="enabling-the-application-integration-for-workday"></a>A kalk.munkanap az alkalmazás integráció engedélyezése
  
Ez a szakasz célja tagolása a Salesforce-alkalmazás integráció engedélyezése.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás-integráció a kalk.munkanap, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-workday-tutorial/IC700994.png "Alkalmazások")

4.  Az **Alkalmazás gyűjtemény**megnyitásához kattintson **Az alkalmazás hozzáadása**, és kattintson a **Hozzáadás az alkalmazások használata a szervezet számára**.

    ![Milyen feladatot szeretne tenni?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Milyen feladatot szeretne tenni?")

5.  A **Keresés mezőbe**írja be a **kalk.munkanap**.

    ![Kalk.munkanap] (./media/active-directory-saas-workday-tutorial/IC701021.png "Kalk.munkanap")

6.  Az eredmény ablaktáblában jelölje ki a **kalk.munkanap**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Kalk.munkanap] (./media/active-directory-saas-workday-tutorial/IC701022.png "Kalk.munkanap")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a kalk.munkanap hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítványok létrehozásáról.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  A **kalk.munkanap** alkalmazás integrációs lapon kattintson a **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-workday-tutorial/IC782920.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be kalk.munkanap felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-workday-tutorial/IC782921.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-workday-tutorial/IC782957.png "Állítsa be az App URL-címe")

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-címet, jelentkezzen be a kalk.munkanap az alábbi minta használatával a felhasználók által használt:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  **Kalk.munkanap válasz URL-cím** mezőbe írja be a kalk.munkanap válasz URL-CÍMÉT az alábbi minta használatával:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]A válasz URL-címet kell rendelkeznie egy alszint tartományt (például: www, wd2, wd3, wd3-impl, wd5, wd5-impl). 
    >Használatával az alábbihoz hasonló: "*http://www.myworkday.com*" működik, de a "*http://myworkday.com*" azonban nem. 
 
4.  A **Configure egyszeri bejelentkezés a kalk.munkanap** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-workday-tutorial/IC782922.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a kalk.munkanap vállalati webhely rendszergazdaként.

6.  Nyissa meg a **menü \> munkaterület**.

    ![Munkaterület] (./media/active-directory-saas-workday-tutorial/IC782923.png "Munkaterület")

7.  Nyissa meg az **Account Administration**.

    ![Fiók felügyelete] (./media/active-directory-saas-workday-tutorial/IC782924.png "Fiók felügyelete")

8.  Nyissa meg a **bérlői beállítás – Biztonság szerkesztése**.

    ![Bérlői biztonsági szerkesztése] (./media/active-directory-saas-workday-tutorial/IC782925.png "Bérlői biztonsági szerkesztése")

9.  **Átirányítás URL-címek** csoportban hajtsa végre az alábbi lépéseket:

    ![Átirányítás URL-címei] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Átirányítás URL-címei")

    egy. Kattintson a **sor hozzáadása**gombra.

    b. A **Bejelentkezési átirányítás URL-cím** mezőben lévő értéket, és a **Mobile átirányítás URL-cím** mezőbe írja be az Azure klasszikus portál **Alkalmazás URL-címet konfigurálni** lapján megadott **Munkanap bérlői webhely URL-CÍMÉT** .
    
    c billentyűkombinációt. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a kalk.munkanap** párbeszédpanel lapon **Egyetlen Sign-Out szolgáltatás URL-cím**másolása, és illessze be a **Kijelentkezés átirányítás URL-címe** mezőben lévő értéket.

    d.  **Környezet** mezőbe írja be a környezetet nevét.  


    >[AZURE.NOTE] A környezet attribútum értékét az érték a bérlői webhely URL-címének területhez tartozik:
    >
    >-   Ha a tartomány nevét a a kalk.munkanap bérlő URL-cím kezdődik impl (pl.: *https://impl.workday.com/\<bérlői\>/login-saml2.htmld*), a **környezet** attribútum végrehajtása kell beállítani.
    >-   Ha valami mást a tartománynév kezdődik, akkor kapcsolatba kell lépnie a megfelelő **környezet** érték kalk.munkanap.

10. A **SAML beállítása** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML beállítása] (./media/active-directory-saas-workday-tutorial/IC782926.png "SAML beállítása")

    egy.  Jelölje ki a **SAML-hitelesítés engedélyezése**.

    b.  Kattintson a **sor hozzáadása**gombra.

11. A SAML Identitásszolgáltatók csoportban hajtsa végre az alábbi lépéseket:

    ![SAML Identitásszolgáltatók] (./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identitásszolgáltatók")

    egy. Az identitás-szolgáltató neve mezőbe írja be a szolgáltató nevét (például: *SPInitiatedSSO*).

    b. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a kalk.munkanap** párbeszédpanel lapon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **kibocsátó** mezőben lévő értéket.

    c billentyűkombinációt. Engedélyezze a **kalk.munkanap Initialted jelentkezzen ki**.

    d. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a kalk.munkanap** párbeszédpanel lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **Kijelentkezés kérése URL-címe** mezőben lévő értéket.


    e. Kattintson az **Identitás szolgáltató nyilvános kulcs tanúsítványt**, és kattintson a **Létrehozás**gombra. 

    ![Hozzon létre] (./media/active-directory-saas-workday-tutorial/IC782928.png "Hozzon létre")

    f. Kattintson a **x509 létrehozása nyilvános kulcs**. 
        
    ![Hozzon létre] (./media/active-directory-saas-workday-tutorial/IC782929.png "Hozzon létre")


1. A **Nézet x509 nyilvános kulcs** csoportban hajtsa végre az alábbi lépéseket: 

    ![Nyilvános kulcs x509 megtekintése] (./media/active-directory-saas-workday-tutorial/IC782930.png "Nyilvános kulcs x509 megtekintése") 

    egy. Az **neve** mezőbe írja be a tanúsítvány nevét (például: *PPE\_SP*).
        
    b. Az **Érvényes a** mezőbe írja be az Érvénytelen attribútumérték a tanúsítvány.
    
    c billentyűkombinációt.  Az **Érvényes szeretne** mezőbe írja be a tanúsítvány attribútumérték érvényes.
        
    >[AZURE.NOTE] Amely letölthető a érvényes dátumot és az érvényes dupla kattintással a letöltött tanúsítvány a dátum. A dátumok jelennek meg a **Részletek** lapon.

    d. A letöltött tanúsítvány **Alap-64 kódolva** fájl létrehozása  

    >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    e.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, és másolja a tartalmát.
    
    f.  Az **igazolás** mezőbe a vágólap tartalmának beillesztése
    
    g.  Kattintson az **OK gombra**.

12.  Hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "Egyszeri bejelentkezés beállítása")

    egy.  Engedélyezze a **x509 titkos kulcs párt**.

    b.  Az **Service Provider azonosítója** mezőbe írja be a **http://www.workday.com**.

    c billentyűkombinációt.  Jelölje ki a **engedélyezése SP kezdeményezett SAML-hitelesítést**.

    d.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a kalk.munkanap** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **IdP SSO szolgáltatás URL-címe** mezőben lévő értéket.
     
    e. Válassza a **nem homorú SP kezdeményezésére hitelesítési kérelmet**.

    f. **Hitelesítési kérése aláírás módszer**jelölje be a **SHA256**. 
        
    ![Hitelesítési módszer aláírás kérése] (./media/active-directory-saas-workday-tutorial/IC782932.png "Hitelesítési módszer aláírás kérése") 
 
    g. Kattintson az **OK gombra**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a kalk.munkanap** lapon kattintson a **Tovább**gombra. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-workday-tutorial/IC782934.png "Beállítás az egyszeri bejelentkezés")

13. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra. 

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Beállítás az egyszeri bejelentkezés")



##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
A kalk.munkanap kiépítéstől próba felhasználó juthat, meg kell a kalk.munkanap támogatási csoport munkatársaitól kaphat.  
A kalk.munkanap támogatási csoportja a felhasználó hoz létre.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Felhasználók hozzárendelése kalk.munkanap, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **kalk.munkanap **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-workday-tutorial/IC782935.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-workday-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).