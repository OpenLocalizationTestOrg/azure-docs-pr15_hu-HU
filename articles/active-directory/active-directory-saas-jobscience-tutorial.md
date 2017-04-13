<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Jobscience |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Jobscience az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Oktatóprogram: Azure Active Directory-integráció a Jobscience
  
Ebben az oktatóanyagban célja integrálása az Azure és Jobscience megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Előfizetés Jobscience egyszeri bejelentkezés engedélyezve
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Jobscience rendelt fogja tudni az alkalmazás a Jobscience vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Jobscience engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Eset")
##<a name="enabling-the-application-integration-for-jobscience"></a>Az alkalmazás integrációját Jobscience engedélyezése
  
Ez a szakasz célja, hogyan Jobscience az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Jobscience, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **jobscience**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Jobscience**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Jobscience hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Jobscience konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a Jobscience vállalati webhely.

2.  Nyissa meg **a telepítő**.

    ![A telepítő] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "A telepítő")

3.  A bal oldali navigációs területén **felügyelete** szakaszában kattintson a **Domain Management** bontsa ki a kapcsolódó szakaszt, és válassza a **Saját tartomány** a **Saját tartomány** lap megnyitásához. 

    ![A tartomány] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "A tartomány")

4.  Ha ellenőrizni szeretné, hogy a tartomány van beállítva megfelelően, győződjön meg arról, hogy a "**Felhasználók rendszerbe 4 lépésben**" az, és tekintse át a "**Saját tartomány beállításai**".

    ![A tartomány felhasználónak rendszerbe] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "A tartomány felhasználónak rendszerbe")

5.  A különböző webes böngészőablakban jelentkezzen be az Azure klasszikus portál.

6.  **Jobscience** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Egyszeri bejelentkezés beállítása")

7.  **Hogyan szeretné, hogy jelentkezzen be az Jobscience felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Egyszeri bejelentkezés beállítása")

8.  A **App URL-címe beállítása** lapon az **Jobscience bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*http://company.my.salesforce.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Állítsa be az App URL-címe")

9.  A **Configure egyszeri bejelentkezés Jobscience a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Egyszeri bejelentkezés beállítása")

10. A vállalat Jobscience webhelyre kattintson a **Biztonsági vezérlők**, és kattintson a **Egyszeri bejelentkezéses beállítások**.

    ![Biztonsági funkciók] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Biztonsági funkciók")

11. **Egyszeri bejelentkezés beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Egyszeri bejelentkezés beállításai")

    1.  Jelölje ki a **SAML engedélyezve van**.
    2.  Kattintson az **Új**gombra.

12. A **SAML egyszeri bejelentkezéses beállítás szerkesztése** párbeszédpanelen végezze el az alábbi lépéseket:

    ![SAML egyszeri bejelentkezéses beállítás] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML egyszeri bejelentkezéses beállítás")

    1.  Az **neve** mezőbe írja be a konfigurációban nevét.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Jobscience** párbeszéd lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket
    3.  A **Szervezet azonosítója** mezőbe írja be a **https://salesforce-jobscience.com**
    4.  **Tallózással keresse meg** a Azure AD-tanúsítvány feltöltése gombra.
    5.  **SAML azonosító típusa**csoportban jelölje ki a **állítás a User objektumban az összevonási Azonosítóját tartalmazza**.
    6.  **SAML identitás helyét**jelölje ki a **identitás a tárgy utasítást NameIdentfier elemének**.
    7.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Jobscience** párbeszéd lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket
    8.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Jobscience** párbeszéd lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Identitás szolgáltató kijelentkezés URL-címe** mezőben lévő értéket
    9.  Kattintson a **Mentés**gombra.

13. A bal oldali navigációs területén **felügyelete** szakaszában kattintson a **Domain Management** bontsa ki a kapcsolódó szakaszt, és válassza a **Saját tartomány** a **Saját tartomány** lap megnyitásához. 

    ![A tartomány] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "A tartomány")

14. A **Saját tartomány** lapon, a **Bejelentkezési oldal védjegyzés** csoportban kattintson a **Szerkesztés**gombra.

    ![Védjegyzés bejelentkezési lapja] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Védjegyzés bejelentkezési lapja")

15. A **Bejelentkezési oldal védjegyzés** lapon a **Hitelesítési szolgáltatás** csoportban jelenik meg a **SAML-féle egyszeri Bejelentkezést beállítások** neve. Jelölje ki azt, és kattintson a **Mentés**gombra.

    ![Márka bejelentkezési lapja] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Márka bejelentkezési lapja")

16. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Egyszeri bejelentkezés beállítása")
  
Az SP bejelentkezési URL-cím kezdeményezett egyszeri bejelentkezéses kattintson a **Biztonsági funkciók** menü szakaszban **az egyszeri bejelentkezés beállításait** .

![Biztonsági funkciók] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Biztonsági funkciók")
  
Kattintson a fenti lépésben létrehozott SSO profilra.  
Ezen az oldalon az egyszeri bejelentkezési URL-CÍMÉT (például a *https://companyname.my.salesforce.com?so=companyid*) cégek számára látható.
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Jobscience, hogy ki kell építenie Jobscience be.  
Jobscience, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Jobscience** vállalati webhely.

2.  Lépjen az beállítása

    ![A telepítő] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "A telepítő")

3.  Nyissa meg a **felhasználókezelés \> felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Felhasználók")

4.  Kattintson az **Új felhasználó**gombra.

    ![Minden felhasználónak] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Minden felhasználónak")

5.  A **Felhasználó szerkesztése** párbeszédpanelen végezze el az alábbi lépéseket:

    ![Felhasználó szerkesztése] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Felhasználó szerkesztése")

    1.  Írja be a keresztnév, Vezetéknév, alias, e-mailben vagy felhasználó neve és a becenév tulajdonságainak be a kapcsolódó szövegdobozok kiépítése kívánt Azure AD-felhasználó.
    2.  Kattintson a **Mentés**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos előtt aktiválva van, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó e-mailben kapja eredményül.

>[AZURE.NOTE] Bármely más Jobscience felhasználói fiók létrehozása eszközöket is használhatja, illetve Jobscience rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Felhasználók hozzárendelése Jobscience, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Jobscience **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).