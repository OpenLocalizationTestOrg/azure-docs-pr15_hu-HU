<properties
   pageTitle="Állítsa be az SQL server tűzfal – áttekintés |} Microsoft Azure"
   description="Megtudhatja, hogy miként állítható be egy SQL-adatbázis tűzfal az való hozzáférés kezelése a kiszolgálón és adatbázis-szintjének tűzfalszabályokat."
   keywords="adatbázis-tűzfal"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Azure SQL-adatbázis tűzfalszabályokat konfigurálni \- – áttekintés


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-firewall-configure.md)
- [Azure portál](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [A PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-VAL](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-adatbázis Azure és más internetes alkalmazások relációs adatbázisból szolgáltatást nyújt. Az adatok védelme érdekében tűzfalak megakadályozása az összes access az adatbázis-kiszolgáló mindaddig, amíg megadhatja, hogy mely számítógépek engedélye. A tűzfal adatbázisok minden kérés származó IP-címe alapján hozzáférést biztosít.

A tűzfalbeállításokat elfogadható IP-címek tartományát megadó tűzfalszabályokat hoz létre. A kiszolgáló és az adatbázis szinteken tűzfalszabályokat hozhat létre.

- **Kiszolgálói szintű tűzfalszabályokat:** Szabályok engedélyezése ügyfelek hozzáférni a teljes Azure SQL-kiszolgálóhoz, ez azt jelenti, hogy a adatbázisokra az ugyanazon logikai kiszolgálón lévő. Ezeket a szabályokat a **fő** adatbázisba kerülnek. Kiszolgálói szintű tűzfalszabályokat beállítható a portálon vagy a Transact-SQL-utasítások használatával.
- **Adatbázis szintű tűzfalszabályokat:** Szabályok engedélyezése az Azure SQL-adatbázis kiszolgálója belül egyes adatbázisához ügyfelek. Szabályok-adatbázisonként hozhat létre, és az egyes adatbázisok mappába kerülnek. (Adatbázis szintű tűzfalszabályokat a **fő** adatbázist hozhat létre.) Ezeket a szabályokat az ugyanazon logikai kiszolgálón lévő egyes (biztonságos) adatbázisokhoz való hozzáférés korlátozása a hasznos lehet. Adatbázis-szintű tűzfalszabályokat csak a Transact-SQL-utasítások használatával állítható be.

**Javaslat:** Microsoft adatbázis szintű tűzfalszabályokat amikor csak lehetséges, hogy az adatbázis több hordozható és a biztonság fokozása használatát javasolja. Használata kiszolgálói szintű tűzfalszabályokat rendszergazdák számára, és amikor sok adatbázisokra, amelyek azonos access követelményeket és nem szeretne időt adatbázisonként egyenként konfigurálása.


## <a name="firewall-overview"></a>Tűzfal – áttekintés

Az Azure SQL-kiszolgálóhoz minden Transact-SQL nyelvben hozzáférés kezdetben a tűzfal blokkolja. Az Azure SQL server használatának megkezdéséhez az Azure portált, és adjon meg egy vagy több kiszolgálói szintű tűzfalszabályokat, hogy engedélyezze a hozzáférést az Azure SQL-kiszolgálóhoz. A tűzfal szabályok használatával adja meg, mely IP-címtartományok az internetről engedélyezett, és hogy Azure alkalmazások megpróbálhatja az Azure SQL-kiszolgálóhoz való csatlakozáshoz.

Szelektív engedélyt csak az adatbázisok az Azure SQL Server-alapú egyikére, létre kell hoznia egy adatbázis szintű szabályt a kívánt adatbázis. Adja meg az IP-címtartományokat meghaladja az IP-címtartományokat a kiszolgálói szintű tűzfal szabályban megadott adatbázis tűzfal szabályhoz, és győződjön meg arról, hogy az ügyfél IP-címét az adatbázis szintű szabály megadott tartomány esik.

Az internetről és Azure kapcsolatfelvételi először hatolhat a tűzfalon keresztül érik az Azure SQL server- vagy SQL-adatbázissal, mielőtt a következő ábrán látható módon.

   ![Diagram leíró tűzfal beállításainak.][1]

## <a name="connecting-from-the-internet"></a>Csatlakozás az internetről

Amikor egy számítógép megpróbál csatlakozni az adatbázis-kiszolgáló az internetről, a tűzfal először ellenőrzi a kérést, szemben a skype_for_businesshoz tűzfalszabályokat származó IP-címe:

- Ha a kérelem IP-címe valamelyik kiszolgálói szintű tűzfalszabályokat a megadott tartományba esik, a kapcsolat az Azure SQL-adatbázis kiszolgálója nyújtott.

- Ha a kérelem IP-címe nem belül a kiszolgálói szintű tűzfal szabályban megadott tartományok egyikére, az adatbázis szintű tűzfalszabályokat beadott. Ha a kérelem IP-címe valamelyik az adatbázis szintű tűzfalszabályokat megadott tartományba esik, a kapcsolat nyújtott csak az adatbázist, amelynek a megfelelő adatbázis-szintű szabály.

- Ha a kérelem IP-címe nem a kiszolgálói szintű vagy az adatbázis-szintű tűzfal szabályok, a csatlakozási kérelem sikertelen a megadott tartományba eső.

> [AZURE.NOTE] Azure SQL-adatbázis eléréséhez a helyi számítógépről, győződjön meg róla, a tűzfal a hálózati és a helyi számítógépen lehetővé teszi, hogy a kimenő kommunikáció 1433 portot.


## <a name="connecting-from-azure"></a>Csatlakozás az Azure

Az alkalmazások az Azure megkísérel csatlakozni az adatbázis-kiszolgálóhoz, a tűzfal ellenőrzi a, hogy az Azure kapcsolat engedélyezve van-e. Beállítás a kezdési és befejezési cím egyenlő 0.0.0.0 tűzfal azt jelzi, hogy ezek a kapcsolatok engedélyezettek. A kapcsolódási kísérletre nem engedélyezett, ha a kérelem nem éri el a Azure SQL-adatbázis azon kiszolgálóját.

> [AZURE.IMPORTANT] Ez a beállítás konfigurálja a tűzfalat, hogy engedélyezze az Azure beleértve kapcsolatok, a többi ügyfél adataitól előfizetésekről minden kapcsolat. Ha ezt a lehetőséget választva ellenőrizze, hogy a bejelentkezés, és felhasználói engedélyek csak az engedélyezett felhasználók hozzáférésének korlátozására.

Kapcsolatok az Azure kétféle módon engedélyezheti:

- A [Microsoft Azure-portálra](https://portal.azure.com/)jelölje be a jelölőnégyzetet, **access server azure szolgáltatások engedélyezése** , amikor új kiszolgáló létrehozása.

- A [Klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=161793), a **beállítás** lapon a kiszolgálón, az **Engedélyezett szolgáltatások** csoportban kattintson az **Igen** gombra **a Microsoft Azure**-szolgáltatásokhoz.

## <a name="creating-the-first-server-level-firewall-rule"></a>Az első kiszolgálói szintű tűzfal szabály létrehozása

Az első kiszolgálói szintű tűzfal beállítás az [Azure portálon](https://portal.azure.com/) vagy a REST API-t vagy a Azure PowerShell programozás útján használatával hozhat létre. További kiszolgálói szintű tűzfalszabályokat hozhatók létre, és kezelhető ezekkel az eljárásokkal, valamint a Transact-SQL nyelvben keresztül. A teljesítmény javítása érdekében kiszolgálói szintű tűzfalszabályokat ideiglenes gyorsítótárba kerül az adatbázis szintjén. A gyorsítótár frissítése, olvassa el a [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). A kiszolgálói szintű tűzfalszabályokat további tudnivalókért lásd: [hogyan: állítsa be az Azure SQL server tűzfal az Azure portálon](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Adatbázis-szintű tűzfalszabályokat létrehozása

Miután beállította az első kiszolgálói szintű tűzfal, érdemes való hozzáférés korlátozása bizonyos adatbázisok. Az IP-címtartományokat a adatbázis szintű tűzfal szabályt, amely kívül esik a kiszolgálói szintű tűzfal szabályban megadott ad meg, csak az adatbázis szintű tartomány IP-címeket tartalmazó ügyfelek érhető el az adatbázist. Beállíthatja, hogy legfeljebb 128 adatbázis szintű tűzfalszabályokat az adatbázis. A fő- és az adatbázisok adatbázis szintű tűzfalszabályokat lehet létrehozni és kezelhetők a Transact-SQL nyelvben. Adatbázis-szintű tűzfalszabályokat konfigurálásával kapcsolatos további tudnivalókért lásd: [sp_set_database_firewall_rule (Azure SQL-adatbázisait)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Tűzfalszabályokat programozás útján kezelése

Az Azure portál kívül tűzfalszabályokat programozás útján a Transact-SQL nyelvben, REST API-val és Azure PowerShell használatával kezelhető. Az alábbi táblázat ismerteti a készlete mindegyik módszernek elérhető parancsokat.


### <a name="transact-sql"></a>A Transact-SQL nyelvben

| Katalógus nézetben vagy a tárolt eljárás                                                           | Szint     | Leírás                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Kiszolgáló    | Megjeleníti az aktuális kiszolgálói szintű tűzfalszabályokat     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Kiszolgáló    | Hoz létre, vagy kiszolgálói szintű tűzfalszabályokat frissítése       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Kiszolgáló    | Kiszolgálói szintű tűzfalszabályokat eltávolítása                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Adatbázis  | Megjeleníti az aktuális adatbázis szintű tűzfalszabályokat   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Adatbázis  | Hoz létre, vagy az adatbázis szintű tűzfalszabályokat frissítése |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Adatbázisok | Adatbázis-szintű tűzfalszabályokat eltávolítása                |

### <a name="rest-api"></a>REST API-VAL

| API                  | Szint  | Leírás                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Lista tűzfalszabályokat](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Kiszolgáló | Megjeleníti az aktuális kiszolgálói szintű tűzfalszabályokat                 |
| [Szabály létrehozása](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Kiszolgáló | Hoz létre, vagy kiszolgálói szintű tűzfalszabályokat frissítése                   |
| [Szabály beállítása](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Kiszolgáló | A tulajdonságok egy meglévő kiszolgálói szintű tűzfal szabály frissítések |
| [Szabály törlése](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Kiszolgáló | Kiszolgálói szintű tűzfalszabályokat eltávolítása                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Parancsmag                                                                                              | Szint  | Leírás                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Kiszolgáló | Az aktuális kiszolgálói szintű tűzfalszabályokat függvény                  |
| [Új AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Kiszolgáló | Létrehoz egy új kiszolgálói szintű tűzfal szabály                         |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Kiszolgáló | A tulajdonságok egy meglévő kiszolgálói szintű tűzfal szabály frissítések |
| [Eltávolítás-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Kiszolgáló | Kiszolgálói szintű tűzfalszabályokat eltávolítása                              |

> [AZURE.NOTE] Lehetnek, hogy be, mint amennyit a tűzfal beállításai csak akkor lépnek érvénybe módosítások késleltetése 5 perc.

## <a name="troubleshooting-the-database-firewall"></a>Az adatbázis tűzfal hibáinak elhárítása

Ha a Microsoft Azure SQL-adatbázis szolgáltatáshoz való hozzáférés nem a várt módon működnek várt, vegye figyelembe az alábbiakat:

- **Helyi tűzfal beállításainak:** Mielőtt a számítógép hozzáfér Azure SQL-adatbázissal, előfordulhat, hozzon létre egy tűzfal kivétel az TCP-portok 1433 a számítógépen. Ha belül az Azure felhő oszlopazonosító kapcsolatok készít, előfordulhat, nyissa meg a további portokat. További tudnivalókért lásd: a **V12 az SQL-adatbázis: belüli és kívüli** [portokon kívül ADO.NET 4.5 és SQL-adatbázis V12 1433](sql-database-develop-direct-route-ports-adonet-v12.md)szakaszát.

- **Hálózati címfordító cím:** Hálózati címfordító Eszközig, hogy az Azure SQL-adatbázishoz való csatlakozáshoz a számítógép által használt IP-cím lehet ugyanaz, mint a számítógép IP konfigurációs beállításai között látható IP-címét. Az IP-cím, a számítógép használatával csatlakozhat az Azure megtekintéséhez jelentkezzen be a portálra, és keresse meg a kiszolgáló, amely az adatbázis **konfigurálása** lapján. Az **Engedélyezett IP-címek** csoportban jelenik meg az **Aktuális ügyfél IP-címe** . Kattintson a **Hozzáadás** az **Engedélyezett IP-címek** engedélyezése ezen a számítógépen a kiszolgáló elérésére.

- **a megengedve lista módosításai nem léptek érvénybe még:** Lehetnek olyan, mint amennyit az Azure SQL-adatbázis tűzfal beállítás érvénybe léptetéséhez módosítások késleltetése 5 perc.

- **Nincs engedélyezve a bejelentkezési vagy helytelen jelszót használt:** Ha bejelentkezési nem rendelkezik engedélyekkel a Azure SQL-adatbázis-kiszolgálón vagy használt beírt jelszó helytelen, az Azure SQL-adatbázis kiszolgálója a kapcsolatot van megtagadja. Csak a tűzfal beállítás létrehozása biztosít az ügyfelek lehetőséget próbálja a csatlakozás a kiszolgálóhoz; minden ügyfél meg kell adnia a szükséges hitelesítő adatokat. Bejelentkezések előkészítésével kapcsolatos további tudnivalókért lásd: az adatbázisok kezelése, a bejelentkezési és a felhasználók Azure SQL-adatbázisban.

- **Dinamikus IP-cím:** Ha rendelkezik internetkapcsolattal rendelkező dinamikus IP-címek és problémái vannak a tűzfalon keresztül első, megpróbálhatnak a következő megoldások egyikét:

 - Kérje meg az Internet Service Provider (Internetszolgáltató) az IP-cím tartomány az ügyfélszámítógépek által elérhető az Azure SQL-adatbázis kiszolgálója rendelt, és adja hozzá az IP-címtartományokat a tűzfal szabály.

 - Statikus IP-cím helyett az ügyfélszámítógépek számára beolvasása, és adja hozzá az IP-címek a tűzfalszabályokat szerint.

## <a name="next-steps"></a>Következő lépések

Mennyi kiszolgáló és adatbázis-szintjének tűzfalszabályokat létrehozásával kapcsolatos cikkekre, lásd:

- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni az Azure portál használatával](sql-database-configure-firewall-settings.md)
- [Azure SQL-adatbázis kiszolgáló és adatbázis-szintjének tűzfalszabályokat az SQL-T használ konfigurálása](sql-database-configure-firewall-settings-tsql.md)
- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni PowerShell használatával](sql-database-configure-firewall-settings-powershell.md)
- [A REST API Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályok konfigurálása](sql-database-configure-firewall-settings-rest.md)

Az adatbázis létrehozására a oktatóanyagban [létrehozása az Azure portálon percben SQL-adatbázishoz](sql-database-get-started.md)című témakör tartalmaz.
A Megnyitás vagy harmadik fél által fejlesztett alkalmazások csatlakoztatása az Azure SQL-adatbázishoz című témakörben kaphat segítséget [ügyfél rövid mintakódok SQL-adatbázishoz](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Adatbázisok navigáljon annak megértéséhez, olvassa el a [adatbázis az access és a bejelentkezési biztonságának kezelési](https://msdn.microsoft.com/library/azure/ee336235.aspx)című témakört.



## <a name="additional-resources"></a>További források

- [Az adatbázis biztonságossá tétele](sql-database-security.md)
- [Biztonság otthon az SQL Server adatbázismotort és Azure SQL-adatbázis](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
