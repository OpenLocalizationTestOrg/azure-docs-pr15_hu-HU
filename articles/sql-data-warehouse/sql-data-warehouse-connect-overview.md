<properties
   pageTitle="Csatlakozás Azure SQL-adatraktár |} Microsoft Azure"
   description="Keresheti meg a kiszolgáló neve és a kapcsolati karakterláncot az Azure SQL-adatraktár való"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Csatlakozás Azure SQL-adatraktár

Ez a cikk segítséget nyújt az első alkalommal kapcsolódás az SQL adatraktár.

## <a name="find-your-server-name"></a>A kiszolgáló nevének megállapítása

Csatlakozás SQL adatraktár első lépése van arra, hogy miként megkeresni a nevét.  Például a kiszolgáló nevét, az alábbi példában is sample.database.windows.net. A teljesen minősített kiszolgálónév megkeresése:

1. Nyissa meg az [Azure-portálon][].
2. **SQL-adatbázisait** gombja 
3. Kattintson az adatbázist, amelyhez csatlakozni szeretne.
4. Keresse meg a kiszolgáló teljes nevét.

    ![Kiszolgáló teljes nevét][1]

## <a name="supported-drivers-and-connection-strings"></a>Támogatott illesztőprogramok és a kapcsolati karakterláncot

Azure SQL-adatraktár [ADO.NET][], [ODBC][], [PHP][]és [JDBC][]támogatja. Kattintson az előző illesztőprogramokat keresse meg a legújabb verziója és a dokumentáció egyik. A kapcsolati karakterláncot az illesztőprogram használni kívánt előrejelzést az Azure portálról, kattintson az **adatbázis-kapcsolatot karakterláncok megjelenítése** az előző példában szereplő kattinthat.  Az alábbiakban is néhány példát a kapcsolati karakterlánc néz ki az egyes illesztőprogram.

> [AZURE.NOTE] Fontolja meg a kapcsolat időtúllépési 300 másodperc is tovább elérhetetlenség rövid ideig a kapcsolat engedélyezése beállítást.

### <a name="adonet-connection-string-example"></a>ADO.NET csatlakozási karakterlánc például

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC-kapcsolati karakterlánc példa

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP csatlakozási karakterlánc például

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Kapcsolat-karakterlánc például JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Csatlakozási beállítások

SQL adatraktár egységesítik az egyes beállítások csatlakoztató és -objektum létrehozása során. Ezeket a beállításokat nem írhatók felül, és tartalmazza:

| Adatbázis beállítása       | Érték                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | TOVÁBB                           |
| [QUOTED_IDENTIFIERS][] | TOVÁBB                           |
| [DÁTUMFORMÁTUM][]         | MDY                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Következő lépések

Csatlakozhat, és a Visual Studio lekérdezés, olvassa el a [lekérdezés, amelynek a Visual Studio][]című témakört. Hitelesítési beállítások kapcsolatos további tudnivalókért lásd: [Azure SQL-adatraktár hitelesítés][].

<!--Articles-->
[A Visual Studio lekérdezés]: ./sql-data-warehouse-query-visual-studio.md
[Adatraktár Azure SQL-hitelesítés]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DÁTUMFORMÁTUM]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portál]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


