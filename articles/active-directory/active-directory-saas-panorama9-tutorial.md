<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Panorama9 |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Panorama9 az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Oktatóprogram: Azure Active Directory-integráció a Panorama9
  
Ebben az oktatóanyagban célja integrálása az Azure és Panorama9 megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Panorama9 egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Panorama9 rendelt fogja tudni az alkalmazás a Panorama9 vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Panorama9 engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Eset")
##<a name="enabling-the-application-integration-for-panorama9"></a>Az alkalmazás integrációját Panorama9 engedélyezése
  
Ez a szakasz célja, hogyan Panorama9 az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Panorama9, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Panorama9**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Panorama9**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Panorama9 hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Panorama9 konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Panorama9** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Panorama9 felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Panorama9** mezőbe írja be a Panorama9 bejelentkezni a felhasználók által használt URL-címet (például: "*https://dashboard.panorama9.com/saml/access/3262*"), és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Panorama9 a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse azt helyileg a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Panorama9 vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **kezelés**elemre, és válassza a **bővítmények**.

    ![Bővítmények] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Bővítmények")

7.  A **bővítmények** párbeszédpanelen kattintson **Az egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Egyszeri bejelentkezés")

8.  **Beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![Beállítások] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Beállítások")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Panorama9** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **identitás szolgáltató URL-címe** mezőben lévő értéket.
    2.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **tanúsítvány ujjlenyomat** mezőben lévő értéket.  

        >[AZURE.TIP]További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    3.  Kattintson a **Mentés**gombra.

9.  Az Azure Active Directory klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Panorama9, hogy ki kell építenie Panorama9 be.  
Panorama9, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Panorama9** vállalati webhely.

2.  A felső sávon kattintson a **kezelés**elemre, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Felhasználók")

3.  Kattintson a **+**.

4.  A felhasználói adatok csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználók] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Felhasználók")

    1.  Az **E-mail** mezőbe írja be a kívánt kiépítése érvényes Azure Active Directory-felhasználó e-mail címét.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE]Bármely más Panorama9 felhasználói fiók létrehozása eszközöket is használhatja, illetve Panorama9 rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Felhasználók hozzárendelése Panorama9, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Panorama9** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).