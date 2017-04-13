<properties
 pageTitle="A Feladatütemező korlátai és az alapértelmezett értékek"
 description="A Feladatütemező korlátai és az alapértelmezett értékek"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>A Feladatütemező korlátai és az alapértelmezett értékek

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>A Feladatütemező kvóták, korlátozások, alapértelmezett és szabályozás

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Az x-ms-kérés-id élőfej

Minden kérelme szemben a Feladatütemező szolgáltatás**x-ms-kérés-id**nevű válasz élőfej adja eredményül. Ez az élőfej-átlátszó értéket tartalmaz, amely egyedileg kell azonosítania a kérést.

Ha egy kérelmet az egységes adatkapcsolat és ellenőrizte, hogy megfelelően megfogalmazott-e a kérést, a ezt az értéket segítségével a Microsoft hibajelentés. Írja be a jelentésben, x-ms-kérés-id, közelítő időpontot a kérést, ez az érték, az előfizetést, feladat a webhelycsoportban, illetve feladat és a művelet, a kérelem próbált típusát azonosítója.

## <a name="see-also"></a>Lásd még:


 [Mi az a Feladatütemező?](scheduler-intro.md)

 [Azure ütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező PowerShell-parancsmagok hivatkozás](scheduler-powershell-reference.md)

 [Azure ütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure ütemező kimenő hitelesítést](scheduler-outbound-authentication.md)
