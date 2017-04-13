<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a AppDynamics |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a AppDynamics az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Oktatóprogram: Azure Active Directory-integráció a AppDynamics

Ebben az oktatóanyagban célja integrálása az Azure és AppDynamics megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy AppDynamics egyszeri bejelentkezés engedélyezve van az előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók AppDynamics rendelt fogja tudni az alkalmazás a AppDynamics vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját AppDynamics engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Eset")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Az alkalmazás integrációját AppDynamics engedélyezése

Ez a szakasz célja, hogyan AppDynamics az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját AppDynamics, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **AppDynamics**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **AppDynamics**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy AppDynamics hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **AppDynamics** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az AppDynamics felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési AppDynamics** mezőbe írja be a felhasználóknak, hogy bejelentkezésre AppDynamics ("*https://companyname.saas.appdynamics.com*") által használt URL-címet, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés AppDynamics a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a AppDynamics vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra, és kattintson a **felügyelet**.

    ![Felügyelete] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Felügyelete")

7.  Kattintson a **Hitelesítési szolgáltató** fülre.

    ![Hitelesítésszolgáltató] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Hitelesítésszolgáltató")

8.  **Hitelesítési szolgáltató** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML konfigurálása] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML konfigurálása")

    1.  **Hitelesítési szolgáltató**jelölje ki a **SAML**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a AppDynamics** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a AppDynamics** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    4.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    5.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **tanúsítvány** mezőben lévő értéket szeretné
    6.  Kattintson a **Mentés**gombra.
        ![Mentés] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Mentés")

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a AppDynamics, hogy ki kell építenie AppDynamics be.  
AppDynamics, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a AppDynamics vállalati webhely rendszergazdaként.

2.  Kattintson a **felhasználók**, és kattintson a **+** a **Felhasználó létrehozása** párbeszédpanel megnyitásához.

    ![Felhasználók] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Felhasználók")

3.  **Create User** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó létrehozása] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Felhasználó létrehozása")

    1.  Írja be a **felhasználónevét**, a **nevét**, **E-mail**, az **Új jelszót**, **Ismételje meg az új jelszó** érvényes AAD fiókkal szeretne-e be a kapcsolódó szövegdobozok kiépítése.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más AppDynamics felhasználói fiók létrehozása eszközöket is használhatja, illetve AppDynamics hozhatók létre az Azure Active Directory-fiókokkal által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Felhasználók hozzárendelése AppDynamics, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **AppDynamics **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
