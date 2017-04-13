<properties
    pageTitle="Logika alkalmazás korlátai és konfigurációs |} Microsoft Azure"
    description="A szolgáltatás korlátai és a konfigurációs értékei logika alkalmazások áttekintése."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logika alkalmazás korlátai és konfiguráció

Az alábbiakban az Azure logika alkalmazásai aktuális korlátozások és konfigurációs adatok információkat.

## <a name="limits"></a>Korlátai

### <a name="http-request-limits"></a>HTTP kérelem korlátai

Ezek a korlátok egyetlen HTTP kérés és/vagy az összekötő-híváshoz

#### <a name="timeout"></a>Határidő

|név|Határérték|Jegyzetek|
|----|----|----|
|Kérelem időtúllépése|1 perc|Az [aszinkron mintázat](app-service-logic-create-api-app.md) vagy [addig, amíg a leállításig](app-service-logic-loops-and-scopes.md) is egyenlíti szükség szerint|

#### <a name="message-size"></a>Üzenet mérete

|név|Határérték|Jegyzetek|
|----|----|----|
|Üzenet mérete|50 MB|Egyes összekötők és az API-khoz nem támogatja a 50MB.  Kérés eseményindító támogatja a 25MB|
|Kifejezés frissítési korlát|131 072 karakterek|`@concat()`, `@base64()`, `string` hosszabb, mint ez nem lehet|

#### <a name="retry-policy"></a>Ismételje meg a házirend

|név|Határérték|Jegyzetek|
|----|----|----|
|Újrapróbálkozások|4|A [házirend-paraméter újra](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) konfigurálható tulajdonsággal|
|Max késleltetése|1 óra értéket|A [házirend-paraméter újra](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) konfigurálható tulajdonsággal|
|Min késleltetése|20 perc|A [házirend-paraméter újra](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) konfigurálható tulajdonsággal|

### <a name="run-duration-and-retention"></a>Futási időtartam és az adatmegőrzési

Ezek a korlátok egy egyetlen logika alkalmazás futtatásához.

|név|Határérték|Jegyzetek|
|----|----|----|
|Futtassa az időtartam|90 napon||
|Tárhely, adatmegőrzés|90 napon|A futtatási kezdési időpontot: az|
|Min ismétlődési gyakoriságát|15 másodperc||
|Max ismétlődési gyakoriságát|500 nap||


### <a name="looping-and-debatching-limits"></a>Hurok- és debatching korlátai

Ezek a korlátok egy egyetlen logika alkalmazás futtatásához.

|név|Határérték|Jegyzetek|
|----|----|----|
|ForEach elemek|5000|A [lekérdezés művelet](../connectors/connectors-native-query.md) segítségével szükség szerint nagyobb tömbök szűrése|
|Amíg iterációk száma|10 000||
|SplitOn elemek|5000||
|ForEach párhuzamos|20|Beállíthatja, hogy egy egymás után következő foreach való hozzáadásával `"operationOptions": "Sequential"` szeretne a `foreach` művelet|


### <a name="throughput-limits"></a>Átviteli korlátai

Ezek az egyetlen logika alkalmazás példány korlátai. 

|név|Határérték|Jegyzetek|
|----|----|----|
|Eseményindítók / szekundum|100|Terjesztheti munkafolyamatok több alkalmazások át igény szerint|

### <a name="definition-limits"></a>Definition korlátai

Ezek a korlátok egyetlen logika alkalmazás meghatározása.

|név|Határérték|Jegyzetek|
|----|----|----|
|A ForEach műveletek|1|Egymásba ágyazott munkafolyamatok, ha ki szeretné terjeszteni a szükség szerint hozzáadhat|
|Egy munkafolyamat műveletek|60|Ha ki szeretné terjeszteni a szükség szerint beágyazott munkafolyamatok is hozzáadhat|
|Engedélyezett művelet beágyazási z|5|Egymásba ágyazott munkafolyamatok, ha ki szeretné terjeszteni a szükség szerint hozzáadhat|
|Régió száma előfizetésenként / flow|1000||
|Eseményindítók egy munkafolyamat|10||
|Maximális karakterszám kifejezés|8192||
|Max `trackedProperties` betűkkel mérete|16000|
|`action`/`trigger`név korlát|80||
|`description`hossz korlát|256||
|`parameters`határérték|50||
|`outputs`határérték|10||

## <a name="configuration"></a>Konfiguráció

### <a name="ip-address"></a>IP-cím

Az [összekötő](../connectors/apis-list.md) hívások az alábbi IP-cím származnak.

Hívások alkalmazásból logika közvetlenül (azaz keresztül [HTTP](../connectors/connectors-native-http.md) vagy [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) tartozhat bármilyen [Azure adatközpont IP-tartományok szükségesek](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Logika alkalmazás terület|Kimenő IP|
|-----|----|
|Ausztrália kelet|40.126.251.213|
|Ausztrália Könyvesbolt is.|40.127.80.34|
|Brazília Dél|191.232.38.129|
|A központi India|104.211.98.164|
|A központi Amerikai Egyesült Államok|40.122.49.51|
|Kelet-ázsiai|23.99.116.181|
|Kelet-Amerikai Egyesült Államok|191.237.41.52|
|Kelet-amerikai 2|104.208.233.100|
|Japán keleti|40.115.186.96|
|Japán nyugati|40.74.130.77|
|A központi Észak-amerikai|65.52.218.230|
|Észak-Európa|104.45.93.9|
|A központi Dél-Amerikai Egyesült Államok|104.214.70.191|
|Délkelet-ázsiai|13.76.231.68|
|Dél-indiai|104.211.227.225|
|Nyugati Európa|40.115.50.13|
|Nyugati indiai|104.211.161.203|
|Nyugati Amerikai Egyesült Államok|104.40.51.248|


## <a name="next-steps"></a>Következő lépések  

- Első lépések logika alkalmazások, kövesse az [összefüggés-alkalmazás létrehozása](app-service-logic-create-a-logic-app.md) oktatóprogram.  
- [Nézet hétköznapi példát és -esetek](app-service-logic-examples-and-scenarios.md)
- [Üzleti folyamatok logika alkalmazások automatizálható](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Megtudhatja, hogy miként rendszerek integrálása összefüggés-alkalmazások](http://channel9.msdn.com/Events/Build/2016/P462)