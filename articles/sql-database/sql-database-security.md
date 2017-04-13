<properties
   pageTitle="SQL-adatbázis biztonsági – áttekintés"
   description="Azure SQL-adatbázis és az SQL Server biztonságáról szól, beleértve a felhőben közötti különbségeket és az SQL Server helyszínen amikor hitelesítés, engedélyezési, kapcsolat biztonsági, titkosítást és megfelelőség."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Az SQL-adatbázis biztonságossá tétele

## <a name="overview"></a>– Áttekintés

Az alábbiakban ismertetjük az Azure SQL-adatbázissal az alkalmazás a adatok réteg biztonságossá tétele alapjait. Különösen ez a cikk jelenik meg, amely forrásokat is tartalmaz, az adatok védelme a hozzáférés korlátozása, és az [első lépések az SQL-adatbázis oktatóprogram](sql-database-get-started.md)létrehozott tevékenység egy adatbázisban ellenőrzése. Biztonsági funkciók elérhető összes változatok az SQL teljes áttekintéséhez lásd: az [SQL Server adatbázismotort és Azure SQL-adatbázis biztonsági központ](https://msdn.microsoft.com/library/bb510589). További információt a [Biztonság és Azure SQL-adatbázis technikai tanulmány](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF) is érhető el.

## <a name="connection-security"></a>Kapcsolat biztonsága

Kapcsolat biztonsági hogyan korlátozása és a biztonságos kapcsolatok az adatbázishoz tűzfalszabályokat és a kapcsolat titkosítása hivatkozik.

Tűzfalszabályokat a kiszolgáló és az adatbázis által használt IP-címeket, amelyek nem lettek kifejezetten whitelisted a kapcsolatfelvételi elutasításához. Ahhoz, hogy az alkalmazás vagy ügyfél gépi nyilvános IP-cím kísérli meg egy új adatbázisokhoz kapcsolódáskor, először létre kell hoznia egy kiszolgálói szintű tűzfal szabályt az Azure klasszikus Portal, a REST API-t vagy a PowerShell használatával. Legjobb módszer mely szerint a kiszolgáló tűzfalon keresztül engedélyezett IP-címtartományokat korlátozni. További tudnivalókért lásd: [Azure SQL-adatbázis tűzfalon keresztül](https://msdn.microsoft.com/library/ee621782).

Összes Azure SQL-adatbázis-kapcsolatot titkosítás (SSL/TLS) minden alkalommal közben adatok "hálózaton átvitt", mind pedig az adatbázisból. A kapcsolati karakterláncot az alkalmazást, meg kell adnia a kapcsolat titkosítása paramétereket és *nem* megbízhatónak az (ez történik meg, ha másolja a vágólapra a kapcsolati karakterláncot az Azure klasszikus portál ki) kiszolgálói tanúsítvány, egyéb esetben a kapcsolat nem ellenőrzi a kiszolgáló azonosítása és téve a "Férfi középső" támadások lesz. ADO.NET-illesztőprogram, például ezeket a kapcsolati karakterlánc paramétereket vannak **titkosítása = True** és **TrustServerCertificate = False**. További tudnivalókért lásd: [Azure SQL-adatbázis kapcsolat titkosítása és a tanúsítvány érvényességi](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Hitelesítés

Hitelesítés hogyan, a személyazonosság az adatbázishoz való csatlakozáskor hivatkozik. SQL-adatbázis kétféle hitelesítési támogatja:

 - **SQL-hitelesítés**, amely egy felhasználónevet és jelszót használ
 - **Azure Active Directory-hitelesítés**, ami az Azure Active Directory által felügyelt identitások használ, és a felügyelt és integrált tartományok támogatott

Az adatbázis létrehozásakor a logikai kiszolgáló meghatározott "kiszolgáló felügyeleti" bejelentkezési felhasználónév és jelszó. A hitelesítő adatokat használ, akkor hitelesítheti magát bármilyen adatbázist a kiszolgálón, mint az adatbázis tulajdonosa, vagy a "dbo." Azure Active Directory-hitelesítés használni kívánt, ha a "Azure Active Directory felügyeleti," Azure Active Directory-felhasználók és csoportok felügyelete engedett nevű egy másik kiszolgáló felügyeleti kell létrehoznia. A rendszergazda is elvégezheti, hogy egy normál kiszolgáló felügyeleti is összes művelet. [Csatlakozás SQL adatbázis által használatával Azure Active Directory-hitelesítés](sql-database-aad-authentication.md) ismertetését létrehozása az Azure Active Directory-hitelesítés engedélyezése Azure Active Directory rendszergazda a következő témakörben talál.

Legjobb módszer a alkalmazás hitelesítést végezni – ezzel a módszerrel korlátozza az engedélyeket az alkalmazás és a kártékony tevékenység kockázatok csökkentése abban az esetben, ha az alkalmazás kódja téve egy SQL-utasítások beszúrását szándékú a kell használni egy másik fiókba. A javasolt megközelítés, ha egy [tárolt adatbázis-felhasználó](https://msdn.microsoft.com/library/ff929188), amely lehetővé teszi, hogy az alkalmazást közvetlenül egy adatbázist az hitelesítést végezni. A tárolókban található adatbázis felhasználó által használt SQL-alapú hitelesítés hajtja végre a következő, a kiszolgáló felügyeleti jelentkezzen be a felhasználó adatbázishoz csatlakoztatva az SQL-T parancs hozhat létre:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Ha létrehozott az Azure Active Directory-rendszergazda, a tárolókban található adatbázis felhasználó hajtja végre a következő az SQL-T parancs az Azure Active Directory-rendszergazdaként a felhasználó-adatbázishoz csatlakoztatva az Azure Active Directory-hitelesítés használó hozhat létre:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

Bármelyik lehetőséget választja az alkalmazás kapcsolati karakterlánc kell adja meg a kiszolgáló felügyeleti bejelentkezés, az adatbázishoz való csatlakozáshoz helyett a felhasználói hitelesítő adatokat.

SQL-adatbázishoz hitelesítése kapcsolatos további tudnivalókért lásd: a [adatbázisok kezelése és bejelentkezések Azure SQL-adatbázisban](sql-database-manage-logins.md).


## <a name="authorization"></a>Engedély
Engedély utal, mire az Azure SQL-adatbázis belül, és ez szabályozza a felhasználói fiók szerepköreit és engedélyeit. Legjobb módszer akkor érdemes engedélyezése a felhasználóknak a legalacsonyabb szükséges jogosultságokkal. Azure SQL-adatbázis megkönnyíti ezt az SQL-T a szerepkör kezelése:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

A kiszolgáló felügyeleti fiókja csatlakozik a tagja db_owner, amelynek hitelesítésszolgáltató valami az adatbázisból. Ehhez a fiókhoz, az üzembe helyezése a séma frissítések és más adatkezelési műveletek menteni. Korlátozott engedélyű "ApplicationUser"-fiókom segítségével kapcsolatba az adatbázis az alkalmazásból a legalacsonyabb jogosultság szükség szerint az alkalmazás.

További korlátozhatja a felhasználó mire alkalmas az Azure SQL-adatbázis módja van:

* [Adatbázis szerepkörök](https://msdn.microsoft.com/library/ms189121) db_datareader és db_datawriter nem nagyobb teljesítményű alkalmazás felhasználói fiókokat vagy kisebb hatékony kimutatásokat létrehozására használható.
* Részletes [engedélyek](https://msdn.microsoft.com/library/ms191291) vezérelhet milyen műveleteket végezheti el a egyes oszlopok, táblák, nézetek, eljárások és az adatbázis más objektumaihoz.
* [Megszemélyesítési](https://msdn.microsoft.com/library/vstudio/bb669087) és [modul aláírási](https://msdn.microsoft.com/library/bb669102) biztonságosan magasabb szintű engedélyek ideiglenes használható.
* [Sor szintű biztonsági](https://msdn.microsoft.com/library/dn765131) is használható a sorokat egy felhasználó határ férhet hozzá.
* [Adatok maszkolás](sql-database-dynamic-data-masking-get-started.md) használható korlátozza a bizalmas adatokat.
* [A tárolt eljárások](https://msdn.microsoft.com/library/ms190782) használható korlátozhatja a műveleteket, amelyeket az adatbázis meg lehessen hozni.

A portál felhasználói fiók szerepkör-hozzárendelés adatbázisok és logikai kiszolgálók kezelése az Azure klasszikus portálról, vagy a Azure erőforrás-kezelő API szabályozza. Ez a témakör a további tudnivalókért lásd: [Azure portál szerepköralapú hozzáférés-vezérlés](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Titkosítás:

Azure SQL-adatbázis segít, hogy titkosítja a az adatok, ha "a többi" az adatok védelme, vagy tárolt adatbázisfájlok és a biztonsági másolatok, [átlátszó adatokat](http://go.microsoft.com/fwlink/?LinkId=526242)használ. Az adatbázis titkosítása, mint egy adatbázis tulajdonosa csatlakozzon, és hajtsa végre:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Más módszerek az adatok titkos kulcsok titkosítására érdemes megfontolni:

* [Cella szintű titkosítás](https://msdn.microsoft.com/library/ms179331.aspx) adott oszlop vagy akár cella adatainak titkosítására különböző titkosítási billentyűkkel.
* Ha hardveres biztonsági modult vagy a titkosítási kulcs hierarchia központi felügyelet alól van szükség, fontolja meg [Egy Azure virtuális az SQL Server Azure kulcs tárolóból elemre](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Mindig titkosított](https://msdn.microsoft.com/library/mt163865.aspx) (előzetes verzió) tesz titkosítás átlátszó alkalmazások, és lehetővé teszi az ügyfelek anélkül, hogy a titkosítási kulcs megosztását SQL-adatbázis titkosítása a ügyfélalkalmazásokban belül bizalmas adatokat.

## <a name="auditing"></a>A naplózás

Naplózás és nyomon követése az adatbázis-események segíthet a szabályozási megfelelőségről karbantartása és gyanús tevékenység azonosítása. SQL-adatbázis naplózás lehetővé teszi, hogy az adatbázis-naplózandó rekord események bejelentkezés Azure tárterület-fiókját. SQL-adatbázis Képletvizsgálat is egyesíti a a Microsoft Power BI használatának megkönnyítését részletezési jelentéseket és elemzéseket. További tudnivalókért olvassa el a [– első lépések az SQL-adatbázis Képletvizsgálat](sql-database-auditing-get-started.md)című témakört.

SQL-adatbázis veszélyt észlelési tartalmaz egy további biztonsági naplózás fölött réteget. Lehetővé teszi, mert a biztonsági figyelmeztetések anomalous tevékenységéről megjelenésükkor veszélyek válaszolni. További tudnivalókért olvassa el a [SQL-adatbázis veszélyt észlelési – első lépések](sql-database-threat-detection-get-started.md)című témakört.  

## <a name="compliance"></a>Megfelelőség

Mellett a fenti és az alkalmazás különböző biztonsági megfelelőségi követelményeknek, Azure SQL-adatbázis is megfelelnek segítő szolgáltatásai vesz részt eseményeket normál és megfelelőségi szabványokat számos ellen hitelesített. További tudnivalókért lásd: a [Microsoft Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/)hol találhatók az [SQL-adatbázis megfelelőségi tanúsítványok](https://azure.microsoft.com/support/trust-center/services/)legfrissebb listája.
