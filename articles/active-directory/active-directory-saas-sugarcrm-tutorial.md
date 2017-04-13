<properties 
    pageTitle="Oktatóprogram: Azure Active Directory integrációs integrációja SugarCRM |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a SugarCRM az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-sugarcrm"></a>Oktatóprogram: Azure Active Directory integrációs SugarCRM integrációja
  
Ebben az oktatóanyagban célja Azure és cukor CRM integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Cukor CRM egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók cukor CRM rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a cukor CRM vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását cukor CRM engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Eset")

##<a name="enabling-the-application-integration-for-sugar-crm"></a>Az alkalmazás integrálását cukor CRM engedélyezése
  
Ez a szakasz célja, hogyan cukor CRM alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-sugar-crm-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását cukor CRM, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Cukor CRM**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Cukor CRM**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Cukor CRM] (./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Cukor CRM")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő cukor CRM tagolása.  
Ez az eljárás részeként szükséges számrendszerben-64 kódolt tanúsítvány feltöltése a cukor CRM-bérlő.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Cukor CRM** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az cukor CRM felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **Cukor CRM bejelentkezési a URL-cím** mezőbe írja be a a felhasználóknak, hogy a bejelentkezés az cukor CRM alkalmazás által használt URL-CÍMÉT (például: "*http://company.sugarondemand.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés cukor CRM a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a cukor CRM vállalati webhely rendszergazdaként.

6.  Kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Rendszergazda")

7.  **Felügyelete** csoportjában kattintson a **Jelszó kezelése**pontra.

    ![Felügyelete] (./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Felügyelete")

8.  Jelölje ki a **SAML-hitelesítés engedélyezése**.

    ![Felügyelete] (./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Felügyelete")

9.  A **SAML hitelesítési** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML-hitelesítés] (./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML-hitelesítés")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a cukor CRM** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a cukor CRM** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **ZAT URL-címe** mezőben lévő értéket.
    3.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a teljes tanúsítvány be **X.509-es tanúsítvány** szövegdoboz.
    5.  Kattintson a **Mentés**gombra.

10. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a cukor CRM** párbeszédpanel lapon jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítést kérő, és válassza a **kész**gombra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználók cukor CRM való bejelentkezéshez, hogy ki kell építenie cukor CRM.  
Cukor CRM, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Cukor CRM** vállalati webhely.

2.  Kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Rendszergazda")

3.  **Felügyelete** csoportjában kattintson a **Felhasználók kezelése**elemre.

    ![Felügyelete] (./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Felügyelete")

4.  Nyissa meg a **felhasználók \> hozzon létre új felhasználót**.

    ![Új felhasználó létrehozása] (./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Új felhasználó létrehozása")

5.  A **Felhasználói profil** lapon hajtsa végre az alábbi lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "Új felhasználó")

    1.  A kapcsolódó szövegdobozok beírja a felhasználónevet, az utolsó érvényes Azure Active Directory-felhasználó nevét és e-mail címét.

6.  **Állapot**válassza az **aktív**.

7.  A jelszó lapon hajtsa végre az alábbi lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "Új felhasználó")

    1.  Írja be a kapcsolódó mezőben lévő értéket a jelszót.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más cukor CRM felhasználói fiók létrehozása eszközöket is használhatja, illetve cukor CRM rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-sugar-crm-perform-the-following-steps"></a>Felhasználók hozzárendelése cukor CRM, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Cukor CRM** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).