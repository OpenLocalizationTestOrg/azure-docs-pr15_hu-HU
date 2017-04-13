<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a DocuSign |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és DocuSign között."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Oktatóprogram: Azure Active Directory-integráció a DocuSign

Ebben az oktatóanyagban célja integrálása az Azure és DocuSign megjelenítéséhez.
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

- Érvényes Azure előfizetés
- A DocuSign bérlői webhelyre



A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1. [Az alkalmazás integrációját DocuSign engedélyezése](#enabling-the-application-integration-for-docusign) 


2. [Egyszeri bejelentkezés beállítása](#configuring-single-sign-on) 


3. [Fiók kiépítési konfigurálása](#configuring-account-provisioning) 


4. [Felhasználók hozzárendelése](#assigning-users) 

    ![Egyszeri bejelentkezés beállítása][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Az alkalmazás integrációját DocuSign engedélyezése

Ez a szakasz célja, hogyan DocuSign az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját DocuSign, hajtsa végre az alábbi lépéseket:

1. Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Egyszeri bejelentkezés beállítása][1]

2. A címtár listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Egyszeri bejelentkezés beállítása][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. A Mit szeretne párbeszédpanelt, kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Egyszeri bejelentkezés beállítása][4]


6. A Keresés mezőbe írja be a **DocuSign**.

    ![Egyszeri bejelentkezés beállítása][5]

7. Az eredmény ablaktáblában jelölje ki a **DocuSign**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Egyszeri bejelentkezés beállítása][6]


## <a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy DocuSign hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1. Az Azure klasszikus portálon **DocuSign alkalmazás integrálását** lapon kattintson **konfigurálása az egyszeri bejelentkezés** a konfigurálása az egyszeri bejelentkezés párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7]

2. **Hogyan szeretné, hogy jelentkezzen be az DocuSign felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a Tovább gombra.

    ![Egyszeri bejelentkezés beállítása][8]

3. Az **Alkalmazás beállításainak megadása** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása][61]

    egy. **Jelentkezzen be az URL-cím** mezőbe írja be `https://account.docusign.com/*`.  

    b. Az **azonosító** mezőbe írja be a `https://account.docusign.com/*`.  
   
    c billentyűkombinációt. Kattintson a **Tovább**gombra. 


    > [AZURE.TIP] A bejelentkezési a URL-CÍMÉT, és az azonosító értékek csak helyőrzők. A témakör későbbi útmutatót beolvashatja a tényleges értékek környezetben foglalkozik.
 

4. A **beállítás az egyszeri bejelentkezés DocuSign a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben.

    ![Egyszeri bejelentkezés beállítása][10]


5. A különböző webes böngészőablakban jelentkezzen be az **DocuSign felügyeleti portál** rendszergazdaként.


6. A bal oldali navigációs menüben kattintson a **tartományok**hivatkozásra.

    ![Egyszeri bejelentkezés beállítása][51]

7. Kattintson a jobb oldali **Állítást tartományt**.

    ![Egyszeri bejelentkezés beállítása][52]

8. A **tartomány állítást** párbeszédpanelen az **Tartomány neve** mezőbe írja be a vállalati tartomány, és válassza a **állítást**. Győződjön meg arról, hogy a tartomány ellenőrzéséhez hivatkozásra, és az Állapot oszlop értéke az aktív.

    ![Egyszeri bejelentkezés beállítása][53]

9. A bal oldali menüben kattintson a **Identitásszolgáltatók**  

    ![Egyszeri bejelentkezés beállítása][54]

10. Kattintson a jobb oldali **Identitásszolgáltató hozzáadása**. 
    
    ![Egyszeri bejelentkezés beállítása][55]

11. Az **Identitás internetszolgáltató beállításai** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása][56]


    egy. **Neve** mezőbe írja be egy egyedi nevet a konfigurációban. Kérjük, ne használjon szóközt.

    b. Az Azure klasszikus portálon kibocsátó URL másolása másokkal, és illessze be a **Identitás szolgáltató kibocsátó** mezőben lévő értéket.

    c billentyűkombinációt. Az Azure klasszikus portálon **Távoli bejelentkezési URL-cím**másolása, és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.

    d. Az Azure klasszikus portálon **Távoli kijelentkezés URL-cím**másolása, és illessze be a **Identitás szolgáltató kijelentkezés URL-címe** mezőben lévő értéket.

    e. Jelölje be a **Bejelentkezési AuthN kérelmet**.

    f. **Küldése AuthN kérelme**jelölje ki a **BEJEGYZÉST**.

    g. **Jelentkezzen ki kérését küldése**jelölje ki a **BEJEGYZÉST**. 


12. **Egyéni attribútum hozzárendelése** csoportjában válassza ki a az Azure Active Directory állítást megfeleltetni kívánt mezőt. Ebben a példában az **EmailCím** állítást **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**értékkel van megfeleltetve. Ez az alapértelmezett állítást nevét az Azure AD-e-mailek állítást. 

    > [AZURE.NOTE] A megfelelő **felhasználói azonosító** segítségével a felhasználók az Azure Active Directory Docusign felhasználói hozzárendelésének térképen. Jelölje ki a megfelelő mezőt, és adja meg a megfelelő értéket a szervezeti beállításai alapján.

    ![Egyszeri bejelentkezés beállítása][57]

13. **Identitás szolgáltató tanúsítványának** csoportban kattintson a **Tanúsítvány hozzáadása**gombra, és töltse ki az Azure Active Directory klasszikus portáljáról letöltött tanúsítvány.   

    ![Egyszeri bejelentkezés beállítása][58]

14. Kattintson a **Mentés**gombra.

15. **Identitásszolgáltatók** csoportban kattintson a **Műveletek**ikonra, és válassza a **Végpontok**.   

    ![Egyszeri bejelentkezés beállítása][59]



10. Az Azure klasszikus portálon térjen vissza az **Alkalmazás-beállítások megadása** lap. 

16. A **DocuSign portált**a **Nézet SAML 2.0-s végpontok** szakaszban, a következő lépésekkel:

    ![Egyszeri bejelentkezés beállítása][60]

    egy. **Service Provider kibocsátó URL**másolása másokkal, és illessze be a **azonosítója** mezőben lévő értéket az Azure klasszikus portálon.

    b. **Service Provider bejelentkezési URL-cím**másolása, és illessze be a **Bejelentkezési a URL-címe** mezőben lévő értéket az Azure klasszikus portálon.

    c billentyűkombinációt.  Kattintson a **Bezárás** gombra  


10. A klasszikus Azure portálon kattintson a **Tovább**gombra. 


15. Az Azure klasszikus portálon jelölje be az **egyszeri bejelentkezéses konfigurációs megerősítő**, és kattintson a **Tovább gombra**.

    ![Alkalmazások][14]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.

    ![Alkalmazások][15]
 

## <a name="configuring-account-provisioning"></a>Fiók kiépítési konfigurálása

Ez a szakasz célja, hogyan engedélyezheti a felhasználó Active Directory felhasználói fiókok DocuSign kiépítési tagolása.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1. Az **Azure klasszikus portál** **DocuSign alkalmazás integrálását** lapon kattintson a **beállítás fiók kiépítési** a felhasználó kiépítési beállítása párbeszédpanel megnyitásához.

    ![Fiók kiépítési konfigurálása][30]

2. A **Beállítások és a rendszergazdai hitelesítő adatait** lapon ahhoz, hogy kiépítési automatikus felhasználói, szükséges jogokkal küldje el a DocuSign fiók hitelesítő adatait, és kattintson a **Tovább**parancsra. 

    ![Fiók kiépítési konfigurálása][31]

3. A **kapcsolat tesztelése** párbeszédpanelen kattintson az **Indítás**gombra, és a sikeres vizsgálat után kattintson a **Tovább gombra**.

    ![Fiók kiépítési konfigurálása][32]

3. A **Megerősítés** lapon kattintson a **Befejezés**gombra.

    ![Fiók kiépítési konfigurálása][33]
 

## <a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Felhasználók hozzárendelése DocuSign, hajtsa végre az alábbi lépéseket:

1. Az **Azure klasszikus portál**létrehozása az aktuális fiók egy tesztfiók.

2. A **DocuSign alkalmazás integrálását** lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Felhasználók hozzárendelése][40]
 

3. Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Felhasználók hozzárendelése][41]


Érdemes most Várjon 10 percet, és ellenőrizze, hogy a fiók DocuSign lett-e szinkronizálva.

Első lépésként ellenőrzés jelölje be a kiépítési állapotát, az oldalon DocuSign alkalmazás integrálása az Azure klasszikus portálon d irányítópult gombra kattintva.

![Felhasználók hozzárendelése][42]

Ciklikus kiépítési sikeresen befejeződött felhasználó egy kapcsolódó állapotát jelzi:

![Felhasználók hozzárendelése][43]


Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához.

Az Access ablaktábla, lásd: Bevezetés az Access panelre.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png