<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Gigya |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Gigya az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Oktatóprogram: Azure Active Directory-integráció a Gigya
  
Ebben az oktatóanyagban célja integrálása az Azure és Gigya megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy Gigya egyszeri bejelentkezéses előfizetés engedélyezve
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Gigya rendelt fogja tudni az alkalmazás a Gigya vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Gigya engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Egyszeri bejelentkezés beállítása")
##<a name="enabling-the-application-integration-for-gigya"></a>Az alkalmazás integrációját Gigya engedélyezése
  
Ez a szakasz célja, hogyan Gigya az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Gigya, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Gigya**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Gigya**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Gigya hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Gigya** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Gigya felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési Gigya** mezőbe írja be az URL-t, a következő mintát "*http://company.gigya.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Gigya a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Gigya vállalati webhely rendszergazdaként.

6.  Nyissa meg a **Beállítások \> SAML bejelentkezési**, és kattintson a **Hozzáadás** gombra.

    ![SAML bejelentkezés] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML bejelentkezés")

7.  A **SAML bejelentkezési** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML konfigurálása] (./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML konfigurálása")

    1.  Az **neve** mezőbe írja be a konfigurációban nevét.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Gigya** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Gigya** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **Egyszeri bejelentkezéses szolgáltatás URL-címe** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Gigya** párbeszédpanel lapon másolja a **Névformátum azonosító** értékét, és illessze be a **Név, azonosító formátuma** mezőben lévő értéket.
    5.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása
        
        >[AZURE.TIP]További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    6.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **X.509-es tanúsítvány** mezőben lévő értéket szeretné.
    7.  Kattintson a **beállítások mentéséhez**.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Gigya, hogy ki kell építenie Gigya be.  
Gigya, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Gigya** vállalati webhely.

2.  Nyissa meg a **felügyeleti \> a felhasználók kezelése**, majd **A felhasználók meghívása**elemre.

    ![Felhasználókezelés] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Felhasználókezelés")

3.  Felhasználók meghívása párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Felhasználók meghívása] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Felhasználók meghívása")

    1.  Az **E-mail** mezőbe írja be a kívánt kiépítése érvényes Azure Active Directory-fiók e-mail alias.
    2.  Kattintson a **felhasználó felkérése**gombra.
    
        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó e-mailt kap.

>[AZURE.NOTE] Bármely más Gigya felhasználói fiók létrehozása eszközöket is használhatja, illetve Gigya rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Felhasználók hozzárendelése Gigya, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Gigya **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).