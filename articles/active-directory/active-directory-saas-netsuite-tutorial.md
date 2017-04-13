<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a NetSuite |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja a NetSuite az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Oktatóprogram: Hogyan NetSuite integrálása az Azure Active Directory

Ebből az oktatóanyagból megtudhatja, miként csatlakozzon a NetSuite környezet az Azure Active Directory (Azure Active Directory). Megismerheti az egyszeri bejelentkezés NetSuite való beállításáról, hogyan engedélyezhető az automatizált felhasználói kiépítési és hozzárendelése a felhasználó hozzáférjen NetSuite. 

##<a name="prerequisites"></a>Előfeltételek

1. Azure Active Directory az [Azure klasszikus portálon](https://manage.windowsazure.com)keresztüli eléréséhez, meg kell rendelkeznie érvényes Azure előfizetést.

2. Rendszergazdai hozzáférés [NetSuite](http://www.netsuite.com/portal/home.shtml) előfizetéshez kell rendelkeznie. Ingyenes próbaverzió fiók vagy a szolgáltatás használhatók.

##<a name="step-1-add-netsuite-to-your-directory"></a>Lépés: 1: NetSuite hozzáadása a címtárhoz

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![A bal oldali navigációs ablaktáblában válassza ki az Active Directory.][0]

2. A **címtár** listából válassza ki a címtárban, amelyeket szeretne NetSuite szeretne hozzáadni.

3. Kattintson a felső menüben **alkalmazásokat** .

    ![Kattintson a alkalmazásokat.][1]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Kattintson a Hozzáadás új alkalmazás hozzáadása gombra.][2]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Kattintson a Hozzáadás a gyűjteményből az alkalmazások.][3]

6. A **Keresés mezőbe**írja be a **NetSuite**. Válasszon **NetSuite** a közül, majd kattintson a **kész** , az alkalmazás hozzáadása gombra.

    ![Adja hozzá a NetSuite.][4]

7. Az első lépések lap mostantól a NetSuite kell megjelennie:

    ![Első lépések lap NetSuite meg Azure Active Directory][5]

##<a name="step-2-enable-single-sign-on"></a>Lépés: 2: Egyszeri bejelentkezés engedélyezése

1. Azure Active Directory NetSuite, az első lépések lapján kattintson a **Konfigurálás egyszeri bejelentkezés** gombra.

    ![A beállítás egyszeri bejelentkezés gomb][6]

2. Egy párbeszédpanel nyílik meg, és látni fogja a képernyő, amely arra utasítja az "Hogyan szeretné felhasználóknak, hogy jelentkezzen be az NetSuite?" Válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Válassza az Azure Active Directory Single Sign-On][7]

    > [AZURE.NOTE] A különböző egyszeri bejelentkezéses beállítással, [kattintson ide a](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work) további tudnivalók

3. Az **Alkalmazás-beállítások megadása** lap a **Válasz** mezőbe írja be a NetSuite bérlői webhely URL-jét a következő formátumokban:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Írja be a bérlői webhely URL-címe][8]

4. A **beállítás az egyszeri bejelentkezés NetSuite a** lapon kattintson a **metaadat-alapú letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben.

    ![Töltse le a metaadatok.][9]

5. Új lap megnyitása a böngészőben, és jelentkezzen be rendszergazdaként vállalat Netsuite webhelyén.

6. Az eszköztáron kattintson a lap tetején kattintson a **telepítés**gombra, majd kattintson a **Telepítés Manager**.

    ![Nyissa meg a telepítő Manager][10]

7. A **Beállítási teendők** listából válassza ki az **Integration**.

    ![Nyissa meg a integrációja][11]

8. **Hitelesítés kezelése** csoportban kattintson a **SAML Single Sign-on**.

    ![Nyissa meg a SAML Single Sign-on][12]

9. A **SAML beállítási** lapon hajtsa végre az alábbi lépéseket:

    - Az Azure Active Directory másolja a **Távoli bejelentkezési URL-cím** értéket, és illessze be az **Identitás-szolgáltató bejelentkezési lapja** NetSuite mezőre.

        ![A SAML beállítási lapra.][13]

    - A NetSuite jelölje ki az **Elsődleges hitelesítési módszer**.

    - Jelölje ki a **IDP metaadatok fájl feltöltése**a **SAMLV2 identitás szolgáltató metaadatok**feliratú mezőbe. Kattintson a **Tallózás** feltölteni a metaadat-alapú fájlt, amelyet a #4 töltött le.

        ![A metaadatok feltöltése][16]

    - Kattintson a **Küldés**gombra.

9. Az Azure Active Directory jelölje be a feltöltött NetSuite tanúsítvány engedélyezése a egyszeri bejelentkezéses konfigurációs megerősítést kérő jelölőnégyzetet. Kattintson a **Tovább gombra**.

    ![A megerősítést kérő jelölőnégyzetet][14]

10. Az utolsó oldalra a párbeszédpanelt írja be az e-mail cím, ha szeretné, hogy a karbantartás egyszeri bejelentkezéses konfigurációt kapcsolódó útmutatója az értesítő e-mailek fogadása. 

    ![Írja be az e-mail címét.][15]

11. Kattintson a **kész** gombra a párbeszédpanel bezárásához. Ezután kattintson a **attribútumok** elemre a lap tetején.

    ![Kattintson a tulajdonságait.][17]

12. Kattintson a **felhasználó attribútum hozzáadása**.

    ![A gombra kattintva adja hozzá a felhasználó attribútumot.][18]

13. Az **Attribútum neve** mezőbe írja be a `account`. Az **Attribútumérték** mezőben írja be az NetSuite fiók azonosítójával. Keresse meg az account ID útmutatást az alábbiakban találhatók:

    ![Adja hozzá az NetSuite fiók azonosítójával.][19]

    - NetSuite kattintson **a telepítő** a felső navigációs menüben.
    - Kattintson a bal oldali navigációs menüt a **Beállítási teendők** csoportban, és válassza ki az **Integration** szakaszt **Web Services beállítások**parancsára.
    - Másolja a NetSuite Fiókazonosítót, és illessze be a **Attribútumérték** mező az Azure Active Directory.

        ![A fiók ID azonosító beszerzése][20]

14. Az Azure Active Directory kattintson a **kész** , a SAML attribútum hozzáadásának befejezéséhez. Az alsó menüben válassza a **Módosítások alkalmazása** .

    ![A módosítások mentéséhez.][21]

15. Mielőtt felhasználók hajthatják végre az egyszeri bejelentkezés NetSuite be, hogy először meg kell adni NetSuite megfelelő engedélyekkel. Kövesse ezeket az engedélyeket az alábbi utasításokat.

    - A felső navigációs menüben kattintson a **telepítés**gombra, majd kattintson a **Telepítés Manager**.

        ![Nyissa meg a telepítő Manager][10]

    - A bal oldali navigációs menüben válassza a **Felhasználók és szerepkörök**, majd kattintson a **Szerepkörök kezelése**elemre.

        ![Ugrás a szerepkörök kezelése][22]

    - Kattintson az **Új szerepkör**.

    - Írjon be egy **nevet** az új szerepkör, és jelölje be a **Egyszeri bejelentkezéses csak** jelölőnégyzetet.

        ![Adjon nevet az új szerepkör.][23]

    - Kattintson a **Mentés**gombra.

    - A felső sávon kattintson az **engedélyek**gombra. Ezután kattintson a **telepítés**gombra.

        ![Nyissa meg az engedélyek][24]

    - Jelölje ki, **állítsa be SAM Single Sign-on**, és kattintson a **Hozzáadás**gombra.

    - Kattintson a **Mentés**gombra.

    - A felső navigációs menüben kattintson a **telepítés**gombra, majd kattintson a **Telepítés Manager**.

        ![Nyissa meg a telepítő Manager][10]

    - A bal oldali navigációs menüben válassza a **Felhasználók és szerepkörök**, majd kattintson a **Felhasználók kezelése**elemre.

        ![Nyissa meg a felhasználók kezelése][25]

    - Jelölje ki a próba-felhasználó. Ezután kattintson a **Szerkesztés**gombra.

        ![Nyissa meg a felhasználók kezelése][26]

    - Szerepkörök párbeszédpanelen jelölje ki azt a szerepkört, nem hozott létre, és kattintson a **Hozzáadás**gombra.

        ![Nyissa meg a felhasználók kezelése][27]

    - Kattintson a **Mentés**gombra.

19. Tesztelje a konfigurációt, olvassa el a [Felhasználók hozzárendelése a NetSuite](#step-4-assign-users-to-netsuite)című szakaszt az alábbi című témakört.

##<a name="step-3-enable-automated-user-provisioning"></a>3 lépés: Az automatizált felhasználói kiépítési engedélyezése

> [AZURE.NOTE] Alapértelmezés szerint a kiépített felhasználók hozzáadódik a legfelső szintű leányvállalat a NetSuite környezet.

1. Azure Active Directory NetSuite, az első lépések lapján kattintson a **konfigurálás felhasználói kiépítési**.

    ![Felhasználói kiépítési konfigurálása][28]

2. A megnyíló párbeszédpanelen írja be a rendszergazdai hitelesítő adataival NetSuite, majd kattintson a **Tovább**gombra.

    ![Írja be a NetSuite rendszergazdai hitelesítő adataival.][29]

3. A Megerősítés lapon írja be az e-mail cím kiépítési sikertelen, ha értesítést szeretne kapni.

    ![Erősítse meg.][30]

4. Kattintson a **kész** gombra a párbeszédpanel bezárásához.

##<a name="step-4-assign-users-to-netsuite"></a>Lépés: 4: Felhasználók hozzárendelése NetSuite

1. Tesztelje a konfigurációt, indítsa el az új vizsgálat fiók létrehozása a címtárban.

2. Rövid útmutató NetSuite lapon kattintson a **Felhasználók hozzárendelése** gombra.

    ![Kattintson a felhasználók hozzárendelése][31]

3. Jelölje ki a próba, és a **hozzárendelése** gomb a képernyő alján kattintson:

 - Ha még nem a engedélyezése automatizált felhasználói kiépítési, akkor látni fogja a kattintva erősítse meg az alábbi üzenet:

        ![Confirm the assignment.][32]

 - Ha engedélyezte a kiépítési automatizált felhasználói, majd megjelenik egy kérdés meghatározásához, hogy milyen típusú szerepkör a felhasználónak kell rendelkeznie NetSuite. Néhány perc múlva újonnan kiépített felhasználók megjelennek a NetSuite környezetben.

4. Tesztelje a egyszeri bejelentkezéses, nyissa meg az Access panelen a [https://myapps.microsoft.com](https://myapps.microsoft.com/), jelentkezzen be a próba-fiókjába, és kattintson a **NetSuite**.

##<a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Integrálhatja a szoftver alkalmazások ismertető oktatóanyagok listája](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
