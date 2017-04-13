<properties
    pageTitle="A frissítés SQL-adatbázis V12 megtervezése |} Microsoft Azure"
    description="Előkészületet és korlátozások verzióra V12 az Azure SQL-adatbázis frissítése részt ismerteti."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Megtervezése és a Felkészülés a SQL-adatbázis V12 frissítése


Ez a témakör ismerteti a tervezési és el kell végeznie az Azure SQL-adatbázisok frissítésének V11 verzióról V12 előkészületet.


Egy új [Azure portál](https://portal.azure.com/) támogatásához V12 a frissítés érhető el.


Az alábbi táblázat további súgótémakörök V12 sorolja fel.


| Cím és a hivatkozás | Tartalom leírása |
| :--- | :--- |
| [SQL-adatbázis V12 újdonságai](sql-database-v12-whats-new.md) | Ismerteti, hogyan V12 teljes eltérés áll fenn, a Microsoft SQL Server Azure SQL-adatbázis közelebbi életre részleteit. |
| [Az SQL-adatbázis V12 adatbázis létrehozása](sql-database-get-started.md) | Ismerteti, hogyan hozhat létre egy új Azure SQL-adatbázis verziójú V12. Azt ismerteti, hogy egy üres adatbázis túl különböző beállításokat. |


## <a name="plan-ahead"></a>A következő megtervezése


A következő részek tanulási és műveleteket el egy felé V12 az Azure SQL-adatbázis frissítése előtt meg kell oldania döntéshez mutatják be.

### <a name="version-clarification"></a>Verzió pontosítása


Ehhez a dokumentumhoz a frissítés a Microsoft Azure SQL-adatbázis V11 verzióról V12 vonatkozik. További hivatalos verziószámát közel vannak a következő két érték jelentése a Transact-SQL-utasítást szerint **kiválasztása @@version; ** :


- 12.0.2000.8 *(vagy újabb verziójával, bit V12)*
- 11.0.9228.18 *(V11)*
 - V11 lett is nevezik SAWA v2.

### <a name="service-tier-planning"></a>Réteg tervezési szolgáltatás


Azure SQL-adatbázis V12 kezdve támogatni fogja csak a szolgáltatási rétegek egyszerű, normál és Premium nevű. A webes és üzleti szolgáltatási réteg V12 a nem támogatott. Ezért ha az Azure SQL-adatbázishoz, amely jelenleg az interneten vagy vállalati szolgáltatási réteg frissítése, el kell döntenie az új szolgáltatási réteg kell.


A Basic, normál és Premium szolgáltatási rétegek kapcsolatos részletes tudnivalókért lásd:

- [SQL-adatbázis szolgáltatási rétegek](sql-database-service-tiers.md)
- [Új szolgáltatási rétegek SQL-adatbázis webes/üzleti adatbázisok frissítése](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Tekintse át a Geo-replikációs konfiguráció


Ha az Azure SQL-adatbázis Geo replikációs van konfigurálva, érdemes a dokumentum aktuális konfigurációját, és a leállítása Geo replikációs, mielőtt megkezdené a frissítési előkészítése műveleteket. Frissítés befejezése után újra kell konfigurálni az adatbázis Geo-replikáció.


A stratégia, hogy a forrás érintetlenül, és tesztelje a egy másolatot az adatbázisról.


## <a name="preparation-actions"></a>Műveletek előkészítése


Miután befejezte a tervezés, elvégezheti az utolsó frissítés fázis: Felkészülés a következő részek művelet lépéseit.


Részletesen ismerteti az utolsó frissítés fázis érhetők el a súgótémaköröket, ez a súgótémakör tetején vannak csatolva.


### <a name="service-tier-actions"></a>Szolgáltatás réteg műveletek


A réteg árak webes és üzleti szolgáltatás V12 a nem támogatott.


Ha a V11 Azure SQL-adatbázis webes vagy vállalati adatbázis, a frissítési folyamat kínál az adatbázis váltani szeretne egy támogatott réteg. A frissítés az egy réteg, amely megfelel az adatbázis terhelést előzményeihez javasolja. Bármely támogatott réteg szívesen is választhat.


Váltás az V11 adatbázis a webes és üzleti réteg csatlakozik, a frissítés megkezdése előtt csökkentheti a frissítés során szükséges lépéseket. Ehhez az új [Azure-portálon](https://portal.azure.com/).


Ha biztos abban, hogy melyik szolgáltatási réteg szeretne váltani, a normál réteg S2 szintjét valószínűleg egy kijelölve kezdeti beállítás. Bármelyik kisebb rétegben lesz a webes és üzleti réteg-nál kevesebb erőforrások volt.


### <a name="suspend-geo-replication-during-upgrade"></a>A replikáció Geo függeszteni a frissítés során


A frissítés V12 az adatbázis aktív Geo replikációs esetén nem futtathatók. Ha be szeretné fejezni a replikáció Geo az adatbázis először kell állítani.


A frissítés befejeződése után is beállíthatja az adatbázisról, hogy újra Geo-replikáció.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Nyissa meg az Azure virtuális Ügyfélcsatlakozás-portok


Ha az ügyfélprogram SQL-adatbázis V12 csatlakozik, az ügyfél-Azure virtuális gépen (virtuális) futása közben, meg kell nyitnia az alábbi port tartományokat a virtuális meg:

- 11000-11999
- 14000-14999


Kattintson [ide](sql-database-develop-direct-route-ports-adonet-v12.md) a portokat SQL-adatbázis V12 kapcsolatban további tájékoztatást. Az SQL-adatbázis V12 teljesítménnyel kapcsolatos fejlesztések a portokat szükséges.


##<a id="limitations"></a>Korlátozások alatt és V12 történő frissítés után


### <a name="portals-for-v12"></a>A V12 portálokhoz


Az Azure három portálokról, és minden legyen különböző tulajdonságainak SQL-adatbázis V12 kapcsolatban.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Azure portál új és előzetes állapot továbbra is jelzi. A portál még nem igazán a teljes általános elérhetőségű kiadás. A portál:
 - Kezelheti a V12-kiszolgálót és az adatbázist.
 - Frissíthet az V11 adatbázis V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Előfordulhat, hogy végül szüntetni a Azure klasszikus portál. A portál:
 - Kezelheti a V12-kiszolgálót és az adatbázist.
 - *Nem* a V11 V12 adatbázis frissítése lehet.


- (http://*yourservername*. database.windows.net)<br/>Az SQL Azure adatbázis klasszikus portálon:
 - Is*nem* V12 kiszolgálók kezelése.


Javasoljuk, hogy csatlakozzon a Visual Studio 2015 (VS2015) az Azure SQL-adatbázisait. VS2015 feladatokat, például az alábbiak használhatók:


- A Transact-SQL-utasítást futtatásához.
- Séma tervezéséhez.
- Egy adatbázis online vagy offline fejlesztése.


Vagy inkább, ingyenesen letöltheti [Visual Studio közösségi 2015](https://www.visualstudio.com/vs/community/), amely VS2015 teljes szolgáltatáskészletet nyújtó változata.


A régebbi Azure klasszikus portálon, az adatbázis lapján, **Nyissa meg a Visual Studióban** VS2015 elindítani a kapcsolatot az Azure SQL-adatbázis számítógépen kattinthat.


Másik lehetőség, az is használhatja az SQL Server Management Studio (SSMS) 2014-es [CU6](http://support.microsoft.com/kb/3031047/) az Azure SQL-adatbázishoz való csatlakozáshoz. További részleteket a blogbejegyzésben vannak:<br/>Az [ügyfél szerszámok Azure SQL-adatbázis frissítéseit](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Korlátozás *során* rendszerre való frissítés V12


A V11 adatbázis használható formában megmaradnak az adatokhoz való hozzáférés V12 a frissítés során. Még több korlátozások érvényesek is célszerű figyelembe venni.


| Korlátozás | Leírás |
| :--- | :--- |
| A frissítés időtartam | Az időtartam, a frissítés attól függ, méretét, edition és a kiszolgálón adatbázisok száma. A frissítési folyamat kiszolgálók napra, különösen a kiszolgálókat, amelynek az adatbázisok futtatható órán keresztül:<br/><br/>*50 GB-nál nagyobb vagy<br/> * A szolgáltatás nem prémium réteg:<br/><br/>Az új adatbázisok a frissítés során a kiszolgálón létrehozásának is frissítési időtartamának növelésével. |
| Nincs Geo replikáció | A replikáció GEO V12 kiszolgálón van, amely jelenleg V11 frissítésének részt nem támogatott. |
| Az utolsó frissítés V12 fázisában röviden adatbázisom | A frissítési folyamat során az adatbázisok V11 kiszolgálója tartozó is elérhetők. A kapcsolat a kiszolgálóra, és az adatbázisok viszont röviden nem érhető el, a végleges fázisban, amikor a Váltás a fölé megkezdi a V11 készen áll a V12.<br/><br/>A Váltás a időszakban terjedhet 40 másodperc 5 percig tart. A legtöbb kiszolgálók esetében váltás fölé várhatóan 90 másodpercek befejezéséhez. Váltás az idő, amely sok adatbázisok, de amikor az adatbázisok nehéz írási munkaterhelésekből kiszolgálók megnöveli. |


### <a name="limitation-after-upgrade-to-v12"></a>Korlátozás *után* V12 frissítése


| Korlátozás | Leírás |
| :--- | :--- |
| Nem állítható vissza V11 | Után egy frissítési helyi az eredmény nem visszaállítva vagy visszavont. |
| Webhely vagy a vállalati szint | Ha a frissítés elindul, a kiszolgáló az új V12 adatbázis már nem ismeri fel, vagy fogadja el a webhely vagy a vállalati szolgáltatási réteg. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Exportálása és importálása *után* frissítés V12


Exportálhatja, vagy V12 adatbázis importálása az [Azure Portal](https://portal.azure.com/)segítségével. Vagy lehet importálása és exportálása a következő eszközök egyikével:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015
- Adatok szintű alkalmazás keretrendszer (DacFx)


Jó helyen jár az eszközök használatához először van vagy annak érdekében, hogy az új V12 szolgáltatásokat támogatja a legújabb frissítések telepítése:


- [Összesítő frissítés 6 az SQL Server Management Studio 2014-es](http://support2.microsoft.com/kb/3031047)
- [SQL Server-adatbázisból a Visual Studio 2013 szerszámok február 2015 frissítése](https://msdn.microsoft.com/data/hh297027)
- [A Skype 2015 február adatok szintű alkalmazás keretrendszer (DacFx) Azure SQL-adatbázis V12](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Az előző eszköz hivatkozások frissítve, vagy azt követően 2015 március 2. Azt javasoljuk, hogy ezek közül az eszközök újabb frissítésekkel használható.


#### <a name="automated-export"></a>Az automatikus exportálása


Automatikus exportálás továbbra is elérhető előnézete.  Nincs ügyfél eszköz frissítések szükségesek automatikus exportálása használata esetén.  Ha úgy dönt, hogy az eredményül kapott fájl készíthet, és importálja-e helyszíni-kiszolgálóra, a fent felsorolt szerszámok frissítések sikeres importálásához van szükség.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>Vissza a törölt V11 adatbázis V12

A következő esetben ebből a cikkből megtudhatja, hogy a törölt V11 Azure SQL-adatbázis alakzatot egy V12 Azure SQL-adatbázis kiszolgálója visszaállíthatók.

1. Tegyük fel, hogy V11 Azure SQL-adatbázis-kiszolgálóval rendelkezik. <br/> A kiszolgáló egy adatbázist, a feleslegessé vált webes és üzleti szolgáltatási réteg van.
2. Az adatbázis törlése
3. A kiszolgáló V12 frissítenie.
4. Ezután meg az adatbázis visszaállítása a kiszolgálóra. <br/> Az adatbázis frissítése V12, a normál szolgáltatási réteg S0 szintjén ily módon történik.
5. Az adatbázis bármelyik támogatott szolgáltatás rétegben válthat, ha S0 nem a beállításait.


### <a name="powershell-cmdlets"></a>PowerShell-parancsmagok


Indítsa el, leállítása vagy figyelheti a frissítést az Azure SQL-adatbázis V12 V11 vagy bármely más előtti-V12 verzió PowerShell-parancsmagok érhetők el.

- [SQL-adatbázis V12 frissítés PowerShell használatával](sql-database-upgrade-server-powershell.md)

Hivatkozás dokumentációjában az alábbi PowerShell-parancsmagok a következő cikkekben talál:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Kezdés-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Leállítás-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


A Stop parancsmag azt jelenti, hogy visszavonása, nem mutasson az egérrel. Nincs mód a frissítést, indítsa újra az elejétől eltérő önéletrajz nem. A Stop parancsmag törli a köteggel és elengedi az összes szükséges források.


## <a name="failure-resolution"></a>Megoldás a hiba


Ha a frissítés páratlan valamiért nem sikerül, az V11 adatbázis marad, aktív, és a szokásos érhető el.


## <a name="related-links"></a>Kapcsolódó hivatkozások


- Microsoft Azure [előzetes verzió szolgáltatások](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
