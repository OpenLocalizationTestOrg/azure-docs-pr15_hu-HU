<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a IdeaScale |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a IdeaScale az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Oktatóprogram: Azure Active Directory-integráció a IdeaScale
  
Ebben az oktatóanyagban célja integrálása az Azure és IdeaScale megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   IdeaScale egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók IdeaScale rendelt fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját IdeaScale engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Eset")
##<a name="enabling-the-application-integration-for-ideascale"></a>Az alkalmazás integrációját IdeaScale engedélyezése
  
Ez a szakasz célja, hogyan IdeaScale az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját IdeaScale, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **IdeaScale**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **IdeaScale**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy IdeaScale hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az IdeaScale konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **IdeaScale** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az IdeaScale felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési IdeaScale** mezőbe írja be a bejelentkezni a IdeaScale alkalmazás a felhasználók által használt URL-CÍMÉT (például: "*https://company.IdeaScale.com*"), és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés IdeaScale a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben metaadat parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a IdeaScale vállalati webhely rendszergazdaként.

6.  Nyissa meg a **közösségi beállítások**.

    ![Közösségi beállítások] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Közösségi beállítások")

7.  Nyissa meg a **biztonsági \> egyszeri bejelentkezés beállítások**.

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Egyszeri bejelentkezés beállításai")

8.  **Egyszeri-bejelentkezés típusa**csoportban jelölje ki a **SAML 2.0-s verziója**.

    ![Egyszeri bejelentkezés típusa] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Egyszeri bejelentkezés típusa")

9.  Az **Egyszeri bejelentkezés beállításai** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Egyszeri bejelentkezés beállításai")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a IdeaScale** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be a **SAML IdP entitás azonosítója** mezőben lévő értéket.
    2.  A tartalom a metaadat-alapú letöltött fájl másolása, és illessze be a **SAML IdP metaadatok** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a IdeaScale** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés sikeres URL-címe** mezőben lévő értéket.
    4.  A **módosítások mentése**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a IdeaScale, hogy ki kell építenie IdeaScale be.  
IdeaScale, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **IdeaScale** vállalati webhely.

2.  Nyissa meg a **közösségi beállítások**.

    ![Közösségi beállítások] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Közösségi beállítások")

3.  Nyissa meg a **alapvető beállításai \> tagok kezelése**.

4.  Kattintson a **tag hozzáadása**elemre.

    ![Tagok kezelése] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Tagok kezelése")

5.  Új tag hozzáadása csoportban hajtsa végre az alábbi lépéseket:

    ![Új tag hozzáadása] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Új tag hozzáadása")

    1.  Az **E-mail cím** mezőbe írja be a kívánt kiépítése érvényes AAD fiók.
    2.  A **módosítások mentése**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos megismerheti az e-mail megelőzően aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozással.

>[AZURE.NOTE] Bármely más IdeaScale felhasználói fiók létrehozása eszközöket is használhatja, illetve IdeaScale rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Felhasználók hozzárendelése IdeaScale, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **IdeaScale **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).