
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>A kapcsolati karakterlánc összehasonlítása


Az alábbi táblázat a kapcsolat karakterláncok, a C# programot kell a helyszíni SQL Server és a felhőben az Azure SQL-adatbázis csatlakoztatása, hogy miben más. A különbségeket félkövér szerepelnek.


| A kapcsolati karakterlánc<br/>Azure SQL-adatbázis | A kapcsolati karakterlánc<br/>A Microsoft SQL Server |
| :-- | :-- |
| Kiszolgáló =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Felhasználói azonosító = {your_loginName_here}**@{your_serverName_here}**;<br/>Jelszó = {your_password_here};<br/>**Adatbázis = {your_databaseName_here};**<br/>**a kapcsolat időtúllépési = 30**;<br/>**Titkosítása = True**;<br/>**TrustServerCertificate = False**; | Kiszolgáló = {your_serverName_here};<br/>Felhasználói azonosító = {your_loginName_here};<br/>Jelszó = {your_password_here}; |


A **adatbázis =** az SQL Server nem kötelező, de az SQL-adatbázis szükséges.


[.NET ADO SqlConnectionStringBuilder tulajdonságai](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) – ismerteti, hogy részletesen összes paramétert.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
