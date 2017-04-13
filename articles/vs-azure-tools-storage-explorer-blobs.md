<properties
    pageTitle="Azure Blob-tárolóhoz erőforrások (előzetes verzió) tároló Intézővel |} Microsoft Azure"
    description="Azure Blob-tárolók és BLOB-tároló Intézővel (előzetes verzió) kezelése"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Azure Blob-tárolóhoz erőforrások tároló Intézővel (előzetes verzió)

## <a name="overview"></a>– Áttekintés

[Azure Blob-tárolóhoz](./storage/storage-dotnet-how-to-use-blobs.md) olyan szolgáltatás, a nagy mennyiségű strukturálatlan adatokat, például a szöveg és a bináris adatokat, bárhol is elérhető a világ HTTP-és HTTPS tárolásához.
Blob-tárolóhoz kattintva jelenítse meg az adatok nyilvánosan a világ, vagy alkalmazás adatok nyilvánosan tárolására használható. Ebben a cikkben megismerheti fogja tároló Explorer (előzetes verzió) készült blob tárolók és BLOB használatával.

## <a name="prerequisites"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

- [Töltse le és telepítse a tárhely Explorer (előzetes verzió)](http://www.storageexplorer.com)
- [Csatlakozás Azure tárterület-fiók vagy szolgáltatás](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Hozzon létre egy blob-tárolóhoz

Az összes BLOB egy blob-tárolóban, egyszerűen logikai csoportja BLOB, amely kell lennie. Fiók tartalmazhat tárolók tetszőleges számú és minden egyes tároló BLOB tetszőleges számú tárolhat.

A következő lépések bemutatják, hogy miként hozhat létre egy blob-tároló tároló Explorerből (előzetes verzió).

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a tárhely fiókot, amelyen belül létrehozandó a blob-tárolóhoz.
1.  Kattintson a jobb gombbal a **Tárolók Blob**, és - - helyi menüből válassza **Blob-tárolóhoz létrehozása**.

    ![Hozzon létre blob tárolók helyi menüje][0]

1.  Szövegdoboz a **Tárolók Blob** -mappa alatti fog megjelenni. Írja be a nevét a blob-tárolóhoz. Szabályokat és korlátozásokat listát [tároló elnevezési szabályai](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) szakaszban látható blob tárolók elnevezési.

    ![Tárolók Blob-szövegmező létrehozásához][1]

1.  Nyomja le az **Enter** feladónak hozza létre a blob-tárolóhoz vagy **Esc** kattintva megszakíthatja a műveletet. A blob-tárolóhoz sikeres létrehozását követően akkor jelenik meg a tároló kijelölt fiók **Blob tárolók** mappában.

    ![Létrehozott BLOB-tárolóhoz][2]

## <a name="view-a-blob-containers-contents"></a>Blob-tároló tartalmának megtekintése

Tárolók BLOB BLOB és mappák (tartalmazhatja BLOB) tartalmaz.

A következő lépések bemutatják, hogy miként tekintheti meg egy blob-tároló tároló Explorerből (előzetes verzió) tartalma:

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a megtekinteni kívánt a blob-tárolóhoz tartalmazó tároló fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Kattintson a jobb gombbal a megtekinteni kívánt blob-tárolóhoz, és – – helyi menüből válassza ki a **Blob-tárolóhoz szerkesztő megnyitása**.
Kattintson duplán a a megtekinteni kívánt blob-tárolóban is.

    ![Nyissa meg a blob tároló szerkesztő helyi menüje][19]

1.  A főablak a blob-tárolóhoz tartalmát jeleníti meg.

    ![BLOB-tárolóhoz szerkesztő][3]

## <a name="delete-a-blob-container"></a>Törölje a blob-tárolóhoz

BLOB tárolók is egyszerűen létrehozni és törölni, szükség szerint. (Egyes BLOB törlésének módját megjelenítéséhez lásd a szakaszban [a blob-tároló kezelése BLOB](./#managing-blobs-in-a-blob-container).)

A következő lépések bemutatják, hogyan lehet tároló Explorerből (előzetes verzió) blob-tároló törlése:

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a megtekinteni kívánt a blob-tárolóhoz tartalmazó tároló fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Kattintson a jobb gombbal a törölni kívánt blob-tárolóhoz, és – – helyi menüből válassza a **Törlés**.
Billentyűkombinációt is használhatja a **törlése** a kijelölt blob-tárolóhoz törlése.

    ![Törölje a blob-tárolóhoz helyi menüje][4]

1.  A megerősítést kérő párbeszédpanelen válassza a **Igen** .

    ![Tároló megerősítő blob törlése][5]

## <a name="copy-a-blob-container"></a>Másolja a blob-tárolóhoz

Tárterület Explorer (előzetes verzió) lehetővé teszi, hogy a blob-tároló másolja a vágólapra, és illessze be egy másik tárterület-fiókba, hogy blob-tárolóhoz. (Az egyes BLOB másolása megtekintéséhez lásd a szakaszban [a blob-tároló kezelése BLOB](./#managing-blobs-in-a-blob-container).)

A következő lépések bemutatják, hogyan lehet blob-tároló másolása egyik tárterület-fiók a másikba.

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a másolni kívánt a blob-tárolóhoz tartalmazó tárolási fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Kattintson a jobb gombbal a másolni kívánt blob-tárolóhoz és – a helyi menüből - válassza a **Másolás Blob-tárolóhoz**.

    ![Másolja a blob-tárolóhoz helyi menüje][6]

1.  Kattintson a jobb gombbal a kívánt "cél" tárhely a fiókot, amelybe be szeretné illeszteni a blob-tárolóhoz, és – a helyi menüből - válassza a **Beillesztés Blob-tárolóhoz**.

    ![Beillesztés blob-tárolóhoz helyi menüje][7]

## <a name="get-the-sas-for-a-blob-container"></a>Ismerkedés a Társítások blob-tárolóhoz

[Megosztott access aláírás (Társítások)](./storage/storage-dotnet-shared-access-signature-part-1.md) a tárterület-fiókjában erőforrások delegált hozzáférést biztosít.
Ez azt jelenti, hogy adhat az anélkül, hogy a fiók hívóbetűk megosztása egy ügyfél korlátozott az időt és a megadott engedélyek tartalmazó adott időszakra vonatkozóan a tárterület-fiókjában objektumok engedélyeit.

A következő lépések bemutatják, hogy miként hozhat létre egy Társítások blob-tárolóhoz:

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a a blob-tároló, amelynek a Társítások első kívánt tartalmazó tárterület-fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Kattintson a jobb gombbal a kívánt blob-tárolóhoz, és – a helyi menüből - jelölje ki az **Első megosztott Access aláírás**.

    ![Ismerkedés a Társítások helyi menüje][8]

1.  A **Megosztott Access-aláírás** párbeszédpanelen adja meg a házirendet, a kezdés és a lejárati dátumot, az időzóna és hozzáférési szintet azt szeretné, hogy az erőforrás.

    ![Társítások lehetőségekhez][9]

1.  Ha végzett a Társítások beállítások megadásával, válassza a **létrehozása**lehetőséget.

1.  A második **Megosztott Access-aláírás** párbeszédpanel, mely tartalmazza az URL-CÍMEK és QueryStrings a tárhely erőforrás eléréséhez használható együtt a blob-tároló ezután jeleníti meg.
Válassza a **Másolás** a vágólapra másolni kívánt URL-címe melletti.

    ![Társítások URL másolása][10]

1.  Ha elkészült, válassza a **Bezárás**.

## <a name="manage-access-policies-for-a-blob-container"></a>Access-házirendek kezelése blob-tárolóhoz

A következő lépések bemutatják, hogy miként kezelheti (Hozzáadás és Eltávolítás) blob-tároló házirendek elérése:

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a a blob-tároló amelynek access házirendek kezelése kívánt tartalmazó tárterület-fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Jelölje ki a kívánt blob-tárolóhoz, és – a helyi menüből - jelölje be az **Access-házirendek kezelése**.

    ![Kezelheti a hozzáférési szabályok helyi menüje][11]

1.  A **Hozzáférési** párbeszédpanelen a felsorolásban talált a kijelölt blob-tárolóhoz már létrehozott access házirendeket.

    ![Hozzáférési házirend-beállításai][12]        

1.  Lépései attól függően, hogy a hozzáférési házirend management tevékenységhez:

    - **Hozzáadás új hozzáférési házirend** - kattintson a **Hozzáadás gombra**. Miután létrehozott, a **Hozzáférési** párbeszédpanelen az újonnan hozzáadott hozzáférési házirend (az alapértelmezett beállítások) jeleníti meg.
    - A **hozzáférési házirend szerkesztése** – minden kívánt szerkesztések és válassza a **Mentés**.
    - **Távolítsa el az access-házirend** - választó **eltávolítása** az eltávolítani kívánt hozzáférési házirend mellett.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Nyilvános jogosultsági szintet blob-tárolóhoz

Alapértelmezés szerint minden blob-tárolóhoz értéke "Nincs nyilvános hozzáférés".

A következő lépések bemutatják, hogyan lehet blob-tároló nyilvános hozzáférési szintet adja meg.

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a a blob-tároló amelynek access házirendek kezelése kívánt tartalmazó tárterület-fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Jelölje ki a kívánt blob-tárolóhoz, és - - helyi menüből válassza ki a **Nyilvános hozzáférési szint beállítása**.

    ![Beállítása a nyilvános hozzáférés szintű helyi menüje][13]

1.  A **Tároló nyilvános hozzáférési szint beállítása** párbeszédpanelen adja meg a kívánt hozzáférési szintet.

    ![Nyilvános hozzáférés szintű beállításainak megadása][14]

1.  Válassza az **alkalmazás**.

## <a name="managing-blobs-in-a-blob-container"></a>Blob-tárolóban BLOB kezelése

Blob-tároló létrehozása után adott blob-tárolóban blob feltöltése, blob letöltése a helyi számítógépre, nyissa meg a helyi számítógépen, és még sok mással blob.

Az alábbi lépéseket a blob-tárolóban lévő bemutatják, hogyan kezelheti a BLOB (és mappák).

1.  Nyissa meg a tárhely Explorer (előzetes verzió).
1.  A bal oldali ablaktáblában bontsa ki a kívánt kezelése a blob-tárolóhoz tartalmazó tároló fiókot.
1.  Bontsa ki a tárterület-fiók **Blob tárolók**.
1.  Kattintson duplán a megtekinteni kívánt blob-tárolóhoz.
1.  A főablak a blob-tárolóhoz tartalmát jeleníti meg.

    ![View blob-tárolóhoz][3]

1.  A főablak a blob-tárolóhoz tartalmát jeleníti meg.

1.  Kövesse ezeket a lépéseket, attól függően, hogy a feladatot kíván végrehajtani:

    - **Fájlok feltöltése a blob-tárolóhoz**

        1.  A főablak eszköztáron válassza a **Feltöltés**, majd **A fájlok feltöltése** a legördülő menüből.

            ![Töltse fel a Fájl menü][15]

        1.  A **fájlok feltöltése** párbeszédpanelen jelölje be a három pontra (**…**) gombra, jelölje ki a feltölteni kívánt fájlokat a **fájlok** szövegdoboz jobb oldalán.

            ![Beállítások a fájlok feltöltése][16]

        1.  Adja meg a **Blob**típusát. Az [első lépések az Azure Blob-tárolóhoz .NET használatával](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) cikkből megtudhatja, a különféle blob közötti különbségeket.

        1.  Tetszés szerint adjon meg egy cél mappát, amelybe feltöltődnek a kijelölt fájlokat. Ha a célmappát nem létezik, létrejön.

        1.  Jelölje ki a **feltölteni**.

    - **Töltse fel a mappa blob-tárolóhoz**

        1.  A főablak eszköztáron válassza a **tölthet fel**, majd a **Mappa töltse fel** a legördülő menüből.

            ![Feltöltés menü mappa][17]

        1.  **Töltse fel a mappa** párbeszédpanelen jelölje be a három pontra (**…**) gombra, jelölje ki a mappát, amelynek tartalmát szeretné feltölteni a **mappa** szövegdoboz jobb oldalán.

            ![Töltse fel a mappa beállításai][18]

        1.  Adja meg a **Blob**típusát. Az [első lépések az Azure Blob-tárolóhoz .NET használatával](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) cikkből megtudhatja, a különféle blob közötti különbségeket.

        1.  Tetszés szerint adja meg, amelybe feltöltődnek a kijelölt mappa tartalmát tároló mappa. Ha a célmappát nem létezik, létrejön.

        1.  Jelölje ki a **feltölteni**.

    - **Töltse le a helyi számítógépre blob**

        1.  Jelölje ki a letölteni kívánt blob.

        1.  Válassza a főablak eszköztár **letöltése**.

        1.  **A letöltött blob mentési helyének megadása** párbeszédpanelen adja meg a helyre, ahol a letöltött blob, és tegyen szeretné a nevét.  

        1.  Válassza a **Mentés**.

    - **Nyissa meg a helyi számítógépen blob**

        1.  Jelölje ki a megnyitni kívánt blob.

        1.  A főablak eszköztáron válassza a **Megnyitás**lehetőséget.

        1.  A blob-mait letölt, és a blob mögöttes fájltípushoz társított alkalmazással megnyitott.

    - **Blob másolja a vágólapra**

        1.  Jelölje ki a másolni kívánt blob.

        1.  A főablak eszköztáron válassza a **Másolás**lehetőséget.

        1.  A bal oldali ablaktáblában nyissa meg a másik blob-tárolóhoz, és duplán rákattintva megtekintéséhez kattintson a főablak.

        1.  A főablak eszköztáron válassza a **Beillesztés** , a blob másolatot szeretne készíteni.

    - **Blob törlése**

        1.  Jelölje ki a törölni kívánt blob.

        1.  Kattintson a főablak eszköztáron válassza a **Törlés**elemre.

        1.  A megerősítést kérő párbeszédpanelen válassza a **Igen** .

## <a name="next-steps"></a>Következő lépések

- A [legújabb tároló Explorer (előzetes verzió) engedje fel a jegyzetek és a videók](http://www.storageexplorer.com)megtekintése
- Megtudhatja, hogyan hozhat [létre az alkalmazások Azure BLOB, a táblázatok, a sorok, és a fájlokat](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png