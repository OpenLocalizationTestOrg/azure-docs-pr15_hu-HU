<properties
    pageTitle="SQL-adatbázis rugalmas készlet árát és a teljesítmény"
    description="Rugalmas adatbázis készletek-specifikus árak adatait."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Rugalmas adatbázis készlet számlázás és árinformációkat

Rugalmas adatbázis készletek vannak számlát kapni, / a következő tulajdonságokat:

- A létrehozás után egy rugalmas készlet számlázva akkor is, ha nincs adatbázis-készletben.
- Egy rugalmas készlet az óránkénti számlát kapni. Ez a teljesítmény szintek egyetlen adatbázisok mint azonos mérési gyakoriság.
- Egy rugalmas készlet eDTUs egy új ugyanannyi átméretezi, ha majd pedig nem terheli szerint eDTUS új mennyiségét mindaddig, amíg a méretező művelet befejeződik. Ezt követi a azonos minta teljesítményével kapcsolatos önálló adatbázisok módosításával.


- Egy rugalmas készlet árát a készlet eDTUs számát alapul. Az ár egy rugalmas készlet a szám és a benne lévő rugalmas adatbázisok hasznosítása független.
- Ár számítja ki (készlet eDTUs száma) x (Egységár / eDTU).

Az Egységár eDTU egy rugalmas készlet nagyobb, mint az Egységár DTU ugyanabban a szolgáltatás rétegben a különálló adatbázis. A részletekért olvassa [SQL-adatbázis árak](https://azure.microsoft.com/pricing/details/sql-database/). 


A eDTUs és szolgáltatás rétegek, című cikkben talál részletes [SQL-adatbázis beállítások és a teljesítmény](sql-database-service-tiers.md).

## <a name="next-steps"></a>Következő lépések

- [Rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool-create-portal.md)
- [Figyelése, kezelése és egy rugalmas adatbázis készlet méretezése](sql-database-elastic-pool-manage-portal.md)
- [SQL-adatbázis beállítások és a teljesítmény: Mi érhető el az egyes szolgáltatási réteg ismertetése](sql-database-service-tiers.md)
- [Csoporttagokkal tartott egy rugalmas adatbázis készlet adatbázisok azonosítására szolgáló PowerShell-parancsprogramot](sql-database-elastic-pool-database-assessment-powershell.md)
