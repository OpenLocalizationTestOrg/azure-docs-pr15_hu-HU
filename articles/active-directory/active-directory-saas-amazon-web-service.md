<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció Amazon Web Service (AWS) |} Microsoft Azure"
    description="Megtudhatja, hogy miként Amazon Web Services (AWS) használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!"
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Oktatóprogram: Azure Active Directory-integráció Amazon Web Service (AWS)

Ebben az oktatóanyagban célja mutatja, hogy miként Amazon Web Service (AWS) integrálása az Azure Active Directory (Azure Active Directory).  
Amazon Web Service (AWS) integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Beállíthatja, hogy, Azure Active Directory, aki rendelkezik hozzáféréssel Amazon Web Service (AWS) 
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Amazon Web Service (AWS) (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Konfigurálja az Azure Active Directory-integráció Amazon Web Service (AWS), az alábbi elemek van szükség:

- Az Azure Active Directory-előfizetéssel
- Az Amazon Web Service (AWS) egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt három fő építőelemek áll:

1. Amazon Web Service (AWS) hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Amazon Web Service (AWS) hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása Amazon Web Service (AWS), akkor kell Amazon Web Service (AWS) hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>A gyűjteményből az Amazon Web Service (AWS) hozzáadásához tegye a következőket:

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1] 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** . 
   
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján. 
   
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**. 
   
    ![Alkalmazások][4]

6. A Keresés mezőbe írja be az **Amazon Web Service (AWS)**.
   
    ![Alkalmazások][5]

7. Az eredmény ablaktáblában jelölje ki az **Amazon Web Service (AWS)**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Alkalmazások][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés tesztelje az Amazon Web Service (AWS) "Britta Simon" nevű próba felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az Azure Active Directory-felhasználónak megfelelőjük felhasználói Amazon Web Service (AWS). Más szóval hivatkozás Azure AD-felhasználó, és a kapcsolódó felhasználó Amazon Web Service (AWS) közötti kapcsolat kell létrehozni.  
A hivatkozás kapcsolat jön létre a értéket a **felhasználó nevét** a **felhasználónév** Amazon Web Service (AWS) értékként Azure Active Directory hozzárendelésével.
 
Beállítása és tesztelése Azure AD az egyszeri bejelentkezés Amazon Web Service (AWS), az alábbi építőelemeket szükséges:

1. **[Azure Active Directory beállítása egyetlen egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Amazon Web Service (AWS) létrehozása tesztelje a felhasználó](#creating-a-halogen-software-test-user)** -, hogy egy partner a Britta Simon az Amazon Web Service (AWS) az Azure Active Directory ábrázolása amelyen csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure Active Directory egyetlen egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása az egyszeri bejelentkezés az Amazon Web Service (AWS) alkalmazást.  
Az Amazon Web Service (AWS) alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs. Az alábbi képernyőképen látható, a.


![Egyszeri bejelentkezés beállítása][27]

**Azure Active Directory egyszeri bejelentkezés beállítása Amazon Web Service (AWS), hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon alkalmazás integrációs **Amazon Web Service (AWS)** lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7]

2. **Hogyan szeretné, hogy jelentkezzen be Amazon Web Service (AWS) felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása][8]

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon kattintson a Tovább gombra. 

    ![Alkalmazás beállításainak konfigurálása][9]
 
4. **Konfigurálása az egyszeri bejelentkezés az Amazon Web Service (AWS)** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.

    ![Egyszeri bejelentkezés beállítása][10]

5. Egy másik böngészőablakban bejelentkezéses Amazon Web Service (AWS) vállalat webhelyéhez rendszergazdaként.

6. **Konzol a kezdőlap**elemre.

    ![Egyszeri bejelentkezés beállítása][11]

7. **Identitás- és a hozzáférés-kezelés**gombra. 

    ![Egyszeri bejelentkezés beállítása][12]

8. Kattintson a **Identitásszolgáltatók**, és kattintson a **Szolgáltató létrehozása**gombra. 

    ![Egyszeri bejelentkezés beállítása][13]

9. A **Szolgáltató beállítása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása][14]

     egy. **Szolgáltató típusa**csoportban jelölje ki a **SAML**.

     b. A **Szolgáltató neve** mezőbe írja be a szolgáltató nevét (például: *fahulladékok*).

     c billentyűkombinációt. A metaadatok letöltött fájl feltöltéséhez kattintson a **Fájl kiválasztása**.

     d. Kattintson a **Tovább**gombra.


10. A **Szolgáltató adatainak ellenőrzése** párbeszédpanel lapon kattintson a **Létrehozás**gombra. 

    ![Egyszeri bejelentkezés beállítása][15]

11. Kattintson a **szerepkörök**elemre, és kattintson az **Új szerepkör létrehozása**. 

    ![Egyszeri bejelentkezés beállítása][16]

12. A **Szerepkör neve beállítása** párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása][17]

    egy. **Szerepkör neve** mezőbe írja be a szerepkör nevét (például: *tesztfelhasználó nevet*).

    b. Kattintson a **Tovább**gombra.

13. **Szerepkör-típus kijelölése** párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása][18]

    egy. Jelölje ki **az identitás-szolgáltató Access szerepkört**.

    b. **Engedélyek biztosítása webes egyszeri bejelentkezés (WebSSO) hozzáférés a SAML szolgáltatók** szakaszban kattintson a **Kijelölés**gombra.


14. Az **Adatvédelmi létrehozása** párbeszédpanelen hajtsa végre az alábbi lépéseket:  

    ![Egyszeri bejelentkezés beállítása][19]
     
     egy. SAML-konferenciaszolgáltatóként, jelölje ki a létrehozott previousley SAML-szolgáltató (például: *fahulladékok*) 

     b. Kattintson a **Tovább**gombra.


15. **Szerepkör megbízható ellenőrzése** párbeszédpanelen kattintson a **Következő lépéssel**. 

    ![Egyszeri bejelentkezés beállítása][32]


16. **Házirend csatolása** párbeszédpanelen kattintson a **Következő lépés**.  

    ![Egyszeri bejelentkezés beállítása][33]


17. A **Véleményezés** párbeszédpanelen hajtsa végre az alábbi lépéseket:   

    ![Egyszeri bejelentkezés beállítása][34]

     egy. Másolja a **Szerepkör információ** értékét.

     b. Másolja a **Szervezetek megbízható** információ értékét.

     c billentyűkombinációt. Kattintson a **szerepkör létrehozása**gombra. 

18. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.

    ![Mi az Azure AD Connect][20]

19. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **kész** **konfigurálása az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Mi az Azure AD Connect][22]


20. Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához. 

    ![Egyszeri bejelentkezés beállítása][21]

21. Kattintson a **felhasználó attribútum hozzáadása**gombra. 

    ![Egyszeri bejelentkezés beállítása][23]

22. A felhasználó attribútum hozzáadása párbeszédpanelen hajtsa végre az alábbi lépéseket. 

    ![Egyszeri bejelentkezés beállítása][24] 

    egy. Az **Attribútum neve** mezőbe írja be a **https://aws.amazon.com/SAML/Attributes/Role**.

    b. Az **Attribútumérték** mezőbe írja be a **[szerepkör információ érték], [megbízható entitás információ érték]**.

    >[AZURE.TIP] Ezeket az értékeket másolta a Véleményezés párbeszédpanel szerepköre létrehozásakor. 

    c billentyűkombinációt. Válassza a **kész** , a **Felhasználó attribútum hozzáadása** párbeszédpanel bezárásához.

23. Kattintson a **felhasználó attribútum hozzáadása**gombra. 

    ![Egyszeri bejelentkezés beállítása][23]


24. A felhasználó attribútum hozzáadása párbeszédpanelen hajtsa végre az alábbi lépéseket. 

    ![Egyszeri bejelentkezés beállítása][25]


     egy. Az **Attribútum neve** mezőbe írja be a **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. Az **Attribútumérték** mezőbe írja be vagy a legördülő listában jelölje ki a **user.userprincipalname** .
     
    ![Egyszeri bejelentkezés beállítása][35]
    

     c billentyűkombinációt. Válassza a **kész** , a **Felhasználó attribútum hozzáadása** párbeszédpanel bezárásához.


25. Kattintson a **módosítások alkalmazásához**. 

    ![Egyszeri bejelentkezés beállítása][26]





### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása

Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Felhasználó típusa jelölje be az új felhasználó a szervezet.
  2. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.
  3. Kattintson a Tovább gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Kattintson a **Vezetéknév** txtbox, típusa, **Simon**.
  
    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
  
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.
  
    b. Kattintson a **kész**gombra.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Amazon Web Service (AWS) teszt felhasználó létrehozása

Ez a szakasz célja Britta Simon Amazon Web Service (AWS) néven felhasználó létrehozása.

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Hozzon létre egy felhasználó neve Britta Simon Amazon Web Service (AWS), hajtsa végre az alábbi lépéseket:

1. Jelentkezzen be rendszergazdaként az **Amazon Web Service (AWS)** vállalati webhely.

2. Kattintson a **Kezdőlap konzol** ikonra. 

    ![Egyszeri bejelentkezés beállítása][11]

3. Kattintson a személyes és a hozzáférés kezelése. 

    ![Egyszeri bejelentkezés beállítása][28]

4. Az irányítópult lapon kattintson a felhasználók elemre, és kattintson az új felhasználók létrehozása. 

    ![Egyszeri bejelentkezés beállítása][29]

5. A felhasználó létrehozása párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása][30]

     egy. Írja be a **Felhasználóneveket megadása** szövegdobozok az Azure Active Directory Brita Simon felhasználónév (userprincipalname).

     b. Kattintson a **létrehozása**gombra.




### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja engedélyezése Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést Amazon Web Service (AWS).

![Felhasználó hozzárendelése][31]

**Britta Simon hozzárendelése CloudPassage, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][26]

2. Az alkalmazások listájában jelölje ki az **Amazon Web Service (AWS)**.

    ![Felhasználó hozzárendelése][27]

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][25]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][29]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Ha az Access panelen az Amazon Web Service (AWS) mozaik gombra kattint, meg kell első automatikusan aláírt-on az Amazon Web Service (AWS) alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















