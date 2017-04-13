<properties
    pageTitle="Azure esemény hubok archív forgatókönyv |} Microsoft Azure"
    description="Esemény hubok archív funkcióval igazolni az Azure Python SDK használó minta."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Esemény hubok archív Útmutató: Python

Esemény hubok archív új jellemzi a esemény hubok, amely lehetővé teszi, hogy automatikusan előadása a adatfolyamok lehetőség Azure Blob-tárolóhoz fiókba az esemény-központban. Ezzel megkönnyíti az feldolgozása adatfolyam valós idejű adatokat a köteg végrehajtásához. Ez a cikk ismerteti, hogyan Python esemény hubok archív használatára. Esemény hubok archív kapcsolatos további tudnivalókért olvassa el a [áttekintése a cikk](event-hubs-archive-overview.md)a című témakört.

Ez a példa az Azure Python SDK segítségével bemutatják, hogy az Archiválás szolgáltatás használatával. A sender.py elküldi szimulált környezeti telemetriai esemény hubok JSON formátumban. Az esemény-központban van beállítva az Archiválás szolgáltatás használata blob-tároló kötegekben adat írni. A archivereader.py olvassa be az alábbi BLOB és létrehoz egy hozzáfűző fájlt egy eszközt, majd az adatokat ír be a .csv fájlt.

Mit fog elvégezhető

1.  Azure Blob-tárolóhoz fiók és az Azure portálon blob tároló benne létrehozása

2.  Hozzon létre egy esemény központi névtér, az Azure portál használatával

3.  Hozzon létre egy esemény hubhoz engedélyezve van, az Azure portálon az archiválás szolgáltatással

4.  Adatok küldése a esemény-hubon keresztül csatlakozott egy Python forgatókönyvvel

5.  Olvassa el a fájlokat a archívumból és feldolgozni azokat egy másik Python forgatókönyvvel

Előfeltételek

1.  Python 2.7.x

2.  Az Azure előfizetéssel

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Azure tároló fiók létrehozása

1.  Jelentkezzen be az [Azure-portálon][].

2.  A portál bal oldali navigációs ablaktáblában kattintson az új, majd kattintson az adatok + tárhely, és válassza a tárterület-fiók.

3.  Töltse ki a tárterület a fiók lap a mezőket, és kattintson a **Létrehozás**gombra.

    ![][1]

4.  Ha megjelent a **Telepítések sikeresen befejeződött** üzenet, kattintson az új tárterület-fiók és az **alapvető tudnivalók** lap kattintson a **BLOB**. Amikor megnyílik a **Blob-szolgáltatás** lap, válassza a **+ tároló** tetején. Nevezze el a tároló **archívumba**, majd zárja be a **Blob-szolgáltatás** lap.

5.  Kattintson a bal oldali lap **hívóbetűk** , és másolja a tárterület-fiók neve és a **azonosító1**értékét. Mentse ezeket az értékeket a Jegyzetfüzet programmal vagy más ideiglenes helyre.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Események küldeni az esemény központi Python parancsfájl létrehozása

1.  Nyissa meg a kedvenc Python szerkesztő, például a [Visual Studio kódot][].

2.  Hozzon létre egy **sender.py**nevű parancsfájlt. A parancsfájl 200 események küld a esemény-központját. Egyszerű környezeti kiejtés JSON küldött legyenek is.

3.  Sender.py illessze be a következő kódot:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Frissítse a névtér neve és a fő érték, amely szerezte be az esemény hubok névtér létrehozásakor használandó előző kódot.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Hozzon létre egy Python parancsprogramot, olvassa el az archivált fájlok

1.  Adja meg a lap, és kattintson a **Létrehozás**gombra.

2.  Hozzon létre egy **archivereader.py**nevű parancsfájlt. Ez a parancsfájl az archivált fájlok beolvassa, és hozzon létre egy eszközt, hogy csak az adatokat, hogy az eszköz írni egy fájlt.

3.  Archivereader.py illessze be a következő kódot:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Ügyeljen arra, hogy a hívást illessze be a megfelelő értékeket a fióknév tárhely és a kulcs `startProcessing`.

## <a name="run-the-scripts"></a>A parancsfájlok futtatása

1.  Nyisson meg egy parancssort, amelynek a Python a utat, és futtassa a parancsok Python kapcsolatban előzetesen szükséges csomagok telepítése:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Ha bármelyik azure-tárhely korábbi verziójában vagy azure szüksége lehet használni a **– frissítése** beállítás

    Előfordulhat, hogy a következő futtatásához is szükséges (a legtöbb rendszeren nem szükséges):

    ```
    pip install cryptography
    ```

2.  A címtár módosítása sender.py és archivereader.py mentett bárhol, és ez a parancs futtatásával:

    ```
    start python sender.py
    ```
    
    Ekkor megjelenik egy új Python folyamatot a feladó futtatásához.

3. Most Várjon néhány percet, amíg az archiválás futtatásához. Az eredeti parancs ablakba írja be a következő parancsot:

    ```
    python archivereader.py
    ```

A archív processzor valamennyi BLOB a tárterület-fiók/tároló töltse le a helyi könyvtár használja. Azt feldolgozni azokat, nem üres, írja be az eredmények csv-fájlként át a helyi directory.

## <a name="next-steps"></a>Következő lépések

Akkor talál további információt esemény hubok megtalálhatók az alábbi hivatkozások:

- [Esemény hubok áttekintése archiválása][]
- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.
- [Esemény hubok – áttekintés][]
 

[Azure portál]: https://portal.azure.com/
[Esemény hubok áttekintése archiválása]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio kódot.]: https://code.visualstudio.com/
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
