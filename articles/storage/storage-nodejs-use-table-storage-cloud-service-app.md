<properties 
    pageTitle="Webalkalmazás (Node.js) táblázat adathordozós |} Microsoft Azure" 
    description="Egy oktatóprogram, amely a Web App Express oktatóprogram épül Azure tárolása és az Azure modul hozzáadásával." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>A tároló node.js webalkalmazáshoz

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban kibővíti az alkalmazás az [Express használata Node.js webalkalmazás] oktatóprogram management adatszolgáltatások dolgozhat a Microsoft Azure ügyfél képtárak Node.js használatával létrehozott lesz. Az alkalmazás Azure telepítheti webes feladatlista-alkalmazás létrehozása kiterjed. A feladatlista lehetővé teszi, hogy a felhasználó beolvasni a tevékenységek, az új tevékenységek felvétele és a feladat megjelölése befejezettként.

A tevékenység elemek Azure-tárolóban lévő tárolja. Azure tároló hibafa tervezett és könnyen hozzáférhető strukturálatlan adattárolás biztosít. Azure tároló több adatstruktúrák, ahol tárolhatja és access-adatok, és kihasználhatja a tárhely szolgáltatásai Node.js vagy REST API-khoz keresztül az Azure SDK szereplő API-khoz is tartalmaz. További tudnivalókért olvassa el a [tárolása és az adatok eléréséhez az Azure-ban]című témakört.

Ebben az oktatóanyagban feltételezi, hogy elvégezte a [Node.js webalkalmazás] és [a Express Node.js][Node.js webalkalmazás használatával Express] oktatóprogramjaihoz.

Ismerkedhet meg:

-   Hogyan dolgozhat a Jade sablon motor
-   Azure adatkezelés szolgáltatások használata

Képernyőkép a kész alkalmazást nem éri el:

![A kész lap az internet Explorerben](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Web.Config tároló hitelesítő adatok beállításával

Azure tároló eléréséhez kell a tároló hitelesítő adatok adják át. Ehhez a web.config Alkalmazásbeállítások csatlakozást.
Ezeket a beállításokat átkerülnek környezet változók csomópontot, amely majd olvassa el az Azure SDK által.

> [AZURE.NOTE] Tárhely hitelesítő adatok csak akkor használatosak, amikor az alkalmazás telepítve van az Azure. A irányító fut, amikor az alkalmazás a tárhely irányító használja.

A következő lépésekkel beolvasni a tárterület-fiók hitelesítő adatait, és vegye fel őket a web.config beállítások:

1.  Ha még nem szerepel nyissa meg az Azure PowerShell először kattintson a **Start** menü **Minden program, Azure**kibontása, kattintson a jobb gombbal az **Azure PowerShell**, és válassza a **Futtatás rendszergazdaként**.

2.  Váltás az alkalmazás tartalmazó mappát. Ha például C:\\csomópont\\tasklist\\WebRole1.

3.  Az Azure Powershell ablakban írja be a következő parancsmagot a tárhely fiókadatok beolvasása:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Ez beolvassa a tárterület-fiókok és a billentyűk a szolgáltatott szolgáltatásban társított fiókja listáját.

    > [AZURE.NOTE] Mivel az Azure SDK létrehoz egy tárterület-fiókot, ha telepíti a szolgáltatást, a tárterület-fiók léteznie kell az üzembe helyezése a korábbi segédvonalak az alkalmazást.

4.  Nyissa meg a megadott használatosak, amikor az alkalmazás telepítve van az Azure környezetben beállításokat tartalmazó **ServiceDefinition.csdef** fájlt:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Szúrja be a következő tiltása a **környezet** elemet, {tároló fiók} helyettesítése és {tároló HÍVÓBETŰ} a fiók nevét, és a telepítéshez használni kívánt az elsődleges kulcs a tárterület-fiókhoz:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![A web.cloud.config fájl tartalma](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Mentse a fájlt, és zárja be a Jegyzettömbbe.

### <a name="install-additional-modules"></a>További modul telepítése

2. A következő parancsot használja telepítéséhez [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok helyi meghajtóra, valamint másként menteni egy bejegyzés őket a **package.json** fájlt:

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Ez a parancs jelenjen meg az alábbihoz hasonló:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>A táblázat szolgáltatását használja a csomópont-alkalmazások

Ebben a szakaszban a az egyszerű alkalmazás egy **task.js** fájlt, amely tartalmazza a tevékenységekhez a modell hozzáadásával az **Expressz** paranccsal létrehozott kiterjed. Fog módosíthatja a meglévő **app.js** , és hozzon létre egy új **tasklist.js** fájlt, amely a modellt.

### <a name="create-the-model"></a>A modell létrehozása

1. A **WebRole1** címtárban **modellek**nevű új könyvtár létrehozása.

2. A **modellek** címtárban **task.js**nevű új fájl létrehozása. Ez a fájl a modellt, az alkalmazás által létrehozott tevékenységek is tartalmaz.

3. A **task.js** fájl elején adja hozzá a szükséges tárak hivatkozni szeretne, a következő kódot:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Ezután hozzáadásának meghatározása és a tevékenység-objektum exportálása kódot. Az objektum a felelős a táblázatban való csatlakozáshoz.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Ezután adja hozzá a következő kódot további módszerek definiálása a a tevékenység objektumra, amely lehetővé teszi a táblában tárolt adatok interakciók:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Mentse és zárja be a **task.js** fájlt.

### <a name="create-the-controller"></a>A vezérlő létrehozása

1. A **WebRole1/útvonalak** címtárban **tasklist.js** nevű új fájl létrehozása, és nyissa meg a szövegszerkesztőben.

2. A következő kód hozzáadása a **tasklist.js**. Az azure és aszinkron modulok, **tasklist.js**által használt betöltése. Ez a **TaskList** függvény, amely a korábban definiált **tevékenység** objektum egy példány átadott is határozza meg:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Továbbra is, hogy milyen módszerekkel **showTasks** **addTask**és **completeTasks**hozzáadásával **tasklist.js** fájl hozzáadása:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Mentse a **tasklist.js** fájlt.

### <a name="modify-appjs"></a>App.js módosítása

1. A **WebRole1** könyvtár nyissa meg a **app.js** fájlt egy szövegszerkesztőben. 

2. A fájlt az elején, adja hozzá az azure modul betöltése, és a táblázat neve és partíciót kulcs beállítása a következő:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. A app.js fájl görgessen le, ahol azt láthatja, hogy a következő parancsot:

        app.use('/', routes);
        app.use('/users', users);

    A fenti sorok cserélje le a kód alább látható módon. Ez elindítja a <strong>feladat</strong> példányát kapcsolatot a tárterület-fiókjába. Ez a <strong>TaskList</strong>, milyen azt közli a táblázat szolgáltatással átadott:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Mentse a **app.js** fájlt.

### <a name="modify-the-index-view"></a>A tárgymutató megjelenítésének módosítása

1. Váltson az könyvtárak **nézetek** , és nyissa meg a **index.jade** fájlt egy szövegszerkesztőben.

2. Az alábbi kód cserélje le a **index.jade** fájl tartalmát. A meglévő tevékenységek, valamint az új tevékenységek felvétele és a meglévőket megjelölése befejeződött űrlap megjelenítésére szolgáló nézet határozza meg.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Mentse, és zárja be a **index.jade** fájlt.

### <a name="modify-the-global-layout"></a>A globális elrendezésének módosítása

A **nézetek** címtárban a **layout.jade** fájl globális sablonként szolgál más **.jade** fájlokat. Ebben a lépésben fog módosíthatja, hogy az [Betöltő Twitter](https://github.com/twbs/bootstrap), amely egy eszközkészlet, amely megkönnyíti, egy jó megjelenésű webhelyet.

1. Töltse le és bontsa ki a fájlokat a [Twitter betöltő](http://getbootstrap.com/). Másolja át a **bootstrap.min.css** fájlt a **betöltő\\eloszlás\\css** mappát a **nyilvános\\stíluslapok** könyvtár a tasklist alkalmazás.

2. A **nézetek** mappából nyissa meg a **layout.jade** a szövegszerkesztőben, és a tartalom lecserélése a következőre:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Mentse a **layout.jade** fájlt.

### <a name="running-the-application-in-the-emulator"></a>Az alkalmazás a irányító fut

A következő parancsot használja az alkalmazás indításához kattintson a irányító.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

A böngésző nyílik meg, és a következő oldal jelenik meg:

![Egy webhely lapozható című saját feladatlista, feladatok és új feladat hozzáadására mezőket tartalmazó tábla.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Elemek hozzáadása az űrlap használatával és a meglévő elemeket befejezettként jelöli meg őket.

## <a name="publishing-the-application-to-azure"></a>Az alkalmazás Azure közzététele


A Windows PowerShell ablakában hívja fel a következő parancsmagot a szolgáltatott Azure szolgáltatás újratelepítenie.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

**Myuniquename** cserélje le egy egyedi nevet az alkalmazáshoz. **Datacentername** cserélje Azure adatközpont, például a **Nyugati USA -beli**nevét.

A telepítés befejezése után meg kell jelennie a válasz, a következőhöz hasonló:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Mint korábban mivel a megadott a **-Indítsa el** beállítást választja, a böngészőben megnyílik, és megjeleníti a közzététel, nincs befejezve Azure futó alkalmazást.

![Egy böngészőablakban, a saját feladatlista lap megjelenítéséhez. Az URL-cím azt jelzi, hogy az oldal küldése jelenlegi a Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Leállítása és az alkalmazás törlése

Miután üzembe az alkalmazásokat, érdemes letiltáshoz, így elkerülheti a költségek vagy összeállítása és más alkalmazások terjesztése ingyenes próbaverzió időszakon belül.

Azure számlák webes szerepkör-példányok felhasznált kiszolgáló idő óránkénti.
Kiszolgáló ideje elfogyasztott mennyiség megjelenítésére, amikor az alkalmazás telepíti, még akkor is, ha a példányok nem fut, és a leállítva állapotban vannak.

Az alábbi lépésekkel állítsa le, és az alkalmazás törlése módját mutatják.

1.  A Windows PowerShell ablakában megakadályozhatja, hogy a szolgáltatás telepítési, a következő parancsmagot a az előző részben létrehozott:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    A szolgáltatás leállítása eltarthat néhány percig, amíg. Ha a szolgáltatás, megjelenik egy üzenet jelzi, hogy egyáltalán nem.

3.  A szolgáltatás törléséhez hívja a következő parancsmagot:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Amikor a rendszer kéri, adja meg az **Y** kattintva törölheti a szolgáltatást.

    A szolgáltatás törlése eltarthat néhány percig, amíg. A szolgáltatás törlése után megjelenik egy üzenet jelzi, hogy törölve lett-e a szolgáltatást.

  [Express használata node.js webalkalmazáshoz]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Tárolja és Azure adatok elérése]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [NODE.js webalkalmazáshoz]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
