<properties 
   pageTitle="A .NET rendszerhez 2.5.1 Azure SDK – kibocsátási megjegyzések" 
   description="A .NET rendszerhez 2.5.1 Azure SDK – kibocsátási megjegyzések" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>A .NET rendszerhez 2.5.1 Azure SDK – kibocsátási megjegyzések

A dokumentum kibocsátási megjegyzések az .NET 2.5.1 engedje fel az Azure SDK tartalmazza. 

##<a name="azure-sdk-for-net-251-release-notes"></a>A .NET rendszerhez 2.5.1 Azure SDK kibocsátási megjegyzések

Új szolgáltatások és a .NET 2.5.1 Azure SDK frissítések a következők:

- **Webes eszközök**bővítményhez kapcsolódó új features\scenarios. 

    - Azure webhelyek Azure alkalmazás szolgáltatás lett átnevezni. További információ, [Azure alkalmazás szolgáltatás és a meglévő Azure-szolgáltatásokat](app-service-changes-existing-services.md).
    - Azure API-alkalmazások (előzetes verzió) támogatási hozzá lett adva, hogy a felhasználók is ASP.NET projekteket API-alkalmazások közzététele, és írja be a hozzáadása > Azure API alkalmazás ügyfél kézmozdulat C# projektek alapján a szerkezettel telepített API-kód létrehozása. 
    - A webhelyek csomópontra Server Explorer elavult az Azure alkalmazás szolgáltatás csomópontra, amely tartalmazza a támogatási csoport alapú Azure API-alkalmazások, a Mobile-alkalmazások és a Web Apps alkalmazások csoportosítása erőforrás helyett.
    - Azure Mobile-alkalmazások (előzetes verzió) támogatási hozzá lett adva, hogy a vevők hozhat létre új projekteket Mobile-alkalmazások hozzáadása Mobile-alkalmazások vezérlők, tegye közzé a projektek és alkalmazások távolról hibáinak.
    - Hozzáadása > Azure API alkalmazás ügyfél kézmozdulat támogatja helyi Swagger JSON fájlok, így webes API fejlesztők Swashbuckle például külső NuGets segítségével Swagger készítése vagy szerkeszthetik azt manuálisan. Ezzel a módszerrel ügyfél fejlesztők segítségével a kód generációs funkciók felhasználása bármely Swagger végpontot, C# projektekben. 
    - Web App és az API-alkalmazás közzétételi párbeszédpanelek van a továbbfejlesztett támogatja a csoportosítás erőforrás Azure portál fogalmának, majd a kijelölés/kibocsátása Azure erőforrás-csoportokat és az alkalmazás szolgáltatás csomagok jelennek meg az új Web App és az API-alkalmazás kiépítési párbeszédpanel. 
    - Azure API alkalmazás Server Explorer csomópontok, amely a API-alkalmazások mély az Azure-portálra, valamint a folyamatos átvitelű napló és távoli hibakeresés hasonló hivatkozásra.

    Ismert problémák és Azure SDK .NET 2.5.1 [Ebben](app-service-release-notes.md#known_issues_2_5_1) a szakaszban az alábbi aktuális korlátozások.


- Visual Studio **HDInsight eszközök** kapcsolódó új features\scenarios engedélyezve vannak a jelen kiadásban. 
    - Helyi ellenőrzési struktúra parancsfájlok. Kattintson az eszköztár megjelenítéséhez, ha valamilyen hiba van-e a parancsprogram az érvényesítés parancsfájl gombra. 
    - Továbbfejlesztett hibakeresési struktúra feladatok. A Visual Studióban fonal naplók elérésével a struktúra feladatok most hibakeresési. Ha az alkalmazás teljesítménnyel kapcsolatos problémákat, vizsgálat alatt fonal naplók hasznos információt nyújt.
    - (Nyilvános előzetes verzió) Kulcsszó automatikus-befejezése és az IntelliSense támogatása a struktúra. Segíti a szerző elkészíti a struktúra parancsfájlok, HDInsight Tools for Visual Studio hozzáadott kulcsszó automatikus-befejezése és az IntelliSense támogatása a struktúra.
    - Vihar támogatás. HDInsight Tools for Visual Studio segítségével a C# topológiák/Spouts/csapszegek vihar kidolgozása most. A fejlett topológia vihar fürtre elküldése, majd megtekintheti a topológia/rögzített/spout állapotát. Rendszer naplókról és a felhasználói naplók kapcsolatos hibák elhárítása a topológiák/csapszegek/Spouts vihar használható. A HDInsight vihar a meglévő JAVA eszközök is használhatja.
    
    További tudnivalókért olvassa el a [HDInsight Hadoop Tools for Visual Studio használatának első lépései](hdinsight-hadoop-visual-studio-tools-get-started.md)című témakört.



##<a id="known_issues_2_5_1"></a>A .NET rendszerhez 2.5.1 Azure SDK ismert hibákat és korlátozásokat

- Azure API-alkalmazások esetén Mobile-alkalmazások telepítési célpontját látható. Web Apps alkalmazások kell Mobile-alkalmazások csak célhelyét addig, amíg a későbbi kiadásokban. 
- Azure API-alkalmazás kiépítési success eredményez, de időnként nem sikerült frissíteni az Azure alkalmazás szolgáltatási tevékenység ablakban végrehajtását. Megoldás: a problémának az Azure-portálon új Azure API alkalmazás állapotának ellenőrzése. 
- Fájl > Új projekt > API-alkalmazás >, mert nincs default/index.html a HTTP-hibát eredményez F5 élmény. Megoldás: manuálisan tallózással keresse meg a /api/values URL-címe. 
- Időnként Server Explorer ikonok jelennek meg sík. Ez a probléma VIEWBEN újraindítását szünteti meg. 
- Ha egy kivétel során (például túllépve kvótahibák vagy ismétlődő Azure API alkalmazás átjáró neve) kiépítési Web Appot vagy az API-alkalmazás, a hibák nyers JSON szöveget jeleníti meg. 
- Szakaszos-projektlétrehozással problémák alkalmazás háttérismeretek be van jelölve, a project létrehozási idejének.
- Előfordul a létrehozott Azure API alkalmazás ügyfél kód névtér hiányzik, manuálisan tartalmaz (vagy Visual Studio figyelmeztetések automatikusan importált) a kód összeállításához szükségük. 
- Web Apps alkalmazások mobilalkalmazás projektek kell közzétenni, de válasszon ki egy webhelyen létrehozott Mobile alkalmazásként az Azure-portálon óta mobilalkalmazás projektek adatbázisban szükséges. 
- A kezdőlap Mobile-appokról használja a kifejezés "mobilszolgáltatás" helyett "mobilalkalmazások" 
- Mobilalkalmazás projektlétrehozással igénybe vehet egy perc hozhat létre. 
- API-alkalmazás során hiba (bizonyos esetekben) kiépítési származik vissza az Azure API jelezve, hogy az engedélyek nem állítható be megfelelően, miközben az API-alkalmazás megfelelően kiépítve, és használatra kész. Manuálisan beállíthatja, hogy az engedélyek az Azure-portálon.
- Alkalmazás háttérismeretek API alkalmazássablonok és a Mobile alkalmazássablonok nem támogatott.
- API-alkalmazás projektek Felhőszolgáltatásba projektek együtt nem használható.
- A C# API-alkalmazás projektsablonok csak érhetők el.
- C# csak támogatott API-alkalmazás felhasználás "Hozzáadása Azure API alkalmazás ügyfél" helyi menüjének használatával.

 
