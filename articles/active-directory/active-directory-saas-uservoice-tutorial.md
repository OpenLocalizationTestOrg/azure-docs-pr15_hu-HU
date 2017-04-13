<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a UserVoice |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a UserVoice az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Oktatóprogram: Azure Active Directory-integráció a UserVoice
  
Ebben az oktatóanyagban célja integrálása az Azure és UserVoice megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   UserVoice bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók UserVoice rendelt fogja tudni az alkalmazás a UserVoice vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját UserVoice engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Eset")

##<a name="enabling-the-application-integration-for-uservoice"></a>Az alkalmazás integrációját UserVoice engedélyezése
  
Ez a szakasz célja, hogyan UserVoice az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját UserVoice, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **UserVoice**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **UserVoice**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![UserVoice] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy UserVoice hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az UserVoice konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **UserVoice** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az UserVoice felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **UserVoice bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. UserVoice.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés UserVoice a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\UserVoice.cer**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a UserVoice vállalati webhely rendszergazdaként.

6.  Az eszköztáron a képernyő tetején kattintson a beállítások gombra, és válassza a menüből webes portál.

    ![Beállítások] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Beállítások")

7.  A **webes portál** lapon a **felhasználói hitelesítés** csoportban kattintson a **Szerkesztés** hivatkozásra a **Felhasználói hitelesítés szerkesztése** párbeszédpanel lap

    ![Webes portál] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Webes portál")

8.  A **Felhasználói hitelesítés szerkesztése** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Szerkesztheti a felhasználói hitelesítés] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Szerkesztheti a felhasználói hitelesítés")

    1.  Kattintson **Az egyszeri bejelentkezés (SSO)**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a UserVoice** párbeszédpanel lapon másolja a **Távoli bejelentkezési URL-cím** értéket, és illessze be a **SSO távoli bejelentkezés** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a UserVoice** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **távoli Sign-Out SSO szövegdoboz**.
    4.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be az **aktuális tanúsítvány SHA1 ujjlenyomat** szövegdoboz.  

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    5.  **Hitelesítési beállítások a Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a UserVoice, hogy ki kell építenie UserVoice be.  
UserVoice, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **UserVoice** bérlői webhelyen.

2.  Nyissa meg a **beállításokat**.

    ![Beállítások] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Beállítások")

3.  Kattintson az **Általános**elemre.

4.  Kattintson a **ügynökök és engedélyek**gombra.

    ![Ügynökök és engedélyek] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Ügynökök és engedélyek")

5.  Kattintson a **Hozzáadás rendszergazdák**elemre.

    ![A rendszergazdák hozzáadása] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "A rendszergazdák hozzáadása")

6.  A **rendszergazdák meghívása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![A rendszergazdák meghívása] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "A rendszergazdák meghívása")

    1.  Az e-mailek texbox írja be az e-mail címet, a fiók rendelkezést szeretne, és kattintson a **Hozzáadás**gombra.
    2.  Kattintson a **Meghívás**gombra.

>[AZURE.NOTE] Bármely más UserVoice felhasználói fiók létrehozása eszközöket is használhatja, illetve UserVoice rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Felhasználók hozzárendelése UserVoice, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **UserVoice **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).