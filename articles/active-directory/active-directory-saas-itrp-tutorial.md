<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ITRP |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a ITRP az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Oktatóprogram: Azure Active Directory-integráció a ITRP
  
Ebben az oktatóanyagban célja integrálása az Azure és ITRP megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   ITRP bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók ITRP rendelt fogja tudni az alkalmazás a ITRP vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját ITRP engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Eset")
##<a name="enabling-the-application-integration-for-itrp"></a>Az alkalmazás integrációját ITRP engedélyezése
  
Ez a szakasz célja, hogyan ITRP az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját ITRP, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **ITRP**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **ITRP**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy ITRP hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az ITRP konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **ITRP** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az ITRP felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe konfigurálása** lapon **ITRP bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. ITRP.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés ITRP a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\ITRP.cer**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a ITRP vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  A bal oldali navigációs területen válassza **Az egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Egyszeri bejelentkezés")

8.  Egyszeri bejelentkezés konfigurálása csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Egyszeri bejelentkezés")

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Egyszeri bejelentkezés")

    1.  Kattintson az **Engedélyezés**gombra.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a ITRP** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Távoli kijelentkezés URL-címe** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a ITRP** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **SAML egyszeri bejelentkezési URL-címe** mezőben lévő értéket.
    4.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Tanúsítvány ujjlenyomat** mezőben lévő értéket.
        
        >[AZURE.TIP]További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    5.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a ITRP, hogy ki kell építenie ITRP be.  
ITRP, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **ITRP** bérlőhöz.

2.  Kattintson az eszköztár a képernyő tetején, a **rekordokat**.

    ![Rendszergazda] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Rendszergazda")

3.  Az előugró menüben válassza a **személyek**lehetőséget.

    ![Személyek] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Személyek")

4.  Kattintson az **Új személy felvétele** ("+").

    ![Rendszergazda] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Rendszergazda")

5.  Új személy hozzáadása párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Felhasználói] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Felhasználói")

    1.  Írja be a **nevét**, **E-mail** fiók érvényes AAD szeretne rendelkezni.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más ITRP felhasználói fiók létrehozása eszközöket is használhatja, illetve ITRP rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Felhasználók hozzárendelése ITRP, hajtsa végre az alábbi lépéseket:

1.  Az Azure Active Directory-portálon próba-fiók létrehozása.

2.  Kattintson a **ITRP **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).