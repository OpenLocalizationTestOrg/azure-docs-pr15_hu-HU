<properties
    pageTitle="A Microsoft Translator bővítmény összefüggés-alkalmazások |} Microsoft Azure"
    description="A Microsoft Translator összekötő REST API-paraméterekkel áttekintése"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>A Microsoft Translator összekötő – első lépések
Csatlakozás Microsoft Translator szöveg fordítása, a nyelvet és az egyéb észleli. A Microsoft Translator a következőkre van lehetősége: 

- Hozza létre az üzleti folyamat, az adatok beolvasása a Microsoft Translator alapján. 
- Szöveg fordítása, a észleli a nyelvet és az egyéb műveletek segítségével. Az alábbi műveletek kap választ, és végezze el a kimenet rendelkezésre álló további lehetőségeket. Például létrehozásakor egy új fájlt a Dropbox, lefordíthatja a Microsoft Translator használata másik nyelvet a fájlban a szöveget.

Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
Microsoft Translator tartalmazza a következő műveleteket. Vannak olyan nincs eseményindítók.

Eseményindítók | Műveletek
--- | ---
Nincs lehetőség | <ul><li>Nyelvfelismerés</li><li>Szöveg felolvastatása</li><li>Szöveg fordítása</li><li>Nyelvek beszerzése</li><li>Ismerkedés a beszéd nyelvek</li></ul>

Összekötők JSON és az XML formátumú támogatja az adatokat.


## <a name="create-a-connection-to-microsoft-translator"></a>Microsoft Translator kapcsolat létrehozása

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger REST API-hivatkozás
Verziójára vonatkozik: 1.0.

### <a name="detect-language"></a>Nyelvfelismerés    
A Forrásnyelv észlel a megadott szöveget.  
```GET: /Detect```

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|lekérdezés|karakterlánc|igen|lekérdezés|nincs lehetőség |Szöveg, amelynek nyelvi azonosítania kell magát|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="text-to-speech"></a>Szöveg felolvastatása    
Egy hang adatfolyamként hangformátumok a beszéd egy adott szöveget alakítja át.  
```GET: /Speak```

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|lekérdezés|karakterlánc|igen|lekérdezés|nincs lehetőség |Szöveg átalakítása|
|nyelvi|karakterlánc|igen|lekérdezés|nincs lehetőség |Nyelvi kódot beszédfelismerés készítése (például: "en-us')|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="translate-text"></a>Szöveg fordítása    
Megfelelője a megadott nyelvnek Microsoft Translator segítségével a szöveget.  
```GET: /Translate```

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|lekérdezés|karakterlánc|igen|lekérdezés|nincs lehetőség |A szöveg fordítása|
|languageTo|karakterlánc|igen|lekérdezés| nincs lehetőség|Célnyelv kód (például: "fr")|
|languageFrom|karakterlánc|nem|lekérdezés|nincs lehetőség |Forrásnyelv; Ha nincs megadva, a Microsoft Translator megpróbálja automatikus észlelése. (Példa: en)|
|kategória|karakterlánc|nem|lekérdezés|Általános |Fordítási kategória (alapértelmezett: általános")|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-languages"></a>Nyelvek beszerzése    
Olvassa be az összes nyelvhez használt Microsoft Translator támogató.  
```GET: /TranslatableLanguages```

Nincsenek a hívás paraméterek. 

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-speech-languages"></a>Ismerkedés a beszéd nyelvek    
Olvassa be a beszéd összefoglaló elérhető nyelveket.  
```GET: /SpeakLanguages``` 

Nincsenek a hívás paraméterek.

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|

## <a name="object-definitions"></a>Objektum definíciók

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Nyelvi: Microsoft Translator fordítható nyelvekhez nyelvi modell

|Tulajdonság neve | Adattípus | Szükséges|
|---|---|---|
|Kód|karakterlánc|nem|
|név|karakterlánc|nem|


## <a name="next-steps"></a>Következő lépések

[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

Térjen vissza az [API-khoz listában](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
