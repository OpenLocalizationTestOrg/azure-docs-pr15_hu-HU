<properties
    pageTitle="A Microsoft Azure-tárterületre ügyféloldali .NET titkosítás |} Microsoft Azure"
    description="Az Azure tárolási ügyfél tárat .NET az Azure tárolási alkalmazások maximális biztonsági támogatja az ügyféloldali titkosítás és a integrálása az Azure kulcs tárolóból elemre."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Ügyféloldali titkosítást és Azure kulcs tárolóból elemre a Microsoft Azure-tárterületre

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>– Áttekintés

Az [Azure tároló ügyfél .NET Nuget csomag tár](https://www.nuget.org/packages/WindowsAzure.Storage) támogatja a titkosított adatok ügyfélalkalmazásaiban jelennek Azure tároló feltöltése, és az ügyfél letöltése közben az adatok visszafejtése előtt. A tár is támogatja a tárhely kulcsfontosságú fiókkezelés integrálása az [Azure kulcs tárolóból elemre](https://azure.microsoft.com/services/key-vault/) .

Lépésenkénti oktatóprogram, amely végigvezeti Önt azon a folyamaton, titkosítja BLOB az ügyféloldali titkosítást és Azure kulcs tárolóból elemre [használja az Azure kulcs tárolóból elemre a Microsoft Azure-tárolóban lévő titkosítása és visszafejtés BLOB](storage-encrypt-decrypt-blobs-key-vault.md)című témakör tartalmaz.

Az ügyféloldali titkosítás Java lásd: az [Ügyféloldali titkosítás a Microsoft Azure-tárterületre Java](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Titkosítás és a boríték technika keresztül visszafejtés

A folyamatok titkosítási és visszafejtése kövesse a boríték beállításnál.

### <a name="encryption-via-the-envelope-technique"></a>A boríték technika keresztül titkosítás:

A boríték technika keresztül titkosítási működik az alábbi módon:

1. Az Azure tároló ügyfél tár egy tartalom titkosítási kulcs (CEK), amely szimmetrikus egyszer használata egy kulcsot hoz létre.
2. Felhasználói adatok titkosított e CEK.
3. A CEK ezt követően (titkosított) a fő titkosítási kulcs (KEK) használatával. A KEK egy kulcs azonosítója jelöl, és egy aszimmetrikus kulcs pár vagy szimmetrikus kulcsot és is lehet helyileg kezeli vagy Azure kulcs tárolókban tárolt.

    A tároló ügyfél tár magát soha nem fér hozzá KEK. A tár elindítja a fő körbefuttatási algoritmus kulcs tárolóra által biztosított. Felhasználók választhatnak használandó egyéni szolgáltatók kulcs körbefuttatási/kicsomagolása ha szükségesnek látja.

4. A titkosított adatok majd feltöltött az Azure tárhelyszolgáltatáshoz. A tördelt billentyűt, és néhány további titkosítási metaadatok tárolja a metaadat-alapú (a blob) vagy a titkosított adatokkal (üzenetek és a táblázat egységek) köztes értékkel.

### <a name="decryption-via-the-envelope-technique"></a>A boríték technika keresztül visszafejtés

A boríték technika keresztül visszafejtés működik az alábbi módon:

1. Az ügyfél tár feltételezi, hogy a felhasználó kezeli a fő titkosítási kulcs (KEK) helyileg vagy Azure kulcs tárolókban. A felhasználó nem kell, hogy az adott kulcs titkosításhoz használt. Ehelyett egy kulcs feloldó, amelyek különböző kulcs azonosítók eredménye a billentyűk kell állíthat be és használja.
2. Az ügyfél tár letölti a titkosított adatok bármilyen titkosítási anyag a szolgáltatás tárolt együtt.
3. A tördelt tartalom titkosítási kulcs (CEK) van, akkor csomagolatlan (visszafejtett) használ a fontosabb billentyű (KEK). Itt újra, az ügyfél tár nincs hozzáférésük KEK. Az egyéni vagy a kulcs tárolóra szolgáltató kicsomagolási algoritmus egyszerűen elindítja.
4. A tartalom titkosítási kulcs (CEK) a titkosított felhasználói adatok visszafejtése majd szolgál.

## <a name="encryption-mechanism"></a>Titkosítási mechanizmusa

A tároló ügyfél tár [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) használ, annak érdekében, hogy a felhasználói adatok titkosítása. Pontosabban [Titkosítás Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) mód AES. Minden egyes szolgáltatás működése némileg eltér, úgy fog ölelik mindegyiket itt.

### <a name="blobs"></a>BLOB

Az ügyfél tár jelenleg csak az egész BLOB titkosítását támogatja. Titkosítása támogatott kifejezetten, amikor a felhasználó használja a **UploadFrom** * módszereket vagy a * *OpenWrite** módot. Letöltések, mind a teljes és tartomány letöltések támogatott.

Titkosítás során az ügyfél tár készítése egy véletlen inicializálni vektoros IV 16 bájt, egy véletlen tartalom titkosítási kulcs (CEK) 32 bájt együtt, és hajtsa végre a boríték titkosítását az ezen információk alapján blob-adatokat. A tördelt CEK és néhány további titkosítási metaadatok blob-metaadatok az szolgáltatása a titkosított blob együtt, majd találhatók.

> [AZURE.WARNING] Ha szerkesztési vagy a saját a blob metaadatok feltöltése, győződjön meg arról, hogy megőrződik a metaadatok szeretne. A feltöltött nélkül a metaadatok, a tördelt CEK, IV és más metaadatok új metaadatok elvesznek, és a blob-tartalom soha nem lesz beolvasható újra.

Egy titkosított blob letöltése magában foglalja a teljes blob használata a **DownloadTo**tartalmát beolvasása*/**BlobReadStream** kényelmesebbé módszereket. A tördelt CEK kicsomagolják, és visszatérhet a felhasználók a visszafejtett adatok együtt a IV (tárolt blob-metaadatok ebben az esetben) használt.

Egy tetszőleges tartomány letöltése (**DownloadRange*** módszerek) a titkosított blob jár a tartomány, kis mennyiségű további adatokért, amelyeket használva a kért tartomány sikeresen visszafejtése annak érdekében, hogy a felhasználók által biztosított beállítása.

Az összes blob-típusok (BLOB letiltani, oldal BLOB és BLOB hozzáfűző) is titkosított/fejthető vissza, a rendszer használatával.

### <a name="queues"></a>Sorok

Üzenetek lehet olyan formátumban, mivel az ügyfél tár határozza meg, olyan egyéni formátumot, a inicializálni vektoros (IV) és a titkosított tartalom titkosítási kulcs (CEK) szerepel az üzenet szövegét.

Titkosítás során az ügyfél tár hoz létre a véletlen IV 16 bájt együtt a véletlen CEK 32 bájtok számát, és boríték titkosítási várólista üzenet szövegének ezen információk alapján végzi el. A tördelt CEK és néhány további titkosítási metaadatok majd hozzáadni az várólista titkosított üzenetet. A szolgáltatás tárolja a módosított üzenet (lásd alább).

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

A tördelt billentyűt visszafejtés, során a várólista üzenet kivonjuk és kicsomagolják. A IV is a várólista üzenet kiolvasott és visszafejteni az várólista üzenet adatait a csomagolatlan kulcs együtt használja. Ne feledje, hogy a titkosítási metaadatok kis (a 500 bájt), így azt számolja várólista üzenet 64KB korlátját felé, míg hatása kezelhető kell lennie.

### <a name="tables"></a>Táblák

Az ügyfél tár entitás tulajdonságai titkosítását támogatja szúrhatók be, és cserélje le a műveletek.

>[AZURE.NOTE] Egyesítés jelenleg nem támogatott. A tulajdonságok egy részét előfordulhat, hogy korábban használatával titkosított egy másik kulcs, mivel a egyszerűen az új tulajdonságok egyesítése és frissíti a metaadatokat eredményezi az adatok elvesztését. A már meglévő entitás olvasni a szolgáltatást a szolgáltatások külön híváskezdeményezési egyesítése vagy igényel, vagy új kulccsal tulajdonságonként, mindkét, amelyek nem alkalmasak a teljesítmény érdekében.

Táblázat-adatok titkosítása működik az alábbi képlettel történik:  

1. Felhasználók adja meg a Tulajdonságok titkosítható.
2. Az ügyfél tár létrehoz egy véletlen inicializálni vektoros IV együtt a véletlen tartalom titkosítási kulcs (CEK) 32 bájt minden entitás 16 bájtok számát, és elvégzi az egyéni tulajdonságok egy új IV tulajdonságonként fakadó titkosítja boríték titkosítási. A titkosított tulajdonság bináris adatokat tárolja.
3. A tördelt CEK és néhány további titkosítási metaadatok két további fenntartott tulajdonságként ezt követően tárolni. Az első fenntartott tulajdonság (_ClientEncryptionMetadata1) IV, verziószám és tördelt kulcs információt tartalmazó karakterlánc tulajdonság értéke. A második fenntartott (_ClientEncryptionMetadata2) tulajdonsága tárolja az információkat a védett bináris tulajdonságot. Ez a második tulajdonság (_ClientEncryptionMetadata2) adatai titkosított magát.
4. Ezen további fenntartott tulajdonságok titkosítás szükséges miatt felhasználók most már rendelkezik csak 250 egyéni tulajdonságok 252 helyett. A szervezet teljes mérete kisebb, mint 1 MB kell lennie.

Figyelje meg, hogy csak a karakterlánc-tulajdonságok titkosítható. Ha más típusú tulajdonságok titkosítható, karakterláncok kell konvertálható. A titkosított karakterláncok bináris tulajdonságként találhatók a szolgáltatást, és azok vissza karakterláncokká alakultak visszafejtés után.

Táblák mellett a titkosítási házirendet felhasználók meg kell adnia a Tulajdonságok titkosítható. Ez történik megadásával bármelyik (az, hogy TableEntity származtatása POCO egységek) [EncryptProperty] attribútum vagy egy titkosító feloldó kérelem beállításai. Egy titkosító feloldó, amely egy partíciót billentyűt, a sor billentyűt, és a tulajdonság neve, amely azt jelzi, hogy van-e ez a tulajdonság titkosítva logikai érték beolvasása a meghatalmazott. Titkosítás során az ügyfél tár fogja használni ezt az információt döntse el, hogy a tulajdonság titkosítani kell-e a vezetékes írása közben. A meghatalmazott is tartalmaz, hogyan tulajdonságok titkosított körül logikájának lehetőséget. (Például ha az X, majd A tulajdonság titkosítása; egyébként titkosítása tulajdonságok a és b) Ne feledje, hogy azt nem szükséges, adja meg az adatokat, vagy entitás lekérdezése olvasása közben.

### <a name="batch-operations"></a>Köteg műveletek

A köteg műveletek az azonos KEK lesz a köteg művelet az összes sort átfogó, mert az ügyfél tár csak lehetővé teszi, hogy egy beállítások objektumra (és ezért egyik házirend/KEK) egy köteg művelet. Jó helyen jár az ügyfél tár belső létrehoz egy új véletlen IV és soronként véletlen CEK a köteg. Ez a probléma definiálása a titkosítási feloldó különböző tulajdonságokat a köteg minden művelet titkosíthatja felhasználók is választhat.

### <a name="queries"></a>Lekérdezések

Lekérdezés hajtanak végre, meg kell adnia a fő feloldó, amely az eredményhalmaz minden kulcs megoldható. Ha a lekérdezés eredménye található entitás nem oldható meg a szolgáltatónak, az ügyfél tár hibaüzenetet fog küldjön. Bármilyen lekérdezést, amely kiszolgálóoldali előrejelzések hajt végre az ügyfél tár hozzáadja Speciális titkosítási metaadatok tulajdonságainak (_ClientEncryptionMetadata1 és _ClientEncryptionMetadata2) alapértelmezés szerint a kijelölt oszlopokat.

## <a name="azure-key-vault"></a>Azure kulcs tárolóból elemre

Azure kulcs tárolóból elemre a védelmét cryptographic billentyűkkel és titkos kulcsok felhő alkalmazások és szolgáltatások által használt segítségével. Azure kulcs tárolóra használ, a felhasználók is titkosítása kulcsok és titkos kulcsok (például a hitelesítési kulcsokat, a tárhely fiók kulcsok, az adatok titkosítási kulcsok. PFX fájlok és a jelszavakat) hardver biztonsági modulokat (HSMs) eső billentyűk segítségével. További tudnivalókért lásd: [Mi az Azure kulcs tárolóra?](../key-vault/key-vault-whatis.md).

A tárhely ügyfél tár használja, a kulcs tárolóra core tár közös keretet Azure végig a billentyűk kezelésére szolgáló biztosítása érdekében. Felhasználók is élvezheti további a használatával a kulcs tárolóra bővítmények tárat. A bővítmények tár hasznos funkciókkal egyszerű és zavartalan Symmetric/RSA helyi és felhőbeli kulcs szolgáltatók köré, valamint összesítési és gyorsítótárazás biztosít.

### <a name="interface-and-dependencies"></a>Felület- és függőségek

Vannak három kulcs tárolóra csomagok:

- Microsoft.Azure.KeyVault.Core IKey és IKeyResolver tartalmazza. Érdemes egy kis nincs függőségek csomagot. A tárhely ügyfél tárat a .NET rendszerhez határozza meg azt a függőség.
- Microsoft.Azure.KeyVault a kulcs tárolóra többi ügyfél tartalmazza.
- Microsoft.Azure.KeyVault.Extensions bővítmény kódot, amely tartalmazza a titkosítási algoritmust és egy RSAKey és egy SymmetricKey példányait tartalmazza. A fő- és KeyVault névtér attól függ, és a funkcionalitást, és egy összesített feloldó (amikor a felhasználók használandó több fő szolgáltató), és gyorsítótárazás-feloldási definiálja. Bár a tárterület-ügyfél tár nem függ közvetlenül a csomag, ha felhasználók szeretné használni az Azure kulcs tárolóból elemre igényelhetnek tárolni, vagy a kulcs tárolóra bővítmények használatával felhasználni a helyi és felhőbeli cryptographic szolgáltatók, azok a csomag lesz szüksége.

Értékes fő billentyűk készült kulcs tárolóból elemre, és a kulcs tárolóból elemre egy szabályozási korlátai szem előtt az adatokkal készült. Ügyféloldali titkosítási kulcs tárolóból elemre a végrehajtásakor az előnyben részesített modell környezetbe szimmetrikus fő kulcsok tárolja a titkos kulcsok a kulcs tárolóból elemre, és gyorsítótárazott helyi meghajtóra. Felhasználók a következőket kell tennie:

1. Hozzon létre egy titkos offline, és töltse fel billentyű tárolóból elemre.
2. A titkos alap azonosító használnak paraméter a titkos titkosításhoz aktuális verziójának feloldásához, és ezeket az adatokat helyi gyorsítótár. Gyorsítótárazás; CachingKeyResolver használata felhasználók várhatóan nem valósítja saját gyorsítótárazás összefüggés.
3. Használja a gyorsítótárazása bemenetként a titkosítási házirend létrehozása során.

Kulcs tárolóra használatára vonatkozó további információt a [titkosítási mintakódok](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples)találhatók.

## <a name="best-practices"></a>Ajánlott eljárások

Titkosítás támogatása csak a .NET rendszerhez, a tárhely ügyfél tár érhető el. Windows Phone és a Windows Runtime jelenleg nem támogatja titkosítást.

>[AZURE.IMPORTANT] A felhasználónevek ezeket a fontos pontokat ügyféloldali titkosítás használata esetén:
>
>- Ha egy titkosított blob írásakor vagy olvasásakor, használja a teljes blob feltöltése és tartomány/egész blob letöltési parancsok. Protocol (protokoll) műveletek, például a helyezi el a továbbfejlesztett fájlblokkolás, blokkokból álló lista helyezi, lapok írni, törlése lapok vagy hozzáfűző blokk; használata egy titkosított blob írás elkerülése Ellenkező előfordulhat, hogy a titkosított blob sérült, és nem olvasható legyen.
>- Táblázatok esetében hasonló kényszer létezik. Vigyázzon, nem frissülnek a titkosított tulajdonságokat a titkosítási metaadatok frissítése nélkül.
>- A titkosított blob metaadatok állít be, ha a titkosítási kapcsolatos metaadatok szükséges visszafejtés, mivel a metaadat-alapú beállítás nem additív előfordulhat, hogy felülírja. Ez akkor is érvényes pillanatképek; Kerülje a metaadat-alapú megadása során egy titkosított blob-pillanatfelvétel létrehozásának. Metaadat-alapú kell beállítani, ne felejtse el, hívja az **FetchAttributes** módszert, először az aktuális titkosítási metaadatokat beszerzése és elkerülése érdekében a egyidejű írás közben a metaadat-alapú beállítása.
>- Engedélyezze a felhasználóknak, hogy csak a titkosított adatokkal együtt kell működniük az alapértelmezett kérelem beállításai a **RequireEncryption** tulajdonság. További információkért lásd az alábbi.


## <a name="client-api--interface"></a>Az ügyfélprogram API-val és a kapcsolat

EncryptionPolicy objektum létrehozásakor felhasználók megadhatja az ikon csak kulcsot (végrehajtási IKey), csak egy feloldó (végrehajtási IKeyResolver), vagy mindkettőt. IKey, amely azonosítja a kulcs azonosítója, és, amely lehetővé teszi a logika körbefuttatási/kicsomagolása egyszerű kulcs típusát. Egy kulcsot a visszafejtés során feloldásához IKeyResolver használják. Azt határozza meg, amely visszaadja egy adott egy kulcs azonosítója IKey ResolveKey módszer. Ez jelzi a felhasználóknak az azt jelenti, hogy több kulcsok, amelyek egynél több helyen kezelhetők közül választhat.

- Titkosítás használja a rendszer mindig és egy kulcsot hiányában hibát eredményez.
- A visszafejtés:
    - A fő feloldó meghívott, ha a kulcs beszerzése megadott. Ha a feloldó van megadva, de nem rendelkezik a fő azonosító megfeleltetés, hiba történt elő.
    - Ha feloldó nincs megadva, de a kulcs van megadva, a kulcs használatos, ha az azonosító felel meg a szükséges kulcs azonosítója. Ha az azonosító értéke nem egyezik meg, hibaüzenetet elő.

A [titkosítási minták](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) egy részletesebb végpontok közötti BLOB, sorok és táblázatok, kulcs tárolóra integrációs együtt alkalmazási helyzetet mutatja be.

### <a name="requireencryption-mode"></a>RequireEncryption mód

Felhasználók tetszés szerint engedélyezheti a hol összes letöltések és feltöltések titkosítani kell üzemmódot. Ebben az üzemmódban kísérletek nélkül az titkosítási házirend fel-vagy a szolgáltatás nem titkosított adatok letöltése sikertelen lesz az ügyfélgépen. A kérelem beállításai objektum **RequireEncryption** tulajdonsága megoldásához szabályozza. Ha az alkalmazás titkosítja Azure-tárolóban lévő összes objektumot, majd beállíthatja, hogy a **RequireEncryption** tulajdonság az alapértelmezett kérelem beállításai az szolgáltatás ügyfél objektum. Ha például **CloudBlobClient.DefaultRequestOptions.RequireEncryption** értékűre **Igaz** titkosítás keresztül az ügyfél objektum elvégzett minden blob műveletekhez.

### <a name="blob-service-encryption"></a>BLOB szolgáltatás titkosítás:

Hozzon létre egy **BlobEncryptionPolicy** objektumot, és beállítja a kérelem beállításai (API vagy egy ügyfél szintjén **DefaultRequestOptions**használatával). Milyen egyéb kezelje a rendszer az ügyfél Library belső.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Upload the encrypted contents to the blob.
    blob.UploadFromStream(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    MemoryStream outputStream = new MemoryStream();
    blob.DownloadToStream(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Várólista szolgáltatást titkosítás:

Hozzon létre egy **QueueEncryptionPolicy** objektumot, és beállítja a kérelem beállításai (API vagy egy ügyfél szintjén **DefaultRequestOptions**használatával). Milyen egyéb kezelje a rendszer az ügyfél Library belső.


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
    queue.AddMessage(message, null, null, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);

### <a name="table-service-encryption"></a>Táblázat szolgáltatás titkosítás:

Nemcsak a titkosítási-házirend létrehozása és beállítása a kérelem beállításai, kell egy **EncryptionResolver** **TableRequestOptions**adja meg, vagy állítsa a [EncryptProperty] attribútum a szervezet.

#### <a name="using-the-resolver"></a>A feloldó használatával


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    {
        EncryptionResolver = (pk, rk, propName) =>
        {
            if (propName == "foo")
            {
                return true;
            }
            return false;
        },
        EncryptionPolicy = policy
    };

    // Insert Entity
    currentTable.Execute(TableOperation.Insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    {
        EncryptionPolicy = policy
    };

    TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
    TableResult result = currentTable.Execute(operation, retrieveOptions, null);

#### <a name="using-attributes"></a>Attribútumok használatával

Ahogy a fenti, ha a szervezet TableEntity hajtja végre, majd a tulajdonságok is kell díszítve helyett a **EncryptionResolver**megadása a [EncryptProperty] attribútum.

    [EncryptProperty]
    public string EncryptedProperty1 { get; set; }

## <a name="encryption-and-performance"></a>Titkosítás és a teljesítmény

Megjegyzendő, hogy titkosítja a további teljesítmény általános tárolási adatok eredményezi. A tartalom billentyűt, és a IV kell létrehozni, titkosítani kell a tartalom magát és további metaadatai kell formázott és feltöltött. Ez a terhelést titkosítását adatok mennyiségét függően változhatnak. Azt javasoljuk, hogy a vevők mindig tesztelje a teljesítmény fejlesztése során kérelmeiket.

## <a name="next-steps"></a>Következő lépések

- [Oktatóprogram: Titkosítása és Azure kulcs tárolóra használata a Microsoft Azure-tárolóban lévő BLOB visszafejtése](storage-encrypt-decrypt-blobs-key-vault.md)
- Az [Azure tároló ügyfél tár .NET NuGet csomag](https://www.nuget.org/packages/WindowsAzure.Storage) letöltése
- Az Azure kulcs tárolóra NuGet [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), az [ügyfél](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/)és a [bővítmények](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) csomagok letöltése  
- A Microsoft [Azure kulcs tárolóra dokumentáció](../key-vault/key-vault-whatis.md)

