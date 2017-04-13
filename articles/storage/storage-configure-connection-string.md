<properties 
    pageTitle="Kapcsolati karakterlánc Azure tárolóhoz beállítása |} Microsoft Azure"
    description="Állítsa be a kapcsolati karakterlánc Azure tárterület-fiókjába. Kapcsolati karakterlánc hitelesítést végezni access tárterület-fiókjába a futásidőben levelezőprogramból szükséges adatokat tartalmazza."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Azure tároló Csatlakozási_karakterlánc konfigurálása

## <a name="overview"></a>– Áttekintés

Kapcsolati karakterlánc a futásidőben a alkalmazásból az Azure tárterület-fiókokban található adatok eléréséhez szükséges hitelesítési adatait tartalmazza. Kapcsolati karakterláncát adhatja meg:

- Csatlakozás az Azure tároló irányító.
- Elérhetik a tárterület-fiókját Azure-ban.
- Via (Társítások) átengedése aláírás Azure-ban megadott erőforrások elérheti.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>A kapcsolati karakterlánc tárolása

Az alkalmazás kell futásidőben a kapcsolati karakterlánc elérése érdekében Azure tárolóhoz kérések hitelesítést végezni. A kapcsolati karakterlánc tárolására szolgáló néhány másik lehetőség közül választhat:

- A kapcsolati karakterlánc tárolhat-alkalmazás fut, az asztal vagy egy eszközön, az egy `app.config `fájl vagy egy `web.config` fájlt. Írja be a kapcsolati karakterlánc **AppSettings** szakaszába.
- Az alkalmazás-Azure felhőszolgáltatásában fut tárolhatja a kapcsolati karakterláncot az [Azure service-konfigurációs sémafájl (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). A kapcsolati karakterlánc írja be a szolgáltatás konfigurációs fájl **ConfigurationSettings** szakaszába.
- A kapcsolati karakterlánc a kódban közvetlenül is használhatja. Az a legtöbb esetben azonban azt javasoljuk, hogy a konfigurációs fájl tárolni a konfigurációs karakterlánc.

A kapcsolati karakterlánc konfigurációs fájlban tárolásához egyszerűen frissíteni a kapcsolati karakterlánc válthat a tárhely irányító és Azure tároló fiókja a felhőben. Csak akkor módosíthatók a kapcsolati karakterláncot, mutasson a cél környezetben.

A kapcsolati karakterlánc függetlenül az alkalmazást futtató futásidőben eléréséhez a [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) osztály is használhatja.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>A tároló irányító a kapcsolati karakterlánc létrehozása

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

További információt a tárhely irányító [használata az Azure tároló irányító fejlesztés és tesztelése](storage-use-emulator.md) témakörben találhat.

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Azure tárterület-fiókhoz kapcsolati karakterlánc létrehozása

Hozzon létre egy kapcsolati karakterláncot, a tárhely Azure-fiókjába, használja a kapcsolati karakterlánc formátuma az alábbi. Jelezheti, hogy szeretné-e HTTPS (ajánlott) keresztül a tárterület-fiókhoz való csatlakozáshoz vagy HTTP, a csere `myAccountName` a tárterület-fiókjához, és a csere nevének `myAccountKey` az access a fiókkulcs:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

A kapcsolati karakterlánc például a következő példa kapcsolati karakterlánc hasonló jelenik meg:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure tároló HTTP és a HTTPS támogatja a kapcsolati karakterlánc; azonban HTTPS használata ajánlott.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>A megosztott access aláírás használatának kapcsolati karakterlánc létrehozása

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Közvetlen tároló zárólap a kapcsolati karakterlánc létrehozása

Közvetlenül a szolgáltatás végpontok megadhatja a kapcsolati karakterláncot az alapértelmezett végpontok használata helyett. Kapcsolati karakterlánc, amely meghatározza az explicit végpont létrehozásához adja meg a teljes szolgáltatás végpontjának az egyes szolgáltatásokhoz, beleértve a protocol (HTTPS (ajánlott) vagy HTTP), a következő formátumban:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Hol lehet szeretne megadni a explicit zárólap egy helyzetleírás, ha szeretne egyéni tartományt a Blob-tároló végpont megfeleltetve. Ebben az esetben az egyéni végpont Blob-tárolóhoz adja meg a kapcsolati karakterláncot, és tetszés szerint adja meg a alapértelmezett végpontokat a többi szolgáltatás, ha az alkalmazás őket.

Az alábbiakban példákat érvényes kapcsolat-karakterlánc, amely a Blob-szolgáltatás explicit zárólap:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

A kapcsolati karakterláncban szereplő végpont értékét használja a program az értekezlet-összehívást URL-címe a Blob-szolgáltatás összeállításához, és azt kívánja, hogy bármely URL-címe, a kód visszaküldött formájában.

Figyelje meg, hogy ha úgy dönt, hogy a szolgáltatás végpontjának a kapcsolati karakterláncot az kihagyása, majd nem tudják használni a kapcsolati karakterlánc szolgáltatás az adatokhoz férjenek hozzá a kódot a.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Kapcsolati karakterlánc létrehozása egy végpont utótaggal

Tárhelyszolgáltatáshoz régiókban, vagy a különböző végpont utótag, példányok a kapcsolati karakterlánc létrehozása például Azure Kína vagy Azure irányítási, használja a következő formátumban a kapcsolati karakterlánc. Adja meg, hogy a tárterület-fiókomhoz HTTP vagy HTTPS keresztül, a csere `myAccountName` a tárterület-fiókja nevét, a csere `myAccountKey` a fiókkulcs access és a csere `mySuffix` URI utótaggal:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


A kapcsolati karakterlánc például a következő kapcsolati karakterlánc hasonlóan kell kinéznie:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Kapcsolati karakterlánc elemzése

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Következő lépések

- [Használja az Azure tároló irányító fejlesztés és tesztelése](storage-use-emulator.md)
- [Azure tároló kezelők](storage-explorers.md)
- [Megosztott Access aláírások (Társítások) használata](storage-dotnet-shared-access-signature-part-1.md)
