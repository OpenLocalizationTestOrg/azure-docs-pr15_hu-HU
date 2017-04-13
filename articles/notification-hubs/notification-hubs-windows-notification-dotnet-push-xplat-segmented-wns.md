<properties
    pageTitle="Értesítés hubok segítségével legfrissebb híreket (Windows univerzális) küldése"
    description="A regisztráció címkék legfrissebb híreket küldhet egy univerzális Windows-alkalmazás Azure értesítés hubok használata."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Legfrissebb hírek küldése értesítés hubok használatával


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan Azure értesítés hubok használatával közvetítése legfrissebb híreket értesítések Windows Phone 8.1 (nem Silverlight) alkalmazást vagy a Windows áruházból. Ha a Windows Phone 8.1 Silverlight céloz, olvassa el a [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) -verziót. Amikor elkészült, használni tudja regisztrálni érdeklik hírek kategóriák megszakítása, és csak a kategóriákhoz tartozó leküldéses értesítések fogadása. Ebben az esetben az sok alkalmazások közös mintájának, ahol értesítések kell el lehet küldeni a csoportok felhasználóját, amely korábban van deklarált őket, RSS olvasót, például zenét ventillátortól, az alkalmazások érdeklődését, és így tovább. 

Egy vagy több _címkék_ felvételével, az értesítési-központban regisztráció létrehozásakor közvetítési esetek engedélyezett. Értesítések fogadására egy címkét, ha az összes eszközök regisztrálta a címke a értesítést kap. Mivel a címkék egyszerűen karakterláncok, nem rendelkeznek, hogy előre be kell építenie. Címkékkel kapcsolatos további tudnivalókért olvassa el az [értesítési hubok Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Előfeltételek

Ez a témakör az alkalmazást, az [első lépések az értesítési hubok]létrehozott épül[get-started]. Ebben az oktatóanyagban indításához kell már befejezése [első lépések az értesítési hubok][get-started].

##<a name="add-category-selection-to-the-app"></a>Az alkalmazás hozzáadása a kategória kiválasztása

Első lépésként a Felhasználóifelület-elemek hozzáadása, amelyek lehetővé teszik a felhasználó számára, válassza a kategóriák regisztrálhatja a meglévő fő lapon. A kategóriák a felhasználó által választott tárolják az eszközön. Az alkalmazás indításakor eszköz regisztráció címkékként a értesítés központ a kijelölt kategóriával jön létre.

1. Nyissa meg a MainPage.xaml project-fájlt, majd a **Rács** elem másolása a következő kódot:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Kattintson a jobb gombbal a **megosztott** projekt **értesítések**nevű új osztály hozzáadása a **nyilvános** módosító hozzáadása a osztály meghatározása, majd a következő **használata** kimutatásokban hozzáadása az új kód fájl:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Az új **értesítés** osztály másolja be a következő kódot:

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Az osztály a helyi tároló hírek, ez az eszköz tartalmazó fogadásához kategóriáinak tárolására használja. Figyelje meg, hogy hívja fel a *RegisterNativeAsync* módszer helyett hívása *RegisterTemplateAsync* regisztrálhatja a kategória, egy sablon regisztráció használatával. 
    
    Azt is nevezze el a sablon ("simpleWNSTemplateExample"), mert célszerű lehet egynél több sablon (például egy bejelentési értesítés) és a csempék másikkal regisztrálása, és szükség nevezhet el annak érdekében, hogy tudja frissíteni, vagy törölje őket.

    Figyelje meg, hogy az eszköz (egy, az egyes sablonok) egy eszközt több Sablonok regisztrálja az adott címkét, ha egy bejövő üzenet célba juttatása, amelyeknél a címke fog több értesítések kézbesítve. Ez a jelenség akkor hasznos, ha a logikai ugyanezt a hibaüzenetet eredményeznek több vizuális értesítések, például megjelenítő jelvény és egy bejelentési is a Windows áruház-alkalmazás rendelkezik.

    A sablonok további tudnivalókért lásd: [sablonok](notification-hubs-templates-cross-platform-push-messages.md).




4. Az **alkalmazás** osztály a következő tulajdonság hozzáadása a App.xaml.cs projektfájlba:

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Ez a tulajdonság létrehozása és egy **értesítés** példány elérése szolgál.

    A fenti kóddal cserélje le a `<hub name>` és `<connection string with listen access>` helyőrzők az értesítési központi nevű, és a kapcsolati karakterláncot az *DefaultListenSharedAccessSignature* korábbi szerezte be.

    > [AZURE.NOTE] Mivel a hitelesítő adatait, amelyek egy ügyfél alkalmazással nem általában biztonságos, csak kell terjesztése meghallgatását access kulcsa az ügyfél-alkalmazást. Meghallgathatja access lehetővé teszi, hogy az értesítések, de a meglévő regisztrációk regisztrálhatja az alkalmazás nem módosíthatók, és nem lehet küldeni a értesítések. A teljes hívóbetű értesítések küldésére, és módosítása meglévő regisztrációk védett kódmentes szolgáltatás használják.

5. A MainPage.xaml.cs adja hozzá a következő parancsot:

        using Windows.UI.Popups;

6. A MainPage.xaml.cs projektfájlba adja hozzá az alábbi módon:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Ez a módszer hoz létre a kategóriák listájában, és az **értesítések** osztály használja a helyi tároló tárolni a listában, és a megfelelő címkék regisztrálása az értesítési központi. A kategóriák megváltozásakor a rendszer az új kategóriák újra a regisztráció.

Az alkalmazás már kategóriák halmazának tárolását helyi tároló az eszközre, és az értesítési-központban regisztrálása, valahányszor a felhasználó megváltoztatja a kijelölés kategóriát.

##<a name="register-for-notifications"></a>Regisztrálás az értesítésekhez

Ezeket a lépéseket az értesítési-központban helyi tároló tárolt a kategóriák használata indításkor regisztrálása.

> [AZURE.NOTE] Mivel a csatornát, a Windows értesítési szolgáltatás (WNS) rendelt URI bármikor módosíthatja, regisztrálnia kell gyakran az értesítési hibák elkerülése érdekében az értesítésekhez. Ez a példa regisztrál az értesítés, valahányszor elindítja az alkalmazást. Gyakran futtatott alkalmazásai többször nap, valószínűleg kihagyhatja regisztrációs sávszélesség megőrzése, ha az előző regisztrációhoz óta eltelt kisebb, mint nap.

1. Nyissa meg a App.xaml.cs fájlt, és frissítse a **InitNotificationsAsync** módszert kell használni a `notifications` előfizetés osztály kategóriák alapján.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Ezzel biztosíthatja, hogy minden alkalommal, amikor elindítja az alkalmazást, a kategóriák helyi tároló és az ezekbe a kategóriákba tartozó egy registeration kéri. Az [első lépések az értesítési hubok] részeként a **InitNotificationsAsync** módszerrel készült[ get-started] oktatóprogram.

3. A MainPage.xaml.cs projektfájlba hozzá a *OnNavigatedTo* módszerrel a következő kódot:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Ezzel frissíti a korábban mentett kategóriák állapotának alapján főoldalra.

Az alkalmazás elkészült, és tárolhatja a kategóriák halmazának regisztrálása az értesítési-központban, a kategóriák kijelölés megváltozásakor használt eszköz helyi tárolására. Ezután azt határozza meg, hogy a kategória értesítéseket küldheti el ehhez az alkalmazáshoz a háttér-kiszolgálói.

##<a name="sending-tagged-notifications"></a>Címkézett értesítések küldése

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Futtassa az alkalmazást, és értesítéseket

1. A Visual Studióban nyomja le az F5 billentyűt állíthat össze, és indítsa el az alkalmazást.

    ![][1]

    Jegyzet, hogy az alkalmazás felhasználói felület biztosít be-és kikapcsolása, amellyel válassza a kategóriák előfizetéséhez.

2. Egy vagy több kategóriák megjelenítése és elrejtése engedélyezése, majd kattintson az **előfizetés**gombra.

    Az alkalmazás a kijelölt kategóriák konvertálja a címkék, és a kijelölt címkék új eszköz regisztráció kéri a értesítés központból. A bejegyzett kategóriák adja vissza, és egy párbeszédpanel jelenik meg.

    ![][19]

4. A kódmentes az új értesítés küldése a következő módszerek egyikét:

    + **Konzol alkalmazás:** indítsa el a konzol alkalmazást.

    + **Java/PHP:** alkalmazás/parancsfájl futtatására.

    A kijelölt kategória értesítések bejelentési értesítés jelenik meg.

    ![][14]

##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban legfrissebb híreket közvetítésével kategória szerint megtanulta azt. Vegye figyelembe az alábbi oktatóanyagok más speciális értesítés hubok esetben kiemelő egyik befejezése:

+ [Értesítés hubok segítségével honosított legfrissebb híreket közvetítése]

    Megtudhatja, hogy miként bontsa ki a legfrissebb híreket alkalmazás küldő honosított értesítések engedélyezése.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Értesítés hubok segítségével honosított legfrissebb híreket közvetítése]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
