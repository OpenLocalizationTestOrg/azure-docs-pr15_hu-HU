####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Az iOS-projekt beállítása Xamarin Studio

1. Xamarin.Studio nyissa meg a **Info.plist**, és frissítse **Az első lépésekhez azonosító** az első lépésekhez azonosító, amely a korábban létrehozott az új alkalmazás azonosítójával.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Görgessen le a **Háttér megnyitása különböző módokban** , és jelölje be a **Háttér mód engedélyezése** és a **távoli értesítések** mezőbe. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Kattintson duplán a projektben a megoldási panelen kattintva nyissa meg a **Project beállításai**.

4.  Válassza a **Bejelentkezés az első lépésekhez iOS** **összeállítása**csoportban, és jelölje ki a megfelelő **identitás** és **létesítése profil** volna csak beállítása ehhez a projekthez. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Ezzel biztosíthatja, hogy a project használja az új profil kód aláírása. Dokumentáció kiépítési hivatalos Xamarin eszköz [Xamarin eszköz kiépítési]című témakör tartalmaz.

####<a name="configuring-the-ios-project-in-visual-studio"></a>Az iOS-projekt beállítása a Visual Studio

1. A Visual Studióban kattintson a jobb gombbal a projektet, és válassza a **Tulajdonságok parancsot**.

2. Az a tulajdonságokat lapok kattintson az **alkalmazás iOS** fülre, és frissítse az **azonosító** , amely a korábban létrehozott azonosítója.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. **Bejelentkezés az első lépésekhez iOS** lapján jelölje ki a megfelelő **identitás** és **létesítése profil** kellett egyszerűen beállítása ehhez a projekthez. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Ezzel biztosíthatja, hogy a project használja az új profil kód aláírása. Dokumentáció kiépítési hivatalos Xamarin eszköz [Xamarin eszköz kiépítési]című témakör tartalmaz.

4. Kattintson duplán a Info.plist engedélyeznie kell a **RemoteNotifications** háttér módokban, és nyissa meg. 



[Kiépítési Xamarin eszköz]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/