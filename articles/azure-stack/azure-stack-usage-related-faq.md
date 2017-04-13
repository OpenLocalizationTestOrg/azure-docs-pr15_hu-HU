<properties
    pageTitle="Használatát kapcsolatos gyakori kérdésekre |} Microsoft Azure"
    description="Azure Papírhalom méter, és Azure használatát API, használatát és jelentett idő, hibakódok összehasonlítása listája."
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

# <a name="azure-stack-usage-api-faqs"></a>Azure Papírhalom használatát API gyakori kérdések
Ez a cikk az Azure Papírhalom használatát API-val kapcsolatos gyakori kérdésekre ad választ.

## <a name="what-meter-ids-can-i-see"></a>Milyen állapotjelzője azonosítók jelennek meg?

Jelenleg használatát a hálózat, a tárhely és a számítási erőforrás szolgáltatók jelentés.

| **Erőforrás-szolgáltató** | **Állapotjelzője azonosító** |**Kijelző neve** | **Egység** | **További információ** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Hálózati** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Nyilvános IP-cím használata | IP-cím |                    
| **Tárhely**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*óra | Táblázatok elfogyasztott teljes kapacitásának |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*óra | Teljes kapacitásának, oldal BLOB elfogyasztott |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*óra  | A várakozási elfogyasztott teljes kapacitásának |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*óra  | Blokkokból álló BLOB elfogyasztott teljes kapacitásának |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Kérelem számát adja meg a 10,000s   | Táblázat a szolgáltatáskérések (10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Bejövő adatok adatok GB-ban | Táblázat szolgáltatás adatok bejövő adatok GB-ban |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress GB-ban | Táblázat szolgáltatás adatok kilépési GB-ban |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | 10,000s számának kérések | BLOB a szolgáltatáskérések (10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Bejövő adatok adatok GB-ban          | BLOB-szolgáltatás adatok bejövő adatok GB-ban 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress GB-ban              | BLOB-szolgáltatás adatok kilépési GB-ban 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | 10,000s számának kérések   | A várakozási a szolgáltatáskérések (10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Bejövő adatok adatok GB-ban         | Várólista szolgáltatást adatok bejövő adatok GB-ban 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress GB-ban  | Várólista szolgáltatást adatok kilépési GB-ban 
| **Számítási** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | Virtuális méret óra | Virtuális gép mérete |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Hogyan végezze el az Azure Papírhalom használatát API-k és összehasonlítása az [Azure használatát API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (jelenleg a nyilvános előzetes verzió)

-   A bérlő használati API egységesek az Azure API, egy kivétellel: a *showDetails* jelző jelenleg nem támogatott Azure egymást fedő.

-   A szolgáltató használatát API csak Azure Papírhalom vonatkozik.

-   Azure-ban elérhető [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) jelenleg Azure egymást fedő nem érhető el.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Mi a különbség használatát és a jelentett idő között?

Látogatottsági adatok jelentések két fő idő érték van:

-   A **jelentett idő**. Amikor valamilyen megadnak a használatát eseményt a használatát rendszerben

-   **Idő használatát**. Ha az Azure Papírhalom erőforrás felhasznált lett idő

Egy adott használatát esemény az értékek eltérés használatát és jelentett idő jelenhet meg. Lehet, hogy a késleltetés, amíg minden olyan környezetben több óra.

Jelenleg *csak a jelentett idő szerint*is lekérdezhetők.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Mit jelentenek a használatát API hibakódok?

| **HTTP-állapotkód** | **Kódszámú hiba jelenik meg** | **Leírás** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400 / – Hibás kérés        | *NoApiVersion*     | Hiányzik a *api-verzió* lekérdezési paraméter.
| 400 / – Hibás kérés        | *InvalidProperty*  | Hiányzik egy tulajdonság, vagy érvénytelen értéket tartalmaz. Az üzenet törzsébe válasz a hibakód: azonosítja a hiányzó tulajdonságot.
| 400 / – Hibás kérés        | *RequestEndTimeIsInFuture*  | Az *ReportedEndTime* értéke a jövőben. Értékek a jövőben nem használhatók az argumentumban.
| 400 / – Hibás kérés        | *SubscriberIdIsNotDirectTenant*    | A szolgáltató API-hívás egy előfizetés azonosítója, amely nem egy érvényes bérlői a hívó használja.
| 400 / – Hibás kérés        | *SubscriptionIdMissingInRequest*   | Az előfizetés azonosítója, a hívó hiányzik.
| 400 / – Hibás kérés        | *InvalidAggregationGranularity*   | Az érvénytelen összesítési Granularitás kérték. Érvényes értékei napi és óránkénti.
| 503                    | *ServiceUnavailable*   | Újrapróbálkozást lehetővé tevő hiba történt, mert a szolgáltatás elfoglalt vagy a hívás folyamatban van folyamatban. |

## <a name="next-steps"></a>Következő lépések
[Számlázás ügyfél- és Azure egymást fedő visszaterhelési](azure-stack-billing-and-chargeback.md)

[Szolgáltató erőforrás-kihasználtság API](azure-stack-provider-resource-api.md)

[Erőforrás-kihasználtság API bérlői](azure-stack-tenant-resource-usage-api.md)
