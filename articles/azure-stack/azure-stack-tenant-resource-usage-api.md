<properties
    pageTitle="Erőforrás-kihasználtság API bérlői |} Microsoft Azure"
    description="Hivatkozás az Erőforrás kihasználtsága API-val, amely lekérése az Azure Papírhalom kihasználtsági információk."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Erőforrás-kihasználtság API bérlői

A bérlő a bérlői API segítségével bérlői saját erőforrás-használati adatok megtekintéséhez. Ez az API egységesek az Azure használatát API-val (jelenleg a magánjellegű előzetes verzió).

A Windows PowerShell-parancsmag **Get-UsageAggregates** segítségével használati adatok, például az első Azure-ban.

## <a name="api-call"></a>API-hívást

### <a name="request"></a>Kérés

A kérés felhasználási részleteket a kért előfizetések és a kért időkereten kap. Nem nincs összehívás törzsébe.

| **A módszer**  | **Kérés URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumentumok

| **Argumentum**             | **Leírás** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure erőforrás-kezelő végpontja a Papírhalom Azure környezetben. |
| *subId*                   | Előfizetés azonosítója annak a felhasználónak a telefonhívás. Ez az API csak a lekérdezés használható egyetlen előfizetés használatát. Szolgáltatók a szolgáltató Erőforrás kihasználtsága API-t lekérdezés használati használható minden bérlők. |
| *reportedStartTime*       | A lekérdezés elindítása. *Dátum és idő* érték UTC szerint megadva, és az órát, például 13:00 elején kell lennie. Napi összesítésre Egyezményes éjfél állítsa ezt az értéket. Formátuma *escape* ISO 8601, például a Skype 2015-06-16T18 % 3a53 % 3a11 % 2b00 % 3a00Z, ahol a kettőspont való % 3a escape és plusz van escape % 2b, úgy, hogy könnyen megjegyezhető URI. |
| *reportedEndTime*         | A lekérdezés befejezési idő. A *reportedStartTime* vonatkozó korlátozások érvényesek ezt az argumentumot is. *ReportedEndTime* értéke nem lehet a jövőben. |
| *aggregationGranularity*  | Két különálló potenciális értékeket tartalmazó választható paraméter: napi és óránként. Az értékek alapján, mint egy adja vissza az adatokat a napi Granularitás, és a másik pedig az óránkénti felbontás. A napi lehetőséget az alapértelmezett érték. |
| *subscriberId*            | Előfizetés azonosítója. Úgy juthat az adatok szűrt, a szolgáltató közvetlen bérlő előfizetés azonosítója szükség. Ha nem előfizetés azonosítója paraméter nincs megadva, a hívás a szolgáltató közvetlen bérlők használati adatainak visszaadása. |
| *API-verzió*             | A kérés létesítéséhez használatos protokollt verziója. 2015-06-01-előnézet kell használnia. |
| *continuationToken*       | Jogkivonat beolvasni a használatát API-szolgáltató a legutóbbi hívást. Ez van szükség, ha a választ értéke nagyobb, mint 1000 sorokat. Ez az a könyvjelző előrehaladásra. Ha nincs megadva, az adatok beolvasásának az elejétől a nap, vagy átadott órát, granularitása alapján. |

### <a name="response"></a>Válasz

BEOLVASÁSA /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 és reportedEndTime = 2015-06-01T00 % 3a00 % 3a00 % 2b00 % 3a00 & aggregationGranularity = napi & api-verzió = 1,0

{

"érték":\[

{

"azonosító": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"név": "sub1-meterID1"

"típus": "Microsoft.Commerce/UsageAggregate",

"Tulajdonságok": {

"subscriptionId": "sub1",

"usageStartTime": "a Skype 2015-03-03T00:00:00 + 00:00",

"usageEndTime": "a Skype 2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" hely\\":\\" Alaszka\\",\\" címkék\\": null\\" additionalInfo\\": null}}",

"quantity":2.4000000000,

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Válasz adatai

| **Argumentum**      | **Leírás** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *azonosító*              | Az összesítés használatát egyedi azonosító |
| *név*            | Összesítés használatát neve |
| *típus*            | Erőforrás meghatározása |
| *subscriptionId*  | Azure felhasználó előfizetés azonosítója |
| *usageStartTime*  | A használati gyűjtő, amelyhez ez használatát összesítés tartozik UTC elindítása |
| *usageEndTime*    | UTC befejezési idő értékét a használati gyűjtő, amelyhez ez használatát összesítés tartozik |
| *instanceData*    | Kulcs – érték párokká a példány részletei (az új formátumra):<br>  *resourceUri*: teljesen minősített az erőforrás-azonosító, erőforrás és példány nevét is beleértve <br>  *hely*: Ez a szolgáltatás futtatásának terület <br>  *címkék*: az erőforrás-címkék, amely meghatározza a felhasználó <br>  *additionalInfo*: további részleteket az erőforrás-felhasználás alatt álló, például operációs rendszer verziója vagy kép típusát |
| *mennyiség*        | Az erőforrás-e időkereten előfordult felhasználás összege |
| *meterId*         | Egyedi azonosító, amely az erőforrás elfogyasztott (is hívott, *ResourceID*) |

## <a name="next-steps"></a>Következő lépések

[Használatát kapcsolatos gyakori kérdések](azure-stack-usage-related-faq.md)
