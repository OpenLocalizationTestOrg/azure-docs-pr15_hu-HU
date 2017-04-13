<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Jitbit segélyszolgálat |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Jitbit segélyszolgálat használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Oktatóprogram: Azure Active Directory-integráció a Jitbit segélyszolgálat
  
Ebben az oktatóanyagban célja Azure és Jitbit segélyszolgálat integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Jitbit segélyszolgálat bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Jitbit segélyszolgálat rendelt fogja tudni az alkalmazás a Jitbit segélyszolgálat vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Jitbit segélyszolgálat engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Eset")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Az alkalmazás integrációját Jitbit segélyszolgálat engedélyezése
  
Ez a szakasz célja, hogyan Jitbit segélyszolgálat az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Jitbit segélyszolgálat**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje be az **IT-részleghez Jitbit**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Jitbit segélyszolgálat hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD. .  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

>[AZURE.IMPORTANT] Ahhoz, hogy a tartományi egyszeri bejelentkezés az IT-részleghez Jitbit bérlői webhelyen, akkor kapcsolatba kell lépnie először a Jitbit segélyszolgálat technikai támogatási kérjen ezzel a funkcióval.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Jitbit segélyszolgálat** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az IT-részleghez Jitbit felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon **Jitbit segélyszolgálat bejelentkezési az URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Jitbit.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Állítsa be az app URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Jitbit segélyszolgálat a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Jitbit Helpdesk.cer**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Jitbit segélyszolgálat vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztár a képernyő tetején, a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Felügyelete")

7.  Kattintson **az általános beállítások**gombra.

    ![Felhasználók, a vállalatok és engedélyek] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Felhasználók, a vállalatok és engedélyek")

8.  **Hitelesítési beállítások** konfigurálása szakaszban tegye a következőket:

    ![Hitelesítési beállítások] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Hitelesítési beállítások")

    1.  Jelölje ki a **engedélyezése SAML 2.0-s egyszeri bejelentkezés** bejelentkezés egyszeri bejelentkezés (SSO) használata **OneLogin**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Jitbit segélyszolgálat** párbeszéd lapon másolja a **Service Provider (SP) által kezdeményezett végpont** értéket, és illessze be a **Végpont URL-címe** mezőben lévő értéket.
    3.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása
        
        >[AZURE.TIP]További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Nyissa meg az alap-64 kódolt tanúsítvány, azt tartalmát a vágólapra másolja és illessze be a **X.509-es tanúsítvány** mezőben lévő értéket szeretné
    5.  A **módosítások mentése**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be az IT-részleghez Jitbit, hogy ki kell építenie Jitbit segélyszolgálat be.  
Az IT-részleghez Jitbit, esetében kézi tevékenység kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **IT-részleghez Jitbit** bérlői webhelyen.

2.  A felső sávon kattintson a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Felügyelete")

3.  Kattintson a **felhasználók, a vállalatok és az engedélyek**parancsra.

    ![Felhasználók, a vállalatok és engedélyek] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Felhasználók, a vállalatok és engedélyek")

4.  Kattintson a **felhasználó hozzáadása**gombra.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Felhasználó hozzáadása")

5.  A létrehozás csoportban írja be azt szeretné, hogy be az alábbi szövegdobozok kiépítése Azure AD-fiók adatait: **Username**, az **e-mailek**, az **Utónév**, a **Vezetéknév**

    ![Hozzon létre] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Hozzon létre")

6.  Kattintson a **létrehozása**gombra.

>[AZURE.NOTE] Bármely más Jitbit segélyszolgálat felhasználói fiók létrehozása eszközöket is használhatja, illetve Jitbit segélyszolgálat rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Felhasználók hozzárendelése Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Jitbit segélyszolgálat **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).