<properties
    pageTitle="Kezelése és figyelése a összekötők és az alkalmazás szolgáltatás API-alkalmazások |} Microsoft Azure"
    description="Összekötők és az alkalmazások logika; API-alkalmazások teljesítmény megtekintése microservices architektúra"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Kezelése és a beépített API-alkalmazások és összekötők figyelése

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

A beépített API-at létrehozott. Mi történik most?

Az Azure-minden API-alkalmazás egy külön webhelyet Azure is. Emiatt egyszerűen megtekintheti hány kérések történik-e, és ellenőrizze, hogy az összekötő használják, hogy mennyi adatot. Is biztonsági mentése az API-alkalmazást, értesítések létrehozása, Tinfoil biztonsági engedélyezése, és adja hozzá a felhasználók és szerepkörök.

Ez a témakör ismerteti az egyes kezelheti az API-alkalmazás különböző beállításokat.

Ezek a beépített funkciók megtekintéséhez nyissa meg az API-alkalmazást az [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040). Ha az API-alkalmazást be van kapcsolva a startboard, jelölje be a nyissa meg a Tulajdonságok parancsot. Is válassza a **Tallózás gombra**, jelölje be a **API-alkalmazásokat**, és válassza ki az API-alkalmazás:

![][browse]

## <a name="see-the-properties-you-entered"></a>Lásd: a megadott tulajdonságok

A API-alkalmazás megnyitásakor vannak több funkciók és feladatok érhető el:

![][settings]

képes vagy:

- **Beállítások** adott információkat jelenít meg az API-alkalmazásba, többek között az előfizetés részletei lapon, és megjeleníti a felhasználókat, akiknek hozzáférésük az API-alkalmazást. Is növelheti vagy csökkentheti az API-alkalmazás funkcióval skála; példányainak száma között más szolgáltatásokat.
- A **elindítása** és **leállítása** gombok segítségével szabályozhatja az API-alkalmazást.
- A mögöttes fájlokat az API-alkalmazás által használt termékfrissítéseinek történik, amikor kattintson a **frissítés** megszerezni a legfrissebb verziójával. Például ha javítás vagy a biztonsági frissítés a Microsoft által kiadott, gombra kattintva **frissítse** automatikusan frissíti az API-alkalmazást, amelyet fel szeretne venni ezt a fix.
- Jelölje ki a **Terv** frissítés vagy az adatok használatát az API-alkalmazás alapján visszalépéssel kapcsolatos. Ez a funkció használatával lásd: az adatok használatát.
- Amikor létrehoz egy összekötőre, amelynek a táblázatok, például az SQL-összekötő való csatlakozáshoz táblázatnév tetszés szerint adhat meg. A séma alapján a táblázatot automatikusan létezik és elérhető akkor, ha **A sémák letöltése**gombra kattint. A letöltött séma segítségével majd vagy átalakítás és a térkép létrehozása.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Az összekötő és a megadott API konfigurációs értékek módosítása

Beállítva, vagy a beépített összekötő létrehozott követően módosíthatja a megadott értékeket. Például ha úgy állította be, hogy az SQL-összekötő, és meg szeretné változtatni a SQL Server vagy táblázat neve, végezheti el ezt a az API-alkalmazás lap az összekötő.

Lépések a következők:

1. Nyissa meg az összekötő vagy API-alkalmazást. Ekkor megnyílik az API-alkalmazás lap.
2. **Alapverzió**kattintson a hivatkozás alatti a Host tulajdonságot. A hivatkozás elnevezett valamit, amit például *slackconnector* vagy *microsoftsqlconnector123*számára:

    ![][apiapphost]

3. Az API-alkalmazás Host lap válassza a **Beállítások**elemre. Válassza a beállítások lap **Beállításai**. A beállítások **Alkalmazásban a beállítások**csoportban jelennek meg:

    ![][hostsettings]

4. Kattintson a beállítás módosításához írja be az új értéket, és **Mentse** a módosításokat szeretne.


## <a name="install-the-hybrid-connection-manager---optional"></a>A hibrid kapcsolatkezelő – nem kötelező telepítése

![][hcsetup]

A hibrid kapcsolatkezelő lehetőséget nyújt a helyszíni rendszerre, például SQL Server vagy SAP csatlakozni. A hibrid kapcsolat használja az Azure Service Bus csatlakozás és a biztonsági az Azure erőforrások és a helyszíni erőforrások között.

Lásd: [a hibrid kapcsolatkezelő Azure alkalmazás szolgáltatás használata](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hibrid kapcsolatkezelő csak egy helyszíni erőforrás csatlakozik a tűzfal mögött szükség. Ha nem csatlakozik a helyszíni rendszerre, a hibrid kapcsolatkezelő valószínűleg nem szerepel a összekötő lap.

## <a name="monitor-the-performance"></a>A teljesítmény figyelését
Teljesítménymutatók beépített funkciók, és minden API alkalmazással létrehozott része. Ezek a mértékek az Azure-ban üzemeltetett App API vonatkoznak. Minta mértékek:

![][monitoring]

képes vagy:

- Jelölje ki a **kérések és hibák** különböző teljesítménymutatók, többek között a gyakran ismert HTTP-hibakódok, például 200, 400 vagy 500 HTTP állapot kódok hozzáadása. Lásd: az adott válaszok idejét, látható, hogy hány kérések az API-alkalmazásba, és látható, hogy mennyi adatot szolgál, és hogy mennyi adatot állapotba. A teljesítménymutatók alapján, létrehozhat e-mail értesítések, ha egy mérőszám eléri az a választás.
- **Használatát**, akkor láthatja mennyi **Processzor** az API-alkalmazást használja, tekintse át az aktuális **Használati kvótája** MB és megtekintheti a maximális adatok használatát a költség réteg alapján. **Becsült töltött** segíthetnek annak eldöntésében, hogy az API-alkalmazást futtató potenciális költsége.
- Jelölje ki a **folyamatok** folyamat Intéző megnyitásához. Ezzel jeleníti meg a webhely-példányok és tulajdonságaik, beleértve a szál darab és a memóriában használatát.

Ezen eszközök segítségével megállapítható, ha az alkalmazás szolgáltatás terv méretezett, vagy el kell méretezett lefelé, az üzleti igények alapján. Ezek a szolgáltatások nincs szükség további eszközök beépített a portálra.

## <a name="control-the-security"></a>A biztonsági vezérlő

API-alkalmazások használata a szerepköralapú biztonsági funkciókat. Ezeket a szerepköröket alkalmazhat a teljes Azure környezetét, például az API-alkalmazások és más erőforrások: Azure. A szerepkörei az alábbiak:

Szerepkör | Leírás
--- | ---
Tulajdonos | Az adatkezelési folyamatok teljes hozzáférést, illetve más felhasználók vagy csoportok hozzáférést is.
Közös munka | Az adatkezelési folyamatok teljes hozzáférést. Nem tud hozzáférést adhat más felhasználók vagy csoportok.
Olvasót | Titkos kulcsok kivételével az összes erőforrás tekintheti meg.
Felhasználói hozzáférés-rendszergazda | Megtekintheti az összes erőforrásokat, szerepkörök létrehozása és kezelése, és létrehozása és kezelése a támogatási jegyeket.

Lásd: [a Microsoft Azure portál szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

Egyszerűen vehet fel felhasználókat, és a rájuk adott szerepkörök hozzárendelése az API-alkalmazás. A portál jeleníti meg az access és a kiosztott szerepkörrel rendelkező felhasználók:

![][access]  

- Válassza a **felhasználók** hozzáadása egy felhasználóhoz, rendeljen egy szerepkört, és eltávolít egy felhasználót.
- Jelölje ki a **szerepkörök** , a felhasználók egy adott szerepkört, hogy egy felhasználó szerepkörbe felvenni, és a felhasználó törlése a szerepkör.


## <a name="more-good-stuff"></a>További jól használható elemek
- Jelölje ki az **API-definíciós** a Swagger automatikusan létrehozott fájl az adott API-alkalmazás megnyitásához.
- Jelölje ki a **függőségek** , a API-diákban fájlok megtekintéséhez. Például ha az SAP-összekötő használata esetén, telepítse a a helyszíni hibrid kapcsolatkezelő további fájlokat. Ezeket a függőségeket az API-alkalmazás lap jelennek meg.

>[AZURE.IMPORTANT] Nyissa meg az API-alkalmazás tulajdonságait, és keresse meg a **Essentials**, ha létezik, nyissa meg az új pengéit **Host** és az **átjáró** hivatkozások:
>
> ![][host]
>
>A Tulajdonságok egyediek a webhelyet, amelyen az API-alkalmazást. Azt javasoljuk, hogy nem frissíti a tulajdonságok, és egy beépített API-alkalmazást vagy connector használatakor, ezeket a tulajdonságokat a legtöbb valójában nem-e alkalmazni. Ha a saját API-alkalmazás létrehozása a Visual Studióban, és üzembe Azure-előfizetéséhez, majd használhatja Host és az átjáró rögzítéséhez. <br/><br/>


>[AZURE.NOTE] Első lépésként logika alkalmazások, mielőtt feliratkozna az Azure-fiók [Próbálja logika alkalmazás](https://tryappservice.azure.com/?appservice=logic)megnyitásához. Létrehozhat egy rövid életű starter logika alkalmazást. Nem kötelező hitelkártyát, és nincs nyilatkozatát.

## <a name="read-more"></a>További információ

[A logikai alkalmazások figyelése](app-service-logic-monitor-your-logic-apps.md)<br/>
[Összekötők és az API-alkalmazások listájának App szolgáltatásban](app-service-logic-connectors-list.md)<br/>
[A Microsoft Azure portál szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)<br/>
[A hibrid kapcsolatkezelő Azure alkalmazás szolgáltatás használata](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
