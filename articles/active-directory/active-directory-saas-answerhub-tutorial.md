<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a AnswerHub |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a AnswerHub az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Oktatóprogram: Azure Active Directory-integráció a AnswerHub

Ebben az oktatóanyagban célja integrálása az Azure és [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software)megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) egyszeri bejelentkezés engedélyezve van az előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók AnswerHub rendelt fogja tudni az alkalmazás a AnswerHub vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját AnswerHub engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Eset")

##<a name="enabling-the-application-integration-for-answerhub"></a>Az alkalmazás integrációját AnswerHub engedélyezése

Ez a szakasz célja, hogyan AnswerHub az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját AnswerHub, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **AnswerHub**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **AnswerHub**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy AnswerHub hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **AnswerHub** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az AnswerHub felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **AnswerHub bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://company.answerhub.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés AnswerHub a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a AnswerHub vállalati webhely rendszergazdaként.
    >[AZURE.NOTE] Ha segítségre van szüksége AnswerHub konfigurálása, AnswerHub a támogatási [csoport](mailto:success@answerhub.com. )munkatársaitól.








6.  Nyissa meg a **felügyelet**.

7.  Kattintson a **felhasználók és csoportok** fülre.

8.  A navigációs ablak bal oldalán, a **Közösségi beállítások** csoportban kattintson a **SAML beállítás**gombra.

9.  Kattintson a **IDP Config** fülre.

10. A **IDP Config** lapon hajtsa végre az alábbi lépéseket:

    ![SAML beállítása] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "SAML beállítása")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a AnswerHub** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **IDP bejelentkezési URL-cím** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a AnswerHub** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **IDP kijelentkezés URL-címe** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a AnswerHub** párbeszédpanel lapon másolja a **Névformátum azonosító** értékét, és illessze be a **IDP névformátum azonosítója** mezőben lévő értéket.
    4.  Kattintson a **kulcsok és a tanúsítványok**gombra.

11. A kulcsok és tanúsítványok lapon hajtsa végre az alábbi lépéseket:

    ![Kulcsok és tanúsítványok] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Kulcsok és tanúsítványok")

    1.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    2.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **IDP nyilvános kulcs (x 509 Format)** mezőben lévő értéket szeretné.
    3.  Kattintson a **Mentés**gombra.

12. A **IDP Config** lapon kattintson a **Mentés**gombra.

13. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a AnswerHub, hogy ki kell építenie AnswerHub be.  
AnswerHub, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **AnswerHub** vállalati webhely.

2.  Nyissa meg a **felügyelet**.

3.  Kattintson a **felhasználók és csoportok** fülre.

4.  A navigációs ablak bal oldalán, a **Felhasználók kezelése** csoportban kattintson a **létrehozhat vagy importálhat felhasználók**elemre.

    ![Felhasználók és csoportok] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Felhasználók és csoportok")

5.  Írja be az **E-mail cím**, **felhasználónév** és **jelszó** egy érvényes Azure Active Directory-fiókját, kiépítése a kapcsolódó szövegdobozok be szeretné, és kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más AnswerHub felhasználói fiók létrehozása eszközöket is használhatja, illetve AnswerHub rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Felhasználók hozzárendelése AnswerHub, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **AnswerHub **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
