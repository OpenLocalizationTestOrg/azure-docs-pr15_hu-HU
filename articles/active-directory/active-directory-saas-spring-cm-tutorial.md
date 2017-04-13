<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a tavaszi CM |} Microsoft Azure" 
    description="Megtudhatja, hogy miként tavaszi CM használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Oktatóprogram: Azure Active Directory-integráció a tavaszi CM
  
Ebben az oktatóanyagban célja bemutatják, hogyan állíthatja be az egyszeri bejelentkezés Azure Active Directory és SpringCM között.
  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   SpringCM egyszeri bejelentkezéses engedélyezett előfizetés
  
Miután elkészült a ebben az oktatóanyagban, az Azure Active Directory-felhasználók SpringCM rendelt fogja tudni az egyszeri bejelentkezés a AAD Access panelen.

1.  Az alkalmazás integrációját SpringCM engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Eset")

##<a name="enabling-the-application-integration-for-springcm"></a>Az alkalmazás integrációját SpringCM engedélyezése
  
Ez a szakasz célja, hogyan SpringCM az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját SpringCM, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **SpringCM**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **SpringCM**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz ismerteti, hogyan engedélyezése a felhasználóknak SpringCM az Azure Active Directory összevonási alapján a SAML protokoll használata a fiók hitelesítést végezni.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **SpringCM** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az SpringCM felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési SpringCM** mezőbe írja be a SpringCM alkalmazás bejelentkezni a felhasználók által használt URL-CÍMÉT, és kattintson a **Tovább gombra**. 

    Az alkalmazás URL-cím a SpringCM bérlői webhely URL-CÍMÉT (például: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés SpringCM a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítványfájl helyileg a számítógépre.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a **SpringCM** vállalati webhely rendszergazdaként.

6.  A menüben, a képernyő tetején az **Ugrás**parancsra, kattintson a **Beállítások**gombra, és a **Fiókbeállítások parancsra** csoportban kattintson a **SAML-féle egyszeri Bejelentkezést**.

    ![SAML egyszeri bejelentkezés] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML egyszeri bejelentkezés")

7.  Identitás szolgáltató konfigurálása csoportban hajtsa végre az alábbi lépéseket:

    ![Identitás szolgáltató konfigurálása] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identitás szolgáltató konfigurálása")

    1.  Töltse fel a letöltött Azure Active Directory-tanúsítvány, kattintson a **Kibocsátó tanúsítvány kiválasztása** vagy **Módosítása kibocsátó tanúsítvány**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés SpringCM a** lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a SpringCM** lapon másolja a **Szolgáltatás URL-címe Singel bejelentkezés** értéket, és illessze be a **Service Provider (SP) által kezdeményezett végpont** mezőben lévő értéket.
    4.  A **SAML engedélyezve**jelölje ki a **engedélyezése**.
    5.  Kattintson a **Mentés**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználók SpringCM való bejelentkezéshez, hogy ki kell építenie SpringCM be.  
SpringCM, ha a feladat manuális kiépítési.

>[AZURE.NOTE] További részletekért olvassa el [létrehozása és SpringCM felhasználó szerkesztése](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Építse SpringCM egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **SpringCM** vállalati webhely.

2.  Kattintson az **Ugrás**gombra, és kattintson a **Címjegyzék**gombra.

    ![Felhasználó létrehozása] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Felhasználó létrehozása")

3.  Kattintson a **felhasználó létrehozása**gombra.

4.  **Felhasználói szerepkör**kiválasztása

5.  Jelölje ki **az aktiválás e-mailek küldése**.

6.  Írja be az utónevet, Vezetéknév és azt szeretné, hogy be a kapcsolódó szövegdobozok kiépítése érvényes Azure Active Directory felhasználói fiók e-mail címét.

7.  A felhasználó hozzáadása **biztonsági csoport**.

8.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más SpringCM felhasználói fiók létrehozása eszközöket is használhatja, illetve SpringCM rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Felhasználók hozzárendelése SpringCM, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **SpringCM** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).




