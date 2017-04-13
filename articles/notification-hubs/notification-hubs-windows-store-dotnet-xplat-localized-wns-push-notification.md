<properties
    pageTitle="Értesítés hubok honosított legfrissebb híreket oktatóprogram"
    description="Megtudhatja, hogy miként honosított legfrissebb híreket értesítések küldésére Azure értesítés hubok használatával."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Értesítés hubok segítségével honosított legfrissebb híreket küldése

> [AZURE.SELECTOR]
- [A Windows áruházból C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan a funkcióval **sablon** az Azure értesítés hubok közvetítése, nyelvi és eszköz honosított legfrissebb híreket értesítések. Ebben az oktatóanyagban együtt kezd a Windows áruházból letölthető létrehozott [Használata értesítés hubok küldése legfrissebb híreket]. Amikor elkészült, fogja tudni érdeklik kategóriák regisztrálhat, adja meg a nyelvet, amelyen az értesítéseket, és csak a kijelölt kategóriák leküldéses értesítések fogadása a választott nyelven.


Ebben az esetben a két részből áll:

- a Windows áruházból letölthető lehetővé teszi, hogy ügyfél adjon meg egy nyelvet, és más legfrissebb híreket kategóriák; előfizetés eszközök

- a háttéradatbázist az értesítéseket, használja az Azure értesítés hubok **címke** és a **sablonok** feautres szórások.



##<a name="prerequisites"></a>Előfeltételek

Meg kell már elvégezték az [Használata értesítés hubok legfrissebb híreket küldése] oktatóprogram, és a kód érhető el, mivel ez az oktatóanyag közvetlenül épít kódot.

Is van szüksége a Visual Studio 2012 vagy újabb verziója.


##<a name="template-concepts"></a>Sablon fogalmak

[Használati értesítés hubok legfrissebb híreket küldése] a beépített különböző hírek kategóriák kapcsolatos értesítések előfizetését **címkék** használt alkalmazás.
Sok alkalmazásokat, azonban több piacokon Megcélozhat és honosítási szükséges. Ez azt jelenti, hogy a tartalom az értesítés maguk kell honosított és a megfelelő színkészletet eszközök kézbesítve.
Ebben a témakörben bemutatjuk a **sablon** funkció az értesítési hubok használatáról a könnyen előadásához honosított legfrissebb híreket értesítéseket.

Megjegyzés: egy honosított értesítések küldésére módja minden címke több verziójának létrehozásához. Például angol, francia és Mandarin támogatásához volna szükséges három különböző címkék az híreket: "world_en", "world_fr", és a "world_ch". Azt szeretné majd el kell küldenie a híreket honosított verzióját minden további címkéket. Ez a témakör a sablonok elkerülése érdekében a száma korlátok, címkék között, és több üzenetet küldeni kötelező használjuk.

Magas szintű sablonok hatékonyan megadhatja, hogy egy adott eszköz egy értesítést kell kapniuk. A sablon a pontos tartalom formátumát hivatkoznia kell az alkalmazás háttéradatbázist által küldött üzenet részét képező tulajdonságok adja meg. Ebben az esetben azt tartalmazó összes támogatott nyelvek területi-felismerése nélkül üzenetet küld el:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ezután azt fogja győződjön meg arról, hogy a megfelelő tulajdonságot hivatkozó sablon regisztrálja eszközök. Egy egyszerű értesítőben szereplő üzenet szeretne Windows áruház-alkalmazás, például a következő sablont minden olyan megfelelő címkékkel regisztrálhat:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Sablonok olyan talál további információt a saját [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikkben nagyon nagy teljesítményű funkciót. 


##<a name="the-app-user-interface"></a>Az alkalmazás felhasználói felületén

Most módosítja, akkor a megszakítása hírek alkalmazás a következő témakörben [Használata értesítés hubok legfrissebb híreket küldése] küldése honosított legfrissebb híreket sablonok használatával létrehozott.

A Windows áruházból letölthető:

A MainPage.xaml, amelyet fel szeretne venni egy területi kombinált lista módosítása:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>A Windows áruházból ügyfél-alkalmazás létrehozása

1. Az értesítések osztályához területi paraméter hozzáadásának lehetőségét kínálja a *StoreCategoriesAndSubscribe* és *SubscribeToCateories* módszereket.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Figyelje meg, hogy hívja fel a *RegisterNativeAsync* módszer helyett hívása *RegisterTemplateAsync*: egy adott értesítés formátumot, amelyben a területi függ, hogy a sablon regisztrálja azt. Azt is nevezze el a sablon ("localizedWNSTemplateExample"), mert célszerű lehet egynél több sablon (például egy bejelentési értesítés) és a csempék másikkal regisztrálása, és szükség nevezhet el annak érdekében, hogy tudja frissíteni, vagy törölje őket.

    Figyelje meg, hogy az eszköz (egy, az egyes sablonok) egy eszközt több Sablonok regisztrálja az adott címkét, ha egy bejövő üzenet célba juttatása, amelyeknél a címke fog több értesítések kézbesítve. Ez a jelenség akkor hasznos, ha a logikai ugyanezt a hibaüzenetet eredményeznek több vizuális értesítések, például megjelenítő jelvény és egy bejelentési is a Windows áruház-alkalmazás rendelkezik.

2. Adja hozzá a következő módszerrel beolvasni a tárolt területi beállításait:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. A MainPage.xaml.cs, az a gomb módosítása kezelő kattintson az aktuális értékét a területi kombinált lista beolvasása, valamint képzések azt az értesítések osztály hívásnak látható módon:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Végül a App.xaml.cs fájl ellenőrizze, hogy frissíteni a `InitNotificationsAsync` beolvasni a területi és a használata, ha az előfizetés módszer:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>A háttéradatbázist honosított értesítés küldése

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Legfrissebb hírek küldése értesítés hubok használatával]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
