<properties
 pageTitle="Mi az Azure ütemező? | Microsoft Azure"
 description="Azure ütemező lehetővé teszi, hogy deklaratív ismertetik a műveletek futtatása a felhőben. Azt, majd ütemez, és ezeket a műveleteket automatikusan elindul."
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
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Mi az Azure ütemező?

Azure ütemező lehetővé teszi, hogy deklaratív ismertetik a műveletek futtatása a felhőben. Azt, majd ütemez, és ezeket a műveleteket automatikusan elindul.  A Feladatütemező végzi [az Azure-portálra](scheduler-get-started-portal.md), a kód, a [REST API -t](https://msdn.microsoft.com/library/mt629143.aspx)vagy a Azure PowerShell használatával.

A Feladatütemező hoz létre, akiknek, és elindítja az ütemezett munkamennyiség.  Nem minden munkaterhelésekből tárolása a Feladatütemező, illetve bármely kód futtatását. Informatikai csak _elindítja a_ kód máshol üzemeltetett – Azure, a helyszíni, vagy egy másik szolgáltatónál. HTTP, HTTPS, a tárhely várólista, bus várólista szolgáltatást vagy a szolgáltatás bus tematikus keresztül indítja el.

A Feladatütemező ütemezi a [feladatok](scheduler-concepts-terms.md), továbbra is a feladat-végrehajtás eredmények előzményeit, hogy egy áttekintheti, és deterministically, és biztos, hogy ütemezi munkaterhelésekből fog futni. Azure WebJobs (Azure-App szolgáltatásban a Web Apps alkalmazások szolgáltatás része) és más funkciók ütemezési Azure használja a háttérben ütemezőt. Az [Ütemező REST API](https://msdn.microsoft.com/library/mt629143.aspx) segítségével kezelheti a kommunikáció a következő műveletek. Ilyen ütemező képes [összetett ütemezések és speciális ismétlődés](scheduler-advanced-complexity.md) egyszerűen.

Több forgatókönyv működhet az ütemező használatát. Példa:

+ _Ismétlődő üzletialkalmazás-műveletek:_ Rendszeres időközönként gyűjtése hírcsatorna Twitter az adatokat.
+ _Napi karbantartást:_ Napi metszési naplók, a biztonsági mentés és egyéb karbantartási műveleteket hajt végre. Ha például a rendszergazda választhat, hogy az adatbázis biztonsági mentése az 1:00 de. minden nap, a következő kilenc hónapra.

A Feladatütemező létrehozása, módosítása, törlése, megtekintése és feladatok és a [feladat gyűjtemények](scheduler-concepts-terms.md) programozás útján, parancsfájlok segítségével és kezelése a portálon teszi lehetővé.

## <a name="see-also"></a>Lásd még:

 [Azure ütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Összetett ütemterveket és a Azure ütemezővel speciális ismétlődés létrehozásának](scheduler-advanced-complexity.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező PowerShell-parancsmagok hivatkozás](scheduler-powershell-reference.md)

 [Azure ütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure ütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)

 [Azure ütemező kimenő hitelesítést](scheduler-outbound-authentication.md)
