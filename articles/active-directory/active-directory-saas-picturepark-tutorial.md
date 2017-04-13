<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Picturepark |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Picturepark az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Oktatóprogram: Azure Active Directory-integráció a Picturepark
  
Ebben az oktatóanyagban célja integrálása az Azure és Picturepark megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Picturepark bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Picturepark rendelt fogja tudni az alkalmazás a Picturepark vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Picturepark engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Eset")

##<a name="enabling-the-application-integration-for-picturepark"></a>Az alkalmazás integrációját Picturepark engedélyezése
  
Ez a szakasz célja, hogyan Picturepark az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Picturepark, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Picturepark**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Picturepark**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Picturepark hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Picturepark konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat értékének beolvasásához](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Picturepark** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Picturepark felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési Picturepark** mezőbe írja be az URL-t, a következő mintát "*http://company.picturepark.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Picturepark a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Picturepark vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **Felügyeleti eszközök**, és válassza a **Kezelőkonzol**.

    ![Kezelőkonzol] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Kezelőkonzol")

7.  Kattintson a **hitelesítés**, és kattintson a **Identitásszolgáltatók**.

    ![Hitelesítés] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Hitelesítés")

8.  **Identitás szolgáltató konfigurálása** csoportban hajtsa végre az alábbi lépéseket:

    ![Identitás szolgáltató konfigurálása] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identitás szolgáltató konfigurálása")

    1.  Kattintson a **hozzáadása**gombra.
    2.  Írja be a konfigurációban nevét.
    3.  Válassza a **Beállítás alapértelmezett nyomtatóként**.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Picturepark** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **Kibocsátó URI** mezőben lévő értéket.
    5.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Kibocsátó hordozható nyomtatás megbízható** mezőben lévő értéket.  

        >[AZURE.TIP]További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    6.  Kattintson a **JoinDefaultUsersGroup**.
    7.  A **felelős** mezőben lévő értéket szeretné az **EmailCím** attribútum, írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Konfiguráció] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Konfiguráció")
    8.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Picturepark, hogy ki kell építenie Picturepark be.  
Picturepark, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Picturepark** bérlői webhelyen.

2.  Kattintson az eszköztáron a képernyő tetején kattintson a **Felügyeleti eszközök**, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Felhasználók")

3.  A **felhasználók áttekintése** lapon kattintson az **Új**gombra.

    ![Felhasználókezelés] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Felhasználókezelés")

4.  A **Felhasználó létrehozása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Felhasználó létrehozása] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Felhasználó létrehozása")

    1.  Írja be a: **E-mail címe**, **jelszót**, **erősítse meg a jelszót**, **Utónév**, **Vezetéknév**, **Vállalati**, **ország**, **ZIP-**, **Város** az érvényes Azure Active Directory-felhasználó, amelyet a kapcsolódó szövegdobozok az objektumtípus rendelkezni.
    2.  Válasszon egy **nyelvet**.
    3.  Kattintson a **létrehozása**gombra.

>[AZURE.NOTE]Bármely más Picturepark felhasználói fiók létrehozása eszközöket is használhatja, illetve Picturepark rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Felhasználók hozzárendelése Picturepark, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Picturepark **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).