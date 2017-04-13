<properties 
    pageTitle="Használja a MongoChef MongoDB protokoll támogatása DocumentDB fiókkal |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használható MongoChef protokoll támogatásával az előzetes verzióhoz elérhető MongoDB DocumentDB fiókkal." 
    keywords="mongochef"
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
    ms.date="08/25/2016" 
    ms.author="anhoh"/>

# <a name="use-mongochef-with-a-documentdb-account-with-protocol-support-for-mongodb"></a>MongoChef használata egy DocumentDB fiók MongoDB protokoll támogatása

Protokoll támogatása MongoChef használatával MongoDB Azure DocumentDB fiókja csatlakozik, a következőket kell tennie:

- Töltse le és telepítse a [MongoChef](http://3t.io/mongochef)
- Használja a DocumentDB fiókját protokollt támogató MongoDB [kapcsolati karakterlánc](documentdb-connect-mongodb-account.md) információt

## <a name="create-the-connection-in-mongochef"></a>A kapcsolat létrehozása a MongoChef  

Protocol (protokoll) támogatása MongoDB DocumentDB fiók hozzáadása a MongoChef kapcsolatkezelő, hajtsa végre az alábbi lépéseket.

1. A protokoll támogatásával az útmutatást követve MongoDB kapcsolatadatainak DocumentDB beolvasásához [Itt](documentdb-connect-mongodb-account.md).

    ![A kapcsolati karakterlánc lap képe](./media/documentdb-mongodb-mongochef/ConnectionStringBlade.png)

2. Kattintson a **Csatlakozás** a Kapcsolatkezelő megnyitásához, majd kattintson az **Új kapcsolat**

    ![Képernyőkép: a MongoChef kapcsolatkezelő](./media/documentdb-mongodb-mongochef/ConnectionManager.png)
    
2. Az **Új kapcsolat** ablakában a **kiszolgálója** lapon adja meg a HOST (FQDN) protokollt támogató MongoDB és a PORT DocumentDB fiók.
    
    ![A MongoChef kapcsolat kezelő kiszolgáló lap képe](./media/documentdb-mongodb-mongochef/ConnectionManagerServerTab.png)

3. Az **Új kapcsolat** ablakában a **hitelesítési** lapon válassza a **Standard (MONGODB-CR vagy SCARM-SHA-1)** hitelesítési mód, és írja be a FELHASZNÁLÓNEVÉT és JELSZAVÁT.  Fogadja el az alapértelmezett hitelesítési db (rendszergazda), vagy adja meg a saját értéket.

    ![A MongoChef kapcsolat manager hitelesítési lap képe](./media/documentdb-mongodb-mongochef/ConnectionManagerAuthenticationTab.png)

4. Az **Új kapcsolat** ablakában a **SSL** lapon jelölje be a **használja az SSL protokoll csatlakozni** jelölőnégyzetet, és az **Elfogadás önaláírt SSL-tanúsítványok** választógomb.

    ![A MongoChef kapcsolat manager SSL lap képe](./media/documentdb-mongodb-mongochef/ConnectionManagerSSLTab.png)

5. A **Kapcsolat tesztelése** gombra kattintva ellenőrizze a kapcsolat adatait, kattintson az **OK gombra** kattintva térhet vissza az új kapcsolat ablakban, és kattintson a **Mentés**.

    ![Képernyőkép: a MongoChef vizsgálatot kapcsolat ablaka](./media/documentdb-mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Hozzon létre egy adatbázist, a webhelycsoport és a dokumentumokat a MongoChef használatával  

Hozzon létre egy adatbázist, a webhelycsoport és a dokumentumok MongoChef használatával, hajtsa végre az alábbi lépéseket.

1. **Kapcsolatkezelő**jelölje ki a kapcsolatot, és kattintson a **Csatlakozás**gombra.

    ![Képernyőkép: a MongoChef kapcsolatkezelő](./media/documentdb-mongodb-mongochef/ConnectToAccount.png)

2. Kattintson a jobb gombbal a host, és válassza az **Adatbázis hozzáadása**.  Adja meg az adatbázis nevét, és kattintson az **OK gombra**.
    
    ![Képernyőkép: a MongoChef hozzáadása adatbázis ikon](./media/documentdb-mongodb-mongochef/AddDatabase1.png)

3. Kattintson a jobb gombbal az adatbázist, és válassza a **Gyűjtemény felvétele**.  Adja meg a webhelycsoport nevét, és kattintson a **Létrehozás**gombra.

    ![Képernyőkép: a MongoChef hozzáadása webhelycsoport ikon](./media/documentdb-mongodb-mongochef/AddCollection.png)

4. A **webhelycsoport** menüpontra, majd kattintson a **Dokumentum hozzáadása**gombra.

    ![Képernyőkép: a MongoChef hozzáadása a dokumentumhoz menüpont](./media/documentdb-mongodb-mongochef/AddDocument1.png)

5. A dokumentum hozzáadása párbeszédpanelen illessze be a következőt, és kattintson a **Dokumentum hozzáadása**gombra.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
            { "firstName": "Thomas" },
            { "firstName": "Mary Kay"}
        ],
        "children": [
        {
            "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
            "pets": [{ "givenName": "Fluffy" }]
        }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }

    
6. Adja hozzá egy másik dokumentumba, az alábbi tartalom ezt az időt.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                "givenName": "Lisa", 
                "gender": "female", 
                "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }

7. Egy minta lekérdezés végrehajtása. Például családoknak, ahol a Vezetéknév "Andersen" kereshet, és a szülők és az állapot mező vissza.

    ![Képernyőkép: a Mongo tölthetők le lekérdezés eredménye](./media/documentdb-mongodb-mongochef/QueryDocument1.png)
    

## <a name="next-steps"></a>Következő lépések

- Ismerkedjen meg DocumentDB protokollt támogató MongoDB [minták](documentdb-mongodb-samples.md).

 
