<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Bonus.ly |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Bonus.ly az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Oktatóprogram: Azure Active Directory-integráció a Bonus.ly

Ebben az oktatóanyagban célja integrálása az Azure és Bonus.ly megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A Bonus.ly próba bérlői webhelyre

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Bonus.ly engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Eset")
##<a name="enabling-the-application-integration-for-bonusly"></a>Az alkalmazás integrációját Bonus.ly engedélyezése

Ez a szakasz célja, hogyan Bonus.ly az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Bonus.ly, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Egyszeri bejelentkezés engedélyezése")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Bonus.ly**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Bonus.ly**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Bonus.ly hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Bonus.ly konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Bonus.ly** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Bonus.ly felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon **Bonus.ly bérlői webhely URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Bonus.LY*", majd kattintson a **Tovább**gombra: 

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Állítsa be az app URL-címe")

4.  A **Configure egyszeri bejelentkezés Bonus.ly a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Bonusly.cer**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Beállítás az egyszeri bejelentkezés")

5.  Egy másik böngészőablakban jelentkezzen be az **Bonus.ly** bérlőhöz.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra, és válassza a **integrációs és az alkalmazások**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  A **Single Sign-On**jelölje be a **SAML**.

8.  A **SAML** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Bonus.ly** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **IdP SSO cél URL-címe** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Bonus.ly** párbeszédpanel lapon másolja a **Kibocsátó azonosító** értékét, és illessze be a **IdP kibocsátó** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Bonus.ly** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **IdP bejelentkezési URL-cím** mezőben lévő értéket.
    4.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Tanúsítvány ujjlenyomat** mezőben lévő értéket.

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

9.  Kattintson a **Mentés**gombra.

10. A Microsoft Azure klasszikus portálon válassza ki a konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Bonus.ly, hogy ki kell építenie Bonus.ly be.  
Bonus.ly, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Egy webböngészőablakban jelentkezzen be az Bonus.ly bérlői webhelyen.

2.  Kattintson a **Beállítások**

    ![Beállítások] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Beállítások")

3.  Kattintson a **felhasználók és a prémium** fülre.

    ![Felhasználók és a prémium] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Felhasználók és a prémium")

4.  Kattintson a **felhasználók kezelése**elemre.

    ![Felhasználókezelés] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Felhasználókezelés")

5.  Kattintson a **felhasználó hozzáadása**elemre.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Felhasználó hozzáadása")

6.  A **Felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Felhasználó hozzáadása")

    1.  Írja be a "**e-mailek**és az **utónevet**, **Vezetéknév**" kiépítése be a kapcsolódó szövegdobozok kívánt érvényes AAD fiókkal.
    2.  Kattintson a **Mentés**gombra.

    >[AZURE.NOTE] A AAD fióktulajdonos előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó e-mailt kap.

>[AZURE.NOTE] Bármely más Bonus.ly felhasználói fiók létrehozása eszközöket is használhatja, illetve Bonus.ly rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Felhasználók hozzárendelése Bonus.ly, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a Bonus.ly alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
