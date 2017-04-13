<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ügyféloldali |} Microsoft Azure" 
    description="Megtudhatja, hogy miként ügyféloldali használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Oktatóprogram: Azure Active Directory-integráció a követ
  
Ebben az oktatóanyagban célja Azure és ügyféloldali integrációja megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Ügyféloldali bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók ügyféloldali rendelt tudnak való egyszeri bejelentkezési az ügyféloldali vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját ügyféloldali engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Eset")
##<a name="enabling-the-application-integration-for-envoy"></a>Az alkalmazás integrációját ügyféloldali engedélyezése
  
Ez a szakasz célja, hogyan ügyféloldali az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját követ, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **követ**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **követ**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Ügyféloldali] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Ügyféloldali")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználók hitelesítő követ fiókjukhoz az összevonási alapján a SAML protokoll használatával Azure Active Directory számára a szerkezet.  
Egyszeri bejelentkezés az ügyféloldali konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **ügyféloldali** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be az ügyféloldali felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Beállítás az egyszeri bejelentkezés")

3.  **App URL-címe beállítása** lapon az **Ügyféloldali bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Envoy.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Állítsa be az app URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés ügyféloldali a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Envoy.cer**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be az ügyféloldali vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra.

    ![Ügyféloldali] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Ügyféloldali")

7.  Kattintson a **vállalat**.

    ![Vállalati] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Vállalati")

8.  Kattintson a **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  **SAML-hitelesítés** konfigurálása csoportban hajtsa végre az alábbi lépéseket:

    ![SAML-hitelesítés] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML-hitelesítés")

    >[AZURE.NOTE] A HQ helyét azonosító értéke az alkalmazás által létrehozott automatikus.

    1.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **ujjlenyomat** mezőben lévő értéket.  

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés az ügyféloldali** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **Identitás szolgáltató HTTP SAML URL-címe** mezőben lévő értéket.
    3.  A **módosítások mentése**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő állíthatja be az ügyféloldali kiépítési felhasználói számára nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen követ, a ügyféloldali ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a ügyféloldali automatikusan létrehozott.
##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Felhasználók hozzárendelése követ, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Az **ügyféloldali **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).