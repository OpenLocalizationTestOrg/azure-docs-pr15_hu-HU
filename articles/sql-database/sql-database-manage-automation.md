<properties
    pageTitle="Azure SQL-adatbázisait Azure automatizálási kezelése |} Microsoft Azure"
    description="További tudnivalók: hogyan az Azure automatizálási szolgáltatásai használhatók a méretezés Azure SQL-adatbázisok kezelése."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Azure SQL-adatbázisait Azure automatizálási kezelése

Ez az útmutató fog megismerteti a Azure automatizálási szolgáltatásai számára, és hogyan használható az Azure SQL-adatbázisok kezelésének egyszerűsítése érdekében.


## <a name="what-is-azure-automation"></a>Mi az Azure automatizálási?

[Azure automatizálási](https://azure.microsoft.com/services/automation/) az Azure szolgáltatásainak egyszerűsítése a felhőbeli adatkezelési folyamat-automatizálása keresztül. Azure automatizálási használ, hosszabb ideig futó, manuális, hiba jobban és gyakran ismételt feladatok automatizálhatók növeléséhez megbízhatóság, a hatékonyság és a time to érték a szervezet számára.

Azure automatizálási tartalmaz egy erősen megbízható és könnyen hozzáférhető munkafolyamat végrehajtása motor, amely az igényeknek, ha a szervezet növekedésével méretezze át. Az Azure automatizálás folyamatok is lehet megrúgni manuálisan, 3rd külső rendszerek, illetve rendszeres időközönként, hogy a tevékenységek fordulhat elő, szükség esetén pontosan.

Csökkentse a terhelést műveleti és szabadítson fel informatikai / DevOps oktatói szűkítheti az üzleti hozzáadó munka értéke áthelyezésével, a felhőbeli adatkezelési feladatok Azure automatizálási automatikusan futtatható.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Hogyan segíthet az Azure automatizálási Azure SQL-adatbázisok kezelése?

A felügyelt Azure SQL-adatbázisban található automatizálás Azure az [Azure SQL-adatbázis PowerShell-parancsmagok](https://msdn.microsoft.com/library/dn546723.aspx) érhetők el az [Azure PowerShell-eszközök](https://msdn.microsoft.com/library/azure/jj156055.aspx)használatával. Azure automatizálási, hogy minden belül a szolgáltatás SQL-adatbázis kezelése tevékenységet végezhet tartalmaz beépített, Azure SQL-adatbázis PowerShell a parancsmagok érhető el. A parancsmagok az Azure automatizálás többi Azure szolgáltatás, összetett Azure szolgáltatásokat és a 3 külső rendszerek automatizálhatja a parancsmagokat párosítás is, is.

Azure automatizálási az azt jelenti, hogy közvetlenül, kommunikáció SQL-kiszolgálók a PowerShell használata SQL-parancsok azáltal tartalmaz.

Az [Azure automatizálási runbook gyűjtemény](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) termék csapat- és más közösségi runbooks Ismerkedés az Azure SQL-adatbázisait, más Azure-szolgáltatások és a 3 külső rendszerek irányításának automatizálása számos tartalmazza. Gyűjtemény runbooks a következők:

 * [SQL-lekérdezések futtatása ellen SQL Server-adatbázishoz](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Függőlegesen méretezése (felfelé vagy lefelé) időközönként az Azure SQL-adatbázis](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [SQL-tábla levágni, ha az adatbázis megközelíti a maximális méretére](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Ha az azok erősen tördelt tárgymutató-táblázatok Azure SQL-adatbázisban](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta Azure automatizálási, és hogyan használható az Azure SQL-adatbázisok kezelése alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne megtudni az Azure automatizálási.

- [Azure automatizálási – áttekintés](../automation/automation-intro.md)
- [Az első runbook](../automation/automation-first-runbook-graphical.md)
- [Azure automatizálási tanulási térkép](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure automatizálási: A SQL Agent a felhőben](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
