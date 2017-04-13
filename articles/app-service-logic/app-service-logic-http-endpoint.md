<properties
   pageTitle="Logika alkalmazások mint hívható végpontok"
   description="Hogyan létrehozása és konfigurálása az eseményindító végpontok és Azure App szolgáltatásban összefüggés-alkalmazásban"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Logika alkalmazások mint hívható végpontok

Logika alkalmazások natív módon is elérhetővé teheti a szinkronizált HTTP végpont eseményindító.  A minta hívható végpontok meghívásához logika alkalmazások, a "munkafolyamat" művelet egy logikai alkalmazásban – beágyazott munkafolyamat is használhatja.

Eseményindítók, amelyen kérések 3 típusát különböztethetjük meg:

* Kérés
* ApiConnectionWebhook
* HttpWebhook

A cikk hátralévő példaként **kérelem** fogjuk használni, de az összes elvek azonos alkalmazása a 2 másfajta indítók.

## <a name="adding-a-trigger-to-your-definition"></a>Az eseményindító definíciójának hozzáadása
Első lépésként az eseményindító hozzáadása, amelyek kaphatnak a bejövő felkérést logikájának alkalmazás definíciójának.  A "HTTP kérés" a kiváltó ok mező kártya hozzáadásához a tervező segítségével megkeresheti. Egy összehívás törzsébe JSON séma határozhatja meg, és a Tervező elemezni, illetve az adatokat át a kézi hatására a munkafolyamaton keresztül tokenek hoz létre.  E javasoljuk, hogy a szervezet minta hasznos JSON séma létrehozásához például [jsonschema.net](http://jsonschema.net) eszköz használata.

![Eseményindító kártya kérése][2]

Miután logika alkalmazás definíciójának, a visszahívás URL-CÍMÉT az alábbihoz hasonló jön létre:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Ez az URL a lekérdezés paraméterek hitelesítéshez használt Társítások kulcsot tartalmaz.

Az Azure-portálon is megnyithatja a végpont:

![][1]

Vagy, hívja fel:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>A kiváltó ok mező URL-biztonság

Logikájának alkalmazás visszahívási URL-ek biztonságosan segítségével az Access megosztott aláírás hozhatók létre.  Az aláírás lekérdezési paraméterként haladt keresztül, és ellenőrizni kell, mielőtt a logika alkalmazás fire lesz.  Egy titkos kulcs logika alkalmazás, a kiváltó ok mező neve és a végrehajtandó művelet egy egyedi kombinációja keresztül jön létre.  Kivéve, ha valaki hozzáfér a titkos logika alkalmazás billentyűt, akkor volna nem tudják érvényes aláírás létrehozásához.

## <a name="calling-the-logic-app-triggers-endpoint"></a>A logikájának alkalmazás eseményindító végpont hívása

Az eseményindító a végpont létrehozása után elindítása-e azt egy `POST` a teljes URL-címét. A szervezet további fejlécek, és minden olyan tartalom is.

Ha a tartalomtípus- `application/json` hivatkozás tulajdonságok belül a kérés tud lesz. Egyéb esetben azt rendszer kezeli egységként bináris, más API-khoz is átkerülnek, de a munkafolyamaton belül a tartalom konvertálás nélkül nem hivatkozni.  Ha például a sikeres `application/xml` tartalom használhatja is `@xpath()` egy xpath kinyerésének elvégzendő vagy `@json()` JSON konvertálása XML-ből.  További tudnivalókat a tartalmakkal fájltípusok: [itt is lehet](app-service-logic-content-type.md)

Ezenkívül a definíció JSON séma is megadhat. Ennek hatására a Tervező, majd felkeresése lépéseket tokenek létrehozásához.  Például a következő láthatóvá válik a `title` és `name` jogkivonat a tervezőben érhető el:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>A tartalom, a bejövő kérelem hivatkozó

A `@triggerOutputs()` függvény fog kimeneti a beérkező kérelem tartalmát. Ha például azt jelenne meg:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Használhatja a `@triggerBody()` helyi elérése a `body` tulajdonság kifejezetten. 

## <a name="responding-to-the-request"></a>A kérés megválaszolása

Egyes kérések, amely megnyitja az logikájának alkalmazást érdemes néhány tartalommal megválaszolása kíván a hívó féllel. **Válasz** állapotkód szövegtörzséből, majd a választ fejlécek összeállításához használható nevű új művelet típus van. Jegyzet, hogy nincs **Válasz** -alakzat nem tartalmaz adatokat, ha a logikai alkalmazás végpont fog *azonnal* **202 elfogadott**válaszol.

![HTTP-válasz művelet][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Válaszok a van:

| A tulajdonság | Leírás |
| -------- | ----------- |
| statusCode | A HTTP-állapotkód a beérkező kérelem válaszolni. Lehet, hogy a bármely érvényes állapotkód 2xx, 4xx vagy 5xx kezdődik. 3xx állapot kódok nem engedélyezett. | 
| szervezet | A szervezet-objektumra, amely a karakterlánc is lehet, JSON objektum vagy az előző lépésre hivatkozott még bináris tartalom. | 
| Élőfejek | Megadhatja, hogy a fejlécek szerepelni a válaszban tetszőleges számú | 

A lépéseket a logika alkalmazásban a kérdésre adott választ szükséges *60 másodperc* az eredeti kérés fogadására, a válasz, **kivéve, ha a munkafolyamat a hívott beágyazott logika alkalmazásként**belül el kell végeznie. Válasz semmi elérése 60 másodperc, majd a beérkező kérelem belül időtúllépést okoz, és a **408 ügyfél időtúllépése** HTTP válasznak.  Beágyazott logikájának alkalmazásai, a szülő logikájának alkalmazás továbbra is szerepelni fognak Várakozás a válaszra befejeztéig, függetlenül attól, időt vesz igénybe.

## <a name="advanced-endpoint-configuration"></a>Speciális végpont beállítása

Logika alkalmazások rendelkezik beépített támogatása a közvetlen hozzáférés végpontot, és mindig használata a `POST` módszer a Futtatás logika alkalmazás indításához. A **HTTP-figyelő** API-alkalmazás korábban is támogatja az URL-cím szegmensek és a HTTP-metódus módosítása. További biztonsági páros beállítása vagy az egyéni tartomány sikerült az API alkalmazás host (a Web app tárolt az API-alkalmazás) hozzáadásával. 

Ez a funkció **API-kezelés**keresztül érhető el:
* [A kérés módjának módosítása](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [A kérés URL-cím szakasz módosítása](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* A **beállítás** lapon a klasszikus Azure portálon API management tartományok beállítása
* (a**hivatkozás szükséges**) alapszintű hitelesítés ellenőrzése házirendjének beállítása

## <a name="summary-of-migration-from-2014-12-01-preview"></a>2014-es – 12-01-előnézet áttérés összefoglalása

|  2014-es – 12-01 – előzetes verzió | 2016-06-01 |
|---------------------|--------------------|
| Kattintson a **HTTP-figyelő** API-alkalmazásba | Kattintson a **kézi eseményindító** (nem kötelező API alkalmazás) |
| HTTP-figyelő "*automatikusan a válasz küldése*" beállítás | A **Válasz** művelet tartalmaznak, illetve nem a munkafolyamat-definíciót. |
| Basic vagy OAuth hitelesítés konfigurálása | VIA API-kezelés |
| HTTP-módszer beállítása | VIA API-kezelés |
| Relatív elérési út konfigurálása | VIA API-kezelés |
| Hivatkozás a bejövő szervezet keresztül`@triggerOutputs().body.Content` | Hivatkozás útján`@triggerOutputs().body` |
| **Válasz küldése HTTP** műveletének a HTTP-figyelő | Kattintson a **HTTP-összehívás megválaszolása** (nem kötelező API alkalmazás)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
