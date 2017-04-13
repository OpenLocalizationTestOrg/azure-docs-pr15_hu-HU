
<properties 
   pageTitle="A .NET 2.7 és .NET 2.7.1 Azure SDK – kibocsátási megjegyzések" 
   description="A .NET 2.7 és .NET 2.7.1 Azure SDK – kibocsátási megjegyzések" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>A .NET 2.7 és .NET 2.7.1 Azure SDK – kibocsátási megjegyzések

##<a name="overview"></a>– Áttekintés

A dokumentum tartalmaz a kibocsátási megjegyzések az Azure SDK .NET 2.7 kiadását. 

A dokumentum .NET 2.7.1 engedje fel az is tartalmaz az kibocsátási megjegyzések az Azure SDK csomagjában talál.

Azure SDK 2.7 csak támogatja a Visual Studio 2015 és a Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) az utolsó támogatott SDK for Visual Studio 2012.

Ebben a kiadásban kapcsolatos részletes tudnivalókért lásd: [Azure SDK 2.7 bejelentése közzé](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) és [Azure SDK 2.7.1 bejelentése kérdések feltevése](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>A .NET rendszerhez 2.7 Azure SDK

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Jelentkezzen be a Visual Studio 2015 javítása

Visual Studio 2015 Azure SDK 2.7 Visual Studio 2015 az új identitás-felügyeleti funkciókat támogatja.  Ide tartoznak a támogatás elérése Azure keresztül szerepkör alapú hozzáférés-vezérlés, felhőalapú megoldás szolgáltatók, DreamSpark és más fiók és az előfizetés típusú fiókokhoz.

Azure SDK 2.7 részét képező bejelentkezési javítása a Visual Studio Skype 2015 vállalati csak érhetők el. Visual Studio 2013 támogatása Azure SDK 2.7.1 is tartalmazni fogja.


###<a name="mobile-sdk"></a>Mobil SDK

**Mobile-alkalmazások** sablonok legújabb [NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) és a beállítási folyamat megfelelően frissül.

###<a name="service-bus"></a>Szolgáltatás Bus 

Általános hibajavítások és a fejlesztések. A részletek, a frissítésekkel és szolgáltatásokkal olvassa el a legújabb [Szolgáltatás Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/)vonatkozó kibocsátási megjegyzések.

###<a name="hdinsight-tools"></a>HDInsight eszközök 

Ebben a kiadásban az alábbi frissítéseket végzett. A frissítések vannak az előzetes verzióban. További tudnivalókért lásd: a [blogban](http://go.microsoft.com/fwlink/?LinkId=619108).

- A feladatok Tez a struktúra grafikonok struktúra
- Teljes struktúra DML IntelliSense-támogatás
- Malac sablon támogatás
- Az Azure-szolgáltatások vihar sablonok

####<a name="breaking-changes"></a>Módosítások megszakítása

- Ha ezzel a verziójával az eszközök frissíteni kell a régi **vihar** projekt. További tudnivalókért lásd: a [blogban](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express már nem támogatott. További tudnivalókért lásd: a [blogban](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Azure alkalmazás szolgáltatás eszközök

Ebben a kiadásban az alábbi frissítéseket Web eszközök Extensions történtek. További információt talál a [blogban](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) . 

- Felvett DreamSpark fiókok támogatása
- Támogatja az új Azure erőforrás Management API-hoz készült Azure eszközök teljes módosítása
- [Felhőalapú](#cloud_explorer) Explorer hozzáadott Azure-alkalmazás szolgáltatás támogatása

####<a name="known-issues"></a>Ismert problémák

A helyek csomópontot, a kiszolgáló Intézőben a Web App telepítési tárolóhely csomópontok nem jelennek meg, és Web App telepítési tárolóhely gyermekcsomópontjainak nem töltődnek be a felhőben Explorer. Ez a probléma megoldódott, és a következő SDK verzióval készült. 


###<a name="cloud_explorer"></a>Visual Studio 2015 felhő Explorer

Azure SDK 2.7 felhő Explorer tartalmaz Visual Studio 2015, amely lehetővé teszi, hogy az Azure erőforrások megtekintése, tulajdonságaik vizsgálata és műveleteket kulcs Fejlesztőeszközök a Visual Studio belül. 

Felhőalapú explorer támogatja az alábbiakat:

- Az Azure erőforrások erőforráscsoport és erőforrástípus nézete 
- Az erőforrások nevét (erőforrástípus nézetben érhető el) szerinti keresése
- Előfizetések és szerepkör alapján Access vezérlő (RBAC) alkalmazott rendelkező erőforrások támogatása 
- A kijelölt erőforrások integrált művelet panel, amely mutatja az adott Fejlesztőeszközök fókuszáló műveletek. Példa: távoli debugger csatolni, a létrehozott virtuális gépeken futó az erőforrás-kezelő Objektumhalomban megjelenítheti diagnosztikai adatok virtuális gépeken futó stb.
- Integrált tulajdonságai panel, amely megjeleníti a fejlesztők/vizsgálat során gyakran szükség Fejlesztőeszközök fókuszáló tulajdonságait 
- Gyors váltás-fiók használata esetén az erőforrások számbavétele (paranccsal beállítások eszköztár) 
- Előfizetések (paranccsal beállítások eszköztár) erőforrások számbavétele használandó szűrése 
- Az erőforrások és erőforrás-csoportok kezelése az Azure-portálra mélyreható hivatkozások 
 
 
###<a name="azure-resource-manager-tools"></a>Azure erőforrás-kezelő eszközök 

Az Azure erőforrás-kezelő eszközök a frissített szerepkör alapján Access vezérlő (RBAC) és az új előfizetés típusú.  Ezek a változások részét képező van használata új tárterület-fiók esetén klasszikus tárolás mellett eltérések tárolni a telepítés során.  

Ha használni szeretné a SDK egy korábbi verzióról Azure erőforráscsoport projektté a SDK 2.7, egy új telepítési parancsfájlt üzembe fiókkal új tároló helyett klasszikus tároló van szükség.  Mielőtt módosítják a projekthez adja meg az új parancsfájlt kéri.  A régi parancsfájl átnevezésével, és meg kell manuálisan bármilyen módosítást az új parancsfájl.
 
 
###<a name="storage-explorer-tools"></a>Tárterület Explorer Eszközök 

- Támogatás a Hozzáfűzés BLOB megtekintéséhez. További információ a [blogbejegyzésben](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Támogatás a prémium tároló kiszolgáló Intéző-fiókokat megtekintéséhez. Azok az egyetlen támogatott típus prémium verzió tároló használata Server Explorer csak jelennek meg lap BLOB prémium verzió tároló használata.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure adatok gyári Tools for Visual Studio 

**Azure gyári Adateszközök** for Visual Studio bemutatása Az alábbiakban az engedélyezett funkciók. Lásd: a [blogban](http://go.microsoft.com/fwlink/?LinkId=617530) további információt.

- **A sablon alapján szerzői**: használata cased jelölje ki sablonok, adatok mozgását sablonok vagy egy végpont adatok integrálása megoldást üzembe helyez, és gyorsan az adatok gyári rutinszerzést kezdheti adatfeldolgozás sablonok alapján. 
- **Integráció a megoldást Explorer létrehozása és adatok gyár szervezetek üzembe helyezése**: létrehozása és folyamatok és a kapcsolódó szervezetek, mint a Visual Studio projektek telepítését. 
- **Integráció a Diagram megtekintése közben szerzői vizuális kapcsolati**: vizuálisan Szerző folyamatok és a Diagram nézetben támogatással adatkészleteket. 
- **Integráció a kiszolgáló Intézővel böngészhető és a kapcsolati már telepített személyekkel**: a kiszolgáló-tallózó Tallózás már telepítette az adatok gyárak és a megfelelő személyek kihasználhatja. A projekt importálása telepített adatok gyár vagy bármely személy (folyamat, csatolt szolgáltatás, adatkészleteket). 
- **JSON érvényesítése a séma és multimédiás intellisense szerkesztése**: hatékony konfigurálása és a multimédiás intellisense és -sémafájlok érvényesítése az adatok gyári szervezetek JSON-dokumentumok szerkesztése 
- **Több elem környezet közzététel**: közzététele szerzője, hogy a fejlesztők, vizsgálatot vagy termékkatalógus környezet folyamatok környezetben külön config-fájlok létrehozása.
- **Malac struktúra, és a .net-alapú adatfeldolgozás támogatás**: malac és struktúra parancsfájlok adatok gyár projektben támogatása. C# projekt hivatkozó .net kezelésére szolgáló támogatása tevékenységet.

##<a name="azure-sdk-for-net-271"></a>A .NET rendszerhez 2.7.1 Azure SDK

A következő szakaszban a .NET 2.7.1 engedje fel az Azure SDK a bevezetett frissítések tartalmazza.
###<a name="hdinsight-tools"></a>HDInsight eszközök 

Részletesebb tájékoztatás a HDInsight eszközök frissítések [blogban](http://go.microsoft.com/fwlink/?LinkId=623831)talál.

- Feladat operátor megtekintése (egy új szolgáltatást) struktúra

    Segít megérteni a struktúra lekérdezés még jobb, a struktúra operátor nézet funkció hozzá lett adva. Csúcs belül az operátorokat megtekintéséhez kattintson duplán a az csúcspont a feladat grafikon. Egy adott operátor, mutasson rá, hogy operátor további részletek megjelenítéséhez.
- Struktúra hiba jel (egy új szolgáltatást)

    Ahhoz, hogy a nyelvhelyességi hibák megjeleníteni kívánt azonnali, a struktúra hiba jelölő funkció hozzá lett adva. Hibaüzenetek jelennek meg azok fokozott és azonnali részletes nyelvtani hibák most már láthatja is, (csak ebben a kiadásban is voltak elküldéséhez a fürthöz struktúra parancsfájl, és várja meg a hibaüzenet adatainak első kis időt).  
- Vihar topológia Graph (új szolgáltatás)

    Megjelenítése nagyon fontos, ha meg szeretné jeleníteni a esetén a várt módon működik-e a topológia. Ebben a kiadásban jelöltük vihar grafikonok megjelenítést. Az a topológia is képi megjelenítés a fontos mérési módja miatt (például egy szín mutatja egy bizonyos vég időjárási "elfoglalt"-e). Dupla választhatja a rögzített/Spout további részletes adatainak megjelenítéséhez.

- Az Azure-portálon (hibajavítás) létrehozott HDInsight fürt támogatása

    Visual Studio most használatával megtekintheti és minden a HDInsight fürt mindegy, hol a fürt létrehozott feladatok nyújt.

- További támogatási IntelliSense & gyorsabb struktúra metaadatok betöltése (javulást)

    További felhasználóbarát javaslatok hozzáadásával azt kell javítani az IntelliSense. Például tábla alias is most is a javasolt az IntelliSense így egyszerűbben írhat a lekérdezést. Azt is, van továbbfejlesztett a struktúra metaadatok betöltése, így csak néhány másodpercig összes adatbázisok, táblázatok és oszlopok a struktúra metastore listáját.

Részletesebb tájékoztatás a HDInsight eszközök frissítések [blogban](http://go.microsoft.com/fwlink/?LinkId=623831)talál.

###<a name="improvements-in-visual-studio-2013"></a>Visual Studio 2013 újdonságai

- Azure SDK 2.7.1 lehetővé teszi, hogy a Visual Studio 2013 Azure fiókok és szerepkör alapú hozzáférés-vezérlés, felhőalapú megoldás szolgáltatók és Dreamspark keresztül előfizetések eléréséhez.
- Az Azure SDK 2.7.1 az új felhő eszköz Intézőt érhető el is Visual Studio 2013.

###<a name="known-issues"></a>Ismert problémák

Az Azure SDK 2.6 vagy telepítésének 2.7.1 a Visual Studio közösségi 2013 a egy nem - angol nyelvű operációs rendszer, hogy a Visual Studio angol és nem angol nyelvű erőforrások nem egyeznek a előfordulhat, hogy figyelmeztetés jelenik meg. Előfordulhat, hogy ez a figyelmeztetés biztonságosan nyitva. Ha a számítógép nem tartalmazott Visual Studio közösségi 2013 az egy korábbi telepítése és telepíti a SDK csomagjában talál a egy nem - angol nyelvű operációs rendszer csak bekövetkezik. A figyelmeztetés jelenik meg a nyelvi csomagot az RTM erőforrások vonatkozik a Visual Studio, de mielőtt frissítés 4 vonatkozik. A figyelmeztetés elrejtésével lehetővé teszi a nyelvi csomagot, továbbra is, és fejezze be a alkalmazása a frissítés 4-es verziójában a nyelvi csomag tartalmát.

LightSwitch projektek sem compatibile a jelenlegi kiadásába. Ez a probléma a következő SDK kiadásában megszűnt lesz.

##<a name="also-see"></a>Lásd még:
[Azure SDK 2.7.1 bejelentése bejegyzésben](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 bejelentése bejegyzésben](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Támogatási és elavulása adatait az Azure SDK .NET és az API-hoz](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
