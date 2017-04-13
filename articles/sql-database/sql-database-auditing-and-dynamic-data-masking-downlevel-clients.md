<properties
    pageTitle="Támogatja az SQL-adatbázis régebbi ügyfelek és végponton módosítja a naplózás |} Microsoft Azure"
    description="Tudjon meg többet SQL-adatbázis régebbi ügyfelek számára nyújtott támogatás és IP-végpontot módosításokat a naplózás."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-adatbázis - támogatja a régebbi ügyfelek és végponton módosítja a naplózás


[Naplózás](sql-database-auditing-get-started.md) automatikusan működik-e, amely támogatja az átirányítást TDS SQL-ügyfelek számára.


##<a id="subheading-1"></a>Régebbi ügyfelek számára nyújtott támogatás

Bármilyen ügyfél által TDS 7.4 is kell támogatja az átirányítást. A kivételek JDBC 4.0-s, amelyben az átirányítási szolgáltatás nem teljesen támogatott, és mely átirányítása a Node.JS az Tedious implementálva. tartalmazzák.

"Régebbi ügyfelek" azaz mely támogatási TDS verzió 7.3 és alatti – a kiszolgáló FQDN a kapcsolat karakterlánca kell módosítani:

A kapcsolati karakterláncot az eredeti kiszolgáló FQDN: <*kiszolgálónév*>. database.windows.net

A kapcsolati karakterláncban módosított kiszolgáló FQDN: <*kiszolgálónév*> .database. **biztonságos**. windows.net

"Régebbi ügyfelek" részleges listáját tartalmazza:

- .NET 4.0-s és az alábbi,
- ODBC-10.0 és az alábbi.
- JDBC (JDBC TDS 7.4 támogatja, miközben a TDS átirányítás nem teljesen támogatott szolgáltatás)
- Fárasztó (a Node.JS)

**Megjegyzés:** Előfordulhat, hogy a fenti kiszolgáló FQDN módosítás is hasznos anélkül, hogy minden adatbázisban (az ideiglenes kezelési) konfigurációs lépés szükség egy SQL Server szintet a naplózás házirend alkalmazása.

##<a id="subheading-2"></a>Végponton megváltozik, amikor a naplózás engedélyezése

Felhívjuk a figyelmét arra, hogy naplózás engedélyezése, ha az IP-végpontot, az adatbázis változik. Ha szigorú tűzfalbeállításokat, frissítse-e tűzfal beállításai lehetőséget.

Az új adatbázis végponton az adatbázis-terület függ:

| Az adatbázis-terület | Lehetséges IP-végpontok |
|----------|---------------|
| Kína Észak  | 139.217.29.176, 139.217.28.254 |
| Kína keleti  | 42.159.245.65, 42.159.246.245 |
| Ausztrália keleti  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Ausztrália Könyvesbolt is. | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazília Dél  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| A központi Amerikai Egyesült Államok  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Kelet-ázsiai   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Kelet-amerikai 2 | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Kelet-Amerikai Egyesült Államok   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| A központi India  | 104.211.98.219, 104.211.103.71 |
| Dél-indiai   | 104.211.227.102, 104.211.225.157 |
| Nyugati India  | 104.211.161.152, 104.211.162.21 |
| Japán keleti   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japán nyugati    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| A központi Észak-amerikai  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Észak-Európa  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| A központi Dél-Amerikai Egyesült Államok  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Délkelet-ázsiai  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Nyugati Európa  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Nyugati Amerikai Egyesült Államok  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Közép Kanadán  | 13.88.248.106 |
| Kanada-keleti  |  40.86.227.82 |
