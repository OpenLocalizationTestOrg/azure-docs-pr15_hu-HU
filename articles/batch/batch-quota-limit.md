<properties
    pageTitle="A köteg szolgáltatás kvóták és korlátai |} Microsoft Azure"
    description="Alapértelmezett Azure köteg kvóták, korlátai és kényszerek, és hogy hogyan kérhet kvóta növeli"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kvóták és a köteg Azure szolgáltatás korlátai

Más Azure szolgáltatásokhoz áll korlátozások bizonyos a köteg szolgáltatáshoz tartozó erőforrások. Ezek a korlátok közül számos csak a alapértelmezett kvóták az előfizetést, vagy a fiók szintjén Azure által alkalmazott. Ez a cikk azt ismerteti, hogy az adott alapértékeit, és hogyan kérheti kvóta nő.

Ha gyártási munkaterhelésekből futtatásához a köteget, előfordulhat, legalább egy, a fenti az alapértelmezett kvótákat növeléséhez. Ha szeretne egy kvóta emelni, nyissa meg egy online [támogatási szolgálatát kérelem](#increase-a-quota) költség nélkül.

>[AZURE.NOTE] Kvóta hitelképesség korlátozott, a nem kapacitás garancia. Ha nagyméretű kapacitás igényeinek, kérjük, forduljon a támogatási Azure.

## <a name="subscription-quotas"></a>Előfizetés kvóták
**Erőforrás**|**Alapértelmezett korlát**|**Maximális érték**
---|---|---
Régió száma előfizetésenként egy köteg fiókok | 1 | 50

## <a name="batch-account-quotas"></a>Köteg fiók kvóták
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>További korlátozások
**Erőforrás**|**Maximális érték**
---|---
[Egyidejű feladatok](batch-parallel-node-tasks.md) / számítási csomópontot. | csomópont magmintákat 4 x számú
[Alkalmazások](batch-application-packages.md) egy köteg fiók        | 20
Egy alkalmazás alkalmazáscsomagok  | 40
Alkalmazás csomag mérete (minden)       | Megközelítőleg 195GB<sup>1</sup>

<sup>1</sup> azure méretkorlátot maximális block blob-méret

## <a name="view-batch-quotas"></a>Köteg kvóták megtekintése

A köteg fiók kvótákat megtekintése a [portálon Azure][portal].

1. A portálon válassza a **Köteg fiókok** , majd válassza ki a köteget fiókot, amely érdekli.

2. Válassza a **Tulajdonságok parancsot** a menüben a lap a köteg fiók

3. A Tulajdonságok lap jelenlegi színösszeállítását a köteg fiók **kvóták** jeleníti meg.

    ![Köteg fiók kvóták][account_quotas]

## <a name="increase-a-quota"></a>Kvóta növelése

Kövesse az alábbi lépéseket kell kvóta növelése az [Azure portál][portal].

1. Jelölje ki a portál irányítópult vagy a kérdőjel ikonra (****?) a **Súgó + támogatási** csempét a portál jobb felső sarkában.

2. Jelölje be az **új támogatási kérelem** > **alapjai**.

3. Az **alapvető tudnivalók** lap: a

    egy. **Hiba típusa** > **Kvóta**

    b. Jelölje ki azt az előfizetést.

    c billentyűkombinációt. **Kvóta típus** > **Köteg**

    d. **Támogatja a terv** > **Kvóta támogatása – része**

    Kattintson a **Tovább**gombra.

4. Kattintson a **probléma** lap:

    egy. Jelölje ki a **súlyosságát** aszerint, hogy az [üzleti hatása][support_sev].

    b. **Részletek**adja meg mindegyik kvótáját módosítani szeretné a köteg fiók nevét és az új korlát.

    Kattintson a **Tovább**gombra.

5. Kattintson a **kapcsolattartási adatok** lap:

    egy. Jelölje ki a **kívánt kapcsolatfelvételi mód**.

    b. Ellenőrizze, és adja meg a szükséges kapcsolattartási adatait.

    Kattintson a **Create** a támogatási kérelem gombra.

Miután a támogatási kérelmet beküldött, Azure támogatási Önnel a kapcsolatot. Látható, hogy a kérés végrehajtása legfeljebb 2 munkanapnak is igénybe vehet.

## <a name="related-topics"></a>Kapcsolódó témakörök

* [Az Azure portálon Azure köteg fiók létrehozása](batch-account-create-portal.md)

* [Azure köteg szolgáltatás áttekintése](batch-api-basics.md)

* [Azure előfizetés és szolgáltatás korlátozások, kvóták és korlátozások](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
