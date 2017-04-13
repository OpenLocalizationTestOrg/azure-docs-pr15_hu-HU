<properties
    pageTitle="Node.js várólista tárhelyet használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használhatja az Azure várólista szolgáltatást létrehozása és törlése a sorok, beszúrása, beszerzése és üzeneteket törölheti. A minta Node.js nyelven íródott."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>A Node.js várólista-tároló használata

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, használja a Microsoft Azure várólista szolgáltatást. A minták kerülnek a Node.js API. Az érintett esetek **beszúrása**, **Bepillantás**, **az első**és **Törlés,** üzenetek, valamint **létrehozásáról és a sorok törlése**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása

Hozzon létre egy üres Node.js alkalmazást. Egy Node.js-alkalmazás létrehozása című témakör nyújt tájékoztatást [létrehozása az Azure alkalmazás szolgáltatás Node.js webalkalmazást] [létre és helyezhetnek üzembe a Node.js alkalmazások, ha egy felhőalapú szolgáltatásba, Azure] használja a Windows PowerShell, és a [építse fel és telepítse az Azure használata a webes mátrix Node.js webalkalmazást].

## <a name="configure-your-application-to-access-storage"></a>Állítson be az Access-tárolóhoz

Azure tároló használatához szükséges az Azure tároló SDK Node.js egy sor olyan kényelmesebbé olyan tárat, amellyel kommunikálni a tároló többi szolgáltatást tartalmazó.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>A csomag beszerzése csomópont csomag Manager (NPM) használatával

1.  Használja a parancssor **PowerShell** (Windows), például **Terminálszolgáltatások** (Mac), illetve **Bash** (Unix), nyissa meg azt a mappát, amelyen létrehozta a minta alkalmazást.

2.  Írja be a **npm telepítse az azure-tárhelyet** a parancssorablakban. A parancs az alábbi példa hasonlít.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Manuális futtatását is lehetővé teszi annak ellenőrzésére, hogy az **ls** parancs a **csomópont\_modulok** mappa létrehozásának. Adott mappában találja az **azure-tároló** csomagra, mely tartalmazza a el szeretné érni tároló tárakat.

### <a name="import-the-package"></a>A csomag importálása

Hol tároló használni kívánt **server.js** fájlt az alkalmazás tetején Jegyzettömb vagy egy másik szövegszerkesztőben használata esetén adja hozzá a következő:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Az Azure tárterület-kapcsolat beállítása

Az azure modul felirat jelenik meg a környezeti változók AZURE\_tároló\_fiók és AZURE\_tároló\_ACCESS\_BILLENTYŰT, vagy az AZURE\_tároló\_kapcsolat\_karakterlánc az Azure tároló fiókhoz való csatlakozáshoz szükséges információkat. Ha ezek a környezeti változók nincsenek beállítva, **createQueueService**hívásakor meg kell adnia a fiók adatait.

Példa a környezeti változók beállítása az [Azure portál](https://portal.azure.com) az Azure-webhelyhez [az Azure-táblából szolgáltatással Node.js webalkalmazás]talál.

## <a name="how-to-create-a-queue"></a>Útmutató: Várólista létrehozása

A következő kódot létrehoz egy **QueueService** objektumra, amely lehetővé teszi, hogy sorban várakozó használata.

    var queueSvc = azure.createQueueService();

**CreateQueueIfNotExists** módszert, amelyek a megadott várólista adja eredményül, ha már létezik vagy létrehoz egy új sor a megadott névvel, ha még nem létezik.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Ha a sor készül, `result.created` értéke igaz. Ha a sor létezik, `result.created` hamis.

### <a name="filters"></a>Szűrők

Választható szűrési műveletek végrehajtott műveletek naplóadatai **QueueService**alkalmazhatók. Szűrési műveletek is elhelyezhet, naplózás, automatikusan újra próbálkozik stb. Szűrők olyan objektumok, egy módszer a aláírással végrehajtása:

    function handle (requestOptions, next)

A módszer után hajtsa végre a előfeldolgozása az kérelem beállításai, hívja fel a "Tovább" továbbítása a következő aláírással visszahívás van szüksége:

    function (returnObject, finalCallback, next)

A visszahívási, és a returnObject (válasza a kérelmet a kiszolgálón) feldolgozás után a visszahívás meghívása Ezután ha létezik továbbra is a további szűrők feldolgozása vagy egyszerűen finalCallback egyébként kerüljenek a szolgáltatás hívás igényeknek megfelelően.

Két szűrő látható, hogy ismét logikájának megvalósítása is megjelennek az Azure SDK Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. A következő objektumot hoz létre **QueueService** , amely a **ExponentialRetryPolicyFilter**használja:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Útmutató: Az üzenet beillesztése várólista

Üzenet beszúrni várólista, hozzon létre egy új üzenetet, és vegye fel a sorban található **createMessage** módszert.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Útmutató: Bepillantás a következő üzenetbe

Az üzenet levettük várólista-e a anélkül, hogy eltávolítja a sor hívja fel a **peekMessages** módszer Bepillantás. Alapértelmezés szerint **peekMessages** betekintés egyetlen üzenetre.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

A `result` az üzenet tartalmazza.

> [AZURE.NOTE] **PeekMessages** használja, ha nincs a várakozási sorban található üzenetek nem hibát ad vissza, azonban nincsenek üzenetei visszatér.

## <a name="how-to-dequeue-the-next-message"></a>Útmutató: A következő üzenetre feldolgozásához

Egy üzenet feldolgozása egy két lépésből álló folyamat:

1. Az üzenet feldolgozásához.

2. Az üzenet törlése

Üzenet feldolgozásához, használja a **getMessages**. Ez az üzenetek láthatatlanná teszi a várakozási sorban található, így más ügyfelek nem lehet feldolgozni azokat. Miután az alkalmazás feldolgozott egy üzenetet, hívja fel a **deleteMessage** , a sor törlése. Az alábbi példában egy üzenetet kapja, majd törli azt:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Alapértelmezés szerint az üzenetet csak rejtett 30 másodpercig, amely után is látható más ügyfelek számára. Egy másik érték megadható használatával `options.visibilityTimeout` **getMessages**együtt.

> [AZURE.NOTE]
> **GetMessages** használja, ha nincs a várakozási sorban található üzenetek nem hibát ad vissza, azonban nincsenek üzenetei visszatér.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Útmutató: Az aszinkron üzenet tartalmának módosítása

Egy üzenet helyi a várakozási sorban található **updateMessage**használatával tartalmát módosíthatja. A következő példa frissíti egy üzenet szövegében:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Útmutató: További lehetőségeket üzenetmozgatót üzenetek

Két módon testre szabhatja az egy sorból üzenetek beolvasása:

* `options.numOfMessages`-Beolvashatja egy köteg az üzenetek (legfeljebb 32.)
* `options.visibilityTimeout`- Vagy lerövidítéséhez invisibility időtúllépés beállítása.

Az alábbi példában a **getMessages** módszer 15 üzenetet kérhet egy hívásban vesz részt. Minden üzenet sablonnal feldolgozásával, majd a for ciklus. Ez a módszer által visszaadott üzenetekhez öt perc értékre állítja a invisibility időtúllépés is.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Útmutató: Ismerkedés a sor hossza

A **getQueueMetadata** metaadatokat arról, hogy a várakozási sorban található, beleértve az üzenetek várakozási sorban közelítő számát adja eredményül.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Útmutató: A lista várakozási sorba

A listában az várólista beolvasásához, használja a **listQueuesSegmented**. Egy adott előtagot szerint szűrt lista lekérdezni **listQueuesSegmentedWithPrefix**használja.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Ha nem kell minden sorban várakozó, `result.continuationToken` is alkalmazható az első **listQueuesSegmented** , vagy a második paraméter **listQueuesSegmentedWithPrefix** a további eredmények beolvasásához.

## <a name="how-to-delete-a-queue"></a>Útmutató: Várólista törléséhez.

Várólista és a benne lévő összes üzenetet törléséhez hívja fel a **deleteQueue** módszer a várakozási sorban található objektum.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Törlés nélkül várólista érkező üzenetek törléséhez használja a **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Útmutató: átengedése aláírások használata

Megosztott Access aláírások (Társítások) is egy biztonságos anélkül, hogy a tárhely fióknév vagy is sorban várakozó részletes hozzáférést biztosít. Társítások gyakran használják a sorok lehetővé tevő mobilalkalmazásban üzeneteket küldhet például korlátozott hozzáférést biztosít.

Egy megbízható alkalmazást, például egy felhőalapú szolgáltatás létrehoz egy, a **QueueService**a **generateSharedAccessSignature** használatával Társítások, és nem megbízható vagy félig megbízható az alkalmazás biztosítja. Ha például mobilalkalmazásban. A Társítások jön létre egy szabályt, amely ismerteti a kezdési és befejezési dátumot, ameddig a Társítások érvényes, valamint a hozzáférési szintet a Társítások jogosult nyújtott használatával.

Az alábbi példa létrehoz egy új megosztott hozzáférési házirendet, amely lehetővé teszi, hogy a Társítások jogosult a várakozási sorban található üzenetek hozzáadásához, és az idő létrehozás után 100 perc múlva lejár.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Figyelje meg, hogy a host meg kell adni információkat is, mivel az szükséges a Társítások jogosult hozzáférni a várólista alkalommal, amikor.

Kattintson az ügyfélalkalmazás **QueueServiceWithSAS** a Társítások a várólista elleni művelet elvégzéséhez használja. Az alábbi példa a várólista csatlakozik, és létrehoz egy üzenetet.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Mivel a Társítások hozzáadása Access jött létre, ha valaki megpróbálja végzett olvasása, frissítése és törlése az üzenetek, a művelet hibát volna vissza.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák

Az Access lista (vezérlés) a hozzáférési házirend beállítása a Társítások is használhatja. Ez akkor hasznos, ha ki szeretne hozzáférni a várakozási sorban található, de egyes ügyfelek a különböző hozzáférési szükséges több ügyfél engedélyezése.

Egy vezérlés access házirendek tömbje segítségével társított egyes házirend azonosítójú működik. Ez a példa két házirendek; határozza meg. egy "felhasználó1" és "felhasználó2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Az alábbi példa az aktuális vezérlés lekérése **Várólista_neve**, majd felveszi az új házirendek **setQueueAcl**használatával. Ezt a megközelítést lehetővé teszi, hogy:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Amikor a vezérlés van megadva, majd készíthet egy Társítások házirend-azonosító alapján. Az alábbi példa létrehoz egy új biztonsági Társítások "felhasználó2":

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta várólista-tároló alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek.

-   Keresse fel az [Azure tároló csapatának blogja][].
-   Keresse fel az [Azure tároló SDK csomópont][] tárházba GitHub.

  [Azure tároló SDK csomóponthoz]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Node.js webalkalmazást létrehozása az Azure alkalmazás szolgáltatás]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Az Azure-táblából szolgáltatással node.js webalkalmazás]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Építse fel és telepítse az Azure Felhőszolgáltatásba egy Node.js alkalmazás]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
  [Létre és helyezhetnek üzembe a megfelelő Node.js web App alkalmazásban való használata a webes mátrix Azure]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
