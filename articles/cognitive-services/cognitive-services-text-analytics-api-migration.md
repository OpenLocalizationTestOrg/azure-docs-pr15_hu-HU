<properties
    pageTitle="A szöveg Analytics API 2-es verziójú frissítés |} Microsoft Azure"
    description="Azure gépi tanulási szöveg Analytics - 2-es verziójú frissítés"
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

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>A szöveg Analytics API 2-es verziójú frissítés #

Ez az útmutató végigvezeti a folyamaton, a kód verzióról a második verzióját használja [az API-t első verzióját](../machine-learning/machine-learning-apps-text-analytics.md) használja. 

Ha még nem használta az API-t, és szeretné, ha többet szeretne, akkor **[További tudnivalók az API itt](//go.microsoft.com/fwlink/?LinkID=759711)** , vagy **[kövesse a rövid útmutató az első lépésekhez](//go.microsoft.com/fwlink/?LinkID=760860)**. Technikai útmutató tekintse át az **[API-definíciós](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>1 része. Új kulcsot beszerzése ###

Első lépésként meg kell egy új API kulcs lekérése az **Azure-portálon**:

1. Nyissa meg azt a szöveget Analytics szolgáltatás a [Cortana üzletiintelligencia-gyűjtemény](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2)keresztül. Ebben az esetben a dokumentáció és kód minták mutató hivatkozásokat is talál.

1. Kattintson a **regisztráció**gombra. Erre a hivatkozásra kattintva megnyílik az Azure adatkezelési portálon, ahol iratkozzon fel a szolgáltatás.

1. Válasszon egy csomagot. Az **5000 tranzakciók/hónap ingyenes réteg**kijelölheti. Egy ingyenes csomagra van, akkor nem ráterheljük a szolgáltatással. Szüksége lesz az Azure előfizetés bejelentkezni. 

1. Miután előfizetett szöveg elemzéséhez, fog egy **API -ja kulcs**fordítani. Másolja a vágólapra a kulcs, mert meg kell azt, az API-szolgáltatások használatakor.

### <a name="part-2-update-the-headers"></a>2 része. A fejlécek frissítése ###

Frissítse a beküldött élőfej értékek alább látható módon. Figyelje meg, hogy nincs-e kódolt a fiókkulcs.

**1-es verzió**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**2-es verzió**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>3 része. Az alap URL-cím frissítése ###

**1-es verzió**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**2-es verzió**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>4a részét. Frissítse a formátumok sentiment, fő kifejezéseket és nyelvek ###

#### <a name="endpoints"></a>Végpontok ####

ELSŐ végpontok van most már elavult, így összes beviteli bejegyzés kérésre kell nyújtani. Frissítse a végpontok az üzenetekhez alább látható módon.

| |1-es verziójú végpontot.|1-es verziójú köteg végpont|2-es verziójú végpont|
|---|---|---|---|
|Hívás típusa|GET|BEJEGYZÉS|BEJEGYZÉS|
|Sentiment|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Fontos kifejezések|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Nyelvek|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Beviteli formátumok ####

Figyelje meg, hogy csak bejegyzés formátum most fogadta a partner, így kell formáznia a bármely bevitel használt korábban a egyetlen dokumentum végpontok lehetőséget. Bemenetben nagybetűk nem.

**(Köteg) 1-es verzió**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**2-es verzió**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Sentiment kimenete ####

**1-es verzió**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**2-es verzió**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Fontos kifejezések kimenete ####

**1-es verzió**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**2-es verzió**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Nyelvek kimenete ####


**1-es verzió**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**2-es verzió**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Kijelző 4. A szükséges témakörökre formátumok módosítása ###

#### <a name="endpoints"></a>Végpontok ####

| |1-es verziójú végpont | 2-es verziójú végpont|
|---|---|---|
|A témakör kimutatására (KÜLDÉS) küldése|```StartTopicDetection```|```topics```|
|A témakör eredmények (GET) beolvasása|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Beviteli formátumok ####

**1-es verzió**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**2-es verzió**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Beküldési eredménye ####

**(KÜLDÉS) 1-es verzió**

Amikor befejezte a feladatot, korábban lenne a következő JSON kimenet, ahol a jobId volna fűzi URL-ból a kimenet kap.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**(KÜLDÉS) 2-es verzió**

A válasz most már tartalmazza az alábbiak szerint élőfej érték hol `operation-location` végpontjának lekérdezik az eredmények használt:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>A művelet eredménye ####

**(GET) 1-es verzió**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**(GET) 2-es verzió**

Mielőtt **a kimenet rendszeres lekérdezik** (a javasolt időszak a percenként) addig, amíg a kimenet adja vissza. 

Ha a témakörök API nem fejeződött, egy állapot olvasási `succeeded` vissza kell. Ez az alább látható módon formátum majd magában foglalja a kimeneti között:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>5 része. Tesztelje! ###

Most már használatba veheti a szoftvert lépjen! Tesztelje a kód egy kis mintával annak érdekében, hogy az adatok sikeres feldolgozásához is.
