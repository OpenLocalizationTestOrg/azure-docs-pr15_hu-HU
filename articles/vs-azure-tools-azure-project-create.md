<properties
   pageTitle="Azure projekt létrehozása a Visual Studio |} Microsoft Azure"
   description="A Visual Studio Azure projekt létrehozása"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>A Visual Studio Azure projekt létrehozása

Az Azure Tools for Visual Studio biztosítanak egy sablont, amely lehetővé teszi az Azure felhőalapú szolgáltatást hozhat létre. Az eszközök segítségével konfigurálása, hibakeresési és a felhőbeli szolgáltatástól telepítse az Azure.

Az Azure felhőalapú szolgáltatás megoldás a következő típusú projektek tartalmazza:

- **Azure projekt**

    Az Azure projekt szerepkör projektekhez társítások tartalmaz a megoldást. A szolgáltatás meghatározása és szolgáltatás konfigurációs fájlokat is tartalmaz. A szolgáltatás-definíciós fájl határozza meg az alkalmazást, például hogy milyen szerepkörök szükségesek, a végpontok és a virtuális gép méretét futtatókörnyezet beállításait. A szolgáltatás konfigurációs fájl szerepkörbe hány példánya futnak és az értékeket a szerepkörbe megadott beállítások konfigurálása Ezekkel a beállításokkal kapcsolatos további tudnivalókért lásd: [Útmutató: a szerepkörök konfigurálása az Azure felhőalapú szolgáltatás a Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Szerepkör a project Web**

    Dolgozó szerepkört hátterének feldolgozás hajt végre. Dolgozó szerepkörbe tudnak kommunikálni, tárolása és az egyéb internetalapú szolgáltatások. Dolgozói szerepkörbe beállíthatja, hogy HTTP, HTTPS vagy TCP végpontok tetszőleges számú.

    - **ASP.NET webes szerepkör**egy előtér ASP.NET-alkalmazások készítéséhez
    - **ASP.NET MVC5 webes szerepkör**
    - **ASP.NET MVC4 webes szerepkör**
    - **ASP.NET MVC3 webes szerepkör**
    - **WCF szolgáltatás webes szerepkör**WCF-szolgáltatások készítéséhez
    - **A Silverlight üzleti alkalmazások webes szerepkör** (a Visual Studio 2012 igényel)

- **Gyorsítótár dolgozói szerepkör**

    Szerepkör szerint az alkalmazás egy dedikált gyorsítótárat.

- **A szolgáltatás Bus várólista dolgozó szerepkör**

    A szolgáltatás bus várólista, amely a munkafolyamat üzenet kezelési funkciók kommunikáció tesz lehetővé. További információért megtudhatja, [hogy miként használata szolgáltatás Bus sorban várakozó](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Az Azure felhőalapú szolgáltatás projekteket létrehozni a Visual Studio

1. Indítsa el a Microsoft Visual Studio rendszergazdaként.

1. A menüsor válassza a **fájl**és az **Új** **Projekt**.

1. A **Projekttípusok** ablaktáblában válassza a **felhőben** Visual C# vagy Visual Basic projektből sablon csomópontot.

1. A **sablonok** munkaablakban válassza az **Azure Felhőszolgáltatásba**.

1. Adja meg a projekt hozzanak használni kívánt .NET-keretrendszer melyik verziója.

1. Írja be a nevét és a projekt helyét és nevét a megoldást. Kattintson az **OK** gombra.

1. **Új Azure projekt** párbeszédpanelen válassza ki a hozzáadni kívánt szerepkörök, és válassza a jobbra mutató nyíl gombot el őket felvenni a megoldás. Megadhatja, hogy minél több szerepkörök.

1. Nevezze át a projekthez, **Új Azure projekt** párbeszédpanelen a szerepkör a hover adott szerepkört, és kattintson a jobb oldalán a szerepkör a **átnevezése** ikonra. Is átnevezheti szerepkörbe belül a megoldás után azt.
