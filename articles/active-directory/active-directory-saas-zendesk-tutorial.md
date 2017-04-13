<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Zendesk |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Zendesk az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Oktatóprogram: Azure Active Directory-integráció a Zendesk
  
Ebben az oktatóanyagban célja integrálása az Azure és Zendesk megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Zendesk bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Zendesk rendelt fogja tudni az alkalmazás a Zendesk vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Zendesk engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Eset")

##<a name="enabling-the-application-integration-for-zendesk"></a>Az alkalmazás integrációját Zendesk engedélyezése
  
Ez a szakasz célja, hogyan Zendesk az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Zendesk, hajtsa végre az alábbi lépéseket:

1.  Az Azure felügyeleti portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Zendesk**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Zendesk**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Zendesk hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Zendesk konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure Active Directory-portálon **Zendesk** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Zendesk felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Állítsa be az app URL-címe")
  
    egy. Az **URL a bejelentkezési Zendesk** mezőbe írja be az URL-CÍMÉT az alábbi minta használatával:`https://<tenant-name>.zendesk.com`

    b. Kattintson a **Tovább**gombra.



4.  A **Configure egyszeri bejelentkezés Zendesk a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a compiter a helyi meghajtóra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Zendesk vállalati webhely rendszergazdaként.

6.  Kattintson a **rendszergazda**.

7.  A bal oldali navigációs ablaktáblában kattintson a **Beállítások**gombra, és kattintson a **Biztonság**gombra.

    ![Biztonsági] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Biztonsági")

8.  A **Biztonság** lapon kattintson a **rendszergazda és ügynökök** fülre.

9.  Válassza a **egyszeri bejelentkezés (SSO) és a SAML**, és válassza a **SAML**.

10. Az Azure Active Directory-portálon **konfigurálása az egyszeri bejelentkezés Zendesk a** lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **SAML egyszeri bejelentkezési URL-címe** mezőben lévő értéket.

11. Az Azure Active Directory-portálon **konfigurálása az egyszeri bejelentkezés Zendesk a** lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Távoli kijelentkezés URL-címe** mezőben lévő értéket.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Egyszeri bejelentkezés")

12. Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Tanúsítvány ujjlenyomat** mezőben lévő értéket.

    >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

13. Kattintson a **Mentés**gombra.

14. Az Azure Active Directory-portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a **Zendesk**, hogy ki kell építenie **Zendesk**be.  
Az **Zendesk**esetében kézi tevékenység kiépítési.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Építse Zendesk egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Zendesk** bérlői webhelyen.

2.  Jelölje ki a **Vevőlistát** fülre.

3.  Jelölje ki a **felhasználó** fülre, és kattintson a **Hozzáadás**gombra.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Felhasználó hozzáadása")

4.  Írja be az e-mail címet, egy meglévő Azure AD-fiók rendelkezést szeretne, és kattintson a **Mentés**.

    ![Új felhasználó] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Új felhasználó")

>[AZURE.NOTE] Bármely más Zendesk felhasználói fiók létrehozása eszközöket is használhatja, illetve Zendesk rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Felhasználók hozzárendelése Zendesk, hajtsa végre az alábbi lépéseket:

1.  Az Azure Active Directory-portálon próba-fiók létrehozása.

2.  Kattintson a **Zendesk **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
