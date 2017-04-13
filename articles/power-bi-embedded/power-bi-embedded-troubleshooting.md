<properties
   pageTitle="Microsoft Power BI beágyazott előzetes verzió – hibaelhárítás"
   description="Microsoft Power BI beágyazott előzetes verzió – hibaelhárítás"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Microsoft Power BI beágyazott előzetes verzió – hibaelhárítás
Ez a cikk a **Power BI beágyazott**elhárítása választ ad.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Csatlakozás SQL Server-karakterláncok beállítása
Karakterlánc összekötő az SQL Server beállításához kövesse az egy bizonyos formátumban kell. Az alábbi van egy példa kapcsolati karakterláncot az SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Többet szeretne tudni az SQL Server kapcsolati karakterláncot, az alábbi cikkekben találhat:

-   [Az SQL Server Csatlakozási_karakterlánc](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Hitelesítő adatainak beállítása
Abban az esetben, ha rendelkezik hitelesítő adatokkal fejlesztési vagy fejlesztői környezet, például felhasználónevet és jelszót előfordulhat, hogy frissítenie kell hitelesítő adatait, amelyek megegyeznek a termelési megoldást.

## <a name="see-also"></a>Lásd még:
- [Minta – első lépések](power-bi-embedded-get-started-sample.md)
- [Mi az a Power BI beágyazott](power-bi-embedded-what-is-power-bi-embedded.md)
