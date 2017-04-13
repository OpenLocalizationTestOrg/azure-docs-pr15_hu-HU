<properties
    pageTitle="Első lépések – tárterület Explorer (előzetes verzió) |} Microsoft Azure"
    description="Azure tároló erőforrások tároló Intézővel (előzetes verzió)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Első lépések – tárterület Explorer (előzetes verzió)

## <a name="overview"></a>– Áttekintés 

Microsoft Azure tároló Explorer (előzetes verzió) egy különálló alkalmazás, amely lehetővé teszi, hogy könnyen adatokkal végzett munkához Azure-tárhely Windows, OS X vagy Linux rendszerhez. Ebben a cikkben megismerheti a kapcsolódik, és az Azure tároló kezelésére használható különböző módszereket.

![Microsoft Azure tároló Explorer (előzetes verzió)][15]

## <a name="prerequisites"></a>Előfeltételek

- [Töltse le és telepítse a tárhely Explorer (előzetes verzió)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Csatlakozás tárterület-fiók vagy szolgáltatás

Tárterület Explorer (előzetes verzió) tároló fiókok csatlakoztatása a számtalan lehetőséget nyújt. Ide tartoznak a kapcsolódásról a tárterület-fiókok és Azure előfizetésekről más megosztott szolgáltatások Azure előfizetések társított tárterület-fiókok csatlakoztatása és még akkor is kapcsolódik, és használja az Azure tároló irányító helyi tároló kezelése:

- [Csatlakozás Azure-előfizetésbe](#connect-to-an-azure-subscription) – az Azure előfizetéshez tartozó tárterület erőforrások kezelésére.
- [Helyi fejlesztési adathordozós munka](#work-with-local-development-storage) - használatával az Azure tároló irányító helyi tárhely kezelése. 
- [Külső tárolóhoz csatolása](#attach-or-detach-an-external-storage-account) – a tárterület-fiókjához fióknév és kulcs egy másik Azure előfizetéshez tartozó tárterület erőforrások kezelése.
- [Csatolás tárterület-fiók használatával Társítások](#attach-storage-account-using-sas) – egy Társítások használja egy másik Azure előfizetéshez tartozó tárterület erőforrások kezelésére.
- [Kapcsolás szolgáltatás használata Társítások](#attach-service-using-sas) – egy adott tárhelyszolgáltatáshoz (blob-tárolóhoz, várólista vagy táblázat) kezelése a Társítások használja egy másik Azure előfizetéshez tartozó.

## <a name="connect-to-an-azure-subscription"></a>Csatlakozás az Azure előfizetéssel

> [AZURE.NOTE] Ha Azure-fiók nincs, akkor [Jelentkezzen be az ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) , vagy [a Visual Studio előfizetői előnyeinek aktiválása](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. A tároló Intézőben (előzetes verzió) **Azure-fiók beállításainak**kiválasztása 

    ![Azure-fiók beállításai][0]

1. A bal oldali ablaktáblában most már bejelentkezett minden Microsoft-fiók jeleníti meg. Másik fiókhoz szeretne csatlakozni, jelölje be a **fiók hozzáadása**, és hajtsa végre legalább egy aktív Azure előfizetéssel társított Microsoft-fiókkal jelentkezzen be a párbeszédpanelek.

    ![Fiók felvétele][1]

1. Miután sikeresen jelentkezzen be Microsoft-fiókkal, a bal oldali ablaktáblában a fiókhoz társított Azure előfizetéseket fog adataival. Jelölje ki az Azure előfizetések, amellyel, amellyel dolgozni szeretne, és válassza az **Alkalmaz**. (Jelölje ki **az összes előfizetésben** be-és kikapcsolása az összes vagy a listában szereplő Azure előfizetéseket egyik kijelölése.)

    ![Jelölje ki az Azure előfizetések][3]

1. Ettől kezdve a bal oldali ablaktáblában jelennek meg a a kijelölt Azure előfizetéseket társított tároló fiókok.

    ![A kijelölt Azure előfizetések][4]

## <a name="work-with-local-development-storage"></a>Helyi fejlesztési tárhely kezelése

Tároló Explorer (előzetes verzió) használata szemben a helyi tároló az Azure tároló irányító használatával teszi lehetővé. Lehetővé teszi, hogy kódírás szemben, és tesztelje a tárhely egy tárterület-fiókkal (mivel a tárterület-fiókot az Azure tároló irányító által éppen emulálása) rendszerre telepíthető Azure feltétlenül nélkül.

>[AZURE.NOTE] Az Azure tároló irányító jelenleg csak a Windows támogatja. 

1. Tárterület Explorer (előzetes verzió) bal oldali ablaktábláján bontsa ki a **(helyi és a kapcsolódó** > **Tároló fiókok** > **(fejlesztés)** csomópontot.

    ![Helyi fejlesztési csomópontot.][21]

1. Ha még nem telepítette az Azure tároló irányító, kérni fogja ezt az információs sáv keresztül. Az információs sáv jelenik meg, ha jelölje ki, **Töltse le a legújabb verzióját**, és telepítse a irányító. 

    ![Azure tároló irányító kérdés letöltése][22]

1. Miután telepítette a irányító, az azt jelenti, hogy létrehozása és használata a helyi BLOB, a sorok és a táblázatokat fognak rendelkezésére állni. Megtudhatja, hogyan dolgozhat minden tároló fióktípus, jelölje ki a megfelelő hivatkozásra az alábbi:

    - [Azure blob-tároló erőforrások kezelése](./vs-azure-tools-storage-explorer-blobs.md)
    - Azure fájlt tároló az erőforrások megosztása – *hamarosan* kezelése
    - Azure várólista tárolási erőforrásokat – *hamarosan* kezelése
    - Azure-táblából tárolási erőforrásokat – *hamarosan* kezelése

## <a name="attach-or-detach-an-external-storage-account"></a>Csatolása vagy leválasztása külső tárterület-fiókkal

Tárterület Explorer (előzetes verzió) lehetővé teszi a csatolhat külső tárterület-fiókra, hogy a tárterület-fiókok könnyen megoszthatók. Ez a szakasz megtudhatja, hogyan csatolhat (és leválasztása a) külső tároló fiókok.

### <a name="get-the-storage-account-credentials"></a>Ismerkedés a tárterület-fiók hitelesítő adatait

Annak érdekében, hogy a külső tároló fiók megosztani, az adott fiókból tulajdonosát kell először - fiók nevét és billentyű - hitelesítő adatok beszerzése a fiók és megosztása ezeket az információkat a személlyel, aki a fiókhoz társított (külső) csatolhat. Tárterület fiók hitelesítő adatok beszerzése teheti az Azure portálon keresztül ezeket a lépéseket követve: 

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
1.  Válassza a **Tallózás gombra**.
1.  Válassza a **tárterület-fiókok**.
1.  Jelölje ki a kívánt tárterület-fiókot a **Tárterület-fiókok** lap.
1.  A kijelölt tárterület-fiók **beállításai** lap válassza a **hívóbillentyűk**.

    ![Az Access billentyűk jelölőnégyzet][5]
    
1.  A **hívóbetűk** lap másolja a vágólapra a **Fióknév TÁRHELY** és a **kulcs 1** értéket, használatra való csatlakozáskor a tárterület-fiókot. 

    ![Hívóbetűk][6]

### <a name="attach-to-an-external-storage-account"></a>Külső tároló fiók csatolása
Szeretne csatolni egy külső tárterület-fiókot, a fiók nevét, és a billentyűk lesz szüksége. *Ismerkedés a tárterület-fiók hitelesítő adatait* szakaszból beszerzése ezeket az értékeket az Azure portálról. Megjegyzendő, hogy a portálon, a fiókkulcs neve "kulcs 1" így egy fiókkulcs kér a tárhely Intéző (előzetes verzió), ahol Ön fogja adja meg (vagy illessze be) a "kulcs 1" értéket. 
 
1.  Tárterület Explorer (előzetes verzió) kattintson a **Csatlakozás Azure tárolóhoz**lehetőséget.

    ![Azure tárolási lehetőség csatlakoztatása][23]

1.  A **Csatlakozás az Azure-tároló** párbeszédpanelen adja meg a fiókkulcs ("kulcs 1" értéket az Azure portálról), és válassza a **Tovább gombra**.

    ![Csatlakozás Azure tároló párbeszédpanel][24] 

1.  **Külső tároló csatolása** párbeszédpanelen adja meg a tárhely fióknevet a **fiók neve** mezőbe, adja meg a többi kívánt beállítást, és jelölje be a **következő** feladónak. 

    ![Külső tároló párbeszédpanel csatolása][8]

1.  A **Kapcsolat összefoglaló** párbeszédpanelen ellenőrizze az adatokat. Ha szeretne módosítható, válassza a **vissza** , és írja be újra a kívánt beállításokat. Miután elkészült, kattintson a **Csatlakozás**.

1.  Miután létrejött, a külső tárterület-fiókot **(külső)** a tárhely fiókot szeretne fűzni a szöveg jelenik meg. 

    ![Csatlakozás külső tároló fiókhoz eredménye][9]

### <a name="detach-from-an-external-storage-account"></a>Egy külső tárterület-fiókból leválasztása

1.  Kattintson a jobb gombbal a leválasztani kívánt külső tárterület-fiókot, és – – helyi menüből válassza ki a **Leválasztás**.

    ![A tárolási lehetőség leválasztása][10]

1.  A megerősítést kérő üzenetpanel jelenik meg, válassza az **Igen gombra** kattintva erősítse meg a terméknek a külső tárterület-fiókból.

## <a name="attach-storage-account-using-sas"></a>Csatolása Társítások tárterület-fiókba

Egy [Társítások (megosztott Access-aláírás)](storage/storage-dotnet-shared-access-signature-part-1.md) ad a rendszergazdák az Azure előfizetés az azt jelenti, hogy hozzáférést a tárterület-fiókjába, ideiglenes jelleggel anélkül, hogy azok Azure előfizetés hitelesítő adatait. 

Ezt mutatják be, hogy tegyük fel, hogy ' a ' felhasználó egy Azure előfizetés rendszergazda, és ' a ' felhasználó szeretne biztosít a tárterület-fiók elérésére bizonyos engedélyekkel rendelkező korlátozott ideig engedélyezése:

1. ' A ' felhasználó egy Társítások (a kapcsolati karakterláncot, a tárterület-fiókom álló) hoz létre, és a megfelelő engedélyekkel rendelkező egy adott időszakra.
1. ' A ' felhasználó megosztja a személy, aki a fiókhoz való hozzáférést a tárterület - biztosít, ebben a példában a Társítások.  
1. Biztosít a fiókhoz tartozó ' a ' felhasználó használja a megadott Társítások csatolása tároló Explorer (előzetes verzió) használja. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>A fiók meg szeretné osztani a Társítások beszerzése

1.  Tárterület Explorer (előzetes verzió) kattintson a jobb gombbal a megosztani kívánt tárterület-fiók, és - a helyi menüből - jelölje ki az **Első megosztott Access aláírás**.

    ![Ismerkedés a Társítások a helyi menü beállítás][13]

1. Az **Access aláírás megosztott** párbeszédpanelen adja meg a időkereten és a fiók kívánt engedélyeket, és válassza a **Create**.

    ![Társítások párbeszédpanel jelenik meg][14]
 
1. A második **Megosztott Access-aláírás** párbeszédpanel jelenik meg a Társítások megjelenítéséhez. Válassza a **Másolás** melletti kattintva másolja a vágólapra a **Kapcsolati karakterlánc** . Válassza a **Bezárás** gombra a párbeszédpanel bezárásához.

### <a name="attach-to-the-shared-account-using-the-sas"></a>A megosztott fiókra a Társítások csatolása

1.  Tárterület Explorer (előzetes verzió) kattintson a **Csatlakozás Azure tárolóhoz**lehetőséget.

    ![Azure tárolási lehetőség csatlakoztatása][23]

1.  A **Csatlakozás az Azure-tároló** párbeszédpanelen adja meg a kapcsolati karakterláncot, és válassza a **Tovább gombra**.

    ![Csatlakozás Azure tároló párbeszédpanel][24] 

1.  A **Kapcsolat összefoglaló** párbeszédpanelen ellenőrizze az adatokat. Ha szeretne módosítható, válassza a **vissza** , és írja be újra a kívánt beállításokat. Miután elkészült, kattintson a **Csatlakozás**.

1.  Miután csatolt, a tárhely fiók hozzáfűzi a fiók nevét, a megadott szöveget (Társítások) jelenik meg.

    ![Eredmény: a csatlakoztatott fiók Társítások][17]

## <a name="attach-service-using-sas"></a>Csatolása Társítások használ szolgáltatási

A szakasz [csatolása tárterület-fiókba Társítások](#attach-storage-account-using-sas) azt szemlélteti, hogyan Azure előfizetés rendszergazda is hozzáférést ideiglenes tárterület-fiókhoz által létrehozása (és megosztása) egy Társítások a tárterület-fiókom. A Társítások Hasonlóképpen, egy adott szolgáltatás (blob-tárolóhoz, várólista vagy táblázat) belül a tárterület-fiók hozhatók létre.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>A szolgáltatás meg szeretné osztani a Társítások létrehozása

Ebben a környezetben szolgáltatás lehet egy blob-tárolóhoz, várólista vagy táblázatot. Az alábbi szakaszok ismertetik a felsorolt szolgáltatás a Társítások létrehozása:

- [Ismerkedés a Társítások blob-tárolóhoz](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- A Társítások beszerzése fájlmegosztás – *hamarosan*
- A Társítások beszerzése várólista – *hamarosan*
- A Társítások első tábla – *hamarosan*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>A Társítások megosztott fiók szolgáltatás csatolása

1.  Tárterület Explorer (előzetes verzió) kattintson a **Csatlakozás Azure tárolóhoz**lehetőséget.

    ![Azure tárolási lehetőség csatlakoztatása][23]

1.  A **Csatlakozás az Azure-tároló** párbeszédpanelen adja meg a Társítások URI, és válassza a **Tovább gombra**.

    ![Csatlakozás Azure tároló párbeszédpanel][24] 

1.  A **Kapcsolat összefoglaló** párbeszédpanelen ellenőrizze az adatokat. Ha szeretne módosítható, válassza a **vissza** , és írja be újra a kívánt beállításokat. Miután elkészült, kattintson a **Csatlakozás**.

1.  Miután csatolt, az újonnan csatlakoztatott szolgáltatás a **(Service Társítások)** csomópontot a jelenik meg.

    ![A megosztott szolgáltatás Társítások csatolása eredménye][20]

## <a name="search-for-storage-accounts"></a>Tárterület partnerek keresése

Ha tárterületet fiókok hosszú listáját, egy gyorsan úgy találhatja meg egy adott tárterület-fiókkal, a keresőmező használata a bal oldali ablaktábla tetején. 

Beírás közben szót a Keresés mezőbe, a bal oldali ablaktáblában csak a tárhely partnereket, amelyek megegyeznek a keresés értéket akár beírta pont jeleníti meg. Az alábbi képernyőfelvételen azt szemlélteti, hogy egy példa hol lehet már szeretne keresni az összes tároló fiókhoz, amikor a tárterület-fiók neve "tarcher" szöveget tartalmazza.

![Tárterület-fiók keresés][11]
    
Ha törölni szeretné a keresést, az **x** gombra a Keresés mezőbe.

## <a name="next-steps"></a>Következő lépések
- [Azure blob tároló erőforrások tároló Intézővel (előzetes verzió)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
