<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a vászonra LMS |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a vászonra LMS az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Oktatóprogram: Azure Active Directory-integráció a vászonra LMS

Ebben az oktatóanyagban célja Azure és vászon integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Vászon bérlői webhelyre

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók vászon rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a vászonra vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás-integráció a vászonra engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Eset")
##<a name="enabling-the-application-integration-for-canvas"></a>Az alkalmazás-integráció a vászonra engedélyezése

Ez a szakasz célja tagolása a vászonra az alkalmazás alkalmazással való integráció engedélyezése hogyan.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás-integráció a vászonra, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **vászonra**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **vásznat**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Vászon] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Vászon")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a vászonra hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés a vászonra az konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **vászon** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be vászonra felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Egyszeri bejelentkezés beállítása")

3.  **Alkalmazás URL-címet konfigurálni** lap, az **Vászon bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát `https://<tenant-name>.instructure.com`, majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés a vászonra** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a vászonra vállalati webhely rendszergazdaként.

6.  Nyissa meg a **Tanfolyamok \> felügyelt fiókok \> a Microsoft**.

    ![Vászon] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Vászon")

7.  A bal oldali navigációs ablakban jelölje be a **hitelesítés**, és kattintson az **Új SAML Config hozzáadása**gombra.

    ![Hitelesítés] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Hitelesítés")

8.  Az aktuális integrációs lapon hajtsa végre az alábbi lépéseket:

    ![Aktuális integrációja] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Aktuális integrációja")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a vászonra** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be a **IdP entitás azonosítója** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a vászonra** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Napló a URL-címe** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a vászonra** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Napló meg URL-címe** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a vászonra** párbeszédpanel lapon másolja a vágólapra az **URL-cím módosítása jelszó** értéket, és illessze be a **Jelszót hivatkozások módosítása** mezőben lévő értéket.
    5.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Tanúsítvány ujjlenyomat** mezőben lévő értéket.  

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    6.  A **Bejelentkezési attribútum** listából válassza ki a **NameID**.
    7.  Az **Azonosító formátum** listából válassza ki a **emailAddress**.
    8.  **Hitelesítési beállítások a Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a vászonra, hogy ki kell építenie Vászon be.  
Vászon, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a **vászonra** bérlői.

2.  Nyissa meg a **Tanfolyamok \> felügyelt fiókok \> a Microsoft**.

    ![Vászon] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Vászon")

3.  Kattintson a **felhasználók**elemre.

    ![Felhasználók] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Felhasználók")

4.  Kattintson az **Új felhasználó hozzáadása**gombra.

    ![Felhasználók] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Felhasználók")

5.  Hozzáadása egy új felhasználó párbeszédpanel lap hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Felhasználó hozzáadása")

    1.  A **Teljes neve** mezőbe írja be a felhasználó nevét.
    2.  Az **E-mail** mezőbe írja be a felhasználó e-mail címét.
    3.  A **bejelentkezési** mezőbe írja be a felhasználó Azure Active Directory e-mail címét.
    4.  Jelölje ki **a felhasználót a fiók létrehozása E-mail**.
    5.  Kattintson a **felhasználó hozzáadása**elemre.

>[AZURE.NOTE] Bármely más vászon felhasználói fiók létrehozása eszközöket is használhatja, illetve vászon rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Felhasználók hozzárendelése a vászonra, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **vászonra **alkalmazás integrációs lap **adhatnak a felhasználóknak**.

    ![Felhasználók hozzárendelése] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Felhasználók hozzárendelése")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
