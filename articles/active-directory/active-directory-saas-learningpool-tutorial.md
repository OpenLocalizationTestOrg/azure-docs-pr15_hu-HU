<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Learningpool |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Learningpool az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Oktatóprogram: Azure Active Directory-integráció a Learningpool
  
Ebben az oktatóanyagban célja integrálása az Azure és Learningpool megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Learningpool egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Learningpool rendelt fogja tudni az alkalmazás a Learningpool vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Learningpool engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Eset")
##<a name="enabling-the-application-integration-for-learningpool"></a>Az alkalmazás integrációját Learningpool engedélyezése
  
Ez a szakasz célja, hogyan Learningpool az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Learningpool, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Learningpool**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Learningpool**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Learningpool hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.
  
A Learningpool alkalmazása egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs várja meg a SAML előfeltételek.  
Az alábbi képernyőképen látható, a.

![SAML jogkivonat attribútumok] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML jogkivonat attribútumok")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Learningpool** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitása elemre.

    ![Attribútumok] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Attribútumok")

2.  Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    ###  

  	|Attribútumnév                |Attribútumérték            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenName   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.mail
  	|urn: oid:2.5.4.4|User.surname

    1.  Az adatok tábla minden sorához a fenti kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.
    3.  Az **Attribútumérték** listából válassza ki az attribútumérték meg abban a sorban látható.
    4.  Kattintson a **kész**gombra.

3.  Kattintson a **módosítások alkalmazásához**.

4.  A böngészőben kattintson a **vissza** nyissa meg újra az **Első lépések** párbeszédpanelt.

5.  Kattintson a **Konfigurálás egyszeri bejelentkezés a** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Singel bejelentkezéses konfigurálása] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Singel bejelentkezéses konfigurálása")

6.  **Hogyan szeretné, hogy jelentkezzen be az Learningpool felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Egyszeri bejelentkezés beállítása")

7.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Learningpool** mezőbe írja be a bejelentkezni a Learningpool alkalmazás a felhasználók által használt URL-CÍMÉT (például: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Állítsa be az App URL-címe")

8.  **Konfigurálása az egyszeri bejelentkezés Learningpool a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Egyszeri bejelentkezés beállítása")

9.  A Learningpool támogatási csoportnak, hogy metaadatokat fájl továbbítása

    >[AZURE.NOTE]Egyszeri bejelentkezés a Learningpool támogatási csoport által engedélyezésük tartalmaz.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Learningpool, hogy ki kell építenie Learningpool be.
  
Nincs teendő konfigurálható kiépítési Learningpool a felhasználó nem.  
Felhasználók kell a Learningpool támogatási csoport által hozható létre.

>[AZURE.NOTE]Bármely más Learningpool felhasználói fiók létrehozása eszközöket is használhatja, illetve Learningpool rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Felhasználók hozzárendelése Learningpool, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Learningpool **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).