<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Salesforce |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja a Salesforce az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Oktatóprogram: Hogyan Salesforce integrálása az Azure Active Directory

Ebből az oktatóanyagból megtudhatja, miként csatlakozzon a Salesforce-környezet az Azure Active Directory. Megismerheti az egyszeri bejelentkezés a Salesforce való beállításáról, hogyan engedélyezhető a automatizált felhasználói kiépítési és hozzárendelése a felhasználók hozzáférjenek a Salesforce.

##<a name="prerequisites"></a>Előfeltételek

1. Azure Active Directory az [Azure klasszikus portálon](https://manage.windowsazure.com)keresztüli eléréséhez, meg kell rendelkeznie érvényes Azure előfizetést.

2. Rendelkeznie kell érvényes bérlői webhelyre a [Salesforce.com-hoz](https://www.salesforce.com/).

> [AZURE.IMPORTANT] A **próbaidőszak** Salesforce.com-fiókot használ, majd fogja nem lehet automatikus felhasználói kiépítési konfigurálása. Próbaidőszakos nem rendelkezik a szükséges API hozzáféréssel engedélyezve, amíg azokat vásárolták.
> 
> A korlátozás mozgásra oktatóprogram elvégzéséhez [ingyenes Fejlesztőeszközök fiók](https://developer.salesforce.com/signup) használatával.

Ha a Salesforce-védőfalas online-környezetben használja, tekintse meg a [Salesforce-védőfalas integrációs oktatóprogram](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Videó oktatóanyagok

Előfordulhat, hogy kövesse az ebben az oktatóprogramban az alábbi videók.

**Oktatóvideó rész: hogyan engedélyezhető az egyszeri bejelentkezés**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Oktatóvideó rész: Hogyan automatizálhatja a felhasználói kiépítése**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Lépés: 1: A könyvtár Salesforce hozzáadása

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![A bal oldali navigációs ablaktáblában válassza ki az Active Directory.][0]

2. A **címtár** listából válassza ki a könyvtár Salesforce szeretne hozzáadni kívánt.

3. Kattintson a felső menüben **alkalmazásokat** .

    ![Kattintson a alkalmazásokat.][1]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Kattintson a Hozzáadás új alkalmazás hozzáadása gombra.][2]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Kattintson a Hozzáadás a gyűjteményből az alkalmazások.][3]

6. A **Keresés mezőbe**írja be a **Salesforce**. Kattintson az eredmények közül válassza ki a **Salesforce** , és kattintson a **kész** , az alkalmazás hozzáadása gombra.

    ![Adja hozzá a Salesforce.][4]

7. Az első lépések lap mostantól a Salesforce kell megjelennie:

    ![Első lépések lap Salesforce-féle Azure Active Directory][5]

##<a name="step-2-enable-single-sign-on"></a>Lépés: 2: Egyszeri bejelentkezés engedélyezése

1. Beállíthatja, hogy az egyszeri bejelentkezés, mielőtt állíthat be, és üzembe egyéni tartományt a Salesforce-környezetben. Ennek módja, tanulmányozza [Állítsa be a tartománynevét](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Salesforce-féle Quick Start lapján az Azure Active Directory kattintson a **Konfigurálás egyszeri bejelentkezés** gombra.

    ![A beállítás egyszeri bejelentkezés gomb][6]

3. Egy párbeszédpanel nyílik meg, és látni fogja a képernyő, amely arra utasítja az "Hogyan szeretné felhasználóknak, hogy jelentkezzen be Salesforce?" Válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Válassza az Azure Active Directory Single Sign-On][7]

    > [AZURE.NOTE] A különböző egyszeri bejelentkezéses beállítással, [kattintson ide a](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work) további tudnivalók

4. Az **Alkalmazás-beállítások megadása** lap töltse ki a **Bejelentkezési a URL-cím** beírásával a Salesforce tartományi URL-CÍMÉT az alábbi formátumban:
 - Enterprise-fiók:`https://<domain>.my.salesforce.com`
 - Fejlesztőeszközök fiók:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Írjon be a bejelentkezési URL-cím][8]

5. A **beállítás az egyszeri bejelentkezés a Salesforce** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben.

    ![Tanúsítvány letöltése][9]

6. Nyissa meg a böngészőt, és a Salesforce-rendszergazdai fiókkal jelentkezzen be egy új lapon.

7. Kattintson a **rendszergazda** navigációs sávján kattintson a **Biztonsági vezérlők** a kapcsolódó szakasz kibontásához. Kattintson a **Egyszeri bejelentkezéses beállításait**.

    ![Kattintson a egyszeri bejelentkezéses beállítások az biztonsági vezérlők elemre][10]

8. **Egyszeri bejelentkezés beállítások** lapján kattintson a **Szerkesztés** gombra.

    ![Kattintson a Szerkesztés gombra.][11]

    > [AZURE.NOTE] Ha nem tudja ahhoz, hogy az egyszeri bejelentkezés a Salesforce-fiók beállításai, szükség lehet a Salesforce-féle ügyfélszolgálatát annak érdekében, hogy a szolgáltatás engedélyezve Önnél.

9. Jelölje ki a **SAML engedélyezve van**, és kattintson a **Mentés**gombra.

    ![Jelölje ki a SAML engedélyezve][12]

10. A SAML egyszeri bejelentkezéses beállításainak megadásához kattintson az **Új**gombra.

    ![Jelölje ki a SAML engedélyezve][13]

11. A **SAML egyszeri bejelentkezéses beállítás szerkesztése** lapon ellenőrizze az alábbi beállításokat:

    ![Képernyőkép a konfigurációk, gondoskodnia kell:][14]

 - A **név** mezőbe írja be egy rövid nevet, ebben a konfigurációban. Egy érték kezeléséről **nevének** automatikus feltöltéséhez a **API neve** mezőben lévő értéket.

 - Az Azure Active Directory másolja a **Kibocsátó URL-címe** értéket, és illessze be a Salesforce **kibocsátó** mezője.

 - A **szervezet azonosítója mezőbe**írja be a Salesforce-tartománynevét az alábbi minta használatával:
     - Enterprise-fiók:`https://<domain>.my.salesforce.com`
     - Fejlesztőeszközök fiók:`https://<domain>-dev-ed.my.salesforce.com`

 - Kattintson a **tallózással keresse meg** vagy **Válassza a fájl** nyissa meg a **Adja meg a feltöltendő fájlt** párbeszédpanelen, jelölje ki a Salesforce-tanúsítványt, és kattintson a **Megnyitás** a tanúsítvány feltöltése gombra.

 - **SAML azonosító típusa**csoportban jelölje ki a **állítás felhasználó salesforce.com felhasználónév tartalmazza**.

 - **SAML identitás helyét**válassza az **identitás a tárgy utasítás NameIdentifier eleme**

 - Az Azure Active Directory a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a Salesforce **Identitás szolgáltató bejelentkezési URL-cím** mezője.

 - A **Service Provider által kezdeményezett kérése kötés**jelölje ki a **HTTP átirányítást**.

 - Végül kattintson a **Mentés** a SAML egyszeri bejelentkezéses beállításait alkalmazni.

12. Kattintson a bal oldali navigációs a Salesforce kattintson a **Domain Management** bontsa ki a kapcsolódó szakaszt, és válassza a **Saját tartomány**.

    ![Kattintson a saját Tartományomban][15]

13. Görgessen le a **Hitelesítési konfigurációs** szakaszt, és kattintson a **Szerkesztés** gombra.

    ![Kattintson a Szerkesztés gombra.][16]

14. A **Hitelesítési szolgáltatás** csoportban válassza ki a SAML-féle egyszeri Bejelentkezést konfigurációs rövid nevét, és kattintson a **Mentés**gombra.

    ![Jelölje ki az egyszeri bejelentkezés konfiguráció][17]

    > [AZURE.NOTE] Ha egynél több hitelesítési szolgáltatás ki van jelölve, majd amikor a felhasználók megpróbálnak egyszeri bejelentkezés a Salesforce-környezet kezdeményezzen kéri jelölje be a milyen hitelesítési szolgáltatás szeretnének való bejelentkezéshez. Ha nem szeretné, hogy ez, majd tegye a következőt **hagyja bejelölve minden hitelesítési szolgáltatás**.

15. Az Azure Active Directory jelölje be a Salesforce feltöltött tanúsítvány engedélyezése a egyszeri bejelentkezéses konfigurációs megerősítést kérő jelölőnégyzetet. Kattintson a **Tovább gombra**.

    ![A megerősítést kérő jelölőnégyzetet][18]

16. Az utolsó oldalra a párbeszédpanelt írja be az e-mail cím Ha szeretné, hogy a karbantartás egyszeri bejelentkezéses konfigurációt kapcsolódó útmutatója az értesítő e-mailek fogadása. 

    ![Írja be az e-mail címét.][19]

17. Kattintson a **kész** gombra a párbeszédpanel bezárásához. Tesztelje a konfigurációt, olvassa el a [Felhasználók hozzárendelése a Salesforce](#step-4-assign-users-to-salesforce)című szakaszt az alábbi című témakört.

##<a name="step-3-enable-automated-user-provisioning"></a>3 lépés: Az automatizált felhasználói kiépítési engedélyezése

1. A Salesforce Azure Active Directory – rövid útmutató az első lapon kattintson a **konfigurálás felhasználói kiépítési** gombra.

    ![A felhasználó kiépítési beállítása gombra][20]

2. A **Configure felhasználói kiépítési** párbeszédpanelen írja be a Salesforce-rendszergazdai felhasználónevével és jelszavával.

    ![Írja be a rendszergazda felhasználónév vagy jelszó][21]

    > [AZURE.NOTE] Ha munkakörnyezetben állítja be, akkor célszerű új rendszergazdai fiók létrehozása a Salesforce kifejezetten az ebben a lépésben. Ehhez a fiókhoz a **Rendszergazda** profil rendelt a Salesforce kell rendelkeznie.

3. A Salesforce-biztonsági jogkivonat juthat, nyissa meg egy új lapon, és jelentkezzen be ugyanazzal a Salesforce rendszergazdai fiókkal. Kattintson a lap jobb felső sarkában kattintson a nevére, és válassza a **Saját beállításai**.

    ![Kattintson a saját nevére, majd a saját beállításai][22]

4. A bal oldali navigációs területen kattintson a **személyes** bontsa ki a kapcsolódó szakaszt, és kattintson a **Alaphelyzetbe saját biztonsági jogkivonat**gombra.

    ![Kattintson a saját nevére, majd a saját beállításai][23]

5. **Állítsa alaphelyzetbe a biztonsági jogkivonat** lapon kattintson a **Biztonsági jogkivonat alaphelyzetbe állítása** gombra.

    ![Olvassa el a figyelmeztetések.][24]

6. Jelölje be a Beérkezett üzenetek mappájának a rendszergazdai fiókhoz társított. Keresse meg a mailt, amely tartalmazza az új biztonsági jogkivonat Salesforce.com.

7. Másolja a jogkivonat, az Azure Active Directory-ablak megnyitásához, és illessze be a **Felhasználó biztonsági jogkivonat** mezőben. Kattintson a **Tovább gombra**.

    ![Illessze be a biztonsági jogkivonat][25]

8. A Megerősítés lapon megadhatja az e-mailben értesítést kapni, a kiépítési hiba esetén. Kattintson a **kész** gombra a párbeszédpanel bezárásához.

    ![Írja be az e-mail címét, ha értesítést szeretne kapni a][26]

##<a name="step-4-assign-users-to-salesforce"></a>Lépés: 4: Felhasználók hozzárendelése a Salesforce

1. Tesztelje a konfigurációt, először új vizsgálat fiók létrehozása a címtárban.

2. A Salesforce első lépések lapon kattintson a **Felhasználók hozzárendelése** gombra.

    ![Kattintson a felhasználók hozzárendelése][27]

3. Jelölje ki a próba, és a **hozzárendelése** gomb a képernyő alján kattintson:

 - Ha még nem a engedélyezése automatizált felhasználói kiépítési, akkor látni fogja a kattintva erősítse meg az alábbi üzenet:

        ![Confirm the assignment.][28]

 - Ha engedélyezte a kiépítési automatizált felhasználói, majd megjelenik egy kérdés meghatározásához, hogy milyen típusú Salesforce-profilt kell. Néhány perc múlva újonnan kiépített felhasználók megjelennek a Salesforce-környezetben.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Ha vannak kiépítési, a Salesforce **fejlesztői** környezet, az egyes profilok rendelkezésre álló licencek nagyon korlátozott számú lesz. Ezért célszerű az **Chatter ingyenes felhasználói** profilhoz, 4,999 licenccel rendelkező felhasználók létrehozására.

4. Tesztelje a egyszeri bejelentkezéses, nyissa meg az Access panelen a [https://myapps.microsoft.com](https://myapps.microsoft.com/), majd jelentkezzen be a próba-fiókjába, és kattintson a **Salesforce**.

##<a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés indexe](active-directory-apps-index.md)
- [Integrálhatja a szoftver alkalmazások ismertető oktatóanyagok listája](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
