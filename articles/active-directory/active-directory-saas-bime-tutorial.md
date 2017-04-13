<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Bime |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Bime az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Oktatóprogram: Azure Active Directory-integráció a Bime

Ebben az oktatóanyagban célja integrálása az Azure és Bime megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Bime bérlői webhelyre

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Bime rendelt fogja tudni az alkalmazás a Bime vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Bime engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-bime-tutorial/IC775552.png "Eset")
##<a name="enabling-the-application-integration-for-bime"></a>Az alkalmazás integrációját Bime engedélyezése

Ez a szakasz célja, hogyan Bime az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Bime, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-bime-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-bime-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-bime-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Bime**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-bime-tutorial/IC775553.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Bime**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Bime hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Bime konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Bime** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bime-tutorial/IC771709.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Bime felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bime-tutorial/IC775555.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe konfigurálása** lapon **Bime bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Bimeapp.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-bime-tutorial/IC775556.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Bime a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Bime.cer**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bime-tutorial/IC775557.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Bime vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron kattintson a **rendszergazda**, majd a **fiók**.

    ![Rendszergazda] (./media/active-directory-saas-bime-tutorial/IC775558.png "Rendszergazda")

7.  A fiók konfigurálása lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bime-tutorial/IC775559.png "Egyszeri bejelentkezés beállítása")

    1.  Jelölje ki a **SAML engedélyezése hitelesítési**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Bime** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Távoli bejelentkezési URL-cím** mezőben lévő értéket.
    3.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Tanúsítvány ujjlenyomat** mezőben lévő értéket.  

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    4.  Kattintson a **Mentés**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-bime-tutorial/IC775560.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Bime, hogy ki kell építenie Bime be.  
Bime, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Bime** bérlői webhelyen.

2.  Kattintson az eszköztáron kattintson a **rendszergazda**, majd a **felhasználók**.

    ![Rendszergazda] (./media/active-directory-saas-bime-tutorial/IC775561.png "Rendszergazda")

3.  A **Felhasználók listában**kattintson az **Új felhasználó hozzáadása** ("+").

    ![Felhasználók] (./media/active-directory-saas-bime-tutorial/IC775562.png "Felhasználók")

4.  A **Felhasználó adatai** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Felhasználó adatai] (./media/active-directory-saas-bime-tutorial/IC775563.png "Felhasználó adatai")

    1.  Írja be az utónevet, Vezetéknév, bejelentkezés, kiépítése kívánt érvényes AAD fiók E-mail.
    2.  Kattintson a Mentés gombra.

>[AZURE.NOTE] Bármely más Bime felhasználói fiók létrehozása eszközöket is használhatja, illetve Bime rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Felhasználók hozzárendelése Bime, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Bime **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-bime-tutorial/IC775564.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-bime-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
