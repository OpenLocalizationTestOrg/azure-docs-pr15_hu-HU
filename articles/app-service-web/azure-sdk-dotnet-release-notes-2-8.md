
<properties 
   pageTitle="A .NET rendszerhez 2,8 Azure SDK – kibocsátási megjegyzések" 
   description="A .NET rendszerhez 2,8 Azure SDK – kibocsátási megjegyzések" 
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
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>A .NET rendszerhez 2,8, 2.8.1 és 2.8.2 Azure SDK

##<a name="overview"></a>– Áttekintés
 
Ez a cikk a kibocsátási megjegyzések (tartalmazza az ismert hibák és bekövetkezett változásokkal) a .NET 2,8, 2.8.1 és 2.8.2 elengedi az Azure SDK tartalmazza. 

Új szolgáltatások és frissítések ebben a kiadásban teljes listáját az [Azure SDK 2,8 Visual Studio 2013 és a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) bejelentése témakörben talál. 

##  <a name="azure-sdk-for-net-28"></a>A .NET rendszerhez 2,8 Azure SDK

### <a name="download-azure-sdk-for-net-28"></a>A .NET rendszerhez 2,8 Azure SDK letöltése

[A .NET rendszerhez 2,8 Visual Studio 2015 Azure SDK](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK .NET 2,8 for Visual Studio 2013-ban](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>Támogatja a .NET 4.5.2. 

####<a name="known-issues"></a>Ismert problémák

Azure .NET SDK 2,8 .NET 4.5.2 létrehozását teszi lehetővé Felhőszolgáltatásba csomagok. Azonban 4.5.2 .NET keretrendszer nem telepíthető az alapértelmezett, engedje fel az addig, amíg a január 2016-OS Vendég Vendég OS képek. Előtt, hogy 4.5.2 .NET keretrendszer egy külön vendég operációs rendszer verziója – November 2015-02 érhető el. Lásd az [Azure Vendég OS kiadásairól és SDK kompatibilitási mátrix](../cloud-services/cloud-services-guestos-update-matrix.md) lap nyomon követéséhez, ha a kép fejlesztésének fog.  Miután a November 2015-02 kép megjelent megadhatja, hogy a kép használata a Felhőbeli szolgáltatástól konfigurációs fájl (.cscfg) fájl frissítésével. A szolgáltatás beállításai fájl állítsa be a osVersion attribútum ServiceConfiguration elem a karakterlánc "Postafiók-VENDÉG-OS-4.26_201511-02". Ha úgy dönt, hogy jelentkezzen be az ezen a képen használja, akkor már nem jelenik meg a vendégként való bekapcsolódáshoz OS az automatikus frissítések. Az automatikus frissítéseket kapni a osVersion kell beállítani "*" és a .NET 4.5.2 csak akkor január 2016-ban az automatikus frissítések keresztül érhető el.

###<a name="azure-data-factory"></a>Azure Data Factory

####<a name="known-issues"></a>Ismert problémák 

Mintaadatok érintő **Gyári Adatsablon** projekt létrehozása, során azure power parancsprogram sikertelen lehet, ha 0.9.8 azure power rendszerhéj verziója telepítve a számítógépen található.

Sikeresen létrehozásához az ilyen típusú projektet, telepítenie kell a [azure power rendszerhéj verziója 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Azure erőforrás-kezelő eszközök 

####<a name="breaking-changes"></a>Módosítások megszakítása

Az Azure erőforráscsoport project által biztosított PowerShell-parancsprogramot frissült ezen verziójában készült új Azure PowerShell-parancsmagok 1.0 verziójú.  Ez a parancsfájl nem működnek a a Visual Studio, amikor a SDK 2,8 előtti verzióját használja.  

Parancsfájlok a SDK korábbi verzióiban készült projekteket nem működik a Visual Studio a 2,8 SDK használata esetén.  Az összes parancsfájl továbbra is a megfelelő verziója az Azure PowerShell-parancsmagok használata kívül Visual Studio.  

A 2,8 SDK Azure PowerShell-parancsmagok 1.0 verziója szükséges.  A SDK más verzióit Azure PowerShell-parancsmagok 0.9.8 verziója szükséges.  További információt talál a [blogban](http://go.microsoft.com/fwlink/?LinkID=623011) .

###<a name="web-tools-extensions"></a>Webes bővítmények eszközök

####<a name="known-issues"></a>Ismert problémák

A következő ismert problémák a következő kiadásában címzettje lesz.

- Alkalmazás szolgáltatás kapcsolatos felhő Server Explorer kézmozdulat nem üzemi környezetben (például Kína Azure vagy Azure Papírhalom ügyfelek) nem működnek. Az alábbi probléma által sújtott területen az előfizetői esetében a közzététel profil letöltése az Azure portálról lehetővé teszi, közzétételi lehetőséget. Elkövetkező kiadásokban Azure Kína és a Papírhalom ügyfelek kézmozdulatok, például "Csatolása Debugger" és "A folyamatos átvitelű naplók megtekintése" javítsa ki. 
- Ügyfelek hibák során alkalmazás szolgáltatás létrehozási, amikor az alkalmazás az összefüggéseket példány, amely üzembe nem kelet-Amerikai Egyesült Államok területen látható. Forgatókönyvekben-alkalmazás szolgáltatás létrehozása a portálon, és a közzététel profil letöltése lehetővé teszi közzétételi esetek. 

###<a name="azure-hdinsight-tools"></a>Azure hdinsight szolgáltatáshoz eszközök

####<a name="new-updates"></a>Új frissítések keresése

- A struktúra lekérdezés végrehajtása a fürthöz szinte nincs terhelést HiveServer2 keresztül, és látható a feladat naplózza valós idejű.
- Az új struktúra feladat végrehajtását nézet meg alaposabban meg is be a feladatok mélyreható részletesebb tájékoztatás található, és lehetséges problémák azonosításához.

Tudnivalókért lásd [Azure SDK 2,8 Visual Studio 2013 és a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>A .NET rendszerhez 2.8.1 Azure SDK

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Ismert problémák a Visual Studio 2013 és a Visual Studio 2015
 
1. Indítóval kezdeményezett WebJob közzé, a helyek fog megjelenítése és a hiba, és nem ütemezett beállítása, de küldi a WebJob az Azure. Azon ügyfelek, akik egy ütemezett feladat le kell használhatja az Azure-portálon állíthatja be a WebJob az ütemezésben. 
2. Python ügyfelek debugger problémák léphetnek fel. Csoport meg egy fix közbeni ehhez, de ügyfelek érinti, kérjük, lehetővé teszik a Microsoft tudja, hogy a fórumainkon, illetve a bejelentése blog, és engedje fel a megjegyzéseket a Megjegyzések szakasz. 
3. Ügyfelek az egyes régiókra (például a Dél-indiai) alkalmazás szolgáltatás áttelepítési hibákkal fog tapasztalni. Ez a portálon megfelel, és azon ügyfelek, akik a probléma az Azure portal segítségével e geo régiók közzététele hozzáférést kérni. Miután azokat a régiókat hozzáférést kérni az Azure portál kiépítési kell működniük. 

##<a name="azure-sdk-for-net-282"></a>A .NET rendszerhez 2.8.2 Azure SDK

A 2.8.2 telepítését követően eszközök ügyfelek a következő hibát tapasztalhatnak.         

- Ha a Windows 10-es használ, és nem telepítette az Internet Explorer, a "Az Internet Explorer nem található" hiba történt jelenhetnek meg.
A probléma megoldásához telepítse az Internet Explorer, a Windows-összetevők és törlése párbeszédpanelen.

Tekintse át az Ez a probléma, ha a Küldés a mosoly a funkcióval jelenteni.

További tudnivalókért lásd [a](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) tartalmakat.
##<a name="other-updates"></a>Frissítések

Frissítések a [közlemény Azure SDK 2,8 bejelentése](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)című témakör tartalmaz.

##<a name="also-see"></a>Lásd még:

[Azure SDK 2,8 bejelentése bejegyzésben](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Támogatási és elavulása adatait az Azure SDK .NET és az API-hoz](https://msdn.microsoft.com/library/azure/dn479282.aspx)

