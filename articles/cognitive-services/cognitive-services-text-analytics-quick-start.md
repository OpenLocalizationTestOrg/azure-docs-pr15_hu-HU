<properties
    pageTitle="Rövid útmutató: gépi tanulási szöveg Analytics API-khoz |} Microsoft Azure"
    description="Azure gépi tanulási szöveg Analytics – rövid útmutató"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>A szöveg Analytics API-k feltárása sentiment, a fő mondatok, a témakörök és a nyelv – első lépések

<a name="HOLTop"></a>

Jelen dokumentum ismerteti, hogyan alkalmazhat beépített a szolgáltatásra vagy az alkalmazást, hogy az a [Szöveg Analytics API -khoz](//go.microsoft.com/fwlink/?LinkID=759711).
Sentiment, a fő mondatok, a témakörök és a nyelvi feltárása a szöveget a következő API-khoz is használhatja. [Kattintson ide az interaktív bemutatása a felület.](//go.microsoft.com/fwlink/?LinkID=759712)

Olvassa el az [API-definíciók](//go.microsoft.com/fwlink/?LinkID=759346) az API-k műszaki dokumentációját.

Ez az útmutató az API-k 2-es verziójú szolgál. További információ: a az API-hoz, [olvassa el a dokumentumra mutató](../machine-learning/machine-learning-apps-text-analytics.md)1-es verziójú.

Ez az oktatóanyag végén fogja tudni programozottan feltárása:

- **Sentiment** – az szöveg pozitív vagy negatív?

- **Kulcs mondatok** – Mik azok a személyek egy külön cikkben tárgyal?

- **Témakörök** - Mik azok a személyek cikkekben számos különböző beszél meg?

- **Nyelvek** - milyen nyelvű szöveget ír?

Látható, hogy ez az API 1 tranzakció elküldött dokumentum használati költségek. Példaként Ha egyetlen telefonál 1000 dokumentumok sentiment kér 1000 tranzakciók fog vonni.



<a name="Overview"></a>
## <a name="general-overview"></a>Általános áttekintés ##

A dokumentum részletes útmutatásra. A célja, hogy végigvezetik Önt a modell képzése, és mutasson az erőforrásokat, amely lehetővé teszi, hogy behelyezi gyártási szükséges lépéseket. A gyakorlat körülbelül 30 percig tart.

Ezek a tevékenységek használni kell a szerkesztő és RESTful végpontjait hívja meg a választott nyelv.

Lássunk hozzá!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>1 – feladat aláírási tagjának a szöveg Analytics API-hoz ####

Ebben a feladatban fog regisztrál az a szöveg analytics szolgáltatás.

1. Keresse meg a **Kognitív szolgáltatások** az [Azure-portálra](//go.microsoft.com/fwlink/?LinkId=761108) , és győződjön meg arról **Szöveg Analytics** API-típus van kiválasztva.

1. Válasszon egy csomagot. Az **5000 tranzakciók/hónap ingyenes réteg**kijelölheti. Egy ingyenes csomagra van, akkor nem ráterheljük a szolgáltatással. Szüksége lesz az Azure előfizetés bejelentkezni. 

1. Töltse ki a többi mezőket, és hozza létre a fiókot.

1. Miután előfizetett szöveg elemzéséhez, keresse meg a a **API billentyűt**. Másolja a vágólapra az elsődleges kulcsot, lesz szükség szerint az API-szolgáltatások használatakor.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Tevékenység-2 - sentiment, fő kifejezéseket és nyelvek ####

Nagyon egyszerűen sentiment, fő kifejezéseket és nyelvek feltárása a kívánt szöveget. Programozottan kaphat ugyanazt az eredményt, a [bemutató élmény](//go.microsoft.com/fwlink/?LinkID=759712) adja eredményül.

>[AZURE.TIP] Sentiment-elemzés ajánlott szöveg felosztása mondatok. Ez általában a sentiment előrejelzések nagyobb pontossággal vezet.

Ne feledje, hogy a támogatott nyelvek az alábbi képlettel történik:

| A szolgáltatás | Támogatott nyelvi kódok |
|:-----|:----|
| Sentiment | `en`(Angol nyelven) `es` (angolul), `fr` (magyar) `pt` (Portugália) |
| Fontos kifejezések | `en`(Angol nyelven) `es` (angolul), `de` (német), `ja` (japán) |


1. Szüksége lesz a fejlécek meg az alábbiakat. Ne feledje, hogy JSON jelenleg az API csak elfogadott beviteli formátumát. XML nem támogatott.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Ezután formázni szeretné a bemeneti sorait a JSON-ban. A sentiment, fő kifejezéseket és nyelvi beállítások formátuma azonos. Ne feledje, hogy minden azonosító egyedinek kell lennie az azonosító által visszaadott a rendszer. Egyetlen dokumentum, amely küldhetők maximális mérete 10 KB-nál, és 1MB beküldött beviteli teljes maximális mérete. 1000-nél több dokumentumok egy hívás be lehet nyújtani. 100 hívások percenkénti mértéke ráta korlátozása létezik – ezért azt javasoljuk, hogy egyetlen hívás dokumentumok nagy mennyiségű céljából. Nyelv nem kötelező paraméter meg kell adni, ha nem angol nyelvű szöveget elemzése. Példa beviteli alább látható, ahol a választható paraméter `language` sentiment az elemzés vagy kulcs kifejezést kinyerésének megtalálható:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. A rendszer a bemeneti sentiment, a fő kifejezések és a nyelvet a **bejegyzés** hívást szeretne kezdeményezni. Az URL-címek a következőképpen jelenik meg:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. A hívás értéket ad vissza egy JSON az azonosítók válasz formázott és tulajdonságok észlelt. Példa a kimenet sentiment az alább látható (a kizárt részletek). Sentiment, amíg az egy 0 és 1 közötti pontszámhoz minden dokumentum adhatók vissza.

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Tevékenység-3 - témakörök a szöveg corpus kimutatására ####

Ez a egy újonnan kiadott API, mely felső észlelt témakörök listáját adja eredményül küldött szöveges rekordok. A tematikus azonosítjuk a fő kifejezést, amely lehet egy vagy több kapcsolódó szavak. Az API tervezve jól a rövid, az emberi írt szöveget, például az ellenőrzések és a felhasználó visszajelzést.

Az API használatához **100 szöveges bejegyzések legalább** benyújtandó, de témakörök feltárása rekordok ezer több száz át lett tervezve. Bármelyik nem angol nyelvű vagy rekordokat legfeljebb 3 szavakat törlődik, és ezért nem rendeli hozzá témaköröket. A témakör kimutatására egy dokumentumot, amely küldhető maximális mérete 30KB, és beküldött beviteli teljes maximális mérete 30MB. A témakör észlelési legfeljebb 5 beküldött elemek 5 percenként ráta.

Van két további **választható** bemeneti paramétereket, amely segítséget nyújt az eredmények minőségének:

- **Állítsa le a szavakat.**  A következő szavak és a Bezárás űrlapok (például többes számú változatát) az egész témakör észlelési folyamat fognak szerepelni. Ezzel a gyakori szavakat (a példában "hiba", "hiba" és "felhasználó" ügyfél panaszok szoftverről a megfelelő beállításokat is lehet). Mindegyik szöveges karakterláncot egyetlen szó kell lennie.
- **Állítsa le a kifejezések** – műveletkifejezés listáját visszaadott kell zárni. Ezzel a kizárása általános témakörök, amelyek nem szeretné látni a találatok között. Ha például "Microsoft" és "Azure" akkor kizárása témakörök a megfelelő beállításokat. Karakterláncok tartalmazhat több szót.

Kövesse ezeket a lépéseket követve témakörök észleli a kívánt szöveget.

1. Formázza a bemeneti a JSON-ban. Ebben az esetben is definiálása leállítása szavak és kifejezések leállítása.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Meghatározott tevékenység 2, használja az azonos fejlécek, végezze el a **bejegyzés** , a témakörök végpont hívása:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Ez ad vissza egy `operation-location` a fejléc, a válasz, ahol az érték az eredményül kapott témaköröket lekérdezés URL-címe:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. A visszaadott lekérdezési `operation-location` rendszeres összehívással **első** . Percenkénti ajánlott.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. A végpont ad vissza, a válasz, beleértve `{"status": "notstarted"}` feldolgozása, mielőtt `{"status": "running"}` feldolgozása közben és `{"status": "succeeded"}` és a kimeneti, amint befejeződött. Majd igénybe vehet, a kimenet, amely a következő formátumban (például a hiba formátumát és a dátumok kellett volna zárni ebben a példában Megjegyzés részletek) lesz:

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Megjegyzendő, hogy a sikeres válasz, a témakörök a `operations` végpont lesz a következő séma:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

A válasz minden részét magyarázatainak a következők:

**témakörök**

| Kulcs | Leírás |
|:-----|:----|
| azonosító | Minden témához egyedi azonosítója. |
| eredmény | A témakör rendelt dokumentumok száma. |
| keyPhrase | Summarizing szót vagy kifejezést a témakörnek. |

**topicAssignments**

| Kulcs | Leírás |
|:-----|:----|
| documentId | A dokumentum azonosítója. Megfelel a bemeneti szereplő azonosítója. |
| topicId | A témakör Azonosítót, amely a dokumentumban hozzá van rendelve. |
| távolság | Dokumentum-témakör viszony pontszámhoz 0 és 1 közötti. Az alsó távolság pontszám az erősebb a témakör viszony van. |

**hibák**

| Kulcs | Leírás |
|:-----|:----|
| azonosító | Egyedi azonosító bemeneti dokumentumot a hiba hivatkozik. |
| üzenet | Hibaüzenet jelenik meg. |

## <a name="next-steps"></a>Következő lépések ##

Gratulálok! Most már befejezése szöveg analytics használata az adatok. Most már kívánt vizsgálja meg a eszközzel például a [Power BI](//powerbi.microsoft.com) adatok ábrázolásához, valamint a szöveges adatok valós idejű nézetének nyújt meglátásait automatizálása.

Szöveg Analytics szolgáltatásait, például sentiment, hogy miként használható egy bot részeként fejlesztési ütemtervé a [Érzelmi Bot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) példa a Bot keretrendszer webhelyen.
