<properties 
   pageTitle="Első lépések a REST API segítségével tó adattár |} Microsoft Azure" 
   description="Tó adattár műveletek hajthatók végre WebHDFS REST API-k használata" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Első lépések az Azure tó adattár REST API-k használata

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)

Ebben a cikkben megtanulhatja WebHDFS REST API-k és az adatok tó áruházból REST API-k használata a fiókok kezelése, valamint az Azure tó adattár formázáshoz műveletek elvégzéséhez. Azure tó adattár közzététele saját REST API-khoz fiók műveletekhez. Jó helyen jár mert tó adattár Fájlrendszerhez és Hadoop ökológiai kompatibilis, támogat WebHDFS REST API-k használata formázáshoz műveletek.

>[AZURE.NOTE] Részletes információkat a REST API tó adattár támogatása, [Azure adatok tó áruházból REST API -val – segédlet](https://msdn.microsoft.com/library/mt693424.aspx)című témakörben.

## <a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Active Directory-alkalmazás létrehozása**. Az Azure Active Directory-alkalmazás segítségével az Azure Active Directory tó adattár alkalmazást hitelesítést végezni. Vannak más módszerekkel hitelesítést végezni az Azure Active Directory, amelyek **végfelhasználói** vagy **szolgáltatás hitelesítés használatával**. Utasításokat és hitelesítést végezni kapcsolatos tudnivalók című témakörben talál [tó áruházzal hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Ez a cikk cURL használja a bemutatják, hogyan készíthet egy tó adattár fiók REST API-hívások.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hogyan hitelesíteni, használja az Azure Active Directory?

Két megközelítés hitelesítést végezni az Azure Active Directory használatával is használhatja.

### <a name="end-user-authentication-interactive"></a>Végfelhasználói hitelesítés (interaktív)

Ebben az esetben az alkalmazás rákérdez a felhasználónak bejelentkezés, és az összes művelet a felhasználó környezetében történik. Interaktív hitelesítéshez, hajtsa végre az alábbi lépéseket.

1. Az alábbi URL-címet a felhasználó átirányítása a alkalmazáson keresztül:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Az ÁTIRÁNYÍTÁSI-URI > kell kell kódolva használata az URL-címet. Igen, https://localhost, használja a `https%3A%2F%2Flocalhost`)

    Céljából ebben az oktatóanyagban cserélje ki a helyőrző feletti URL-címét, és illessze be a böngésző címsorában. Az Azure login azonosítania irányítja. Miután sikeresen jelentkezzen be, a válasz a böngésző címsorában jelenik meg. A válasz lesz a következő formátumban:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Rögzítés a kérdésre adott választ engedélyezési kódot. Ebben az oktatóprogramban az engedélyezési kód másolása a böngésző címsorába, és adja át a bejegyzés kérésben a token végpont alább látható módon:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Ebben az esetben a \<ÁTIRÁNYÍTÁS-URI > nem szükséges kódolt.

3. A válasz egy JSON-objektum, amely egy hozzáférési jogkivonat tartalmazza (például `"access_token": "<ACCESS_TOKEN>"`) és a frissítési jogkivonat (például `"refresh_token": "<REFRESH_TOKEN>"`). Az alkalmazás segítségével az jogkivonat Azure tó adattár és a frissítési jogkivonat elérésekor jelenik meg egy másik hozzáférési jogkivonat, ha egy hozzáférési jogkivonat lejár.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Amikor lejár az jogkivonat, kérheti egy új jogkivonat, használja a frissítés jogkivonat, alább látható módon:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Interaktív felhasználóhitelesítés kapcsolatos további tudnivalókért olvassa el a [Adja meg az adatfolyam engedélyezési kód](https://msdn.microsoft.com/library/azure/dn645542.aspx)című témakört.

### <a name="service-to-service-authentication-non-interactive"></a>Szolgáltatás hitelesítés (nem interaktív)

Ebben az esetben a az alkalmazás biztosítja a saját hitelesítő adatok a műveletek elvégzéséhez. Ahhoz, hogy ez el kell küldenie a bejegyzés kérelmének olyanra, mint az alább látható módon. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

A kimenet a kérelem fogja tartalmazni egy jóváhagyási jogkivonat (-lel `access-token` az alábbi kimeneti), hogy később továbbítja a REST API-hívások. Mentse a hitelesítési token szövegfájlban; Ez a cikk későbbi részében lesz szüksége.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Ez a cikk használja a **nem interaktív** módon. Interaktív (szolgáltatás hívások) kapcsolatos további tudnivalókért olvassa el a [szolgáltatást, hogy a hívások hitelesítő adataival](https://msdn.microsoft.com/library/azure/dn645543.aspx)című témakört.

## <a name="create-a-data-lake-store-account"></a>Adatok tó Store-fiók létrehozása

Ehhez a művelethez a REST API alapuló definiált hívást [ide](https://msdn.microsoft.com/library/mt694078.aspx).

A következő cURL paranccsal. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

A fenti parancs cserélje le \< `REDACTED` \> a jóváhagyási jogkivonat a lekért korábbi verziójában. A parancs a kérelem tartalom megadva **input.json** fájlban található a `-d` fenti paraméter. A input.json fájl tartalmát a következőhöz hasonló:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappák létrehozása egy tó adattár fiókban

Ehhez a művelethez a WebHDFS REST API alapuló definiált hívást [ide](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

A következő cURL paranccsal. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

A fenti parancs cserélje le \< `REDACTED` \> a jóváhagyási jogkivonat a lekért korábbi verziójában. Ez a parancs **mytempdir** nevű tó adattár fiókja gyökérmappájában a hoz létre.

Ha a művelet sikeresen befejeződött választ jelennek meg kell megjelennie:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Lista-mappák egy tó adattár fiókban

Ehhez a művelethez a WebHDFS REST API alapuló definiált hívást [ide](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

A következő cURL paranccsal. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

A fenti parancs cserélje le \< `REDACTED` \> , a jóváhagyási jogkivonat lekért korábbi verziójában.

Ha a művelet sikeresen befejeződött választ jelennek meg kell megjelennie:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Töltse fel az adatok tó adattár-fiókba

Ehhez a művelethez a WebHDFS REST API alapuló definiált hívást [ide](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

A WebHDFS REST API adatfeltöltés két lépésből áll, alatti című cikkben ismertetett módon.

1. Kérelem HTTP ELHELYEZNI anélkül, hogy fel kell tölteni a fájlt adatokat küldi. A következő parancsot, a csere ** \<yourstorename >** a tó adattár nevével együtt.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Ez a parancs a kimeneti kell tartalmazni fogja ideiglenes átirányítás URL-címet, például egy alább látható módon.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Most egy másik HTTP ELHELYEZNI kérés ellen az URL-cím szerepel a listában, a válasz a **hely** tulajdonság be kell mutatnia. Csere ** \<yourstorename >** a tó adattár nevével együtt.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    A kimenet a következőhöz hasonló lesz:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Olvassa el az adatok tó adattár fiókból

Ehhez a művelethez a WebHDFS REST API alapuló definiált hívást [ide](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Adatok beolvasása egy adatok tó áruházból fiók két lépésből áll.

* Először küldje el a végpont ellen GET kérésekben `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Ez ad vissza egy helyet a következő GET kérés elküldése.
* A GET kérésekben szemben a végpont küldje `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Ekkor megjelenik a fájl tartalmát.

Jó helyen jár, mivel a bemeneti paramétereket az első és második lépésében között nincs különbség, használhatja a `-L` az első kérelem paramétert. `-L`beállítás lényegében két kérések egyesíti az egyik és az új helyre a kérelem ismételt cURL láthatóvá válik. Végül a kérelem-hívások kimenetét jelenik meg, például alábbi ábrán látható. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Fájl átnevezése a fájl egy tó adattár fiókban

Ehhez a művelethez a WebHDFS REST API alapuló definiált hívást [ide](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

A következő cURL paranccsal nevezze át a fájlt. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Törölhetők a fájlok tó adattár fiókból

Ehhez a művelethez a WebHDFS REST API alapuló definiált hívást [ide](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

A következő cURL paranccsal törölhetők a fájlok. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Tó adattár fiók törlése

Ehhez a művelethez a REST API alapuló definiált hívást [ide](https://msdn.microsoft.com/library/mt694075.aspx).

A következő cURL paranccsal tó adattár fiók törlése. Csere ** \<yourstorename >** a tó adattár nevével együtt.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Lásd még:

- [Nyissa meg a nagy forrásadatok alkalmazások kompatibilis Azure tó adattárhoz](data-lake-store-compatible-oss-other-applications.md)
 
