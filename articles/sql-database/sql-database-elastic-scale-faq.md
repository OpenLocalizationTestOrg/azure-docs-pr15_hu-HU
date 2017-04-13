<properties 
    pageTitle="Rugalmas Azure SQL-skála gyakran ismételt kérdések |} Microsoft Azure" 
    description="Gyakori kérdések az SQL Azure-adatbázis rugalmas skála." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Rugalmas Adatbáziseszközök – gyakori kérdések 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>/ Shard és nincs sharding kulcs egyetlen-bérlő esetén hogyan végezze el tudom feltöltése a sharding billentyűt a séma információk?

A séma információ objektum egyesítés esetek felosztása csak szolgál. Ha egy alkalmazás eleve egyetlen bérlői, nem kell megadni a felosztott egyesítése eszköz, és ezért nincs szükség a séma információ objektum kitöltéséhez.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>E már kiépítve adatbázis és Shard térkép vezető már rendelkezik, hogyan tegye lehet rögzíteni az új adatbázis egy shard?

Olvassa el **[az alkalmazás használatáról a rugalmas adatbázis ügyfél tár egy shard hozzáadása](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Végezze el rugalmas Adatbáziseszközök mennyibe?

A rugalmas adatbázis ügyfél-tár használata nem merülnek fel minden olyan költségeket. Költségek csak az Azure SQL-adatbázisait shards és a Shard térkép kezelő használt, valamint a webes/dolgozó szerepkörök, a felosztott egyesítése eszköz kiépítése a felmerülés.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Miért saját hitelesítő adatok nem működik egy shard egy másik kiszolgálóról való hozzáadásakor?
Ne használja a hitelesítő adatok formájában "felhasználó ID=username@servername”, Ehelyett egyszerűen használata" felhasználói azonosító = felhasználónév ".  Ügyelni kell, hogy a "felhasználónév" jelentkezzen be a shard engedéllyel rendelkezik.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Kell Shard térkép kezelőjének létrehozása és feltöltése a shards, minden alkalommal indul el az alkalmazásokat?

Nem – a Shard térkép Manager (például a **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) létrehozása egy olyan egyszeri művelet.  Az alkalmazás kell használni a hívás **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** indítási időben.  Nem kell egy alkalmazás tartomány csak egy ilyen hívás.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Rugalmas Adatbáziseszközök használatával kapcsolatban, hogy hogyan lehet beszerezni választ kaphat? 

Kérjük, keresse meg a nekünk az [Azure SQL-adatbázis-fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Ha egy adatbázis-kapcsolatot sharding kulccsal, e is továbbra is az azonos shard más sharding billentyűi lekérdezés adatait.  A működés Ez?

A skála rugalmas API-k a sharding billentyű a megfelelő adatbázis kapcsolat ad azonban nem nyújt sharding kulcs szűrés.  Szükség esetén **Hol** záradékok felvétele a lekérdezés hatókörének korlátozása a megadott sharding billentyűt.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Van lehetőség a különböző Azure-adatbázis edition minden shard a shard beállítása?

Igen, a shard az egyes adatbázist, és így egy shard lehet egy Premium edition miközben egy másik lehet, hogy egy Standard edition. További a shard kiadását méretezheti felfelé vagy lefelé a shard élettartama során többször.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>A felosztott egyesítés eszköz rendelkezés jelent (vagy törlése) adatbázis felosztása és egyesítése művelet során? 

nem. Műveletek **felosztása** a céladatbázisban léteznie kell, a megfelelő séma használata és a Shard térkép kezelő regisztrálva.  Az **Egyesítés** művelet a shard törlése a shard térkép kezelő és törölje az adatbázist.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 