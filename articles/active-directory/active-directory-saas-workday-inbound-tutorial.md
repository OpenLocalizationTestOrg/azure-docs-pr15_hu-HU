<properties 
    pageTitle="Oktatóprogram: Bejövő szinkronizálás kalk.munkanap beállítása |} Microsoft Azure" 
    description="Megtudhatja, hogyan szeretné alkalmazni a személyes adatok forrásainak kalk.munkanap Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Oktatóprogram: Bejövő szinkronizálás kalk.munkanap konfigurálása


Ebben az oktatóanyagban célja mutatja, hogy a lépéseket kell elvégeznie a kalk.munkanap és Azure Active Directory személyek importálása a kalk.munkanap az Azure Active Directory. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Egy érvényes Azure Active Directory prémium verzióra szóló előfizetés
-   A kalk.munkanap bérlői webhelyre
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1. A kalk.munkanap az alkalmazás integráció engedélyezése 


2. Integrációs rendszer felhasználó létrehozása 


3. Biztonsági csoport létrehozása 


4. A biztonsági csoport számára oszt ki az integration rendszer felhasználói 


5. Biztonsági csoport beállításaival 


6. Biztonsági házirend-módosítások aktiválása 


7. Az Azure Active Directory konfigurálása felhasználói importálása 



##<a name="enabling-the-application-integration-for-workday"></a>A kalk.munkanap az alkalmazás integráció engedélyezése
  
Ez a szakasz célja tagolása a kalk.munkanap az alkalmazás alkalmazással való integráció engedélyezése hogyan.

### <a name="steps"></a>A lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Alkalmazás hozzáadása")

  
5. A Keresés mezőbe írja be a **kalk.munkanap**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Gallerry az alkalmazás hozzáadása")

6. Az eredmény ablaktáblában jelölje ki a kalk.munkanap, és válassza az alkalmazás hozzáadása a Kész gombra.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Alkalmazás gyűjtemény")





## <a name="creating-an-integration-system-user"></a>Integrációs rendszer felhasználó létrehozása

### <a name="steps"></a>A lépéseket:


1. A **Kalk.munkanap munkaterület**, írja be a felhasználó létrehozása a Keresés mezőbe, és kattintson a **Integrációs rendszer felhasználó létrehozása**gombra. 

    ![Felhasználó létrehozása] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Felhasználó létrehozása")



2. A felhasználónév és jelszó megadásával integráció a rendszer új felhasználó a **Integráció a rendszer felhasználói létrehozása** -feladat végrehajtása  Hagyja üresen a szükséges új jelszavát a következő bejelentkezés lehetőséget, mivel ez a felhasználó fog kell bejelentkezés programozás útján. Kilépés a munkamenet-időtúllépés perc az alapértelmezett értékére 0, ami megakadályozza, hogy a felhasználó munkamenetek időtúllépés idő. 

    ![Integráció a rendszer felhasználói létrehozása] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Integráció a rendszer felhasználói létrehozása")
 




## <a name="creating-a-security-group"></a>Biztonsági csoport létrehozása

Ebben az oktatóanyagban tagolt esetben szeretne megkötés nélkül integrációs rendszer biztonsági csoport létrehozása, és rendelje hozzá a felhasználót.

### <a name="steps"></a>A lépéseket:

1. Írja be biztonsági csoport létrehozása a Keresés mezőbe, és kattintson a **Biztonsági csoport létrehozása**gombra. 

    ![CreateSecurity csoport] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity csoport")
 

2. A biztonsági csoport létrehozása-feladat végrehajtása  Válassza a integrációs rendszer biztonsági csoport – megkötés nélkül hozhat létre egy olyan biztonsági csoportot, amelyhez tagokat kifejezetten hozzáadódik, a típus, központjaként biztonsági csoport legördülő listából. 

    ![CreateSecurity csoport] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity csoport")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>A biztonsági csoport számára oszt ki az integration rendszer felhasználói

### <a name="steps"></a>Lépéseket:


1. Adja meg a biztonsági csoport szerkesztése a Keresés mezőbe, és kattintson a **Biztonsági csoport szerkesztése**gombra. 

    ![Biztonsági csoport szerkesztése] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Biztonsági csoport szerkesztése")
 
 

2. Kereshet, és jelölje ki az új integrációs biztonsági csoport nevét. 

    ![Biztonsági csoport szerkesztése] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Biztonsági csoport szerkesztése")

 

3. Az új integrációs rendszer felhasználó hozzáadása az új biztonsági csoport. 

    ![Rendszer biztonsági csoport] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Rendszer biztonsági csoport")  




## <a name="configuring-security-group-options"></a>Biztonsági csoport beállításaival

Ebben a lépésben, adja meg az ** **első** , és** az objektumok, az alábbi tartomány biztonsági házirendek által biztosított műveleteket az új biztonsági csoport engedélyeket:

- Külső fiók kiépítése

- Dolgozói adatokat: Nyilvános dolgozó-jelentések

- Dolgozói adatokat: Az összes beosztások

- Adatok dolgozó: Aktuális munkaerő-információ

- Dolgozói adatokat: Dolgozói profil üzleti cím

 
### <a name="steps"></a>A lépéseket:

1. A Keresés mezőbe írja be a tartomány biztonsági házirendeket, és válassza a tartomány biztonsági házirendek működési terület hivatkozásra.  

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Tartomány biztonsági házirendek")  
 

2. Keresse meg a rendszer, és jelölje ki a **rendszer** funkcionális területet.  Kattintson az **OK gombra**.  

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Tartomány biztonsági házirendek")  


3. A listában, a rendszer funkcionális terület biztonsági házirendeket bontsa ki a biztonsági felügyelet, és jelölje be a tartomány biztonsági házirendje külső fiókot kiépítési.  

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Tartomány biztonsági házirendek")  


4. Kattintson a **Engedélyeinek módosítása**, és ezután **Engedélyeinek szerkesztése**párbeszédpanel lapján az új biztonsági csoport hozzáadása a listáját **, és **kérjen** ** integrációs engedélyekkel rendelkező biztonsági csoportokat. 

    ![Engedélyek szerkesztése] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Engedélyek szerkesztése")  

 

5. Ismételje meg a lépés: 1, a fenti vissza szeretne térni a képernyőhöz funkcionális területeket, kijelölésére szolgáló, keressen a munkaerő-, jelölje be a személyzeti funkcionális területre, és kattintson az **OK gombra**.

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Tartomány biztonsági házirendek")  
 

6. Bontsa ki a személyzeti funkcionális terület biztonsági házirendek listájának dolgozó adatok: személyzeti és ismételt lépés: 4, a fenti ezekre a hátralévő biztonsági házirendek:

     - Dolgozói adatokat: Nyilvános dolgozó-jelentések

     - Dolgozói adatokat: Az összes beosztások

     - Adatok dolgozó: Aktuális munkaerő-információ

     - Dolgozói adatokat: Dolgozói profil üzleti cím


    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Tartomány biztonsági házirendek")  







## <a name="activating-security-policy-changes"></a>Biztonsági házirend-módosítások aktiválása

### <a name="steps"></a>A lépéseket:

1. Írja be a keresőmezőbe aktiválása, és kattintson a hivatkozásra, függőben lévő biztonsági módosítások aktiválása gombra. 

    ![Aktiválása] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktiválása") 
 

2. Kezdje el a függőben lévő biztonsági módosítások aktiválása tevékenység naplózási célokra Megjegyzés beírásával, és kattintson **az OK**gombra. 

    ![Függőben lévő biztonsági aktiválása] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Függőben lévő biztonsági aktiválása")   
 

3. A következő képernyőn a feladat elvégzéséhez a megerősítés címkézni jelölőnégyzet bejelölésével, és kattintson **az OK**gombra. 

    ![Függőben lévő biztonsági aktiválása] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Függőben lévő biztonsági aktiválása")  





## <a name="configuring-user-import-in-azure-ad"></a>Az Azure Active Directory konfigurálása felhasználói importálása

Ez a szakasz célja a szerkezet beállítása a személyek importálása a kalk.munkanap Azure AD.


### <a name="steps"></a>A lépéseket:


1. A **kalk.munkanap** alkalmazás integrációs lapon kattintson a **konfigurálás felhasználói importálása** a **Kiépítési beállítása** párbeszédpanel megnyitásához.


2. A **Beállítások és a rendszergazdai hitelesítő adatait** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**: 

    ![Beállítások és a rendszergazdai hitelesítő adatok] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Beállítások és a rendszergazdai hitelesítő adatok")  
 
    egy. A kalk.munkanap felügyeleti felhasználó neve mezőbe írja be a nevét, annak a felhasználónak az létrehozása létrehozott-integráció rendszer felhasználói szakaszban.

    b. A kalk.munkanap rendszergazdai jelszó mezőbe írja be a jelszót a felhasználó a létrehozása létrehozott-integráció rendszer felhasználói szakaszban.

    c billentyűkombinációt. A kalk.munkanap bérlői webhely URL-cím mezőbe írja be az URL-cím vagy a kalk.munkanap bérlő.


3. A **kapcsolat tesztelése** lapon kattintson az **Indítás** erősítse meg a kapcsolat gombra, és kattintson a **Tovább gombra**. 

    ![A kapcsolat tesztelése] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "A kapcsolat tesztelése")  
 

4. A **létesítése** beállítások lapon kattintson a **Tovább**gombra. 

    ![Kiépítési beállításai] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Kiépítési beállításai")


5. A **kiépítési indítása** párbeszédpanelen kattintson a **Befejezés**gombra. 

    ![Indítsa el a kiépítési] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Indítsa el a kiépítési")
 

Ezután nyissa meg a **felhasználók** szakaszban, és ellenőrizze, hogy importálta munkanap felhasználói.



## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)
