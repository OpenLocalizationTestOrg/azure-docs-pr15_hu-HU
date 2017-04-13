<properties
    pageTitle="A Microsoft Azure-tárterületre ügyféloldali Python titkosítás |} Microsoft Azure"
    description="Az Azure tároló ügyfél tárat Python ügyféloldali titkosítását támogatja, a maximális biztonság az Azure tárterület-alkalmazásokhoz."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>A Microsoft Azure-tárterületre ügyféloldali Python titkosítás

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>– Áttekintés

Az [Azure tároló ügyfél-tár Python](https://pypi.python.org/pypi/azure-storage) támogatja az adatok ügyfélalkalmazásaiban jelennek titkosítja Azure tároló feltöltése, és az ügyfél letöltése közben az adatok visszafejtése előtt.

>[AZURE.NOTE] Az Azure tároló Python tárban van előzetes verzióban.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Titkosítás és a boríték technika keresztül visszafejtés
A folyamatok titkosítási és visszafejtése kövesse a boríték beállításnál.

### <a name="encryption-via-the-envelope-technique"></a>A boríték technika keresztül titkosítás:
A boríték technika keresztül titkosítási működik az alábbi módon:

1.  Az Azure tároló ügyfél tár egy tartalom titkosítási kulcs (CEK), amely szimmetrikus egyszer használata egy kulcsot hoz létre.

2.  Felhasználói adatok titkosított e CEK.

3.  A CEK ezt követően (titkosított) a fő titkosítási kulcs (KEK) használatával. A KEK egy kulcs azonosítója jelöl, és lehet egy aszimmetrikus kulcs pár vagy a szimmetrikus termékkulccsal, amely helyileg kezeli.
A tároló ügyfél tár magát soha nem fér hozzá KEK. A tár elindítja a fő körbefuttatási algoritmus a KEK által biztosított. Felhasználók választhatnak használandó egyéni szolgáltatók kulcs körbefuttatási/kicsomagolása ha szükségesnek látja.

4.  A titkosított adatok majd feltöltött az Azure tárhelyszolgáltatáshoz. A tördelt billentyűt, és néhány további titkosítási metaadatok tárolja a metaadat-alapú (a blob) vagy a titkosított adatokkal (üzenetek és a táblázat egységek) köztes értékkel.

### <a name="decryption-via-the-envelope-technique"></a>A boríték technika keresztül visszafejtés
A boríték technika keresztül visszafejtés működik az alábbi módon:

1.  Az ügyfél tár feltételezi, hogy a felhasználónak van a fő Titkosítókulcs kezelése (KEK) helyi meghajtóra. A felhasználó nem kell, hogy az adott kulcs titkosításhoz használt. Ehelyett egy a fő, oldja fel másik kulcs azonosítók kulcsok, feloldó kell állíthat be és használja.

2.  Az ügyfél tár letölti a titkosított adatok bármilyen titkosítási anyag a szolgáltatás tárolt együtt.

3.  A tördelt tartalom titkosítási kulcs (CEK) van, akkor csomagolatlan (visszafejtett) használ a fontosabb billentyű (KEK). Itt újra, az ügyfél tár nincs hozzáférésük KEK. Egyszerűen csak az egyéni szolgáltató kicsomagolási algoritmus indítja el.

4.  A tartalom titkosítási kulcs (CEK) a titkosított felhasználói adatok visszafejtése majd szolgál.

## <a name="encryption-mechanism"></a>Titkosítási mechanizmusa  
A tároló ügyfél tár [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) használ, annak érdekében, hogy a felhasználói adatok titkosítása. Pontosabban [Titkosítás Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) mód AES. Minden egyes szolgáltatás működése némileg eltér, úgy fog ölelik mindegyiket itt.

### <a name="blobs"></a>BLOB
Az ügyfél tár jelenleg csak az egész BLOB titkosítását támogatja. Pontosabban titkosítás esetén támogatott funkciók használók **létrehozása*** módszereket. Letöltések, mind a teljes és tartomány letöltések támogatottak, és a mindkettő parallelization feltöltése és letöltése érhető el.

Titkosítás során az ügyfél tár készítése a véletlen inicializálni vektoros IV 16 bájt, a véletlen tartalom titkosítási kulcs (CEK) 32 bájt együtt, és hajtsa végre a boríték titkosítási az ezen információk alapján blob-adatok. A tördelt CEK és néhány további titkosítási metaadatok blob-metaadatok az szolgáltatása a titkosított blob együtt, majd találhatók.

>[AZURE.WARNING] Ha szerkesztési vagy a saját a blob metaadatok feltöltése, győződjön meg arról, hogy megőrződik a metaadatok szeretne. A feltöltött nélkül a metaadatok új metaadatok a tördelt CEK, IV és más metaadatok elvesznek, és a blob-tartalom soha nem lesz beolvasható újra.

Egy titkosított blob letöltése magában foglalja a teljes blob használata az **első**tartalmát beolvasása * kényelmesebbé módszereket. A tördelt CEK kicsomagolják, és visszatérhet a felhasználók a visszafejtett adatok együtt a IV (tárolt blob-metaadatok ebben az esetben) használt.

Egy tetszőleges tartomány letöltése (**első*** tartomány paraméterekkel módszerek átadott) a titkosított blob jár a tartomány, kis mennyiségű további adatokért, amelyeket a kért tartomány sikeresen visszafejtése használható annak érdekében, hogy a felhasználók által biztosított beállítása.

Továbbfejlesztett fájlblokkolás BLOB és lap BLOB csak lehet titkosított/visszafejtés használ, a rendszer. Jelenleg nem támogatja a titkosított hozzáfűző BLOB.

### <a name="queues"></a>Sorok
Üzenetek lehet olyan formátumban, mivel az ügyfél tár határozza meg, olyan egyéni formátumot, a inicializálni vektoros (IV) és a titkosított tartalom titkosítási kulcs (CEK) szerepel az üzenet szövegét.

Titkosítás során az ügyfél tár hoz létre a véletlen IV 16 bájt együtt a véletlen CEK 32 bájtok számát, és boríték titkosítási várólista üzenet szövegének ezen információk alapján végzi el. A tördelt CEK és néhány további titkosítási metaadatok majd hozzáadni az várólista titkosított üzenetet. A szolgáltatás tárolja a módosított üzenet (lásd alább).

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

A tördelt billentyűt visszafejtés, során a várólista üzenet kivonjuk és kicsomagolják. A IV is a várólista üzenet kiolvasott és visszafejteni az várólista üzenet adatait a csomagolatlan kulcs együtt használja. Ne feledje, hogy a titkosítási metaadatok kis (a 500 bájt), így azt számolja várólista üzenet 64KB korlátját felé, míg hatása kezelhető kell lennie.

### <a name="tables"></a>Táblák
Az ügyfél tár entitás tulajdonságai titkosítását támogatja szúrhatók be, és cserélje le a műveletek.

>[AZURE.NOTE] Egyesítés jelenleg nem támogatott. A tulajdonságok egy részét előfordulhat, hogy korábban használatával titkosított egy másik kulcs, mivel a egyszerűen az új tulajdonságok egyesítése és frissíti a metaadatokat eredményezi az adatok elvesztését. A már meglévő entitás olvasni a szolgáltatást a szolgáltatások külön híváskezdeményezési egyesítése vagy igényel, vagy új kulccsal tulajdonságonként, mindkét, amelyek nem alkalmasak a teljesítmény érdekében.

Táblázat-adatok titkosítása működik az alábbi képlettel történik:

1.  Felhasználók adja meg a Tulajdonságok titkosítható.

2.  Az ügyfél tár létrehoz egy véletlen inicializálni vektoros IV együtt a véletlen tartalom titkosítási kulcs (CEK) 32 bájt minden entitás 16 bájtok számát, és elvégzi az egyéni tulajdonságok egy új IV tulajdonságonként fakadó titkosítja boríték titkosítási. A titkosított tulajdonság bináris adatokat tárolja.

3.  A tördelt CEK és néhány további titkosítási metaadatok két további fenntartott tulajdonságként ezt követően tárolni. Az első fenntartott tulajdonság (\_ClientEncryptionMetadata1) IV, verziószám és tördelt kulcs információt tartalmazó karakterlánc tulajdonsága. A második fenntartott tulajdonság (\_ClientEncryptionMetadata2), hogy tárolja az információkat a védett bináris tulajdonsága. A második tulajdonságban tárolt adatok (\_ClientEncryptionMetadata2) maga is titkosítva.

4.  Ezen további fenntartott tulajdonságok titkosítás szükséges miatt felhasználók most már rendelkezik csak 250 egyéni tulajdonságok 252 helyett. A szervezet teljes mérete kisebb, mint 1MB kell lennie.

    Figyelje meg, hogy csak a karakterlánc-tulajdonságok titkosítható. Ha más típusú tulajdonságok titkosítható, karakterláncok kell konvertálható. A titkosított karakterláncok bináris tulajdonságként találhatók a szolgáltatást, és azok vissza karakterláncokká alakultak (nyers karakterláncok, nem a típus EdmType.STRING EntityProperties) visszafejtés után.

    Táblák mellett a titkosítási házirendet felhasználók meg kell adnia a Tulajdonságok titkosítható. Ez a bármelyik tárolása tulajdonságokból TableEntity objektumok a típus értéke EdmType.STRING elvégezhető, és IGAZ vagy a tableservice objektum beállítása a encryption_resolver_function titkosítása. Egy titkosító feloldó, akkor a függvény, amely egy partíciót billentyűt, a sor billentyűt, és a tulajdonság neve logikai érték, amely azt jelzi, hogy van-e ez a tulajdonság titkosítva adja eredményül. Titkosítás során az ügyfél tár fogja használni ezt az információt döntse el, hogy a tulajdonság titkosítani kell-e a vezetékes írása közben. A meghatalmazott is tartalmaz, hogyan tulajdonságok titkosított körül logikájának lehetőséget. (Például ha az X, majd A tulajdonság titkosítása; egyébként titkosítása tulajdonságok a és b) Ne feledje, hogy azt nem szükséges, adja meg az adatokat, vagy entitás lekérdezése olvasása közben.

### <a name="batch-operations"></a>Köteg műveletek
Az egyik titkosítási házirend vonatkozik a köteg minden sor. Az ügyfél tár belső létrehoz egy új véletlen IV és soronként véletlen CEK a köteg. Ez a probléma definiálása a titkosítási feloldó különböző tulajdonságokat a köteg minden művelet titkosíthatja felhasználók is választhat.
Ha egy köteg helyi vezető jön létre a tableservice batch() módszerrel keresztül, a tableservice titkosítás házirend automatikusan alkalmazandó a köteget. Hívja fel a konstruktor kifejezetten létrejön egy köteg, ha a titkosítási házirendet kell egy paraméterként és balra változatlanul a köteg élettartamot.
Figyelje meg, hogy a személyek, a köteg (szervezetek nem titkosított elvégzése a köteget, az tableservice titkosítási házirend használata, időben) a köteg titkosítás házirend használatával való beszúrása titkosított.

### <a name="queries"></a>Lekérdezések
Lekérdezés hajtanak végre, meg kell adnia a fő feloldó, amely az eredményhalmaz minden kulcs megoldható. Ha a lekérdezés eredménye található entitás nem oldható meg a szolgáltatónak, az ügyfél tár hibaüzenetet fog küldjön. Bármilyen lekérdezést, amely kiszolgáló egymás előrejelzések hajt végre, az ügyfél-dokumentumtár speciális titkosítási metaadatok tulajdonságainak hozzáadja (\_ClientEncryptionMetadata1 és \_ClientEncryptionMetadata2) a kijelölt oszlopok alapértelmezés szerint.

>[AZURE.IMPORTANT] A felhasználónevek ezeket a fontos pontokat ügyféloldali titkosítás használata esetén:
>
>- Ha egy titkosított blob írásakor vagy olvasásakor, használja a teljes blob feltöltése és tartomány/egész blob letöltési parancsok. Egy titkosított blob protokoll műveletek, például a helyezi el a továbbfejlesztett fájlblokkolás, blokkokból álló lista elhelyezni, írja be lapok, illetve törlése; használatával írás elkerülése Ellenkező előfordulhat, hogy a titkosított blob sérült, és nem olvasható legyen.
>
>- Táblázatok esetében hasonló kényszer létezik. Vigyázzon, nem frissülnek a titkosított tulajdonságokat a titkosítási metaadatok frissítése nélkül.
>
>- A titkosított blob metaadatok állít be, ha a titkosítási kapcsolatos metaadatok szükséges visszafejtés, mivel a metaadat-alapú beállítás nem additív előfordulhat, hogy felülírja. Ez akkor is érvényes pillanatképek; Kerülje a metaadat-alapú megadása során egy titkosított blob-pillanatfelvétel létrehozásának. Metaadat-alapú kell beállítani, ne felejtse el, hívja az **get_blob_metadata** módszert, először az aktuális titkosítási metaadatokat beszerzése és elkerülése érdekében a egyidejű írás közben a metaadat-alapú beállítása.
>
>- Engedélyezze a felhasználóknak, hogy csak a titkosított adatokkal együtt kell működniük a szolgáltatás objektum **require_encryption** jelölő. További információkért lásd az alábbi.

A tároló ügyfél tár vár, a megadott KEK és a fő feloldó, a következő kapcsolat végrehajtásához. Függőben lévő és a tár befejezése után lesznek integrálva [Azure kulcs tárolóból elemre](https://azure.microsoft.com/services/key-vault/) támogatás Python KEK kezelése.


## <a name="client-api--interface"></a>Az ügyfélprogram API-val és a kapcsolat
Tárolási szolgáltatás objektum (azaz blockblobservice) létrehozása után a felhasználó előfordulhat, hogy hozzárendelése értékeket a mezőkre, amelyeket az titkosítási házirend alkotnak: key_encryption_key, key_resolver_function, és require_encryption. Felhasználók biztosíthat KEK csak csak egy feloldó, vagy mindkettőt. key_encryption_key, amely azonosítja a kulcs azonosítója, és, amely lehetővé teszi a logika körbefuttatási/kicsomagolása egyszerű kulcs típusát. egy kulcsot a visszafejtés során feloldásához key_resolver_function használják. Egy adott egy kulcs azonosítója érvényes KEK adja vissza. Ez jelzi a felhasználóknak az azt jelenti, hogy több kulcsok, amelyek egynél több helyen kezelhetők közül választhat.

A KEK az alábbi módszerek az adatok sikeres titkosítása kell végrehajtása:
- wrap_key(cek): a megadott CEK (bájt) használata a felhasználó megválasztott algoritmus fussa. A tördelt kulcs lekérdezése
- get_key_wrap_algorithm(): úgy veheti körül a billentyűk algoritmus adja eredményül.
- get_kid(): Ez KEK a karakterlánc-azonosító adja vissza.
A KEK sikeresen visszafejteni az adatokat a következő módszerekkel kell végrehajtása:
- unwrap_key (cek, algoritmus): a karakterlánc megadott algoritmus használatával a megadott CEK csomagolatlan formájában adja eredményül.
- get_kid(): Ez KEK egy karakterlánc-azonosítót adja vissza.

A fő feloldó kell legalább megvalósítása visszaadó, fő azonosítóra megadott végrehajtási a fenti felületén a megfelelő KEK módszer. Ez a módszer csak van a szolgáltatás objektum key_resolver_function tulajdonságának kiosztani.

- Titkosítás használja a rendszer mindig és egy kulcsot hiányában hibát eredményez.
- A visszafejtés:
    - A fő feloldó meghívott, ha a kulcs beszerzése megadott. Ha a feloldó van megadva, de nem rendelkezik a fő azonosító megfeleltetés, hiba történt elő.
    - Ha feloldó nincs megadva, de a kulcs van megadva, a kulcs használatos, ha az azonosító felel meg a szükséges kulcs azonosítója. Ha az azonosító értéke nem egyezik meg, hibaüzenetet elő.

      A titkosítási minták azure.storage.samples <fix URL>BLOB, a sorok és a táblázatok részletesebb végpontok közötti alkalmazási helyzetet mutatja be.
        Minta megvalósítás KEK és fő feloldó szerepelnek a mintafájlok KeyWrapper és KeyResolver rendre.

### <a name="requireencryption-mode"></a>RequireEncryption mód
Felhasználók tetszés szerint engedélyezheti a hol összes letöltések és feltöltések titkosítani kell üzemmódot. Ebben az üzemmódban kísérletek nélkül az titkosítási házirend fel-vagy a szolgáltatás nem titkosított adatok letöltése sikertelen lesz az ügyfélgépen. A szolgáltatás objektum **require_encryption** jelölő a jelenség szabályozza.

### <a name="blob-service-encryption"></a>BLOB szolgáltatás titkosítás:
Objektum állítható be a titkosítási házirend mezők a blockblobservice. Milyen egyéb kezelje a rendszer az ügyfél Library belső.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Várólista szolgáltatást titkosítás:
Objektum állítható be a titkosítási házirend mezők a queueservice. Milyen egyéb kezelje a rendszer az ügyfél Library belső.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Táblázat szolgáltatás titkosítás:
Nemcsak a titkosítási-házirend létrehozása és beállítása a kérelem beállításai, kell egy **encryption_resolver_function** adja meg a **tableservice**, vagy állítsa a titkosítása attribútum a EntityProperty.

### <a name="using-the-resolver"></a>A feloldó használatával

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Attribútumok használatával
Fent említett tulajdonság jelölhető titkosításhoz EntityProperty objektum tárolja őket, és állítsa a titkosítása mezőben.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Titkosítás és a teljesítmény
Megjegyzendő, hogy titkosítja a további teljesítmény általános tárolási adatok eredményezi. A tartalom billentyűt, és a IV kell létrehozni, titkosítani kell a tartalom magát és további metaadatokat kell formázott és feltöltött. Ez a terhelést titkosítását adatok mennyiségét függően változhatnak. Azt javasoljuk, hogy ügyfelei mindig tesztelje a teljesítmény fejlesztése során kérelmeiket.

## <a name="next-steps"></a>Következő lépések

- Az [Azure tároló ügyfél tár Java PyPi csomag](https://pypi.python.org/pypi/azure-storage) letöltése
- [Azure tároló ügyfél dokumentumtár a Python forráskód a GitHub](https://github.com/Azure/azure-storage-python) letöltése
