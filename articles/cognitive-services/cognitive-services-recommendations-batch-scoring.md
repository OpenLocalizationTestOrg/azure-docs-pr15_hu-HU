
<properties
    pageTitle="Javaslatok első kötegekben: a gép tanulási javaslatok API |} Microsoft Azure"
    description="Azure gépi tanulási javaslatok – első javaslatok egy csoportba"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Javaslatok kötegekben beszerzése

>[AZURE.NOTE] Javaslatok első kötegekben bonyolultabb mint javaslatok egy első egyszerre. Jelölje be az API-k egyetlen kérelem javaslatok beszerzése információt:

> [Javaslatok elemről elemre](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Felhasználó-elem javaslatok](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Köteg pontozási csak akkor működik, a 2016 július 21 után létrehozott buildjeiben.


Vannak helyzetek, amelyben kell beszereznie javaslatok egynél több elemet egyszerre. Ha például, amelyek érdekelhetik a javaslatok-gyorsítótár létrehozása, vagy akár elemzése áll ajánlások típusú.

Köteg pontozási műveleteket, mint azt hívja őket, műveletek, amelyek aszinkron. A kérelmet, megvárja, amíg a művelet befejezéséhez és az eredmények majd gyűjtése szüksége.  

További kell pontos, ezek az itt leírt lépések:

1.  Hozzon létre egy Azure tároló tároló, ha nincs telepítve egyik már.
2.  Töltse fel a bemeneti fájl, amely ismerteti az egyes Azure Blob-tárolóhoz ajánlási igényeinek.
3.  A pontozási köteg feladat kick-Start.
4.  Várja meg, amíg az aszinkron művelet befejezéséhez.
5.  A művelet befejeződése után feljegyzése az eredmények Blob-tárolóhoz

Vegyük végigvezetik egyes lépéseket.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Tárterület tároló létrehozása, ha nincs telepítve egyik már

Az [Azure-portálra](https://portal.azure.com) , és hozzon létre egy új tárterület-fiókot, ha nincs telepítve egyik már. Ehhez nyissa meg **Új** > **adatok** + **tároló** > **Tárterület-fiókot**.

Miután tároló fiókkal rendelkezik, a hol tárolja azt a bemeneti és kimeneti a köteg végrehajtás blob tárolók létrehozásához szükséges.

Itt leíró bemeneti fájl feltöltése vegyük Blob-tárolóhoz – mindegyik a ajánlási kér hívja a fájl input.json.
Után tároló, feltölteni a fájlt, amely ismerteti az egyes a kérelmeket, amely a javaslatok szolgáltatásból végrehajtásához szükséges.

Egy köteg végezheti el az adott építés csak egy típusú kérést. Azt fogja ismertetik a következő szakaszban határozza meg ezt az információt. Most tegyük fel, hogy azt fogja hajt végre elem javaslatok adott építés ki. A bemeneti fájl akkor az egyes kérelmek (ebben az esetben, a rendező elemek) bemeneti adatokat tartalmazza.

Ez a példa a input.json fájl néz ki:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Amint látható, a fájl egy JSON fájlt, ahol a kérések mindegyikének javaslatok összehívásokkal szükséges információkat. A kéréseket, amelyeket kell teljesíteni hasonló JSON fájl létrehozása, és másolja a vágólapra az imént létrehozott Blob-tárolóban lévő tárolóhoz.

## <a name="kick-start-the-batch-job"></a>A köteg feladat kick-Start

A következő lépésként küldje egy köteg új feladatot. További tudnivalókért olvassa el az [API-hivatkozás](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Az API összehívás törzsében kell megadása a helyek, ahol a fájlok hiba, a kimeneti és a beviteli kell tárolni. Meg kell definiálása, amelyek helyekre eléréséhez szükséges hitelesítő adatait. Ezeken kívül kell néhány paramétert alkalmazása az egész tétel (típusa javaslatok szeretne kérni, a modell/készítése használja, a hívás, találatok számát, és így tovább.)

Íme egy példa arról, milyen összehívás törzsébe kell:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Az alábbi néhány fontos Megjegyzés:

-   Jelenleg **authenticationType** mindig legyen **PublicOrSas**.

-   Ismerkedés a javaslatok API-t olvasási és írási a/Blob-tároló fiókja lehetővé teszi a megosztott Access-aláírás (Társítások) jogkivonat szüksége. [A javaslatok API](../storage/storage-dotnet-shared-access-signature-part-1.md)lapon található Társítások tokenek létrehozásához részletes tájékoztatást.

-   Jelenleg támogatott csak **APINÉV** **ItemRecommend**, amelyet az javaslatok elemről elemre. Kötegelés jelenleg nem támogatja felhasználói-elem javaslatok.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Várja meg az aszinkron művelet befejezéséhez

A köteg művelet indításakor a válasz a művelet helyfüggő fejléc, amellyel az információkat, amelyek a műveletet nyomon követéséhez szükséges adja eredményül.
A művelet csak akkor nyomon követéséhez a művelet build művelet működnek, mint a [Művelet állapota API beolvasásához]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), használatával nyomon követni.

## <a name="get-the-results"></a>Az eredmény érhető el

Miután a művelet befejeződött, feltéve, hogy nem léptek fel hibák, az eredmények gyűjthet a eredményéből Blob-tároló.

Az alábbi példában megjelenítése a kimenet néz. Ebben a példában egy köteg csak két kérelem (fentebb) az eredmény megjelenítése azt.

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Korlátozása

-   Előfizetésenként egyszerre csak egy köteg feladat hívható meg.
-   Egy feladat beviteli parancsfájl 2 MB-nál több nem lehet.
