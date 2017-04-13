<properties
    pageTitle="Gépi tanulási API-khoz: Szöveg Analytics |} Microsoft Azure"
    description="A Microsoft gépi tanulási szöveg Analytics API-khoz sentiment analysis, a kulcs kifejezést kivonása, a nyelvfelismerés és a tematikus kimutatás strukturálatlan szöveg elemzéséhez használható."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Gépi tanulási API-khoz: Szöveg elemzésének Sentiment, főbb kifejezést kivonása, a nyelvfelismerés és a tematikus kimutatásra

>[AZURE.NOTE] Ez az útmutató az API 1-es verziójú szolgál. 2-es verzióját, [**olvassa el a dokumentumhoz**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). 2-es verziójú ettől kezdve ez az API az előnyben részesített verzióját.

## <a name="overview"></a>– Áttekintés

A szöveg Analytics API szöveg analytics [webszolgáltatásokhoz](https://datamarket.azure.com/dataset/amla/text-analytics) Azure gépi tanulási épülő együttese. Az API strukturálatlan szöveg, például sentiment elemzés, fő kifejezést kivonása, nyelvfelismerés és témakör észlelési tevékenységek elemzéséhez használható. Oktatóanyag adatok nélkül ez az API használatára van szükség: csak az a szöveg adatokat vihet. Ez az API a legjobb előadásához az osztályjegyzetfüzet az előrejelzések feldolgozási technikák speciális természetes nyelvi használja.

A [bemutató webhely](https://text-analytics-demo.azurewebsites.net/), ahol is talál [a minták](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) a szöveg analytics megvalósításáról a és C# Python megtekintheti a szöveg analytics működését.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Sentiment elemzése

Az API-t egy pontszám 0 és 1 közötti adja eredményül. Értékek közelébe 1 pozitív sentiment, azt jelzik, miközben közelébe 0 értékek negatív sentiment jelzik. Sentiment pontszámhoz jön létre, osztályozási módszereket. A besorolás a bemeneti szolgáltatásai n g-szolgáltatások részét, beszéd címkéket, és a word beágyazásokat hozza létre. Angol jelenleg az egyetlen támogatott nyelvi.
 
## <a name="key-phrase-extraction"></a>Fő kifejezést kivonása

Az API-t a beírt szöveg beszélő főbb pontjaira jelző karakterláncok listáját adja eredményül. A Microsoft Office összetett természetes nyelvi feldolgozása eszközkészlet technikákat alkalmaz azt. Angol jelenleg az egyetlen támogatott nyelvi.

## <a name="language-detection"></a>Nyelvfelismerés

Az API 0 és 1 közötti az észlelt nyelvi és egy pontszám adja eredményül. 1 közelébe statisztikákat ezzel azt jelzi, hogy a meghatározott nyelvi teljesül 100 %-os bizonyosodjon. Összesen 120 nyelvek használata támogatott.

## <a name="topic-detection"></a>A témakör kimutatására

Ez a egy újonnan kiadott API, mely felső észlelt témakörök listáját adja eredményül küldött szöveges rekordok. A tematikus azonosítjuk a fő kifejezést, amely lehet egy vagy több kapcsolódó szavak. Az API használatához 100 szöveges bejegyzések benyújtandó, minimum, de a témakörök feltárása rekordok ezer több száz át lett tervezve. Figyelje meg, hogy ez az API charges minden elküldött szöveges rekord 1 tranzakció. Az API tervezve jól a rövid, az emberi írt szöveget, például az ellenőrzések és a felhasználó visszajelzést.

---

## <a name="api-definition"></a>API meghatározása

### <a name="headers"></a>Élőfejek

Győződjön meg róla, a megfelelő fejlécek szerepeltetni a kérelmet, amelyet a következő lesz:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

A fiókkulcs a fiókjából az [Azure Datamarket](https://datamarket.azure.com/account/keys)a található. Figyelje meg, hogy jelenleg csak JSON fogadja el a rendszer bemeneti és kimeneti formátumban. XML nem támogatott.

---

## <a name="single-response-apis"></a>Egyetlen válasz API-hoz

### <a name="getsentiment"></a>GetSentiment

**URL-CÍME** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Példa kérés**

A hívás alatt a "Helló, világ" kifejezést sentiment-elemzés kérő azt:

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Válasz az alábbi képlettel történik ez ad eredményül:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Példa kérés**

A hívás, az alábbi hogy kérő a fő mondatok található szöveg "Volt az egyedi decor és rövid oktatói maradjon egy csodás szállások":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Válasz az alábbi képlettel történik ez ad eredményül:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Példa kérés**

Az alábbi GET hívás azt kérő a sentiment a fő mondatok *Helló, világ* szöveg esetében az

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Válasz az alábbi képlettel történik ez ad eredményül:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Választható paraméterek**

`NumberOfLanguagesToDetect`nem kötelező paraméter. Az alapértelmezett érték 1.

---

## <a name="batch-apis"></a>Köteg API-hoz

A szöveg Analytics szolgáltatás lehetővé teszi, hogy sentiment és a billentyű-kifejezés kevesebb kivonást köteg módban. Figyelje meg, hogy az egyes rekordok pontszáma száma egy tranzakcióként. Példaként Ha egyetlen telefonál 1000 rekordok sentiment kér 1000 tranzakciók fog vonni.

Ne feledje, hogy az azonosítók a rendszerbe az azonosítók, a rendszer által visszaadott. A webszolgáltatás nem ellenőrzi, hogy a következő azonosítók egyediek-e. A feladata a hívó egyediségének ellenőrzése. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL-CÍME** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Példa kérés**

A bejegyzés alatt hívás azt kérő vannak a "Helló, világ", "Hello élőlá nemzetközi" és "Hello saját nemzetközi" az összehívás törzsében mondatot hangulati elemek:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

A kérelem szervezet:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

Az alábbi válaszként kattint, a szöveg azonosítók társított értékek listája:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Példa kérés**

Ebben a példában azt kérő hangulati a fő kifejezések a következő szövegeket az elemek listáját: 

* "Volt az egyedi decor és rövid oktatói maradjon egy csodás szállások"
* "Volt egy lenyűgöző build konferencia nagyon érdekes megbeszélések a"
* "A forgalom rémes volt, látogasson el a airport három órát töltöttem"

A kérés a végpontra bejegyzés hívásként:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

A kérelem szervezet:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

Az alábbi válaszként kattint, a szöveg azonosítók társított fő mondatok listája:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "angol",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "francia",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>A témakör észlelési API-hoz

Ez a egy újonnan kiadott API, mely felső észlelt témakörök listáját adja eredményül küldött szöveges rekordok. A tematikus azonosítjuk a fő kifejezést, amely lehet egy vagy több kapcsolódó szavak. Figyelje meg, hogy ez az API charges minden elküldött szöveges rekord 1 tranzakció.

Az API használatához 100 szöveges bejegyzések benyújtandó, minimum, de a témakörök feltárása rekordok ezer több száz át lett tervezve.


### <a name="topics--submit-job"></a>Témakörök – küldés feladat

**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Példa kérés**


A bejegyzés hívás az alábbi azt kérő témakörök egy sor olyan 100 cikkek, ahol az első és utolsó beviteli cikkek jelennek meg, és két StopPhrases számításba veszi.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

A kérelem szervezet:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

Az alábbi a válasz a JobId a beküldött projektre vonatkozóan kap:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Egy szót vagy több olyan word mondatok, amelyek nem vissza kell megjelölése témakörökben listája. Túl általános témakörök kiszűrésére használható. Ha például egy adatkészlet szállások véleményeivel kapcsolatban, "szállások" és "hostel" lehet kijelölve leállítása egy kifejezést.  

### <a name="topics--poll-for-job-results"></a>Témakörök – a szavazás feladat eredménye

**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Példa kérés**

A sikeres-ból az eredmények a "Küldés feladat" lépés – által eredményül adott JobId. Azt javasoljuk, hogy felhívja a végpont percenként, amíg az állapot "Kész" = a válaszban. A projekt körülbelül 10 perc tart teljes, vagy hosszú, sok ezres a rekordok feladatokhoz.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Miközben feldolgozása, a válasz a következők:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Az API kimeneti JSON formátum a következő formában adja eredményül:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


A válasz minden részét tulajdonságainak a következők:

**TopicInfo tulajdonságai**

| Kulcs | Leírás |
|:-----|:----|
| TopicId | Minden témához egyedi azonosítója. |
| Eredmény | A témakör rendelt rekordok számát. |
| KeyPhrase | Summarizing szót vagy kifejezést a témakörnek. 1 vagy több szót is lehet. |

**TopicAssignment tulajdonságai**

| Kulcs | Leírás |
|:-----|:----|
| Azonosító | A rekord azonosítója. Megfelel a bemeneti szereplő azonosítója. |
| TopicId | A témakör Azonosítót, amely a rekord hozzá van rendelve. |
| Távolság | A megbízhatóság, hogy a témakör a rekord tartozik. Távolság közelebbi nullára azt jelzi, hogy nagyobb megbízhatóság. |
