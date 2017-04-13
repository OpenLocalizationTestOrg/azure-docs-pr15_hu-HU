<properties
    pageTitle="A Xamarin Blob-tárolóhoz használata |} Microsoft Azure"
    description="Az Azure tároló ügyfél tárat Xamarin lehetővé teszi, hogy a fejlesztők számára, hogy az eredeti kezelőfelülete iOS, Android és Windows áruház-alkalmazás létrehozása. Ebből az oktatóanyagból megtudhatja, hogy hogyan Xamarin hozhat létre új alkalmazást használó Azure Blob-tárolóhoz."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Használatáról a Xamarin Blob-tárolóhoz

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>– Áttekintés

IOS, Android és Windows áruház-alkalmazás létrehozása a saját felhasználói felülettel rendelkező Xamarin lehetővé teszi, hogy a fejlesztők használni egy megosztott C# kód helye. Ebből az oktatóanyagból megtudhatja, hogy miként, egy Xamarin alkalmazás Azure Blob-tároló használatára. Ha azt szeretné, ha többet szeretne tudni a Azure tárolására, mielőtt a kódot be van, című témakörben [a Microsoft Azure-tárterületet](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Új Xamarin-alkalmazás létrehozása

Ez az első lépések, az azt létre Android, az iOS és az Windows függvénynél alkalmazást. Az alkalmazás egyszerűen tároló létrehozása, és ez a tároló blob feltölteni. Fogjuk használni a Visual Studio a Windows-e az első lépések, de az azonos learnings Xamarin Studio segítségével a Mac OS rendszerben alkalmazás létrehozása során alkalmazható.

Kövesse ezeket a lépéseket a-alkalmazás létrehozása:

1. Ha még nem tette, töltse le és telepítse [az Visual Studio Xamarin](https://www.xamarin.com/download).
2. Nyissa meg a Visual Studióban, és hozzon létre egy üres alkalmazás (natív megosztott): **Fájl > Új > a Project > platformok > üres App(Native Shared)**.
3. Kattintson a jobb gombbal a megoldás a megoldás-tallózó panelen, és válassza a **NuGet csomagok kezelése megoldás**. Keresse meg a **WindowsAzure.Storage** , és telepítse a legújabb stabil a megoldás az összes projekt.
4. Építse fel, és futtassa a projekt.

Most már rendelkeznie kell olyan alkalmazás, amely lehetővé teszi, hogy egy gombra, amely egy számláló megnöveli.

> [AZURE.NOTE] Az Azure tároló ügyfél tárat Xamarin jelenleg támogatott az alábbi projekttípusok: natív megosztott Xamarin.Forms megosztott, Xamarin.Android és Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>A tároló létrehozása, és töltse fel a blob

Ezután jegyezze fel kód a megosztott osztály `MyClass.cs` , amely a tároló hoz létre, és blob feltölti a tárolóba. `MyClass.cs`a következő hasonlóan kell kinéznie:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Ellenőrizze, hogy a tényleges fiók nevét, és a fiókkulcs "your_account_name_here" és "your_account_key_here" lecserélése. Akkor használhatja a megosztott osztály-iOS, Android és Windows Phone alkalmazás. Egyszerűen felvehet `MyClass.createContainerAndUpload()` minden projekt. Példa:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Futtassa az alkalmazást

Most már az alkalmazás futtathatja az Android és Windows Phone irányító. Ez az alkalmazás is futtathatja az iOS irányító, de ehhez macre. Adott útmutatást a művelet olvassa el a dokumentáció [Visual Studio Macre való](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/) csatlakozáshoz

Amikor az alkalmazás futtatásához hoz létre a tároló `mycontainer` a tárterület-fiókjában. A blob, tartalmaznia kell `myblob`, amelynek van-e a szöveget, `Hello, world!`. A [Microsoft Azure tároló Explorer](http://storageexplorer.com/)használatával ellenőrizheti.

## <a name="next-steps"></a>Következő lépések

A bevezetés, egy platformok alkalmazás létrehozása az Azure tároló használó Xamarin megtanulta azt. Egy helyzetleírás Blob-tárolóban lévő összpontosít a bevezetés kifejezetten. Azonban műveleteket hajthat végre sokkal több a, nem csak a Blob-tárolóhoz, hanem a táblázat, fájl és várólista-tároló. Kérjük, olvassa el az alábbi cikkekben további információt:
- [Azure Blob-tárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-blobs.md)
- [Első lépések a .NET használatával Azure Táblatárolóhoz](storage-dotnet-how-to-use-tables.md)
- [Azure várólista-tároló .NET használata – első lépések](storage-dotnet-how-to-use-queues.md)
- [A Windows Azure-tárhelyet – első lépések](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
