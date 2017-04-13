<properties
    pageTitle="Csatlakozás a Xamarin.Forms alkalmazásban Azure-tárolóhoz"
    description="Képek hozzáadása a Teendők lista Xamarin.Forms mobilalkalmazás útján Azure blob-tárolóhoz"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Csatlakozás a Xamarin.Forms alkalmazásban Azure-tárolóhoz

## <a name="overview"></a>– Áttekintés

Az Azure Mobile-alkalmazások ügyfél- és kiszolgálóoldali SDK támogatja a kapcsolat nélküli szinkronizálás kapcsolatos CRUD műveletek szemben a /tables végpont strukturált adatok. Az adatok tárolása általában adatbázisban vagy hasonló áruházból, és általában adatok áruház nem tárolja nagy bináris adatokat hatékony. Egyes alkalmazások is van kapcsolódó-adatok (például blob-tárolóhoz, fájlok megosztások) máshol tárolt, és engedélyezni szeretné a /tables végpont a rekordok és más adatok között társítások létrehozása hasznos lehet.

Ez a témakör bemutatja, hogyan támogatása képek hozzáadása a Mobile-alkalmazások Teendők lista quickstart útmutató. Az oktatóprogram [Xamarin.Forms alkalmazás létrehozása]először be kell fejezni.

Ebben az oktatóanyagban tárterület-fiók létrehozása, és a kapcsolati karakterlánc hozzáadása a mobilalkalmazás kódmentes. Az új Mobile-alkalmazások típus új öröklő majd hozzáadásának `StorageController<T>` a kiszolgáló projekthez.

>[AZURE.TIP] Ebben az oktatóanyagban [companion minta](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) érhető el, amelyek telepíthető saját Azure-fiók van. 

## <a name="prerequisites"></a>Előfeltételek

* Töltse ki az [egy Xamarin.Forms-alkalmazás létrehozása] oktatóprogram, amely felsorolja az egyéb előfeltételek. Ez a cikk a kész, hogy az oktatóprogram alkalmazás használja.

>[AZURE.NOTE] Ha szeretné az első lépések Azure alkalmazás szolgáltatás, mielőtt regisztrál az Azure-fiók, nyissa meg a [Alkalmazás szolgáltatás próbálja meg](https://tryappservice.azure.com/?appServiceName=mobile). Azonnal létrehozhat egy rövid életű starter mobilalkalmazás az alkalmazás szolgáltatás – nem kötelező hitelkártya, és nincs nyilatkozatát.

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

1. Tárterület-fiók létrehozása az [Azure tároló fiók létrehozása]oktatóprogram követve. 

2. Az Azure-portálon nyissa meg az újonnan létrehozott tárterület-fiókjába, majd kattintson a **billentyűk** ikonra. Másolja a vágólapra az **elsődleges kapcsolati karakterláncot**.

3. Nyissa meg a mobilalkalmazásban kódmentes. Az **Összes beállítások** -> **Alkalmazásbeállítások** -> **Kapcsolati karakterláncot**, hozzon létre egy új kulcsot nevű `MS_AzureStorageAccountConnectionString` és az tárolás fiókjából másolt értéket. Írja be az **egyéni** használja.

## <a name="add-a-storage-controller-to-the-server"></a>A kiszolgáló egy tároló vezérlő hozzáadása

Kell egy új vezérlő hozzáadása a kiszolgáló-projektet, amely a lesz a biztonsági jogkivonat kérelmekre válaszolni az Azure tárolására, valamint egy rekordot vissza, amelyek megfelelnek fájlok listája:

- [A tároló vezérlő hozzáadása a kiszolgáló projekthez](#add-controller-code)
- [A tároló vezérlő regisztrálja útvonalak](#routes-registered)
- [Ügyfél- és kommunikáció](#client-communication)

###<a name="add-controller-code"></a>A tároló vezérlő hozzáadása a kiszolgáló projekthez

1. A Visual Studióban nyissa meg a .NET-kiszolgáló projektet. Adja hozzá a Nuget csomag [Microsoft.Azure.Mobile.Server.Files]. Győződjön meg arról, jelölje be az **előzetes tartalmazza**.

2. A Visual Studióban nyissa meg a .NET-kiszolgáló projektet. Kattintson a jobb gombbal a **vezérlők** mappát, és válassza a **Hozzáadás** -> **vezérlő** -> **Webes API-2 vezérlő - üres**. A vezérlő neve `TodoItemStorageController`.

3. Adja hozzá a következő utasítások segítségével:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Az alap osztály módosítása `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Az alábbi módszerek hozzáadása az osztály:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Frissítse a webes API konfiguráció attribútum továbbítás beállítása. **Startup.MobileApp.cs**, adja meg a következő sort a `ConfigureMobileApp()` módszer definícióját után a `config` változó:

        config.MapHttpAttributeRoutes();

7. A kiszolgáló projekt közzététele a mobilalkalmazásban kódmentes.

###<a name="routes-registered"></a>A tároló vezérlő regisztrálja útvonalak

Az új `TodoItemStorageController` elérhetővé teszi a rekord kezeli a két alszint erőforrások:

- StorageToken

    + HTTP POST: Létrehoz egy tároló token
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Kérdezi le fájlok listája a bejegyzéshez tartozó
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP törlése: Törli a fájl erőforrás-azonosító megadott fájl
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Ügyfél- és kommunikáció

Megjegyzendő, hogy `TodoItemStorageController` jelent *nem* feltöltésének vagy letöltésének blob útvonal van. Ennek oka az, a mobil ügyfélalkalmazásba kommunikáljon blob tároló *közvetlenül* annak érdekében, hogy ezek a műveletek végrehajtása után egy Társítások token (megosztott Access-aláírás) keresztül biztonságosan hozzáférhet az egy adott blob vagy tároló első. Az egy fontos építészeti tervezés, különben a program az access-tárolóhoz szeretné korlátozni méretezhetőség és elérhetőségét a mobil kódmentes. Ehelyett közvetlenül az Azure tároló útján a mobil ügyfélprogram is kihasználhatja a funkciókat, például automatikus szétválasztás és geo-eloszlás.

Aláírás átengedése a tárterület-fiókjában erőforrások delegált hozzáférést biztosít. Ez azt jelenti, hogy adhat az anélkül, hogy a fiók hívóbetűk megosztása egy ügyfél korlátozott az időt és a megadott engedélyek tartalmazó adott időszakra vonatkozóan a tárterület-fiókjában objektumok engedélyeit. További tudnivalókért olvassa el a [Ismertetése a megosztott Access aláírások]című témakört.

Az alábbi ábrán ügyfél- és kiszolgálóoldali interaktív műveletet. Egy fájl feltöltése, mielőtt az ügyfél a biztonsági jogkivonat kéri a szolgáltatásból. A szolgáltatás a tárhely kapcsolati karakterlánc használatával készítése egy új Társítások, és adja vissza az ügyfél számára. A Társítások időben korlátozott, és csak egy adott fájlra vagy tároló engedélyei korlátozza. A mobil ügyfélprogram majd használja a Társítások és az Azure tároló ügyfél SDK blob-tárolóhoz töltse fel a fájlt.

![A biztonsági jogkivonat kérése](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Kép támogatási hozzáadása az ügyfél-alkalmazás frissítése

Nyissa meg a Xamarin.Forms quickstart útmutató projekt Visual Studio vagy a Xamarin Studio. Telepíti a Nuget csomagok és a hordozható tár projekt és az iOS, Android és Windows update ügyfél projektek:

- [Nuget csomagok hozzáadása](#add-nuget)
- [IPlatform illesztő hozzáadása](#add-iplatform)
- [FileHelper osztály hozzáadása](#add-filehelper)
- [Egy fájl szinkronizálási eseménykezelő hozzáadása](#file-sync-handler)
- [TodoItemManager frissítése](#update-todoitemmanager)
- [A Részletek nézet felvétele](#add-details-view)
- [A fő nézet frissítése](#update-main-view)
- [Az Android projekt frissítése](#update-android), [iOS projekt](#update-ios), [a Windows-projekt](#update-windows)

>[AZURE.NOTE] Ebben az oktatóanyagban csak tartalmazza az Android, az iOS és a Windows áruházból platformok, nem a Windows Phone.

###<a name="add-nuget"></a>Nuget csomagok hozzáadása

Kattintson a jobb gombbal a megoldást, és válassza a **megoldások kezelése Nuget csomagjai**. A következő Nuget csomagok hozzáadása a megoldást az **összes** projekt. Ügyeljen arra, hogy ellenőrizze a **előzetes tartalmazza**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Kényelmesebbé Ez a példa a [PCLStorage] tár használ, de nem szükséges, az Azure Mobile-alkalmazások ügyfél SDK csomagjában talál.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>IPlatform illesztő hozzáadása

Hozzon létre egy új felület `IPlatform` a fő hordozható tár projekt. Ezt követi a [Xamarin.Forms DependencyService] minta betöltése a jobb oldali platform-specifikus osztály futásidőben. Platform-specifikus megvalósítás később ad hozzá, mindegyik az ügyfél projekteket.

1. Adja hozzá a következő utasítások segítségével:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. A végrehajtás lecserélése a következőre:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>FileHelper osztály hozzáadása

1. Hozzon létre egy új osztály `FileHelper` a fő hordozható tár projekt. Adja hozzá a következő utasítások segítségével:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Adja hozzá a osztály meghatározása:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Egy fájl szinkronizálási eseménykezelő hozzáadása

Hozzon létre egy új osztály `TodoItemFileSyncHandler` a fő hordozható tár projekt. Az osztály az Azure SDK arra a kód egy fájl hozzáadása vagy eltávolítása a visszahívás tartalmazza.

Az Azure mobil ügyfélprogram SDK nem tárolják bármely fájl adatok: az ügyfél SDK elindítja a példányába `IFileSyncHandler` amelyek viszont határozza meg, és hogy hogyan fájlok helyi eszközre találhatók.

1. Adja hozzá a következő utasítások segítségével:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Cserélje ki az osztályjegyzetfüzet meghatározása a következőket: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>TodoItemManager frissítése

1. A **TodoItemManager.cs**, vegye ki a megjegyzésjeleket a sor `#define OFFLINE_SYNC_ENABLED`.

2. Az **TodoItemManager.cs**, adja meg a következő utasítások segítségével:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. Konstruktorához `TodoItemManager`, adja hozzá a következő után a hívást `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Cserélje le a hívást, konstruktorának `InitializeAsync` a következőre. Ezzel biztosíthatja, hogy nincsenek visszahívást rekordjainak módosítási a a helyi tárolóba. A fájl szinkronizálási funkció ezeket a visszahívás használja a fájl szinkronizálási kezelő indításához.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. A `SyncAsync()`, adja hozzá a következő után a hívást `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Adja hozzá a következő módszerekkel `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>A Részletek nézet felvétele

Ebben a szakaszban az új Részletek nézet teendők elem ad hozzá. A nézet jön létre, ha egy felhasználó kijelöli a teendők elemet, és lehetővé teszi, hogy megjelenjen egy elemet az új kép.

1. Egy új osztály **TodoItemImage** hozzáadása a hordozható tár projekt következő végrehajtásával:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. **App.cs**szerkesztése. Cserélje le a inicializálni `MainPage` a következőre:
    
        MainPage = new NavigationPage(new TodoList());

3. Adja hozzá a következő tulajdonság **App.cs**:

        public static object UIContext { get; set; }

4. Kattintson a jobb gombbal a hordozható tár projekt, és válassza a **Hozzáadás** -> **Új elem** -> **platformok** -> **Űrlapok Xaml-lapot**. A nézet neve `TodoItemDetailsView`.

5. Nyissa meg a **TodoItemDetailsView.xaml** , és a típusának a ContentPage törzsében lecserélése a következőre:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. **TodoItemDetailsView.xaml.cs** szerkesztése, és adja hozzá a következő utasítások segítségével:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Csere végrehajtásának `TodoItemDetailsView` a következőre:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>A fő nézet frissítése 

A Részletek nézet megnyitásához, a teendők elem kijelölésekor a fő nézet frissítéséhez.

Cserélje le a végrehajtását **TodoList.xaml.cs**, `OnSelected` a következőre:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Az Android projekt frissítése

Platform-specifikus kód hozzáadása a Android projekt, beleértve a kódot-fájlok letöltéséről és használatáról a a kamera készítsen egy új képet. 

Ez a kód a Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) használ a jobb oldali platform-specifikus osztály futásidőben betöltéséhez.

1. A összetevő **Xamarin.Mobile** hozzáadása az Android projekthez.

2. Adja hozzá az új osztály `DroidPlatform` következő végrehajtását. Cserélje ki a projekt fő névtere "YourNamespace".

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **MainActivity.cs**szerkesztése. A `OnCreate`, adja hozzá a következő, a hívás előtt `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Az iOS-projekt frissítése

Platform-specifikus kód hozzáadása a iOS projekthez.

1. A összetevő **Xamarin.Mobile** hozzáadása az iOS-projektet.

2. Adja hozzá az új osztály `TouchPlatform` következő végrehajtását. Cserélje ki a projekt fő névtere "YourNamespace".

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **AppDelegate.cs** szerkesztése, és vegye ki a megjegyzésjeleket a hívást `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Frissítés a Windows-projekt

1. [A Windows 8.1 SQLite](http://go.microsoft.com/fwlink/?LinkID=716919)Visual Studio-bővítmény telepítése. További tudnivalókért lásd: az oktatóanyagot, [a Windows-alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. **Package.appxmanifest** szerkesztése, és jelölje be a **webkamera** lehetőséget.

3. Adja hozzá az új osztály `WindowsStorePlatform` következő végrehajtását. Cserélje ki a projekt fő névtere "YourNamespace".

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Összefoglalás

Ez a cikk az új fájlok támogatása a az Azure Mobile ügyfél- és kiszolgálóoldali SDK használata készült Azure tároló leírását. 

- Tárterület-fiók létrehozása, és a kapcsolati karakterlánc felvétele a mobilalkalmazásban kódmentes. Csak a kódmentes magában az Azure tároló billentyűvel: a mobil ügyfélprogram Társítások jogkivonat (megosztott Access-aláírás) kér, valahányszor Azure tároló eléréséhez szükséges. Társítások tokenek Azure-tárolóban lévő kapcsolatos további információért olvassa el a [Ismertetése a megosztott Access aláírások]című témakört.

- Hozzon létre egy vezérlő adott alosztályok `StorageController` annak érdekében, hogy a biztonsági jogkivonat kérések kezelésére, valamint a rekord társított fájlokat. Alapértelmezés szerint fájlok társítva egy rekordot a rekord azonosítójával részeként a tároló neve; a vezérlő viselkedése megvalósítása megadásával testre szabható `IContainerNameResolver`. A biztonsági jogkivonat házirend testre is szabható.

- Ne tárolja az Azure mobil ügyfélprogram SDK ténylegesen bármely fájl adattárolásra. Inkább az ügyfél SDK elindítja az a `IFileSyncHandler`, mely akkor úgy dönt, hogy hogyan (és ha) fájlok helyi eszközre vannak tárolva. A szinkronizálási kezelő regisztrált az alábbi képlettel történik:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`Ha az Azure mobil ügyfélprogram SDK van szüksége az adatok (például a feltöltés folyamat részeként) neve. Ez lehetővé teszi lehetővé teszi kezelése hogyan (és ha) fájlok helyi eszközre vannak tárolva, és ezeket az információkat szükség esetén vissza.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`a fájl szinkronizálási folyamat részeként meghívott. Egy hivatkozást, és FileSynchronizationAction számbavétel érték találhatók, így eldöntheti, hogy az alkalmazás hogyan kezelje az esemény (például amikor létrejön vagy frissül automatikusan letölti egy fájlt, a fájlok törlését a helyi eszközről, hogy a fájl kiszolgálói törlésekor).

- A `MobileServiceFile` lehet használt online vagy offline módban, akár egy `IMobileServiceTable` vagy `IMobileServiceSyncTable`, illetve. Kapcsolat nélküli esetben a feltöltés akkor fordul elő, ha az alkalmazás felhívja `PushFileChangesAsync`. Ennek hatására a kapcsolat nélküli művelet várólista feldolgoztatni; az egyes fájlművelet az Azure mobil ügyfélprogram SDK meghívják a `GetDataSource` módszer a `IFileSyncHandler` példány beolvasásához a feltöltés a fájl tartalmát.

- Annak érdekében, hogy egy elem fájlok beolvasásához, hívja fel a "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` példány. Ezt a módszert adja eredményül a megadott adatok elemhez tartozó fájlok listája. (Megjegyzés: Ez a *helyi* műveletet, és a fájlokat az objektum állapota alapul, amikor a legutóbbi szinkronizálása ad vissza. Úgy juthat az frissített fájlok listája a kiszolgálóról, érdemes kezdeményez egy szinkronizálási művelet először.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- A fájl szinkronizálási funkció annak érdekében, hogy a leküldéses vagy ki során az ügyfél a fogadott rekordjával rekord módosítása értesítések használja a helyi tárolóba. Ez a szinkronizálási helyi helyi és kiszolgáló értesítések bekapcsolásával érhető el a `StoreTrackingOptions` paraméter. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Más áruházból nyomon követés lehetőségeket lehet elérni, például csak helyi, vagy csak kiszolgáló értesítéseket. Felvehet és saját egyéni visszahívási használata a `EventManager` tulajdonsága `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Ajánlatos hozzáadása vagy eltávolítása a fájlok rekordból módosításával blob-tárolóhoz közvetlenül, mivel a társítás elnevezési keresztül érhető el. Ebben az esetben azonban mindig **frissítse a rekordot időbélyegző, a társított BLOB módosításakor**. Az Azure Mobile ügyfél SDK mindig frissíti egy rekordot, amikor hozzáadása vagy eltávolítása egy fájlt. 

    Ez a követelmény oka, hogy bizonyos mobilügyfelek már van a rekord helyi tároló. Ezek az ügyfelek egy növekményes ki hajthatja végre, amikor nem lesznek visszaadva ezt a rekordot, és az ügyfél nem fog lekérdezhetők az új hozzájuk kapcsolódó fájlokat. Ez a probléma elkerülése érdekében javasolt a rekord időbélyegző frissítése, mely nem használja az Azure Mobile ügyfél SDK blob tároló változásokról végrehajtásakor.

- Az ügyfél projekt betöltése a jobb oldali platform-specifikus osztály futásidőben a [Xamarin.Forms DependencyService] minta használja. Ez a példa a definiált felületet `IPlatform` az egyes platform-specifikus projektek megvalósítás.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Xamarin.Forms alkalmazás létrehozása]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[A megosztott Access aláírások ismertetése]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Azure tároló fiók létrehozása]:  ../storage/storage-create-storage-account.md#create-a-storage-account
