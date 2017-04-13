<properties 
    pageTitle="Egy MongoDB protokoll támogatása DocumentDB fiók csatlakoztatása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként protokoll támogatásával az előzetes verzióhoz elérhető MongoDB egy DocumentDB fiókhoz való csatlakozáshoz. Csatlakozzon a MongoDB kapcsolati karakterláncot." 
    keywords="mongodb kapcsolat-karakterlánc"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Hogyan szeretne kapcsolódni egy DocumentDB fiók MongoDB protokoll támogatása

Megtudhatja, hogy miként MongoDB csatlakozási karakterlánc URI szabványos formátumú MongoDB protokoll támogatása az Azure DocumentDB fiókhoz való csatlakozáshoz.  

## <a name="get-the-accounts-connection-string-information"></a>A fiók kapcsolat-karakterlánc beállításai

1. Új ablakban jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. A fiók lap **Bal oldali navigációs** sávon kattintson a **Kapcsolati karakterlánc**. Nyissa meg a **Fiók lap**, hogy a Jumpbar a kattintson a **További szolgáltatások**, **DocumentDB (NoSQL)** gombra, és válassza a DocumentDB fiók MongoDB protokoll támogatása.

    ![Az összes beállítások lap képe](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. A **Kapcsolati karakterlánc adatok** lap megnyílik, és minden illesztőprogram használatának MongoDB, például előre épített kapcsolati karakterlánc a fiókhoz való csatlakozáshoz szükséges információ.

    ![A kapcsolati karakterlánc lap képe](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Kapcsolati karakterlánc vonatkozó követelmények

Fontos tudni, hogy DocumentDB támogatja a normál MongoDB a kapcsolati karakterlánc URI formátuma, pár konkrét követelmények: DocumentDB fiókok hitelesítési és a biztonságos kommunikáció SSL szükséges.  Így a kapcsolati karakterlánc formátuma:

    mongodb://username:password@host:port/[database]?ssl=true

Ha az értékeket ez karakterlánc érhetők el az a kapcsolati karakterlánc a fent látható lap.

- A username (kötelező)
    - A fióknév DocumentDB
- A jelszó (kötelező)
    - DocumentDB fiók jelszava
- A Host (kötelező)
    - Teljesen minősített tartománynév a DocumentDB fiók
- A port (kötelező)
    - 10250
- Adatbázis (nem kötelező)
    - A kapcsolat által használt alapértelmezett adatbázis
- az SSL = true (kötelező)

Például érdemes megvizsgálni a fiókot, a kapcsolati karakterlánc adatokat fent látható.  Van érvényes kapcsolati karakterláncot:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>A C# illesztőprogram összekapcsolása az MongoDB
Már említettük, mint a minden DocumentDB fiók hitelesítési és a biztonságos kommunikáció SSL szükség. Míg a MongoDB a kapcsolati karakterlánc URI formátuma támogatja az ssl = true lekérdezési karakterláncot, dolgozik együtt a MongoDB C# illesztőprogram használatát igényli MongoClientSettings objektum egy MongoClient létrehozásakor.  A fenti fiókadatok adni, a következő kódrészletet szemlélteti, hogyan lehet csatlakozni a fiókjához, és a "Tevékenységek" adatbázis használata.

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Következő lépések


- Megtudhatja, hogyan [használható MongoChef](documentdb-mongodb-mongochef.md) protokollal DocumentDB fiókkal támogatása MongoDB.
- Ismerkedjen meg DocumentDB protokollt támogató MongoDB [minták](documentdb-mongodb-samples.md).

 
