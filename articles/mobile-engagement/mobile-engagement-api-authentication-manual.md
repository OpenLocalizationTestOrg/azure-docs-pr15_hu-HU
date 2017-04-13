<properties 
    pageTitle="Mobil tetszés szerint elmélyedhet REST API - kézi beállítás a hitelesítéshez"
    description="Megtudhatja, hogy miként Mobile tetszés szerint elmélyedhet REST API-khoz hitelesítés manuális beállítása" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Mobil tetszés szerint elmélyedhet REST API - kézi beállítás a hitelesítéshez

Ez a [Mobile tetszés szerint elmélyedhet REST API-hoz való hitelesítés](mobile-engagement-api-authentication.md)egy függelék dokumentációt. Győződjön meg arról, hogy először a helyi első olvassa el. Ez azt ismerteti, hogy egy másik módja az egyszeri beállításához a Mobile tetszés szerint elmélyedhet REST API-k az Azure-portálon a hitelesítés beállítása. 

>[AZURE.NOTE] Az alábbi utasításokat az [Active Directory-ban](../resource-group-create-service-principal-portal.md) alapulnak, és igényeihez vannak szabva hitelesítési Mobile tetszés szerint elmélyedhet API-hoz szükséges. Ezért a hivatkozott Ha azt szeretné, a részletek lépéseit megértéséhez. 

1. Jelentkezzen be az Azure-fiókjába a [Klasszikus portálon](https://manage.windowsazure.com/)keresztül.

2. A bal oldali ablaktáblában válassza ki **Az Active Directory** .

     ![Jelölje ki az Active Directoryban][1]

3. Az **Active Directory alapértelmezett** válassza az Azure portálján. 

     ![Válassza a címtár][2]

    >[AZURE.IMPORTANT] Ez a módszer csak működik, ha az Active Directory-fiókját az alapértelmezett dolgozik, és nem fog működni, ha ez az Active Directoryban, Ön által létrehozott fiókjában mivel foglalkoznak. 

4. Az alkalmazások címtárában megtekintéséhez kattintson a **alkalmazások**elemre.

     ![alkalmazások megtekintése][3]

5. Kattintson a **hozzáadni**. 

     ![alkalmazás hozzáadása][4]

6. Kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**

     ![Új alkalmazás][5]

6. Töltse ki az alkalmazás nevére, és válassza ki a alkalmazást, **És/vagy WEBES API WEB APPLICATION** , és kattintson a Tovább gombra.

     ![név alkalmazás][6]

7. Bármely üres URL biztosítanak **SIGN-ON URL-cím** és az **Alkalmazás azonosítója URI**. Nem használható a forgatókönyv, és a saját maguk URL-címek érvényesítése nem volt.  

     ![szolgáltatásalkalmazás tulajdonságainak][7]

8. Végén található ez az alábbiakhoz hasonlóan a korábban megadott nevű AAD alkalmazás lesz. Ez a **AD\_alkalmazás\_neve** , és jegyezze le azt.  

     ![alkalmazás neve][8]

9. Kattintson az alkalmazás nevére, és kattintson a **beállítás**.

     ![alkalmazás beállítása][9]

10. Jegyezze fel az ügyfél-azonosító szerint használt **ügyfél\_azonosító** az API-hívások. 

     ![alkalmazás beállítása][10]

11. Görgessen lefelé a **billentyűk** szakaszig, és vegyen fel egy kulcsot lehetőleg 2 évben (lejárati) időtartammal rendelkező, és kattintson a **Mentés**gombra. 

     ![alkalmazás beállítása][11]


12. Azonnal másolja a vágólapra az értéket, amely csak akkor jelenik meg most és nem van tárolva, így nem jelenik meg bármikor újra az billentyű látható. Ha azt elveszti majd be kell új kulcs generálása. Ez lesz az **CLIENT_SECRET** a API-hívások. 

     ![alkalmazás beállítása][12]

    >[AZURE.IMPORTANT] A kulcs lejár az időtartam, így ügyeljen arra, hogy megújítani azt, amikor az idő egyébként az API-hitelesítés, hogy már nem működik a megadott végén. Is törölheti és hozza létre újból a kulcsot, ha úgy gondolja, hogy azt napvilágra kerülhetett.
 
13. Kattintson a **NÉZET VÉGPONTOK** gombra most amely nyílik meg, az **Alkalmazás végpontok** párbeszédpanel. 

    ![][13]

14. Alkalmazás végpontok párbeszédpanelen másolja a **OAUTH 2.0-s JOGKIVONAT VÉGPONTOT**. 

    ![][14]

15. A végpont a következő képernyőn esetén a a **TENANT_ID** így jegyezze le azt az URL-cím a globálisan egyedi azonosítója lesz: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Most fog a Folytatás tartományi az engedélyeket az alkalmazás. Erre kattintva nyissa meg az [Azure portál](https://portal.azure.com)fog rendelkezni. 

17. Kattintson **Az erőforrás csoportok** , és keresse meg a **Mobile tetszés szerint elmélyedhet** erőforráscsoport.  

    ![][15]

18. Kattintson a **Mobil tetszés szerint elmélyedhet** erőforráscsoport, és kattintson az ide a **Beállítások** lap. 

    ![][16]

19. Kattintson a beállítások lap a **felhasználóra** , és kattintson a **Hozzáadás gombra** kattintva vegye fel a felhasználót. 

    ![][17]

20. **Jelölje ki azt a szerepkört** gombja

    ![][18]

21. Kattintson a **tulajdonos**

    ![][19]

22. Az alkalmazás nevét a keresési **AD\_alkalmazás\_neve** a Keresés mezőbe. Nem jelenik meg ez az alábbi alapértelmezés szerint. Miután megtalálta, jelölje ki, majd kattintson a **Jelölje ki** a lap alján. 

    ![][20]

23. Kattintson a lap az **Access hozzáadása** akkor megjelenik **1 a felhasználó**, a 0 csoportok. Ez a lap a módosítás megerősítéséhez kattintson az **OK gombra** . 

    ![][21]

Most befejezése a szükséges konfigurációs AAD, és az összes beállította az API-k hívni. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



