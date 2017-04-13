<properties
    pageTitle="Értesítés hubok segítségével legfrissebb híreket (Windows Phone) küldése"
    description="Azure értesítés hubok használatával lebonyolított címke használatával legfrissebb híreket küldése a Windows Phone-at."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Legfrissebb hírek küldése értesítés hubok használatával

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan Azure értesítés hubok használatával közvetítése legfrissebb híreket értesítések a Windows Phone 8.0-ban és 8.1 Silverlight alkalmazásba. Ha céloz meg a Windows áruházból, vagy a Windows Phone 8.1 alkalmazást, olvassa el a [Windows univerzális](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verzióra. Amikor elkészült, használni tudja regisztrálni érdeklik hírek kategóriák megszakítása, és csak a kategóriákhoz tartozó leküldéses értesítések fogadása. Ebben az esetben az sok alkalmazások közös mintájának, ahol értesítések kell el lehet küldeni a csoportok a felhasználók, amely korábban már van deklarálva érdekes őket, például az RSS-olvasó, az alkalmazások zenét ventillátortól stb.

Egy vagy több _címkék_ felvételével, az értesítési-központban regisztráció létrehozásakor közvetítési esetek engedélyezett. Értesítések fogadására egy címkét, ha az összes eszközök regisztrálta a címke a értesítést kap. Mivel a címkék egyszerűen karakterláncok, nem rendelkeznek, hogy előre be kell építenie. Címkékkel kapcsolatos további tudnivalókért olvassa el az [értesítési hubok Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Előfeltételek

Ez a témakör az alkalmazást, az [első lépések az értesítési hubok]létrehozott épül. Ebben az oktatóanyagban indításához kell már befejezése [értesítés hubok – első lépések].

##<a name="add-category-selection-to-the-app"></a>Az alkalmazás hozzáadása a kategória kiválasztása

Első lépésként a Felhasználóifelület-elemek hozzáadása, amelyek lehetővé teszik a felhasználó számára, válassza a kategóriák regisztrálhatja a meglévő fő lapon. A kategóriák a felhasználó által választott tárolják az eszközön. Az alkalmazás indításakor eszköz regisztráció címkékként a értesítés központ a kijelölt kategóriával jön létre.

1. Nyissa meg a MainPage.xaml projektfájl, majd cserélje le a **Rács** elemek nevű `TitlePanel` és `ContentPanel` kódot a következő:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. A projekt **értesítések**nevű új osztály létrehozása a **nyilvános** módosító hozzáadása a osztály definíció, majd a következő **használata** kimutatásokban hozzáadása az új kód fájl:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Az új **értesítés** osztály másolja be a következő kódot:

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Az osztály elszigetelt tárolására, hogy az eszköz fogadása hírek kategóriáinak tárolására használja. Az ezekben a kategóriákban [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítési regisztráció segítségével regisztrálhatja módszerek is tartalmaz.


4. Az **alkalmazás** osztály a következő tulajdonság hozzáadása a App.xaml.cs projektfájlban. Cserélje le a `<hub name>` és `<connection string with listen access>` helyőrzők az értesítési központi nevű, és a kapcsolati karakterláncot az *DefaultListenSharedAccessSignature* korábbi szerezte be.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Mivel a hitelesítő adatait, amelyek egy ügyfél alkalmazással nem általában biztonságos, csak kell egyenletesen meghallgatását access kulcsa az ügyfél-alkalmazást. Meghallgathatja access lehetővé teszi, hogy az értesítések, de a meglévő regisztrációk regisztrálhatja az alkalmazás nem módosíthatók, és nem lehet küldeni a értesítések. A teljes hívóbetű értesítések küldésére, és módosítása meglévő regisztrációk védett kódmentes szolgáltatás használják.

5. A MainPage.xaml.cs adja hozzá a következő parancsot:

        using Windows.UI.Popups;

6. A MainPage.xaml.cs projektfájlba adja hozzá az alábbi módon:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Ez a módszer hoz létre a kategóriák listájában, és az **értesítések** osztály használja a helyi tároló tárolni a listában, és a megfelelő címkék regisztrálása az értesítési központi. A kategóriák megváltozásakor a rendszer az új kategóriák újra a regisztráció.

Az alkalmazás már kategóriák halmazának tárolását helyi tároló az eszközre, és az értesítési-központban regisztrálása, valahányszor a felhasználó megváltoztatja a kijelölés kategóriát.

##<a name="register-for-notifications"></a>Regisztrálás az értesítésekhez

Ezeket a lépéseket az értesítési-központban helyi tároló tárolt a kategóriák használata indításkor regisztrálása.

> [AZURE.NOTE] Mivel a csatornát, a Microsoft leküldéses értesítést szolgáltatás (MPNS) rendelt URI bármikor módosíthatja, regisztrálnia kell gyakran az értesítési hibák elkerülése érdekében az értesítésekhez. Ebben a példában az értesítésekhez regisztrálja, valahányszor elindítja az alkalmazást. Gyakran futtatott alkalmazásai többször nap, valószínűleg kihagyhatja regisztrációs sávszélesség megőrzése, ha az előző regisztrációhoz óta eltelt kisebb, mint nap.


1. Nyissa meg a App.xaml.cs fájlt, és hozzáadja a **aszinkron** **Application_Launching** módszer, és cserélje le az értesítő hubok regisztráció programkódjának felvett [értesítés hubok – első lépések] a kódot a következő:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Ezzel biztosíthatja, hogy minden alkalommal, amikor elindítja az alkalmazást, a kategóriák helyi tároló és az ezekben a kategóriákban a regisztrációkor kéri.

2. Adja hozzá a következő kódot, amely a **OnNavigatedTo** módszer a MainPage.xaml.cs projektfájl:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Után kap egy megerősítést kérő arra vonatkozóan, hogy a kategóriák előfizetés befejeződött, futtassa a konzol alkalmazás egyes kategóriákat az értesítéseket küldeni. Győződjön meg arról, hogy csak a kategória előfizetett a értesítést kap.

    ![][3]

Ez a témakör befejezése.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Értesítés hubok – első lépések]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

