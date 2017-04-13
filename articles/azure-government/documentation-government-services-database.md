<properties
    pageTitle="Azure kormányzati dokumentáció |} Microsoft Azure"
    description="Ez ez a témakör a szolgáltatást, és útmutatást összehasonlítás Azure kormányzati alkalmazások fejlesztéséhez"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure kormányzati adatbázisok

##  <a name="sql-database"></a>SQL-adatbázis

Olvassa el a<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft biztonsági központ SQL adatbázismotor</a> és [Azure SQL-adatbázis nyilvános dokumentációját](https://azure.microsoft.com/documentation/services/sql-database/) metaadatok láthatóság konfigurációs és ajánlott eljárások a védelem további útmutatást.

### <a name="variations"></a>Változatok

SQL-adatbázis V12 érhető el általában az Azure kormányzati.

A cím, az SQL Azure-kiszolgálók az Azure kormányzati különbözik:

Szolgáltatás típusa|Azure nyilvános|Azure kormányzati
---|---|---
SQL-adatbázis|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Megfontolandó szempontok

Az alábbi információk az Azure SQL az Azure kormányzati oszlopazonosító sorolja fel:

| Megengedett szabályozott/ellenőrzött adatok | Szabályozni/ellenőrzött adatok nem engedélyezett. |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Az összes adat tárolása és dolgozza fel a Microsoft Azure SQL Azure-kormányzati szabályozott adatokat tartalmazhat. Adatbáziseszközök adatátvitel adatok Azure kormányzati szabályozni kell használnia. | Azure SQL-metaadatok nem engedélyezett ellenőrzött exportálás adatokat tartalmazza. A metaadatok létrehozása és fenntartása a tárterület-termék megadott konfigurációs adatait tartalmazza.  A következő mezőkben szabályozott/ellenőrzött adatok nem kell megadni: az adatbázis neve előfizetés nevét, az erőforrás csoportok, kiszolgálónév, Server felügyeleti bejelentkezés, telepítési nevek, erőforrások nevei, erőforrás címkék

## <a name="azure-redis-cache"></a>Azure vgx.dll gyorsítótár

Ez a szolgáltatás és használatához a részletekért olvassa el [Azure vgx.dll gyorsítótár nyilvános dokumentációt](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Változatok

Az URL-címeit, elérésére és Azure vgx.dll gyorsítótárból az Azure kormányzati kezelése eltérőek:

Szolgáltatás típusa|Azure nyilvános|Azure kormányzati
---|---|---
Gyorsítótár végpont|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure portál|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Parancsfájlok és kódot kell a megfelelő végpontok és a környezetek számlája. További információ megtudhatja, [hogy miként csatlakozhat az Azure Government Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Megfontolandó szempontok

Az alábbi adatokat az Azure vgx.dll gyorsítótárból az Azure kormányzati oszlopazonosító sorolja fel:

| Megengedett szabályozott/ellenőrzött adatok | Szabályozni/ellenőrzött adatok nem engedélyezett. |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Az összes adat tárolása és Azure vgx.dll gyorsítótárból feldolgozásban Azure kormányzati szabályozott adatokat tartalmazhat. | Azure vgx.dll gyorsítótár-metaadatok nem engedélyezett ellenőrzött exportálás adatokat tartalmazza. A következő mezőkben szabályozni/ellenőrzött adatok nem kell megadni: neve VNET, előfizetés nevét, erőforráscsoport, erőforrás-címkék, nevét vgx.dll tulajdonságok gyorsítótár.  

##  <a name="next-steps"></a>Következő lépések

A kiegészítő információk és frissítések fizessen elő a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati Blog.</a>
