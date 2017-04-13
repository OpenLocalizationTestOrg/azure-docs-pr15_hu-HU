<properties
   pageTitle="Erőforrás-kezelő sablon függvények |} Microsoft Azure"
   description="A függvény egy erőforrás-kezelő Azure-sablon segítségével beolvashatja értékek, használhatja a karakterláncok és numerics és beolvasni a telepítési információ ismerteti."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Azure erőforrás-kezelő sablon függvények

Ez a témakör ismerteti a egy erőforrás-kezelő Azure-sablon is használhatja az összes függvények.

Sablon funkciók és a paraméterek és nagybetűk között. Ha például erőforrás-kezelő és old meg **variables('var1')** **VARIABLES('VAR1')** ugyanaz, mint. Ha értékelni, kivéve, ha a függvény kifejezetten módosítja eset (például toUpper vagy toLower), a függvény megőrzi az esetet. Előfordulhat, hogy az egyes erőforrástípus függetlenül, hogy hogyan függvények kiértékelése eset követelményeknek.

## <a name="numeric-functions"></a>Numerikus függvények

Erőforrás-kezelő az alábbi funkciókat nyújt az egészérték részt veszi figyelembe a:

- [hozzáadása](#add)
- [copyIndex](#copyindex)
- [DIV](#div)
- [int](#int)
- [maradék](#mod)
- [MUL számú](#mul)
- [Sub](#sub)


<a id="add" />
### <a name="add"></a>hozzáadása

**(operand1 operand2) hozzáadása**

A két megadott egész számok összegét adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| operand1                           |   igen    | Az egész szám első hozzáadni.
| operand2                           |   igen    | Második egész hozzáadni.

Az alábbi példa összeadja a két paraméterrel.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Adja vissza az aktuális index egy közelítését hurok. 

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| eltolás                           |   nem    | Az összeg hozzáadni a jelenlegi ismétlési érték.

Ez a funkció egy objektum **másolása** mindig használják. Teljes körű leírását **copyIndex**használatáról olvassa el a [erőforrások az Azure erőforrás-kezelő több példányának létrehozása](resource-group-create-multiple.md)című témakört.

A következő példa bemutatja a Másolás ismétlődve és az index érték szerepel a név. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>DIV

**div (operand1, operand2)**

A két megadott egész számok az egész osztás függvény.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| operand1                           |   igen    | Az egész éppen osztani.
| operand2                           |   igen    | Az egész szám, amellyel osztani. Nem lehet 0.

Az alábbi példában egy vagy több paraméter egy másik paraméterrel elosztja.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>int

**Int(valueToConvert)**

A megadott érték konvertálása egész szám.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   igen    | Az egész a konvertálandó érték. Milyen típusú érték állhat karakterlánc vagy egész szám.

Az alábbi példában a felhasználó által megadott paraméter konvertál egész szám.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>maradék

**maradék (operand1, operand2)**

A megadott két egész számokat használ egész részleg maradékát adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| operand1                           |   igen    | Az egész éppen osztani.
| operand2                           |   igen    | Egész szám, amely osztása, nem lehet a 0-tól különböző.

Az alábbi példa eredménye egy vagy több paraméter egy másik paraméterrel való osztás maradéka.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>MUL számú

**MUL számú (operand1, operand2)**

Adja eredményül a megadott két egész számok a szorzást.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| operand1                           |   igen    | Első egész szám szorzása a csillag.
| operand2                           |   igen    | Összeszorzandó második egész szám.

A következő példa szorzatát számítja ki egy vagy több paraméter egy másik paraméterrel.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>Sub

**Sub (operand1, operand2)**

A két megadott egész számok a kivonás eredménye.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| operand1                           |   igen    | Egész szám, amely az elsőből kivont.
| operand2                           |   igen    | Egész szám, amely költségből.

A következő példa kivonja azt egy másik paraméter egy paramétert.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Karakterláncfüggvények

Erőforrás-kezelő az alábbi funkciókat nyújt a karakterláncok használata:

- [Base64](#base64)
- [összefűzés](#concat)
- [hossza](#lengthstring)
- [padLeft](#padleft)
- [Csere](#replace)
- [kihagyása](#skipstring)
- [felosztása](#split)
- [karakterlánc](#string)
- [karakterlánc](#substring)
- [jegyzetelés](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [vágása](#trim)
- [uniqueString](#uniquestring)
- [URI](#uri)


<a id="base64" />
### <a name="base64"></a>Base64

**Base64 (inputString)**

A bemeneti karakterlánc base64 ábrázolása lekérdezése

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| inputString                        |   igen    | A karakterlánc base64 ábrázolhatók adja vissza.

A következő példa bemutatja a base64 függvény használatát.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>összefűzés - karakterlánc

**összefűzés (karakterlánc1, karakterlánc2, karakterlánc3,...)**

Több megadott értékeket egyesíti, és az összefűzött karakterlánc adja eredményül. 

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| karakterlánc1                        |   igen    | Egy karakterlánc érték való összefűzésére.
| További karakterláncok             |   nem     | A karakterlánc-értékek összefűzése.

Ez a funkció is igénybe vehet az argumentumokat tetszőleges számú, és elfogadhatja vagy karakterláncok, vagy a paraméterek tömbök. Egy példa a tömbök szereplő című témakörben [összefűzés - tömb](#concatarray).

A következő példa bemutatja, hogyan egyesítheti több megadott értékeket egy összefűzött karakterlánc jeleníthető meg.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>hossz - karakterlánc

**length(String)**

A karakterlánc a karakterek számát adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| karakterlánc                        |   igen    | A használható karakterek száma az első karakterlánc érték.

Példa a tömb hossza használja, látható [hossza - tömb](#length).

Az alábbi példa a karakterek száma karakterláncként adja vissza. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**padLeft (valueToPad, totalLength paddingCharacter.)**

Hozzáadásával karakterek balra teljes megadott hosszúságú eléréséig egy jobbra igazított karakterláncot ad vissza.
  
| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| valueToPad                         |   igen    | A karakterlánc vagy int, a Jobbra zárás gombra.
| totalLength                        |   igen    | A visszaadott karakterlánc karaktereinek száma.
| paddingCharacter                   |   nem     | Bal – kitöltő teljes hossza eléréséig használandó elválasztó karakter. Az alapértelmezett érték szóköz található.

A következő példa bemutatja, hogyan szegélynél a felhasználó által megadott paraméter értéke nulla karakter hozzáadásával, amíg a karakterlánc eléri a 10 karakter. Ha az eredeti paraméter 10 karakternél hosszabb, karaktereket nem kerülnek.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>Csere

**Csere (originalString, oldCharacter newCharacter.)**

Egy karakterrel összes előfordulását egy új karakterláncot ad vissza, a megadott karakterláncban lecseréli az egy másik karaktert.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| originalString                     |   igen    | Az összes előfordulását egy karakterrel váltja fel egy másik karaktert tartalmazó karakterlánc.
| oldCharacter                       |   igen    | Az eredeti karakterláncból eltávolítjuk karakter.
| newCharacter                       |   igen    | Adja hozzá az eltávolított karakter helyett karakter.

A következő példa bemutatja az összes szaggatott vonal eltávolításáról a felhasználó által megadott karakterláncot.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Ugrás - karakterlánc
**Ugrás (originalValue, numberToSkip)**

A megadott szám, a karakterláncban után az összes karakter karakterláncot ad vissza.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| originalValue                      |   igen    | A karakterlánc kihagyása használni.
| numberToSkip                       |   igen    | Kihagyása karakterek száma. Ha ez az érték 0 vagy annál kisebb, a visszaadott karakterlánc karaktereinek. Ha nagyobb, mint a szöveg hossza, üres karakterláncot ad vissza. 

Példát a tömb kihagyása használja olvassa el a [kihagyása – tömb](#skip)című témakört.

Az alábbi példa a megadott számú karaktert a karakterláncban átugorja.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>felosztása

**a felosztott (inputString, delimiterString)**

**a felosztott (inputString, delimiterArray)**

Az összefűzendő karakterláncokat a bemeneti karakterlánc, amely a megadott határoló határolja tartalmazó tömb karakterláncok lekérdezése

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| inputString                        |   igen    | A karakterlánc fel szeretne osztani.
| elválasztó karakterrel                          |   igen    | Az elválasztó szeretne használni, akkor lehet, egyetlen karakterlánc vagy karakterláncok tömbje.

A következő példa kettéválaszthatja-e a bemeneti karakterlánc egy vesszővel elválasztva.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

A következő példa kettéválaszthatja-e a bemeneti karakterlánc egy vesszővel vagy pontosvesszővel.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>karakterlánc

**String(valueToConvert)**

A megadott érték konvertálása karakterlánc.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   igen    | A konvertálandó karakterlánc érték. Bármilyen típusú érték alakítható, objektumok és tömbök együtt.

Az alábbi példában a felhasználó által megadott paraméterértékeket karakterláncok konvertálja.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>karakterlánc

**karakterlánc (stringToParse, startIndex, hossz)**

Egy részét, amely a megadott Karakterpozíció kezdődő és tartalmazza a megadott számú karaktert adja vissza.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| stringToParse                     |   igen    | Az eredeti karakterlánc, amelyből azon részét kivonjuk.
| startIndex                         | nem      | A nulla alapú kezdő karakter pozícióját az azon részét.
| hossza                             | nem      | A karakterlánc karaktereinek számát.

Az alábbi példa az első három karaktert olvas paraméter.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>jegyzetelés - karakterlánc
**jegyzetelés (originalValue, numberToTake)**

A karakterlánc elején álló karakterlánc megadott számú karaktert adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| originalValue                      |   igen    | A karakterlánc érvénybe a karaktereket.
| numberToTake                       |   igen    | A végrehajtandó karakterek száma. Ha ez az érték 0 vagy annál kisebb, üres karakterláncot ad vissza. Ha nagyobb, mint az adott karakterlánc hossza, a visszaadott karakterlánc karaktereinek.

Példa tömböt a take használja olvassa el a [készítése - tömb](#take)című témakört.

A következő példa megnyitja a megadott számú karaktert a.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Kisbetű – nagybetű alakítja a megadott karakterláncot.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| stringToChange                     |   igen    | Kisbetű konvertálandó karakterlánc.

Az alábbi példában a felhasználó által megadott paraméter kisbetű konvertálja.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Nagybetűs alakítja a megadott karakterláncot.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| stringToChange                     |   igen    | A nagybetűs átalakítása karakterláncot.

Az alábbi példa a felhasználó által megadott paraméter alakítja a nagybetűs.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>vágása

**Trim (stringToTrim)**

A kezdő és záró szóköz karakterek eltávolítja a megadott karakterláncot.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   igen    | Az vágásának karakterlánc.

Az alábbi példa az elválasztó karaktert a felhasználó által megadott paraméter levágja.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**uniqueString (baseString,...)**

Karakterláncot hoz létre mérvadó kivonat paraméterként megadott értékek alapján. 

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| baseString      |   igen    | A karakterlánc a kivonat függvény egyedi karakterlánc létrehozása.
| igény szerint további paramétereket    | nem       | Az érték, amely meghatározza az olyan szintű egyediségét létrehozásához szükség szerint annyi karakterláncok is hozzáadhat.

Ez a funkció akkor hasznos, ha létre kell hoznia egy erőforrás egy egyedi nevet. Megadhatja, hogy az eredmény egyediségét körének korlátozni paraméterértékeket. Megadhatja, hogy-e le az előfizetést, erőforráscsoport vagy telepítési egyedi nevét. 

A visszaadott érték nem véletlen karakterlánc, de inkább egy kivonat függvény eredménye. A visszaadott érték 13 karakter hosszú. Még nem globálisan egyedi. Az érték az elnevezésük is egységes hozhat létre egy találó nevet a előtaggal egyesítéséhez érdemes. A következő példa bemutatja a visszaadott érték formátumának. Természetesen a tényleges érték a megadott paramétereket változhat.

    tcvhiyu5h2o5o

Az alábbi példák bemutatják, hogyan uniqueString használatával hozzon létre egy gyakran használt szintek egyedi értéket.

Az előfizetés korlátozódik egyedi

    "[uniqueString(subscription().subscriptionId)]"

Az erőforráscsoport korlátozódik egyedi

    "[uniqueString(resourceGroup().id)]"

Az erőforráscsoport telepítésének korlátozódik egyedi

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
A következő példa bemutatja, hogyan hozhat létre egy egyedi nevet a erőforráscsoport alapján tárterület-fiók (Ez a név nincs erőforráscsoport belül egyedi if helyes összeállítás ugyanúgy).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>URI

**URI (BaseUri elemet meg, relativeUri)**

Abszolút URI a BaseUri elemet meg és a relativeUri karakterláncot hoz létre.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| a BaseUri elemet meg                            |   igen    | Az alap uri-karakterlánc.
| relativeUri                        |   igen    | A relatív uri karakterlánc hozzáadása az alap URI-karakterlánccal.

A **BaseUri elemet meg** paraméter értéke is elhelyezhet egy fájlon, de csak az alap utat a URI kiszámításakor. Például átadása **http://contoso.com/resources/azuredeploy.json** , a BaseUri elemet meg paraméter eredményét az alap URI **http://contoso.com/resources/**.

A következő példa bemutatja, hogyan értéke a szülő-sablon alapján beágyazott sablon mutató hivatkozás létrehozása.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Array függvény

Erőforrás-kezelővel néhány függvény a tömbben értékeket.

- [összefűzés](#concatarray)
- [hossza](#length)
- [kihagyása](#skip)
- [jegyzetelés](#take)

Az érték határolja karakterlánc értékek tömbjét című témakörben kaphat [felosztása](#split).

<a id="concatarray" />
### <a name="concat---array"></a>összefűzés - tömb

**összefűzés (tömb1, tömb2, tömb3,...)**

Több tömb egyesíti, és a összefűzendő tömböt adja eredményül. 

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| Tömb1                        |   igen    | Egy tömb való összefűzésére.
| További tömb             |   nem     | Tömbök való összefűzésére.

Ez a funkció is igénybe vehet az argumentumokat tetszőleges számú, és elfogadhatja vagy karakterláncok, vagy a paraméterek tömbök. Példa megadott értékeket kifejezések olvassa el a [összefűzés - karakterlánc](#concat)című témakört.

A következő példa bemutatja, hogyan két tömb egyesítéséhez.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>hossz - tömb

**length(Array)**

Egy tömb elemek számát adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| tömb                        |   igen    | A tömb elemek száma az első használni.

A tömb e függvény használatával adja meg a közelítések számának, ha az erőforrások létrehozása. A következő példában a paraméter **siteNames** a webhelyek létrehozásakor használandó tömbje volna vonatkoznak.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Tömb Ez a függvény használatáról további tudnivalókért olvassa el a [létrehozása az Azure erőforrás-kezelő erőforrások több példányával](resource-group-create-multiple.md)című témakört. 

Példa használatára hosszúságú karakterlánc értéket tartalmazó olvassa el a [hossz - karakterlánc](#lengthstring)című témakört.

<a id="skip" />
### <a name="skip---array"></a>kihagyása – a tömb
**Ugrás (originalValue, numberToSkip)**

Egy tömb összes elemét, a megadott szám, a tömb adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| originalValue                      |   igen    | A tömb kihagyása használni.
| numberToSkip                       |   igen    | Kihagyása elemek száma. Ha ez az érték 0 vagy annál kisebb, a visszaadott a tömb összes elemét. Ha nagyobb, akkor a tömb hosszát, üres tömböt adja vissza. 

Példát a karakterlánc kihagyása használja olvassa el a [átugrása - karakterlánc](#skipstring)című témakört.

Az alábbi példa a tömbben lévő elemek a megadott számú átugorja.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>vessen – tömb
**jegyzetelés (originalValue, numberToTake)**

A tömb indítása olyan elemeket a megadott számú tömböt adja eredményül.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| originalValue                      |   igen    | A tömb elemei állapotba.
| numberToTake                       |   igen    | A végrehajtandó elemek száma. Ha ez az érték 0 vagy annál kisebb, üres tömböt adja vissza. Ha nagyobb, mint a megadott tömb hossza, a visszaadott a tömb összes elemét.

Példát a karakterlánc take használja olvassa el a [készítése - karakterlánc](#takestring)című témakört.

A következő példa megnyitja a megadott számú a tömb elemeit.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Telepítési érték függvény

Erőforrás-kezelő az alábbi funkciókat nyújt ahhoz, hogy a sablon és a kapcsolódó telepítésben szereplő értékek szakaszok értékeket:

- [telepítési](#deployment)
- [Paraméterek](#parameters)
- [változók](#variables)

Értékek erőforrásokat, az erőforrás csoportok vagy előfizetések című témakörben kaphat [erőforrás függvények](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>telepítési

**Deployment()**

Adja vissza az aktuális környezetben működésével kapcsolatos információkat.

Ez a funkció az objektumot, a telepítés során átadott adja eredményül. A tulajdonságok, a visszaadott objektumban e telepítési objektumát átadott hivatkozásként vagy a soros objektumként függően eltérőek lehetnek. 

Amikor a telepítő objektum átadott a soros, például amikor Azure PowerShell **- TemplateFile** paraméter segítségével mutasson a helyi fájl, a visszaadott az objektumnak a következő formátumban:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Ha az objektum átadott hivatkozásként, például a **- TemplateUri** paraméter használatával egy távoli objektum mutasson az objektum a következő formátumban ad vissza. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

A következő példa bemutatja a deployment() használata hivatkozni szeretne egy másik sablont az URI a szülő-sablon alapján.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>Paraméterek

**paraméterek (parameterName)**

A paraméter értékét adja eredményül. A paraméterek szakasz sablon kell megadni a meghatározott paraméter neve.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| parameterName                      |   igen    | Vissza a paraméter neve.

A következő példa bemutatja egy egyszerűsített a paraméterek függvény használata.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>változók

**változók (variableName)**

Változó értékét adja eredményül. A megadott változó nevét kell megadni a sablon változói szakaszát.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| változó neve                      |   igen    | A visszaadandó változó neve.

A következő példa változó értéket használja.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Erőforrás-függvények

Erőforrás-kezelő az alábbi funkciókat nyújt a első erőforrás értékeket:

- [listKeys és a listája, {érték}](#listkeys)
- [szolgáltatók](#providers)
- [hivatkozás](#reference)
- [resourceGroup](#resourcegroup)
- [resourceId](#resourceid)
- [előfizetés](#subscription)

Értékek paraméterek, változók vagy az aktuális környezetben című témakörben kaphat [telepítési érték függvények](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>listKeys és a listája, {érték}

**listKeys (erőforrásnév vagy resourceIdentifier, apiVersion)**

**{} Értéke (erőforrásnév vagy resourceIdentifier, apiVersion) lista**

Az erőforrás-bármilyen, amely támogatja a lista művelet értékeket adja vissza. A leggyakoribb használatát **listKeys**. 
  
| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| Erőforrásnév vagy resourceIdentifier |   igen    | Az erőforrás egyedi azonosítója.
| apiVersion                         |   igen    | Erőforrás futási idejű állapotát API verziója.

Minden művelet, amely a **lista** kezdődő lehet a sablon függvényt használja. Az elérhető műveletek nem csak **listKeys**, hanem műveletek, például a **listában**, **listAdminKeys**és **listStatus**is tartalmazza. Annak megállapításához, hogy melyik erőforrástípus van egy lista műveletet, használja az alábbi PowerShell-parancsot.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Vagy lekérése az Azure CLI a listában. A következő példa beolvassa **apiapps**az összes művelet, és használja a JSON segédprogram [jq](http://stedolan.github.io/jq/download/) csak a lista műveleteket szűréséhez.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

A resourceId a [resourceId függvény](#resourceid) használatával, illetve a következő formátumban kell megadni **{providerNamespace} / {erőforrástípus} / {erőforrásnév}**.

A következő példa bemutatja, hogyan lépjen vissza az elsődleges és másodlagos kulcsok a kimeneti értékeket szakasz tárterület-fiókból.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

A visszaadott listKeys az objektumnak a következő formátumban:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>szolgáltatók

**szolgáltatók (providerNamespace; [erőforrástípus])**

Ad egy erőforrás-szolgáltatóval, és a támogatott erőforrástípus felvilágosítást. Ha nem ad meg egy erőforrás típusa, a függvény a támogatott típusok az erőforrás-szolgáltatóhoz.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   igen    | A szolgáltató Namespace
| Erőforrástípus                       |   nem     | Az erőforrás belül a megadott névteret típusát.

Minden egyes támogatott típusú a következő formában adja vissza. Nem biztos a tömb rendezést.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

A következő példa bemutatja a szolgáltató függvény használatát:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>hivatkozás

**hivatkozás (erőforrásnév vagy resourceIdentifier; [apiVersion])**

Egy másik forráshoz futási idejű állapotát jelző objektumot adja vissza.

| Paraméter                          | Szükséges | Leírás
| :--------------------------------: | :------: | :----------
| Erőforrásnév vagy resourceIdentifier |   igen    | Név vagy egy erőforrás egyedi azonosítója.
| apiVersion                         |   nem     | Az adott erőforrás API verziója. Ez a paraméter kihagyása az erőforrás nincs kiépítve belül ugyanazt a sablont.

A **hivatkozás** függvényt egy futtatókörnyezet állam értékére származik, és ezért nem használható a változók szakaszban. Sablon kimeneti értékeket részében használható.

A hivatkozás függvénnyel, implicit módon az elemeket rekordként, hogy egy erőforrás függ, hogy egy másik erőforrás, ha a hivatkozott erőforrás már kiépítve belül ugyanazt a sablont. Nem kell a tulajdonsággal is **dependsOn** . A függvény nem kiértékeli a hivatkozott erőforrás telepítési befejeztéig.

Az alábbi példa a tárterület-fiókot, amelyet ugyanazt a sablont a telepítik hivatkozik.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Az alábbi példa a tárterület-fiókot, amelyet a nincs telepítve az ebben a sablonban, de a erőforrásként üzembe helyezéséhez ugyanaz az erőforrás csoporton belül létezik hivatkozik.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

A visszaadott objektumból a blob-végpont URI, például egy adott érték meghallgathatja, az alábbi példában látható módon.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Az alábbi példa a tárterület-fiók egy másik erőforráscsoport hivatkozik.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

A tulajdonságok az objektumra a **hivatkozás** függvény által visszaadott erőforrástípus típusától függően változnak. A tulajdonságok neve és erőforrástípus értékének megjelenítéséhez, amely visszaadja a **Kimenet** szakaszában az objektumot egyszerű sablon létrehozása. Ha ilyen típusú meglévő erőforrás, a sablon csak új erőforrások telepítése nélkül adja vissza az objektum. Ha nem az adott típusú meglévő erőforrás, a sablon üzembe helyezése a csak az adott típusú, és az objektum adja eredményül. Ezt követően ezeket a Tulajdonságok hozzáadása más sablonok kell értékeinek dinamikusan beolvasásához a telepítés során. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>resourceGroup

**resourceGroup()**

Egy objektumot, amely az aktuális erőforráscsoport adja vissza. 

A visszaadott objektum van, a következő formátumban:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Az alábbi példában az erőforrás csoport helyét hozzárendelése egy webhely helyét.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>resourceId

**resourceId ([subscriptionId], [resourceGroupName], erőforrástípus, resourceName1, [resourceName2]...)**

Az egyedi azonosító egy erőforrás lekérdezése 
      
| Paraméter         | Szükséges | Leírás
| :---------------: | :------: | :----------
| subscriptionId    |   nem     | Alapértelmezett értéke az aktuális előfizetést. Adja meg ezt az értéket, ha egy másik előfizetésben erőforrást kell.
| resourceGroupName |   nem     | Alapértelmezett értéke aktuális erőforráscsoport. Adja meg ezt az értéket, ha egy másik erőforráscsoport erőforrást kell.
| Erőforrástípus      |   igen    | Többek között az erőforrás-szolgáltató névtér erőforrás típusú.
| resourceName1     |   igen    | Erőforrás neve.
| resourceName2     |   nem     | Következő erőforrás nevét a szakasz Ha erőforrás van beágyazva.

Ha az erőforrás neve nem egyértelmű, vagy nem kiépített ugyanabban a sablonban használhatja ezt a funkciót. Az azonosító jelez a következő formátumban:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Az alábbi példa szemlélteti az erőforrás-azonosítók egy olyan webhelyet, és az adatbázis számára beolvasása. A webhely **myWebsitesGroup** nevű erőforráscsoport létezik, és az adatbázis csak az aktuális erőforráscsoport sablon.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Gyakran akkor használja ezt a funkciót egy alternatív erőforráscsoport egy tárterület-fiókkal vagy egy virtuális hálózati használatakor. A tárterület-fiók vagy a virtuális hálózati használható különböző több erőforrás csoportok; Emiatt nem szeretné, hogy törlésekor őket egy erőforrás egy csoportot. A következő példa bemutatja, hogyan egy erőforrás egy külső erőforráscsoport az egyszerűen használható:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>előfizetés

**Subscription()**

Az előfizetés olvashat a következő formátumban adja eredményül.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

A következő példa bemutatja az előfizetés függvény, a kimeneti értékeket szakasz neve. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Következő lépések
- Az erőforrás-kezelő Azure sablonban szakaszok leírását tanulmányozza a [szerzői Azure erőforrás-kezelő sablonok](resource-group-authoring-templates.md) című témakört.
- Több sablonok egyesítéséhez lásd: a [csatolt sablonokkal a Azure-kezelő eszközzel](resource-group-linked-templates.md)
- Megadott számú alkalommal találta létrehozása egy típusú erőforrás esetén lásd: [erőforrások az Azure erőforrás-kezelő több példányának létrehozása](resource-group-create-multiple.md)
- A létrehozott sablont telepítéséről megtekintéséhez kattintson a [Deploy egy alkalmazást, az erőforrás-kezelő Azure-sablon](resource-group-template-deploy.md)

