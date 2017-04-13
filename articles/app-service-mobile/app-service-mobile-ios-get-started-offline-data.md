<properties
    pageTitle="Kapcsolat nélküli szinkronizálás engedélyezése az Azure mobilalkalmazás (iOS)"
    description="Alkalmazás szolgáltatás Mobile-alkalmazások használata az iOS-alkalmazás offline adatok gyorsítótár és szinkronizálása"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Az iOS mobilalkalmazásának offline szinkronizálás engedélyezése

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>– Áttekintés

Ebben az oktatóprogramban az iOS Mobile-alkalmazások Azure a kapcsolat nélküli szinkronizálás szolgáltatás foglalkozik. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat. Változások tárolódnak helyi adatbázisban; Ha az eszköz vissza online állapotban, ezek a változások a rendszer szinkronizálja a távoli kódmentes.

Ha az első használatának Azure Mobile alkalmazással, először töltse ki az oktatóprogram [egy iOS-alkalmazás létrehozása]. Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, hozzá kell adnia az adatokat az access bővítmény csomagok a projekthez. További információt a kiszolgáló kiterjesztés csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd: a témakör [Offline adatok szinkronizálása az Azure Mobile-alkalmazások].

## <a name="review-sync"></a>Tekintse át az ügyfél szinkronizálási kódot.

Az ügyfél már letöltött az oktatóprogram [egy iOS-alkalmazás létrehozása] a projektben támogatása a kapcsolat nélküli szinkronizálás használata a helyi alapadatok-alapú adatbázis kódot. Ez a szakasz már az oktatóprogram kód található összefoglalását. A szolgáltatás elvi áttekintése olvassa el a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások]című témakört.

Az Azure Mobile-alkalmazások kapcsolat nélküli szinkronizálás szinkronizálási funkció lehetővé teszi a felhasználóknak helyi adatbázist másokkal, ha a network nem érhető el. Ezek a funkciók használni az alkalmazást, a szinkronizálási környezetében inicializálni `MSClient` és a helyi tárolóba hivatkozást. A táblázat keresztül majd hivatkozás a `MSSyncTable` felület.

1. Figyelje meg a tag típusú **QSTodoService.m** (cél-C) vagy a **ToDoTableViewController.swift** (Swift) `syncTable` van `MSSyncTable`. A szinkronizálási táblázat felületet, helyett használja a kapcsolat nélküli szinkronizálás `MSTable`. Szinkronizálási táblázat használata esetén az összes művelet nyissa meg a helyi tárolóba, és a távoli kódmentes explicit leküldéses és ki a műveleteket csak szinkronizált.

    Szinkronizálási táblázat mutató hivatkozás, használja a módszerrel `syncTableWithName` a `MSClient`. Kapcsolat nélküli szinkronizálás funkció eltávolításához használja `tableWithName` helyette.

2. Minden olyan tábla műveletek végrehajtása előtt inicializálni kell a helyi tárolóba. Az alábbiakban a megfelelő kódot. 
    
    **Cél-C**:
    
    Az a `QSTodoService.init` metódus:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    Az a `ToDoTableViewController.viewDidLoad` módszer:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Ezzel létrehoz egy helyi tárolóba a felületén `MSCoreDataStore`, amely szerepel a mobil alkalmazások SDK csomagjában talál. Megteheti helyette egy adjon egy másik helyi tárolóba végrehajtása a `MSSyncContextDataSource` Protocol (protokoll). 
    
    Emellett az első paraméterként `MSSyncContext` egy ütközést kezelő megadására szolgál. Mivel azt letelik `nil`, azt az ütközés sikertelen az ütközés alapértelmezett kezelő kap.
    
3. Most vegyük a tényleges szinkronizálási művelet végrehajtása, és az adatok beolvasása a távoli kódmentes.

    **Cél-C**:
    
    `syncData`először verembe küldi egyesíthető változtatás, majd a hívások `pullData` az adatok beolvasása a távoli kódmentes. Az viszont a módszerrel `pullData` lekérdezés megfelelő új adatokat kapja:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    A cél-C nyelvű a `syncData`, először hívás `pushWithCompletion` a szinkronizálási környezetben. Ez a módszer akkor tagja `MSSyncContext` (nem pedig a szinkronizálás tábla magát), mert a tartalomtípusban változásokat át az összes táblázat fog. Csak azok a rekordok helyben (CUD műveletek) révén valamilyen módon módosított küld a kiszolgálóra. Kattintson a segítő `pullData` neve, mely hívások `MSSyncTable.pullWithQuery` távoli adatok beolvasásához, és a helyi adatbázis tárolásához.
    
    A gyors verziójában nem nincs hívást `pushWithCompletion`. Ennek oka az, a leküldéses művelet nem feltétlenül szükséges. Ha minden függőben lévő változásokat az a táblázat, amely a leküldéses művelet tesz a szinkronizálási környezetben, húzás mindig problémákat leküldéses először. Ha egynél több szinkronizálási tábla, célszerű azonban leginkább hívható explicit módon leküldéses annak érdekében, hogy minden egységes kapcsolódó táblák között.
    
    A cél-C és a Swift verzióiban, a módszerrel `pullWithQuery` beolvasásához kívánja a rekordok szűréséhez lekérdezés Itt adhatja meg. Ebben a példában a lekérdezés csak az összes rekordot ad a távoli `TodoItem` táblázat.
    
    A második paraméter `pullWithQuery` egy lekérdezés azonosítója, amellyel *szinkronizálási egyre növekvő tendenciát mutat*. Növekményes szinkronizálási olvassa be a csak azokat a rekordokat módosítás az utolsó szinkronizálás, a rekord használata óta `UpdatedAt` időbélyeg (nevű `updatedAt` a a helyi tárolóba.) A lekérdezés azonosítója kell lennie, amely az alkalmazás minden logikai lekérdezés egyedi leíró karakterlánc. Lemondja a növekményes szinkronizálási, fázisban `nil` , a lekérdezés azonosítóját. Figyelje meg, hogy ez lehet esetleg nem hatékony, mivel azt beolvassa az összes rekordot minden ki művelet.

5. Cél-C alkalmazás szinkronizálja, amikor azt módosíthatja vagy adatok hozzáadása, a felhasználó a frissítés kézmozdulat hajt végre, és kattintson a launch. A gyors alkalmazás szinkronizálja a frissítés kézmozdulat végrehajtásakor és rovatok. 

Mivel az alkalmazás szinkronizálja, valahányszor adatokat módosított (cél-C), vagy amikor az alkalmazás indítása (cél-C és Swift), az alkalmazás feltételezi, hogy a felhasználó online. Egy másik szakaszban frissíteni fogjuk az alkalmazást, hogy a felhasználók szerkeszthetik, akkor is, ha legyenek kapcsolat nélkül is.

## <a name="review-core-data"></a>Tekintse át az alapvető adatmodell

Az alapvető offline adattár használata esetén szüksége adott táblák és mezők az adatmodell meghatározása. A minta alkalmazás már van ugyanerre a megfelelő formátumot, az adatmodelleket. Ebben a részben fog végigvezetjük az alábbi táblázatok és azok hogyan használhatók.

- Nyissa meg a **QSDataModel.xcdatamodeld**. Definiált – négy táblát, három a SDK által használt, és a teendő az egyik tábla elemek maguk:     *MS_TableOperations: az elemeket, amelyeket a kiszolgálóval szinkronizálni kell nyomon    * MS_TableOperationErrors: nyomon követéséhez a hibák, amelyek a kapcsolat nélküli szinkronizálás során     *MS_TableConfig: az utolsó nyomon frissíteni az utolsó szinkronizálás művelet az összes ki művelet időpontja    * TodoItem : A teendők elemek tárolásához. **A rendszer oszlopok **createdAt**, **updatedAt**és** a választható Rendszertulajdonságok.

>[AZURE.NOTE] Az Azure Mobile alkalmazások SDK fenntartja oszlopnevek az aktív "**``**". Ne használja ezt az előtagot a rendszer oszlopok eltérő bármi, egyéb esetben a oszlopnevek fogja módosítani a távoli kódmentes használata esetén.

- A kapcsolat nélküli szinkronizálás funkció használatakor, meg kell határoznia a rendszer tábláit alább látható módon.

    ### <a name="system-tables"></a>Rendszer-táblázatokban

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Attribútum  |    Típus     |
  	|----------- |   ------    |
  	| azonosító         | Egész 64  |
  	| itemId     | Karakterlánc      |
  	| Tulajdonságok | Bináris adatokat |
  	| táblázat      | Karakterlánc      |
  	| tableKind  | Egész 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Attribútum  |    Típus     |
  	|----------- |   ------    |
  	| azonosító         | Karakterlánc      |
  	| operationId | Egész 64 |
  	| Tulajdonságok | Bináris adatokat |
  	| tableKind  | Egész 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Attribútum  |    Típus     |
  	|----------- |   ------    |
  	| azonosító         | Karakterlánc      |
  	| kulcs        | Karakterlánc      |
  	| keyType    | Egész 64  |
  	| táblázat      | Karakterlánc      |
  	| érték      | Karakterlánc      |

    ### <a name="data-table"></a>Adattábla

    **TodoItem**

  	| Attribútum    |  Típus   | Megjegyzés:                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| azonosító           | Karakterlánc, a kötelezőként megjelölt  | elsődleges kulcs a távoli áruházból                            |
  	| befejezése     | Logikai érték | Teendők elem mezője                                        |
  	| szöveg         | Karakterlánc  | Teendők elem mezője                                        |
  	| createdAt | Dátum    | (nem kötelező) térképek createdAt rendszer tulajdonság         |
  	| updatedAt | Dátum    | (nem kötelező) térképek updatedAt rendszer tulajdonság         |
  	| verzió   | Karakterlánc  | (nem kötelező) használt feltárása ütközéseket, térképek verzióra |


## <a name="setup-sync"></a>Az alkalmazás szinkronizálás működésének módosítása

Ebben a részben, módosítja az alkalmazást, hogy nincs szinkronban alkalmazás indítása, vagy ha beszúrása és elemek frissítése, de csak akkor, ha a végrehajtott kézmozdulat frissítés gombjára.

**Cél-C**:

1. Ha el szeretné távolítani a hívást **viewDidLoad** módjának módosítása a **QSTodoListViewController.m**, `[self refresh]` módszer végén. Most az adatok nem szinkronizálható a kiszolgálóval alkalmazás indításkor, de inkább helyi tárolóba tartalmát.

2. A **QSTodoService.m**, módosítsa a definícióját `addItem` , hogy azt nem lehet szinkronizálni a program beszúrja az elem után. Távolítsa el a `self syncData` blokkolni, és cserélje ki a következőket:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Definíciója módosítható `completeItem` mint feljebb; Távolítsa el a Tiltás `self syncData` és a csere a következőre:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. A `viewDidLoad` **ToDoTableViewController.swift**, a Megjegyzés ki szeretné állítani a szinkronizálást a kezdőképernyőn alkalmazás e két sorra. Ez a cikk írási időben a Swift teendők alkalmazást nem frissíti a szolgáltatást, amikor valaki felveszi vagy elem, a csak az alkalmazás indítása befejeződik.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Az alkalmazás tesztelése

Ebben a részben az érvénytelen URL-kapcsolat nélküli forgatókönyv hasonlóan fog csatlakozni. Adatok elemek hozzáadásakor azok fog tartani a helyi Core adatokat tároló, de nem szinkronizálta a mobil kódmentes a.

1. A mobil App URL-cím **QSTodoService.m** módosítása érvénytelen URL-címének, és futtassa újra az alkalmazást:

    **Cél-C** QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Adja hozzá az egyes teendők elemeket. Lépjen ki a simulatort használja (vagy az alkalmazás kényszerített bezárása), majd indítsa újra. Győződjön meg arról, hogy a módosítások van már az állandó.

3. A távoli TodoItem táblázat tartalmának megtekintése:

    + Egy Node.js kódmentes válassza az [Azure-portálra](https://portal.azure.com/), és a Mobile alkalmazásban kódmentes kattintva **Egyszerűen táblázatokat** > **TodoItem** tartalmának megtekintéséhez a `TodoItem` táblázat.
    + A .NET kódmentes olvassa el a táblázatok tartalmának a SQL-például SQL Server Management Studio eszközben, vagy a többi ügyfél, például Fiddler vagy Postman.

    Ellenőrizze, hogy az új elemek *nem* szinkronizált a kiszolgálóra:

4. Módosítsa az URL-cím vissza, hogy a helyes-e a **QSTodoService.m** a, és futtassa újra az alkalmazást. Hajtsa végre a frissítés kézmozdulat lefelé a listában az elemek adatok alapján. Ekkor megjelenik a léptetőnyíl előrehaladását.

5. Az TodoItem adatok ismét megtekintése. Az új és megváltozott TodoItems jelenjenek meg.

## <a name="summary"></a>Összefoglalás

A kapcsolat nélküli szinkronizálás szolgáltatás támogatásához azt használja a `MSSyncTable` felület és a inicializált `MSClient.syncContext` egy helyi áruházzal. Ebben az esetben a helyi tárolóba volt alapadatok-alapú adatbázis.

Egy alapadatok helyi tárolóba használatakor, meg kell adnia a [helyes Rendszertulajdonságok](#review-core-data)több táblázat.

A normál CRUD műveletek Azure Mobile-appokról működik, mintha az alkalmazás továbbra is csatlakoztatva van, de az összes művelet fordulhat elő, szemben a helyi tárolóba.

Szeretett volna a helyi tárolóba szinkronizálja a kiszolgálóval, ha azt használni a `MSSyncTable.pullWithQuery`módot.


## <a name="additional-resources"></a>További források

* [Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]

* [Fedőlap cloud: Azure mobil szolgáltatások a kapcsolat nélküli szinkronizálás] \(Megjegyzés: a videó a mobil szolgáltatások, de kapcsolat nélküli szinkronizálás az Azure Mobile-alkalmazások hasonló módon működik\)

<!-- URLs. -->


[Az iOS-alkalmazás létrehozása]: app-service-mobile-ios-get-started.md
[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Felhőalapú fedőlap: Kapcsolat nélküli szinkronizálás az Azure mobil szolgáltatások]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
