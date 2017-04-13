<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Onit |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Onit az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatizált kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Oktatóprogram: Azure Active Directory-integráció a Onit
  
Ebben az oktatóanyagban célja integrálása az Azure és Onit megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy Onit egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Onit rendelt fogja tudni az alkalmazás a Onit vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Onit engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-onit-tutorial/IC791166.png "Eset")
##<a name="enabling-the-application-integration-for-onit"></a>Az alkalmazás integrációját Onit engedélyezése
  
Ez a szakasz célja, hogyan Onit az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Onit, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-onit-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-onit-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-onit-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Onit**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-onit-tutorial/IC791167.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Onit**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Onit hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Onit konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).
  
A Onit alkalmazása egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs várja meg a SAML előfeltételek.  
Az alábbi képernyőképen látható, a.

![Egyszeri bejelentkezés] (./media/active-directory-saas-onit-tutorial/IC791168.png "Egyszeri bejelentkezés")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Onit** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitása elemre.

    ![Attribútumok] (./media/active-directory-saas-onit-tutorial/IC791169.png "Attribútumok")

2.  Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    
  	|Attribútumnév|Attribútumérték|
  	|---|---|
  	|név|User.userPrincipalName|
  	|e-mailben|User.mail|

    1.  Az adatok tábla minden sorához a fenti kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.
    3.  Az **Attribútumérték** listából válassza ki az attribútumérték meg abban a sorban látható.
    4.  Kattintson a **kész**gombra.

3.  Kattintson a **módosítások alkalmazásához**.

4.  A böngészőben kattintson a **vissza** nyissa meg újra az **Első lépések** párbeszédpanelt.

5.  Kattintson a **Konfigurálás egyszeri bejelentkezés a** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-onit-tutorial/IC791170.png "Egyszeri bejelentkezés beállítása")

6.  **Hogyan szeretné, hogy jelentkezzen be az Onit felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-onit-tutorial/IC791171.png "Egyszeri bejelentkezés beállítása")

7.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Onit** mezőbe írja be a bejelentkezni a Onit alkalmazás a felhasználók által használt URL-CÍMÉT (például: "*https://ms-sso-test.onit.com*"), és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-onit-tutorial/IC791172.png "Állítsa be az App URL-címe")

8.  A **Configure egyszeri bejelentkezés Onit a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-onit-tutorial/IC791173.png "Egyszeri bejelentkezés beállítása")

9.  A különböző webes böngészőablakban jelentkezzen be a Onit vállalati webhely rendszergazdaként.

10. A felső sávon kattintson a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-onit-tutorial/IC791174.png "Felügyelete")

11. Kattintson a **Szerkesztés Corporation**.

    ![Szerkesztés Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Szerkesztés Corporation")

12. Kattintson a **Biztonság** fülre.

    ![Vállalat adatainak szerkesztése] (./media/active-directory-saas-onit-tutorial/IC791176.png "Vállalat adatainak szerkesztése")

13. A **Biztonság** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-onit-tutorial/IC791177.png "Egyszeri bejelentkezés")

    1.  Jelölje ki a **Hitelesítési stratégia**, **egyszeri bejelentkezés, és a jelszavát**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Onit** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Idp cél URL-címe** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Onit** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Idp kijelentkezés URL-címe** mezőben lévő értéket.
    4.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Idp tanúsítvány ujjlenyomat (SHA1)** mezőben lévő értéket.  

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    5.  **Egyszeri bejelentkezés típusa**jelölje ki a **SAML**.
    6.  Az **Egyszeri bejelentkezés gomb szöveg** mezőbe írja be a szöveg, amely tetszik gombra.
    7.  Válassza a **Bejelentkezés egyszeri bejelentkezéssel: a következő tartományokat felhasználókhoz szükséges**, írja be a kapcsolódó mezőben lévő értéket a próba-felhasználó e-mail címét, és kattintson a **frissítés**.
        ![Szerkesztés Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Szerkesztés Corporation")

14. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-onit-tutorial/IC791179.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Onit, hogy ki kell építenie Onit be.  
Onit, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Onit** vállalati webhely.

2.  Kattintson a **felhasználó hozzáadása**elemre.

    ![Felügyelete] (./media/active-directory-saas-onit-tutorial/IC791180.png "Felügyelete")

3.  A **Felhasználó hozzáadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-onit-tutorial/IC791181.png "Felhasználó hozzáadása")

    1.  Írja be a **nevét** , és be a kapcsolódó szövegdobozok kiépítése szeretne egy érvényes AAD fiók **E-mail címét** .
    2.  Kattintson a **létrehozása**gombra.  

        >[AZURE.NOTE] A fióktulajdonos kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más Onit felhasználói fiók létrehozása eszközöket is használhatja, illetve Onit rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Felhasználók hozzárendelése Onit, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Onit **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-onit-tutorial/IC791182.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-onit-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).