<properties
   pageTitle="Az elosztott tranzakciók felhő adatbázisok között"
   description="Rugalmas adatbázis-tranzakciók, az Azure SQL-adatbázis áttekintése"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Az elosztott tranzakciók felhő adatbázisok között

Rugalmas adatbázis-tranzakciók, Azure SQL-adatbázis (SQL-adatbázis) lehetővé teszi az SQL-adatbázis több adatbázisok átterjedő tranzakciók futtatása. Rugalmas adatbázis-tranzakciók, az SQL-adatbázis használata ADO .NET .NET-alkalmazások érhetők el, és integrálása a már jól ismert alkalmazásprogramozási felület [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) osztályok használatával. A tárban című témakörben kaphat [a .NET-keretrendszer 4.6.1 (webes telepítő)](https://www.microsoft.com/download/details.aspx?id=49981).

A helyszíni ilyen példa általában szükséges futtató Microsoft elosztott tranzakciók koordinátora (MSDTC). Mivel MSDTC Platform-mint-a-szolgáltatásalkalmazás Azure-ban nem érhető el, az azt jelenti, hogy elosztott tranzakciók koordinálása most közvetlenül integrálva van az SQL-adatbázis. Alkalmazások bármely elosztott tranzakciók elindítására SQL-adatbázis csatlakoztatása, és átlátszó az adatbázisok valamelyikével fog koordinálása a elosztott tranzakciók, az alábbi ábrán látható módon. 

  ![Azure SQL-adatbázisokkal rugalmas adatbázis-tranzakciók elosztott tranzakciók ][1]

## <a name="common-scenarios"></a>Gyakori alkalmazási területek

Rugalmas adatbázis-tranzakciók, az SQL-adatbázis tudják az alkalmazások számos különböző SQL-adatbázisait tárolt adatok atomi módosításához. Az előnézet ügyféloldali fejlesztési élményt C# és .NET koncentrál. Később a termék tervezett a kiszolgálóoldali használhatóság az SQL-T használja.  
Rugalmas adatbázis-tranzakciók célként az alábbi esetekben:

* Azure-ban több adatbázis-alkalmazások: Ebben az esetben az adatok függőlegesen particionált több SQL DB-adatbázisok között úgy, hogy a különböző típusú más adatbázisokat a helyezkednek el. Néhány művelet a két vagy több adatbázis tartott adatmódosítás szükség. Az alkalmazás rugalmas adatbázis-tranzakciók használja, a változtatások koordinálása adatbázisok közötti, valamint atomicity.

* Azure-ban sharded adatbázis-alkalmazások: Ebben az esetben a az adatok réteg használja a [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md) vagy a saját-sharding vízszintesen partition az adatokat számos SQL-adatbázis-adatbázisok között. Egy hangjelzéseket használatieset atomi módosításokat végrehajtani sharded több bérlői alkalmazáshoz, ha a módosítások időtartomány bérlők szükség. Például érdemes az egyik bérlőből egy másikra, két különböző adatbázisok tartózkodó átvitel. A második eset még külön sharding kapacitás van szükség, amiből általában az, hogy bizonyos atomi műveleteket kell-e több adatbázisok ugyanahhoz a bérlőhöz használható különböző egymástól nagyméretű bérlői webhelyre igazodik. A harmadik eset még atomi frissítések adatbázisok közötti replikált adatok hivatkozni szeretne. Ezek azok a sorok mentén atomi, feldolgozott, műveletek összehangolt most is, a minta használatával több adatbázisok között.
Rugalmas adatbázis-tranzakciók kétfázisú segítségével tranzakció atomicity biztosítása adatbázisok között. Egyetlen tranzakción belül egyszerre 100-nál kevesebb adatbázisok tranzakciók közelítését. Ezek a korlátok ne lép életbe, de egyik teljesítmény és rugalmas adatbázis-tranzakciók ezek a korlátok túllépésekor érheti sikerességéről kell számítania.


## <a name="installation-and-migration"></a>Telepítés és áttelepítés

Rugalmas adatbázis-tranzakciók, az SQL-adatbázis lehetőségeit a .NET-dokumentumtárak System.Data.dll és System.Transactions.dll frissítések állnak rendelkezésre. A DLL-ek győződjön meg arról, hogy kétfázisú használatos annak biztosítására, atomicity szükség. Szeretné kezdeni, fejlesztett alkalmazások használatával rugalmas adatbázis-tranzakciók, telepítse a [.NET-keretrendszer 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) vagy újabb verziója. A .NET-keretrendszer korábbi verziójában forgalmi tranzakciók elosztott tranzakció való előléptetése meghiúsul, és kivételt emelni kell.

A telepítés után a is használhatja a elosztott tranzakció API-khoz System.Transactions az SQL-adatbázis-kapcsolatot. Ha meglévő MSDTC alkalmazások használatával ezeket API-hoz, egyszerűen újraépítéséhez alkalmazást a meglévő alkalmazások .NET 4.6 a 4.6.1 telepítése után keretrendszer. A projektek Megcélozhat .NET 4.6, ha automatikusan fogja használni a frissített DLL-ek az új keretrendszer verzióról, és API együtt SQL-adatbázis-kapcsolatot a hívások elosztott tranzakciók most fog sikerülni.

Ne feledje, hogy rugalmas adatbázis-tranzakciók nincs szükség MSDTC telepítése. Ehelyett rugalmas adatbázis-tranzakciók közvetlenül kezelhetők által és SQL-adatbázis belül. Ez jelentősen leegyszerűsíti a felhőben esetek óta MSDTC álló telepítés már nem szükséges elosztott tranzakciók használata SQL-adatbázis. Szakasz 4 részletesen ismerteti, hogyan rugalmas adatbázis-tranzakciók és a felhő alkalmazások Azure együtt szükséges .NET-keretrendszer telepítését.

## <a name="development-experience"></a>Fejlesztési élmény

### <a name="multi-database-applications"></a>Több adatbázis-alkalmazások

A példa a már jól ismert alkalmazásprogramozási felület .NET System.Transactions használja. A TransactionScope osztály a .NET-környezeti tranzakció hoz létre. ("Környezeti tranzakció" az egyik, amely az aktuális szál él.) Minden kapcsolat megnyitni a TransactionScope belül a tranzakció részt. Ha másik adatbázisok részt, a tranzakció automatikusan való elosztott tranzakció magasabb szintű. Az így létrejövő tranzakció szabályozza, hogy a jóváhagyás elvégzéséhez a hatókör beállításáról.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Sharded adatbázis-alkalmazások
 
Rugalmas adatbázis-tranzakciók, az SQL-adatbázis is támogat koordinálása elosztott tranzakciók, amikor használni a OpenConnectionForKey módszer a rugalmas adatbázis ügyfél tár nyissa meg a kapcsolatok méretezett el adatok a réteg. Fontolja meg az esetekben, ahol szükséges módosítások egységessége garantálja keresztül számos különböző sharding kulcs értékeit. A különböző sharding legfontosabb értékeit szolgáltatója shards kapcsolatok vannak brokered OpenConnectionForKey használatával. Az általános lehetőséget választja a kapcsolatok megadható különböző shards, hogy az elosztott tranzakció tranzakció alapú garanciákkal biztosítva van szükség. A következő kódot minta, amely bemutatja ezt a módszert. Azt feltételezi, hogy egy shardmap nevű változó jelző shard térkép a rugalmas adatbázis ügyfél-tárból:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Azure Cloud Services .NET telepítése

Azure itt host .NET-alkalmazások számos szeretne rendelni. A különböző ajánlataiban összehasonlítása a [Azure alkalmazás szolgáltatás, a Felhőszolgáltatások, és a virtuális gépeken futó összehasonlító](../app-service-web/choose-web-site-cloud-service-vm.md)érhető el. Ha vendégként OS a ajánlja fel a .NET rugalmas tranzakciók szükséges 4.6.1 kisebb, akkor frissítenie kell a vendégként való bekapcsolódáshoz OS 4.6.1. 

Az Azure-alkalmazás szolgáltatások frissítéseire a vendégként való bekapcsolódáshoz operációs rendszer jelenleg nem támogatottak. Az Azure virtuális gépeken futó, egyszerűen jelentkezzen be a virtuális, és futtassa a legújabb a .NET-keretrendszer telepítőjét. Azure Felhőszolgáltatások frissítenie kell a üzembe indítási feladatainak be egy újabb .NET-verzió telepítését. Az fogalmak és a lépések [.NET telepítése egy felhőalapú szolgáltatás szerepkörei](../cloud-services/cloud-services-dotnet-install-dotnet.md)ismertetett.  

Figyelje meg, hogy a telepítő a .NET rendszerhez 4.6.1 előírhatja ideiglenes további tárterületet a .NET 4.6 mint a telepítő Azure cloud services bootstrapping során. Ahhoz, hogy a sikeres telepítést, kell növelése az Azure felhőszolgáltatásba a ServiceDefinition.csdef fájl LocalResources részében, illetve az Indítás tevékenység környezet beállításainak ideiglenes tárterület az alábbi példa látható módon:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Több kiszolgálóin tranzakciók

Rugalmas adatbázis-tranzakciók különböző logikai kiszolgálóin található Azure SQL-adatbázis használata támogatott. Tranzakciók cross logikai server határok, ha a résztvevő kiszolgálók először kölcsönös kommunikációs kapcsolat bevinni kell. Miután a kapcsolati kapcsolat létrejött bármilyen a két kiszolgáló az adatbázist a kiszolgálóról adatbázisokkal rugalmas tranzakciók vehet részt. A kettőnél több logikai kiszolgálók tartó tranzakciók kommunikáció kapcsolat kell lennie bármely pár logikai kiszolgálók helyet.

Az alábbi PowerShell-parancsmagok használata az rugalmas adatbázis-tranzakciók határokon kiszolgáló közötti kommunikáció kapcsolatok kezelése:

* **Új-AzureRmSqlServerCommunicationLink**: Ezzel a parancsmaggal használható kapcsolat létrehozása az új kommunikációs között két logikai Azure SQL-adatbázis kiszolgálója. A kapcsolat esetén szimmetrikus ami azt jelenti, hogy mindkét kiszolgáló kezdeményezhet tranzakciók a kiszolgálóval.
* **Get-AzureRmSqlServerCommunicationLink**: Ezzel a parancsmaggal használható beolvasásához a kapcsolatok kommunikációs és tulajdonságaik.
* **Eltávolítás-AzureRmSqlServerCommunicationLink**: Ezzel a parancsmaggal kommunikációs meglévő kapcsolat eltávolításához használja. 


## <a name="monitoring-transaction-status"></a>Transaction állapot ellenőrzése

Az SQL-adatbázis dinamikus kezelése nézetek (DMVs) használatával monitor állapotát és előrehaladását a folyamatban lévő rugalmas adatbázis-tranzakciók. Az SQL-adatbázis elosztott tranzakciók tranzakciók kapcsolódó összes DMVs fontosak. A megfelelő listáját DMVs itt talál: [tranzakció kapcsolódó dinamikus kezelése nézetek és a függvények (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Ezek a DMVs különösen hasznosak:

* **sys.dm\_tran\_aktív\_tranzakciók**: az aktuálisan aktív tranzakciók és állapotuk sorolja fel. A UOW (munka egység) oszlop azonosíthatja a különböző gyermek tranzakciók ugyanazon elosztott tranzakció tartoznak. Ugyanazon elosztott tranzakción belül az összes tranzakció UOW azonos értékkel végrehajtása. További információt a [DMV dokumentációjában](https://msdn.microsoft.com/library/ms174302.aspx) találhat.
* **sys.dm\_tran\_adatbázis\_tranzakciók**: tranzakciók, például a naplóban tranzakció helyzetének további információval szolgál. További információt a [DMV dokumentációjában](https://msdn.microsoft.com/library/ms186957.aspx) találhat.
* **sys.dm\_tran\_zárolások**: a megtalálható zárolt jelenleg folyamatban lévő tranzakcióinak információt tartalmaz. További információt a [DMV dokumentációjában](https://msdn.microsoft.com/library/ms190345.aspx) találhat.

## <a name="limitations"></a>Korlátozások 

Rugalmas adatbázis-tranzakciók, az SQL-adatbázis jelenleg alkalmazni az alábbi korlátozások vonatkoznak:

* Csak a tranzakciókat SQL DB-adatbázisok közötti támogatottak. Más [X parancsra, és nyissa meg a XA](https://en.wikipedia.org/wiki/X/Open_XA) erőforrás-szolgáltatók és SQL-adatbázis-Ön kívüli adatbázisok nem részvétel rugalmas adatbázis-tranzakciók. Ez azt jelenti, hogy rugalmas adatbázis-tranzakciók nem tudja a helyszíni SQL Server és Azure SQL-adatbázisait keresztül egymástól. A helyszíni elosztott tranzakciókat továbbra is használhatja a MSDTC. 
* Csak egyeztetett ügyfél .NET-alkalmazásból tranzakciók támogatottak. Kétmintás T-SQL nyelvben például ELOSZTOTT tranzakciók MEGKEZDÉSÉHEZ kiszolgálóoldali támogatása a tervezett, de még nem áll rendelkezésre. 
* Csak a Azure SQL-adatbázis V12 adatbázisok támogatottak.
* WCF szolgáltatásban tranzakciók nem támogatottak. Ha például, amely a tranzakciók végrehajtja a WCF szolgáltatás módszer van. A hívás a tranzakciók hatálya alá kerülő megegyezik egy [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception)sikertelen lesz.

## <a name="additional-resources"></a>További források

Még nem rugalmas adatbázis funkciók használata az Azure-alkalmazások? Nézze meg a [Dokumentáció térképen](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). A kérdésekre kérjük, keresse meg a szolgáltatás frissítéseiről és az [SQL-adatbázis fórumát](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) , vegyen fel őket a [SQL-adatbázis Visszajelzési fórum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



