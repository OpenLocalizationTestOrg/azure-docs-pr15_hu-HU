<properties
   pageTitle="A JavaScript API jelentések használata |} Microsoft Azure"
   description="A Power BI beágyazott, a JavaScript API jelentések használata"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>A JavaScript API Power BI-jelentések használata

A Power BI JavaScript API lehetővé teszi, hogy könnyen beágyazása a Power BI szolgáltatásban az alkalmazásba. A API-val különböző jelentés elemeket, például a lapok és a szűrők az alkalmazások programozás útján másokkal. Interaktív Ez a funkció lehetővé teszi a Power BI szolgáltatásban az alkalmazás integráltabb része.

Az alkalmazás a Power BI-jelentés beágyazása egy iframe, amely helyezkedik el az alkalmazás részét képező használatával. Az iframe működik-e valamelyik oszlopazonosító között az alkalmazás és a jelentést, amint az alábbi képen látható. 

![A Power BI beágyazott iframe Javascript API nélkül](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Az iframe beágyazási a folyamat sokkal egyszerűbbé teszi, de a JavaScript API nélkül a jelentést, és az alkalmazás nem tud kapcsolatba egymással. Kapcsolati hiánya teheti az érzi, hogy a jelentés nem része nagyon az alkalmazást. A jelentés és az alkalmazás igazán szüksége kommunikációhoz a másikba, ahogy az alábbi képen.

![A Power BI beágyazott iframe Javascript API-val](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

A Power BI JavaScript API lehetővé teszi az iframe oszlopazonosító keresztül biztonságosan továbbíthatja kódírás. Ezzel az alkalmazás programozás útján végrehajtson egy műveletet a jelentésben, és a műveleteket, amelyeket a felhasználók hozhatnak létre a jelentést belül események meghallgatásához.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Mire használhatók a Power BI JavaScript API-val?
A JavaScript API-val jelentéseket, nyissa meg azt a jelentés lapjait, jelentés szűrése és kezelheti a beágyazás események. Az alábbi ábra mutatja az API szerkezete.

![A Power BI JavaScript API-diagram](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Jelentések kezelése
A Javascript API lehetővé teszi a jelentést, és az oldal szintjén viselkedés kezelése:

- A Power BI konkrét jelentést biztonságosan beágyazása az alkalmazás - [beágyazása bemutató alkalmazás](http://azure-samples.github.io/powerbi-angular-client/#/scenario1) kipróbálása
  - Jogkivonat beállítása
- A jelentés konfigurálása
  - Engedélyezése és letiltása a szűrőterület és a lap navigációs ablak – próbálja meg a [beállításokat a bemutató alkalmazás frissítése](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Lapok és a szűrők alapértelmezéseinek - próbálja meg, az [alapértelmezett értékek bemutató beállítása](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Adja meg, és kilépés a teljes képernyős módban

[További tudnivalók a jelentés beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Nyissa meg azt a jelentés lapjait
A JavaScript API enbales felfedezése a jelentés minden oldalát, és az aktuális lap beállítása. Próbálja meg a [navigációs bemutató alkalmazást](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[További tudnivalók az Oldalválasztás](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Jelentés szűrése
A JavaScript API alapvető és speciális szűrési lehetőségek beágyazott jelentések és a jelentés oldalainak biztosít. Próbálkozzon a [Szűrés bemutató alkalmazást](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), és tekintse át az egyes bevezető kódot.  


#### <a name="basic-filters"></a>Egyszerű szűrők
Egy egyszerű szűrés egy oszlop vagy a hierarchia szintre kerül, és belefoglalásához vagy kizárásához értékek listáját tartalmazza.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Speciális szűrők
Speciális szűrők használata a logikai operátorok és vagy vagy, és fogadja el az egy vagy két feltétel, saját operátor és értéket. Támogatott feltételek esetén:

- Nincs lehetőség
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Tartalmaz
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Van
- IsNot
- Üres
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[További információ a szűrésről](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Események kezelése
Mellett az iframe adatokat küld, a az alkalmazás is tartalmaz az alábbi események az iframe-ból érkező információkat:

- Beágyazása
  - betöltése
  - hibaüzenet
- Jelentések
  - pageChanged
  - dataSelected (hamarosan)

[További tudnivalók az események kezelése](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Következő lépések
További információt a Power BI JavaScript API-t tanulmányozza az alábbi hivatkozások:

- [A JavaScript API Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Objektummodelljéhez](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- A minták
  - [Szög](http://azure-samples.github.io/powerbi-angular-client)
  - [Decemberéig](https://github.com/Microsoft/powerbi-ember)
- [Élő bemutatása](https://microsoft.github.io/PowerBI-JavaScript/demo/)
