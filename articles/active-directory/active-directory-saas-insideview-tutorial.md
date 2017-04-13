<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a InsideView |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a InsideView az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Oktatóprogram: Azure Active Directory-integráció a InsideView
  
Ebben az oktatóanyagban célja integrálása az Azure és InsideView megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   InsideView bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók InsideView rendelt fogja tudni az alkalmazás a InsideView vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját InsideView engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Eset")
##<a name="enabling-the-application-integration-for-insideview"></a>Az alkalmazás integrációját InsideView engedélyezése
  
Ez a szakasz célja, hogyan InsideView az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját InsideView, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **InsideView**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **InsideView**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy InsideView hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **InsideView** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az InsideView felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe beállítása** lapon az **InsideView válasz URL-cím** mezőbe írja be a InsideView egyszeri bejelentkezési URL-címet (például: `https://my.insideview.com/iv/<STS Name>/login.iv`), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés InsideView a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a InsideView vállalati webhely rendszergazdaként.

6.  Az eszköztáron a képernyő tetején kattintson a **rendszergazda**, **SingleSignOn beállítások**, és kattintson a **SAML hozzáadása**gombra.

    ![Egyszeri bejelentkezés SAML beállításai] (./media/active-directory-saas-insideview-tutorial/IC794135.png "Egyszeri bejelentkezés SAML beállításai")

7.  **Hozzáadás egy új SAML** csoportban hajtsa végre az alábbi lépéseket:

    ![Egy új SAML hozzáadása] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Egy új SAML hozzáadása")

    1.  Az **STS neve** mezőbe írja be a konfigurációban nevét.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a InsideView** párbeszédpanel lapon másolja a **Service Provider (SP) által kezdeményezett végpont** értéket, és illessze be a **SamlP/WS-Fed Unsolicated végpont** mezőben lévő értéket.
    3.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása
        
        >[AZURE.TIP]További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **STS tanúsítvány** mezőben lévő értéket szeretné
    5.  Az **Crm felhasználói azonosító leképezése** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  Az **Crm E-mail leképezése** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Az **Crm Utónév leképezése** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  Az **Utónév Crm-leképezés** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Kattintson a **Mentés**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a InsideView, hogy ki kell építenie InsideView be.  
InsideView, ha a feladat manuális kiépítési.
  
Felhasználók és a InsideView létrehozott névjegyek beszerzése, lépjen kapcsolatba a customer success Manager alkalmazást, vagy e-mailt küldi**support@insideview.com**

>[AZURE.NOTE] Bármely más InsideView felhasználói fiók létrehozása eszközöket is használhatja, vagy InsideView hozhatók létre az Azure Active Directory-fiókokkal által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Felhasználók hozzárendelése InsideView, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **InsideView **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).