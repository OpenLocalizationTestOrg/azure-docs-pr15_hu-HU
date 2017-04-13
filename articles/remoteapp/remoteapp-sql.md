<properties
   pageTitle="Az SQL Azure Azure RemoteApp |} Microsoft Azure"
   description="Megtudhatja, hogy miként használhatja az SQL Azure Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>Az SQL Azure Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Gyakran ügyfelek dönt, hogy a Windows-alkalmazásokat a felhőben Azure RemoteApp azokat is szeretné adataik, például az SQL-kiszolgálók áttelepítése egy teljes felhő telepítéshez a felhőben. Ebben a csoportban adhatja teljes felhőben tárolt megoldás, bármilyen eszközzel, bárhonnan az Azure RemoteApp bármikor is elérhető. Az alábbiakban hivatkozások és hivatkozások útmutatást segítséget nyújt a folyamat együtt.  

## <a name="migrate-your-sql-data"></a>Az SQL-adatok áttelepítése

Indítsa el, áttelepítésére, az [SQL Server-adatbázis Azure SQL-adatbázishoz](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Azure RemoteApp beállítása
A Windows Azure RemoteApp alkalmazások üzemeltetni. Az alábbi van nagyon magas szintű lépésenkénti útmutató:

1.     A [virtuális Azure RemoteApp sablont](remoteapp-imageoptions.md)hozhat létre. 
2.     A szükséges alkalmazás telepítése a virtuális.
3.     Konfigurálja az alkalmazást, így az SQL-adatbázis csatlakozik, és győződjön meg arról, hogy működik.
4.     Sysprep és leállítási a virtuális. Rögzítés a képként Azure való használatra. **Megjegyzés:** Meg kell gondoskodni arról, hogy az alkalmazás képes megőrzése a KCS2 kapcsolati információ sysprep folyamatát. Ha az alkalmazás nem tudja, amely megőrzi a KCS2 kapcsolati adatokat, érdemes lehet ellenőrizni, hogy hogyan azt adhatja meg a kapcsolati karakterláncot az alkalmazás a szállító folytasson.
5.     A saját kép importálása az Azure RemoteApp tár, jelölje ki a megfelelő földrajzi hely, amely a SQL Azure-példányban. 
6.     Az azonos adatközpontban, mint az SQL Azure környezetben, a fenti sablonnal RemoteApp gyűjtemény telepítése és az alkalmazás Közzététel gombra. Az azonos adatközpontban, mint az SQL Azure Azure RemoteApp üzembe helyezése telepítési segítséget nyújt a biztosítása érdekében a leggyorsabb kapcsolat sebessége és csökkentése a késés. 

## <a name="app-and-sql-configuration-considerations"></a>Alkalmazás és az SQL konfigurációs megfontolások:
Létezik néhány tudnivaló Azure SQL RemoteApp használatakor:

[Az Azure SQL-adatbázis tűzfal konfigurálásának](../sql-database/sql-database-firewall-configure.md)ismertetése. Egy olyan az cikk államokból "kezdetben a Azure SQL-adatbázis kiszolgálója minden hozzáférést blokkolja a tűzfalon keresztül. Annak érdekében, hogy az Azure SQL-adatbázis kiszolgálója használatának megkezdéséhez kell a klasszikus portált, és adjon meg egy vagy több kiszolgálói szintű tűzfalszabályokat, amelyek lehetővé teszik az Azure SQL-adatbázis kiszolgálója a hozzáférést. A tűzfal szabályok használatával adja meg, mely IP-címtartományok az internetről áll rendelkezésre, és engedélyezi vagy tiltja Azure alkalmazások megpróbálhatja az Azure SQL-adatbázis-kiszolgálóhoz való csatlakozáshoz."

Is, ha a számítógép csatlakozik az adatbázis-kiszolgáló az internetről, a tűzfal ellenőrzi, a kérést, szemben a skype_for_businesshoz kiszolgálói szintű származó IP-címe (ha szükséges), és adatbázis szintű tűzfalszabályokat. "A kérelem IP-címe valamelyik kiszolgálói szintű tűzfalszabályokat a megadott tartományba esik, ha a kapcsolat nyújtott Azure SQL-adatbázis kiszolgálója." Ennélfogva eljuttatjuk kérését IP-tartományok használatát, nem csupán egyes forrás IP-címek.

Lépésenkénti kövesse a [Útmutató: az Azure-portálon SQL-adatbázis tűzfal beállításainak](../sql-database/sql-database-configure-firewall-settings.md) az IP-tartomány megadásához. Az SQL-tűzfalszabályokat beállításakor adja meg az IP-tartomány, a megadott alhálózat az Azure RemoteApp gyűjteményét. Ennek lehetővé kell az SQL-adatbázis kapcsolódni, akkor is, ha az azok fog dinamikusan-hozzárendelt IP-címek ARA kiszolgálókhoz.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha egy ügyfélalkalmazás használata, a felület fájlkiszolgálón kapcsolódó egy SQL Azure RemoteApp az adatbázis-adott Azure is vagy a helyszíni lassú ennek oka lehet, néhány miért.  

- Az Azure az eszközről a hálózati késés fel hangosítva. Ugrás a legjobb és leggyorsabb hálózati kapcsolat, akkor a legjobb teljesítmény elérése érdekében. Általános eszközként [azurespeed.com](http://azurespeed.com/) használatával tesztelje a eszközök késése Azure adatközpont.  
- Az Azure RemoteApp üzemeltetett ügyfél app leterhelt. Egy másik, például prémium számlázási számlázási csomagra kijelölése hatékonyabbá tehetők a teljesítmény. Egy másik trükk az alkalmazás használata van más erőforrások: az aktív munkamenet közben hajtsa végre a ctrl + alt + end billentyűkombinációt, amelynek a biztonsági Társítás képernyő indítsa el, válassza a Feladatkezelő, és tekintse meg az erőforrás-kihasználtság az alkalmazás.
- Az SQL server leterhelt, vagy nincs optimalizálva. Hajtsa végre az SQL-útmutatást az alábbiakkal. 

