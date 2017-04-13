<properties
    pageTitle="Azure függvények időzítő eseményindító |} Microsoft Azure"
    description="Megtudhatja, hogyan időzítő indítók Azure függvények használata."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure működik, függvények, esemény feldolgozása, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure függvények időzítő eseményindító

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogyan időzítő indítók konfigurálása az Azure függvények. Az időzítőszolgáltatás hívás függvényeket alapuló ütemezés, egyszeri vagy ismétlődő elindítja.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Az időzítőszolgáltatás eseményindító Function.JSON

A *function.json* fájl tartalmaz egy ütemterv kifejezést. Ha például a következő ütemterv fut, a függvény percenként:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Az időzítőszolgáltatás eseményindító több példány skála meg automatikusan kezeli: egy adott timer függvény csak egyetlen példányát minden példányára fog futni.

## <a name="format-of-schedule-expression"></a>Az ütemterv kifejezést formázása

Az ütemterv kifejezést értéke 6 mezőket [CRON kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression) : `{second} {minute} {hour} {day} {month} {day of the week}`. 

Megjegyzés: a {második} mező az online keresése cron kifejezések számos kihagyja, ha azok közül a felesleges mező beállításához is. 

Íme néhány egyéb ütemezési kifejezés példa:

Az 5 percenként elindítása:

```json
"schedule": "0 */5 * * * *"
```

Szeretne elindítani egyszer óránként tetején:

```json
"schedule": "0 0 * * * *",
```

A két óránként elindítása:

```json
"schedule": "0 0 */2 * * *",
```

A óránként csak egyszer a 9-es AM és 17 óra elindításának:

```json
"schedule": "0 0 9-17 * * *",
```

Indíthatja el a 9:30 de naponta:

```json
"schedule": "0 30 9 * * *",
```

Indíthatja el a 9:30 de minden hét.napja:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Az időzítőszolgáltatás eseményindító C# kód példa

A C# kód példa ír egyetlen naplót minden alkalommal, amikor a függvény dátumra.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
