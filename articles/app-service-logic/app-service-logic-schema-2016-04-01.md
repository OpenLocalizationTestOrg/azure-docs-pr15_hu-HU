<properties 
    pageTitle="Új séma verzió 2016-06-01 |} Microsoft Azure" 
    description="Megtudhatja, hogy miként írhat a legújabb logika alkalmazások JSON definíciója" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Új séma verzió 2016-06-01

Az új sémafájl és az API verzió összefüggés-alkalmazások javítása, amely a megbízhatóságát számát és a könnyű kezelés alkalmazása logika alkalmazások van. 3 főbb különbségek vannak:

1. Keresési tartományok, hogy mely műveleteket tartalmazó műveletek gyűjteménye hozzáadásával.
1. Feltételek és hurkok azok osztályú műveletek
1. Végrehajtási sorrendje részletesebb keresztül `runAfter` tulajdonság (felváltó `dependsOn`)

Információt a Skype 2015-08-01-előnézet séma 2016-06-01 sémára logika alkalmazás verziójáról [olvassa el az alábbi frissítési szakaszt.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. keresési tartományok

A legfontosabb változások a séma egyik hatókörök és az azt jelenti, hogy műveletek ágyazható egymásba hozzáadásával.  Ez a hasznos, ha egy sor csoportosítása, illetve ha a műveletek (például egy feltételt tartalmazhat egy másik feltétel) egymásba ágyazva beágyazása erre szolgáló.  További részleteket a hatókör szintaxisa is megtalálható [Itt](app-service-logic-loops-and-scopes.md), de egy egyszerű hatókör példa alatt találhatók:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. a feltételeket, és a módosítások hurkok

A séma korábbi verzióiban a feltételeket és hurkok egyetlen művelethez tartozó paraméterek volt.  Ezt a korlátozást még oldották meg a sémában, és most feltételek és hurkok jelenik meg ezt a műveletet.  További információt [a jelen cikkben](app-service-logic-loops-and-scopes.md)található, és így nézhet ki egy egyszerű példa egy feltétel művelet:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. a RunAfter tulajdonság

Az új `runAfter` tulajdonság helyére lép `dependsOn` pontosabb engedélyezni futtatása rendezés érdekében.  `dependsOn`"a művelet futtatta és sikeres volt," szinonimái azonban sokszor kell művelet végrehajtása, ha az előző művelet sikerült, nem sikerült, vagy a kihagyott volt.  `runAfter`lehetővé teszi, hogy rugalmasság érdekében.  Olyan objektum, amely megadja az összes művelet névvel után fog működni, és az állapot ', amely az eseményindító elfogadhatók tömb határozza meg.  Szeretné futtatni, miután sikeresen A és B lépést, ha például sikeres volt, vagy nem sikerült, a következő módon állítja össze `runAfter` tulajdonság:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Séma 2016-06-01-re frissítés

Az új 2016-06-01-séma való frissítés csak néhány másodperc.  Részletes tudnivalókat a módosításokat a séma [Ebben a cikkben](app-service-logic-schema-2016-04-01.md)találhatók.  A frissítési folyamat magában foglalja a frissítési parancsprogram mentése új logika alkalmazásként, és esetleg a régi logika alkalmazást, ha szükséges felülírása futtatása.

1. Nyissa meg az aktuális logika alkalmazást.
1. Kattintson a **Frissítés séma** gombra az eszköztáron
   
    ![][1]
   
    A frissített definíció visszatér.  Másolja és illessze be a erőforrás definícióját, ha van szüksége, de azt **ajánljuk** a **Mentés másként** gombra annak érdekében, az összes kapcsolat sikerült hivatkozások érvényesek a frissített logika alkalmazásban.
1. Kattintson a **Mentés másként** gombra az eszköztáron a frissítés a lap.
1. Töltse ki a nevét és logikai hálózatok alkalmazás állapota, majd kattintson a frissítés logika alkalmazások terjesztése **létrehozása** .
1. Ellenőrizze a frissített logika alkalmazás meg a várt módon működik.

    >[AZURE.NOTE] A kézi vagy felkérés eseményindító használja, ha a visszahívás URL-CÍMÉT az új logika alkalmazásban megváltoztak.  Az új URL-cím használatával ellenőrizze végpontok közötti működik, illetve átmásolhatja a meglévő logika alkalmazás előző URL-címek megőrzéséhez fölé.

1. *Nem kötelező.* Használja a **Adatfeliratsor** gombot (a **Frissítés séma** ikonra a fenti képen szomszédos) eszköztár írja felül az előző logika alkalmazást az új verzióval séma.  Ez a szükséges, csak akkor, ha szeretne megtartása ugyanaz az erőforrás-azonosító vagy kérése az eseményindító URL-CÍMÉT az logika alkalmazást.

### <a name="upgrade-tool-notes"></a>Jegyzetek eszköz frissítése

#### <a name="condition-mapping"></a>A feltétel hozzárendelése

Az eszköz láthatóvá válik a IGAZ és hamis ág műveletek együtt a frissített definícióban hatókör csoportba szeretne a legjobb munkamennyiség.  A Lekérdezéstervező mintát kifejezetten `@equals(actions('a').status, 'Skipped')` mint jelenjen meg egy `else` művelet.  Ha az eszköz észleli, hogy nem ismeri fel mintázatok fog esetleg azonban az IGAZ és hamis ág külön feltételek létrehozása.  Lehet, hogy újra megfeleltetett műveletek frissítés közzé, ha szükséges.

#### <a name="foreach-with-condition"></a>A feltétel ForEach
  
Az előző mintának elemenként feltételt tartalmazó foreach ciklus replikálhatók szűrőművelet új sémában.  Ez célszerű automatikusan történik a frissítés.  A feltétel előtt (csak az elemek, amelyek megegyeznek a feltétel tömb térni) foreach le szűrőművelet válik, és a tömbben van átadott foreach művelet.  Egy példa a [tartalom](app-service-logic-loops-and-scopes.md) megtekintése

#### <a name="resource-tags"></a>Erőforrás-címkék

Erőforrás-címkéket a program eltávolítja a frissítés, és meg kell őket újra beállítása a frissített munkafolyamatot.

## <a name="other-changes"></a>Egyéb módosítások

### <a name="manual-trigger-renamed-to-request-trigger"></a>Átnevezett kérelem eseményindító kézi eseményindító

A típus `manual` elavult és átnevezni `request` , milyen a `http`.  Ez a minta létrehozásához használja az eseményindító típusú egységesebb.

### <a name="new-filter-action"></a>Új "szűrő" művelet

Ha egy nagy tömb és szűrheti azokat az elemeket kevesebb beállítási le kell dolgozik, akkor az új "szűrő" típus is használhatja.  Fogadja tömb és egy feltétel és fog értékelése az egyes elemekre vonatkozó feltételt és a feltételnek megfelelő elemeket tömb vissza.

### <a name="foreach-and-until-action-restrictions"></a>ForEach és addig, amíg a művelet korlátozások

A foreach és addig, amíg a leállításig korlátozódik egyetlen művelet.

### <a name="trackedproperties-on-actions"></a>A műveletek TrackedProperties

Műveletek ekkor rendelkezésére áll egy további tulajdonság (az azonos szintű `runAfter` és `type`) nevű `trackedProperties`.  Egy objektum, amely meghatározza az egyes művelet ráfordítások és a kimeneti értékeket, amelyeket a munkafolyamat részeként kibocsátott Azure diagnosztikai telemetriai szerepeltetni kíván.  Példa:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Következő lépések
- [A logikai alkalmazás munkafolyamat-definíciót használata](app-service-logic-author-definitions.md)
- [Logika alkalmazás telepítési sablon létrehozása](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
