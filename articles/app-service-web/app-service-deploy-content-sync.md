<properties
    pageTitle="Szinkronizálási Azure alkalmazás szolgáltatás felhő mappa tartalmát"
    description="Megtudhatja, hogy miként telepítse az alkalmazást Azure alkalmazás szolgáltatás keresztül tartalom szinkronizálása egy felhőalapú mappából."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Szinkronizálási Azure alkalmazás szolgáltatás felhő mappa tartalmát

Ebből az oktatóanyagból megtudhatja, hogy hogyan [Azure alkalmazás](http://go.microsoft.com/fwlink/?LinkId=529714) szolgáltatás a népszerű felhőbeli tároló szolgáltatások, például a Dropbox és a onedrive-on a tartalmak szinkronizálásának telepítéséhez. 

## <a name="overview"></a>Tartalom szinkronizálása telepítési áttekintése

Az igény szerinti tartalom szinkronizálása telepítési [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki) alkalmazás szolgáltatás integrálva van-e kapcsolva. Az [Azure-portálon](https://portal.azure.com)kijelöl egy mappát a felhőben tárolt, az alkalmazás kódot és a tartalom abban a mappában, és alkalmazás szolgáltatás szinkronizálni a egyetlen kattintással. Tartalom szinkronizálása telepítéséhez és Szerkesztés a Kudu folyamat használja. 
    
## <a name="contentsync"></a>Tartalom szinkronizálása telepítési engedélyezése
Ahhoz, hogy a tartalom szinkronizálása az [Azure-portált](https://portal.azure.com), kövesse az alábbi lépéseket:

1. Az alkalmazás fel az Azure-portált, kattintson a **Beállítások** > **Telepítési forrás**. **Válassza a forrás**gombra, majd válassza a **OneDrive** vagy **Dropbox** telepítéshez forrásaként. 

    ![Tartalom szinkronizálása](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Mögöttes közötti különbségek az API-k, mert a **a OneDrive vállalati verzió** jelenleg nem támogatott. 

2. Ahhoz, hogy egy adott előre definiált kijelölt elérési elérése a OneDrive vagy Dropbox, ahol az összes a alkalmazás szolgáltatás tartalmakat szeretne tárolni alkalmazás szolgáltatás engedélyezési munkafolyamat befejezése.  
    Az alkalmazás szolgáltatás engedélyezési után platform képet ad a hozzon létre egy tartalom mappát a a kijelölt tartalom elérési útját, vagy válassza ki a kijelölt tartalom elérési útját a meglévő tartalom mappa lehetőséget. A kijelölt tartalom elérési utak csoportban a felhőalapú tárolási fiókok használható a szinkronizálási alkalmazás szolgáltatás a következők:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. A kezdeti tartalom szinkronizálása után a tartalom szinkronizálása az Azure portál elérhető igény kezdeményezhető. A **telepítés** lap telepítési előzmények érhető el.

    ![Telepítési előzmények](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
További információt a Dropbox-telepítéshez részére elérhető a [Deploy innen: Dropbox lehetőséget](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


