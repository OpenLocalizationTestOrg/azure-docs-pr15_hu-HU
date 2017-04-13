<properties 
    pageTitle="Hogyan Blitline használt kép feldolgozása - Azure szolgáltatás útmutató" 
    description="Megtudhatja, hogy miként Blitline szolgáltatással képek kezeléséhez Azure alkalmazáson belül." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Azure és Azure tároló Blitline használata

Ez az útmutató fog bemutatják, hogyan Blitline szolgáltatások eléréséhez és hogyan Blitline feladatok kezdeményezése.

## <a name="what-is-blitline"></a>Mi az Blitline?

Blitline egy felhőalapú kép szolgáltatás, ahol a vállalati szintű kép feldolgozás egy részét, az ár össze saját maga szeretné kerülni, a feldolgozása.

A FAKT, hogy kép feldolgozása nem történt és újra, általában az újonnan létrehozott webhely minden az alapoktól. Azt a látja ezt, mert azt már készített korábban őket egy millió időpontok is. Azt úgy döntött, hogy esetleg érdemes idő egynapos azt csak adunk mindenki számára. Megtudhatja, hogyan teheti, akkor gyorsan és hatékonyan, és mindenki munkahelyi időközben mentése tudjuk.

További tudnivalókért olvassa el a [http://www.blitline.com](http://www.blitline.com)című témakört.

## <a name="what-blitline-is-not"></a>Mi Blitline nincs...

Egyértelművé teheti, hogy mi Blitline esetében hasznos lehet az, érdemes gyakran könnyebben azonosítható a Blitline nem mire előre áthelyezése előtt.

- Blitline nincsenek képek feltöltése HTML-vezérlőkhöz. Elérhető képek nyilvánosan vagy Blitline elérje a rendelkezésre álló korlátozott engedélyekkel kell rendelkeznie.

- Nem Blitline feldolgozása, például Aviary.com élő képe

- Blitline nem fogadja el a képek feltöltését, akkor nem leküldéses a képek Blitline kattintva közvetlenül. Kell Azure tároló vagy más helyekről Blitline támogatja a leküldéses őket, és ezután adja meg az Blitline helyre látogasson el őket.

- Blitline nagymértékben párhuzamos, és végezze el a bármely szinkronizált feldolgozás. Ami azt jelenti, hogy kell közölje velünk a postback_url, és azt tudhatja meg, amikor azt végzett feldolgozása.

## <a name="create-a-blitline-account"></a>Blitline-fiók létrehozása

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>A Blitline feladat létrehozása

Blitline JSON használ, a ki szeretne venni egy képet a műveletek megadását. A JSON néhány egyszerű mezők tevődik össze.

A legegyszerűbb példája az alábbi képlettel történik:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Itt van még JSON, amely megnyílik egy "src" kép "... boys.jpeg" és méretezze 240 x 140, hogy a képre.

Az alkalmazás azonosítója valami a Azure a **Csatlakozási adatok** és **kezelése** lapján található. A titkos azonosító, amely lehetővé teszi, hogy a feladat futtatható Blitline.

A "Mentés" paraméter azonosítja információt, ahová át szeretné helyezni a képet, van feldolgozása, akkor azt. Trivial ebben az esetben még nem definiált egyet. Ha meg van adva a nincs hely Blitline tárolni fogja azt helyileg (és ideiglenes) egy egyedi felhőalapú helyen. Úgy juthat az adott helyen a JSON Blitline által visszaadott, hogy a Blitline tudni fogja. Az "kép" azonosító szükség, és visszaküldi, azonosítja a adott mentett képet.

Itt támogatjuk *függvényekkel* kapcsolatos további információkat talál: <http://www.blitline.com/docs/functions>

Itt a feladat lehetőségekről dokumentáció is megtalálhatók: <http://www.blitline.com/docs/api>

Ha befejezte a JSON kell tennie mindössze **BEJEGYZÉST** , hogy`http://api.blitline.com/job`

JSON vissza, amely valahogy így jelenik meg:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


EMTK Blitline megkapta a kérést, azt még mozgatófogantyúit, hogy egy feldolgozás várakozási sorban található, és befejezését követően a kép érhetők el a: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_alkalmazás\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Kép mentése az Azure tárterület-fiókba

Ha fiókja van, az Azure tárhely, feldolgozott képek leküldéses az Azure tárolóba Blitline egyszerűen lehet. "Azure_destination" hozzáadásával meghatározhatja a helyét és Blitline, hogy a leküldéses engedélyeit.

Lássunk egy példát:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Saját CAPITALIZED értékeket kitöltésével elküldheti a JSON http://api.blitline.com/job való és a "src" kép életlenítés szűrővel feldolgozott és Azure cél majd kerül.

###<a name="please-note"></a>Ne feledje:

A Társítások teljes Társítások URL-címét, beleértve a fájlnevet, a célfájl kell tartalmaznia.

Példa:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Erről Blitline's Azure tároló dokumentumok legújabb verzióját is [Itt](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Következő lépések

Látogasson el a további információt az egyéb szolgáltatások blitline.com:

* Blitline API végpont dokumentumok <http://www.blitline.com/docs/api>
* Blitline API függvények <http://www.blitline.com/docs/functions>
* Blitline API példák <http://www.blitline.com/docs/examples>
* Harmadik rész Nuget tár <http://nuget.org/packages/Blitline.Net>
