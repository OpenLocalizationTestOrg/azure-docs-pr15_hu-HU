

Leküldéses értesítések Apple leküldéses értesítést szolgáltatás (APNS) – az alkalmazás-nyilvántartást, létre kell hoznia egy új push certificate, alkalmazás azonosítója és a projekt kiépítési profil Apple developer Portal. Az alkalmazás azonosítója, a beállítások, amelyek lehetővé teszik az alkalmazás küldéséhez és fogadásához a leküldéses értesítéseket is tartalmaz. Ezeket a beállításokat a leküldéses értesítést tanúsítvány szükséges hitelesítést végezni az Apple leküldéses értesítést szolgáltatás (APNS) küldéséhez és fogadásához a leküldéses értesítéseket fogja tartalmazni. Részletesebben olvashat az dokumentációjában hivatalos [Apple leküldéses értesítéseket kezelő szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkId=272584) .


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>A tanúsítvány aláírási kérése fájl a leküldéses tanúsítvány létrehozása

Ezeket a lépéseket végigvezetik Önt a tanúsítvány-aláírási kérést létrehozása. Ez a leküldéses tanúsítványt az APN használandó generálni lesz.

1. Mac-gépen, futtassa a kulcslánc Access eszközt. A **segédprogramok** mappát vagy a **többi** mappából, kattintson az Indítás parancsot is megnyitható.

2. Kattintson **a kulcslánc Access**, bontsa ki a **Tanúsítvány Segéd**, majd **tanúsítványszolgáltatótól... tanúsítvány kérése**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Jelölje ki a **Felhasználó E-mail címét** és a **Gyakori nevét** , győződjön meg arról, hogy **lemezre mentése** legyen kijelölve, és kattintson a **Folytatás**gombra. Üresen hagyja a **Hitelesítésszolgáltató E-mail cím** mezőt, mert nem szükséges.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Írja be a tanúsítvány-aláírási kérése (CSR) fájl nevét a **Mentés másként**, jelölje ki a helyet, **ahová**a, majd kattintson a **Mentés**gombra.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    A kijelölt helyre; ez menti a CSR-fájl az alapértelmezett helye az asztalon. Ne feledje, hogy a választott a fájl helyét.


####<a name="register-your-app-for-push-notifications"></a>Regisztráljon az alkalmazás a leküldéses értesítések

Az alkalmazás új Explicit alkalmazás azonosító létrehozása az Apple, és is a leküldéses értesítések beállítása.  

1. Nyissa meg az [iOS létesítése portál](http://go.microsoft.com/fwlink/p/?LinkId=272456) az Apple Developer Center a jelentkezzen be az Apple ID-, a **azonosítók**, kattintson a, majd kattintson az **Appban azonosítóit**, és végül kattintson a a **+** jelentkezzen be az új alkalmazás regisztrálni.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Az új alkalmazás a következő három mezők frissítése, és kattintson a **Folytatás**gombra:

    * **Név**: írja be egy jól értelmezhető nevet az alkalmazás az **Alkalmazás azonosítója leírás** csoport **neve** mezőben.

    * **Az első lépésekhez azonosító**: a **Közvetlen alkalmazás azonosítója** csoportban írja be **Az első lépésekhez azonosító** formájában `<Organization Identifier>.<Product Name>` említett az [Alkalmazás telepítési útmutatója](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Ezt meg kell egyezniük, mi is használják a XCode, Xamarin vagy Cordova projekt az alkalmazás.

    * **Leküldéses értesítések**: a **Leküldéses értesítések** beállítást, az **Alkalmazás** szakaszában, jelölje be.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  A jóváhagyás az alkalmazás azonosítója képernyőn tekintse át a beállítást, és ellenőrzését követően őket a **Küldés** gombra

4.  Miután elküldte az új alkalmazás azonosítója, a **regisztrációs teljes** képernyő jelenik meg. Kattintson a **kész**gombra.

5. A fejlesztői központ Appban azonosítóit keresse meg az imént létrehozott, és kattintson a sorban a alkalmazás azonosítója. Kattintás az alkalmazás azonosítója sor jelenik meg az app adatait. Kattintson a **Szerkesztés** gombra a képernyő alján.

6. Görgessen a képernyő aljára, és a **Fejlesztés leküldéses SSL-tanúsítvány**szakaszban a **Tanúsítvány létrehozása** gombra.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Ekkor megjelenik a "IOS tanúsítvány hozzáadása" segéd.

    > [AZURE.NOTE] Ebben az oktatóprogramban egy fejlesztés tanúsítványt használja. Ugyanezt az eljárást használatos egy gyártási tanúsítvány regisztrálásakor. Önnek elég gondoskodnia arról értesítések küldésekor ugyanazt a tanúsítvány típusa használt-e.

7. **Válassza a fájl**gombra, tallózással keresse meg azt a helyet, amelybe mentette a CSR a leküldéses tanúsítvány. Ezután kattintson a **Létrehozás**gombra.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. A tanúsítvány a portálja által létrehozását követően kattintson a **Letöltés** gombra.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Ez az aláíró tanúsítvány letöltések, és menti a fájlt a számítógépen a letöltések mappában.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Alapértelmezés szerint a letöltött fájl fejlesztési tanúsítvány nevű **aps_development.cer**.

9. Kattintson duplán a letöltött push certificate **aps_development.cer**. Ezzel telepíti az új tanúsítvány a Kulcsláncból, az alább látható módon:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] A tanúsítvány neve eltérő lehet, de a rendszer előtaggal **fejlesztési Apple iOS leküldéses szolgáltatások:**.

10. A kulcslánc Access programban kattintson a jobb gombbal az új leküldéses tanúsítvány éppen létrehozott **tanúsítványok** kategóriájában. Kattintson az **Exportálás**gombra, a fájl nevét, és válassza a **.p12** formátumot, kattintson a **Mentés**gombra.

    Ne feledje, hogy a fájl nevét és helyét a exportált .p12 leküldéses tanúsítvány. Az APN-hitelesítés engedélyezése az Azure klasszikus portál feltöltésével lesz.



####<a name="create-a-provisioning-profile-for-the-app"></a>Az alkalmazás kiépítési profil létrehozása

1. Vissza az <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS létesítése portálon</a>válassza a **Kiépítési profilokat**, válassza a **mind**lehetőséget, és kattintson a **+** gombra kattintva hozzon létre egy új profilt. Ekkor megjelenik a **Hozzáadás iOS Provisiong profil** varázsló

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. **IOS alkalmazások fejlesztéséhez** **fejlesztés** alatt válassza a profil provisiong típusúként, és kattintson a **Tovább**gombra.


3. Ezután jelölje ki az imént létrehozott az **Alkalmazás azonosítója** legördülő listából alkalmazás azonosítója, és kattintson a **Folytatás** gombra

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. **Jelölje be a tanúsítványok** képernyőn jelölje be a kód aláírása használt fejlesztési tanúsítványt, és kattintson a **Tovább**gombra. Az aláíró tanúsítvány, ne az imént létrehozott leküldéses tanúsítvány.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Ezután jelölje ki a teszteléshez használandó **eszközök** , és kattintson a **Folytatás** gombra

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Végül válassza a profil nevét a **Profilnév**, és kattintson a **Létrehozás**gombra.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
