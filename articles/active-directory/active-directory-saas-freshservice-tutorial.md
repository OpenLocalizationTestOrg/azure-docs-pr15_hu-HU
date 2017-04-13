<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a FreshService |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a FreshService az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Oktatóprogram: Azure Active Directory-integráció a FreshService
  
Ebben az oktatóanyagban célja integrálása az Azure és FreshService megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   FreshService egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók FreshService rendelt fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját FreshService engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Eset")
##<a name="enabling-the-application-integration-for-freshservice"></a>Az alkalmazás integrációját FreshService engedélyezése
  
Ez a szakasz célja, hogyan FreshService az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját FreshService, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **FreshService**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **FreshService**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy FreshService hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az FreshService konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **FreshService** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az FreshService felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Egyszeri bejelentkezés beállítása")

3.  **App URL-címe beállítása** lapon az **URL a bejelentkezési FreshService** mezőbe írja be a jelentkezzen be az alkalmazás Freshdesk a felhasználók által használt URL-cím (például: "*http://democompany.freshservice.com/*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés FreshService a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a FreshService vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Rendszergazda")

7.  Az **Ügyfél-portálon**kattintson a **Biztonság**gombra.

    ![Biztonsági] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Biztonsági")

8.  A **biztonsági beállításai** szakaszban tegye a következőket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Egyszeri bejelentkezés")

    1.  Váltás az **egyszeri bejelentkezési OnON**.
    2.  Jelölje ki a **SAML-féle egyszeri Bejelentkezést**.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a FreshService** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **SAML bejelentkezési URL-cím** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a FreshService** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    5.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Biztonsági tanúsítványt ujjlenyomat** mezőben lévő értéket.
    
        >[AZURE.TIP]További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a FreshService, hogy ki kell építenie FreshService be.  
FreshService, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **FreshService** vállalati webhely.

2.  A felső sávon kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Rendszergazda")

3.  A **Felhasználók kezelése** csoportban kattintson a **igénylő**.

    ![Igénylő] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Igénylő")

4.  Kattintson az **Új igénylő**.

    ![Új igénylő] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Új igénylő")

5.  **Új igénylő** csoportban hajtsa végre az alábbi lépéseket:

    ![Új igénylő] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Új igénylő")

    1.  Adjon meg egy érvényes Azure Active Directory-fiókját, amelyet **Utónév** és **e-mailek** tulajdonságainak kiépítése a kapcsolódó szövegdobozok.
    2.  Kattintson a **Mentés**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozás

>[AZURE.NOTE] Bármely más FreshService felhasználói fiók létrehozása eszközöket is használhatja, illetve FreshService rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Felhasználók hozzárendelése FreshService, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **FreshService **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).