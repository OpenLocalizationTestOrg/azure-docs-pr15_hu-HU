<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Panopto |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Panopto az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Oktatóprogram: Azure Active Directory-integráció a Panopto
  
Ebben az oktatóanyagban célja integrálása az Azure és Panopto megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Panopto bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Panopto rendelt fogja tudni az alkalmazás a Panopto vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Panopto engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Eset")
##<a name="enabling-the-application-integration-for-panopto"></a>Az alkalmazás integrációját Panopto engedélyezése
  
Ez a szakasz célja, hogyan Panopto az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Panopto, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Panopto**.

    ![Appkication gyűjtemény] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Panopto**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Panopto hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Panopto** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Panopto felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **Panopto bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Panopto.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Állítsa be az app URL-címe")

4.  A **Configure egyszeri bejelentkezés Panopto a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Panopto vállalati webhely rendszergazdaként.

6.  Eszköztár a bal oldalon a **rendszer**hivatkozásra, és válassza a **Identitásszolgáltatók**.

    ![Rendszer] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Rendszer")

7.  Kattintson a **szolgáltató hozzáadása**gombra.

    ![Identitásszolgáltatók] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Identitásszolgáltatók")

8.  A SAML szolgáltató csoportban hajtsa végre az alábbi lépéseket:

    ![Szoftver konfigurálása] (./media/active-directory-saas-panopto-tutorial/IC777672.png "Szoftver konfigurálása")

    1.  A **Szolgáltató típusa** listából válassza ki a **SAML20**
    2.  Az **Példányának neve** mezőbe írja be a példány nevét.
    3.  **Rövid leírást** mezőbe írja be egy rövid leírást.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Panopto** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    5.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Panopto** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **Bepattog a weblap URL-címe** mezőben lévő értéket.
    6.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    7.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **Nyilvános kulcshoz** mezőben lévő értéket szeretné
    8.  Kattintson a **Mentés**gombra.
        ![Mentés] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Mentés")

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő konfigurálható kiépítési Panopto a felhasználó nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen Panopto, a Panopto ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a Panopto automatikusan létrehozott.

>[AZURE.NOTE]Bármely más Panopto felhasználói fiók létrehozása eszközöket is használhatja, illetve Panopto hozhatók létre az Azure Active Directory-fiókokkal által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Felhasználók hozzárendelése Panopto, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Panopto **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).