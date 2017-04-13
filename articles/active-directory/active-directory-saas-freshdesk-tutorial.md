<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Freshdesk |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Freshdesk az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Oktatóprogram: Azure Active Directory-integráció a Freshdesk
  
Ebben az oktatóanyagban célja integrálása az Azure és Freshdesk megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Freshdesk bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Freshdesk rendelt fogja tudni az alkalmazás a Freshdesk vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Freshdesk engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Eset")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Az alkalmazás integrációját Freshdesk engedélyezése
  
Ez a szakasz célja, hogyan Freshdesk az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Freshdesk, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Freshdesk**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Freshdesk**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Freshdesk hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Freshdesk konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Freshdesk** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Freshdesk felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe konfigurálása** lapon **Freshdesk bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Freshdesk.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Freshdesk a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Freshdesk.cer**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Freshdesk vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Rendszergazda")

7.  Az **Általános beállítások** lapon kattintson a **Biztonság**gombra.

    ![Biztonsági] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Biztonsági")

8.  A **biztonsági beállításai** szakaszban tegye a következőket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Egyszeri bejelentkezés")

    1.  **Egyszeri bejelentkezés (SSO)**jelölje be **a**.
    2.  Jelölje ki a **SAML-féle egyszeri Bejelentkezést**.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Freshdesk** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **SAML bejelentkezési URL-cím** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Freshdesk** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    5.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Biztonsági tanúsítványt ujjlenyomat** mezőben lévő értéket.  

        >[AZURE.TIP]További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    6.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Freshdesk, hogy ki kell építenie Freshdesk be.  
Freshdesk, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Freshdesk** bérlői webhelyen.

2.  A felső sávon kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Rendszergazda")

3.  Kattintson az **Általános beállítások megadása** lap **ügynökök**.

    ![Ügynökök beállítása] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Ügynökök beállítása")

4.  Kattintson az **Új ügynök**.

    ![Új ügynök] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Új ügynök")

5.  Ügynök adatai párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Ügynök adatainak] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Ügynök adatainak")

    1.  **Teljes neve** mezőbe írja be a kívánt kiépítése Azure AD-fiók nevére.
    2.  Az **E-mail** mezőbe írja be a kívánt kiépítése Azure AD-fiók e-mail címét Azure AD.
    3.  Az a **cím** mezőbe írja be a kívánt kiépítése Azure AD-fiók címét.
    4.  Jelölje ki azt **a szerepkört ügynökök**, és kattintson a **hozzárendelni**.
    5.  Kattintson a **Mentés**gombra.
    
        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos előtt aktiválva van, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó e-mailben kapja eredményül.

>[AZURE.NOTE] Bármely más Freshdesk felhasználói fiók létrehozása eszközöket is használhatja, illetve Freshdesk rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Felhasználók hozzárendelése Freshdesk, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Freshdesk **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).