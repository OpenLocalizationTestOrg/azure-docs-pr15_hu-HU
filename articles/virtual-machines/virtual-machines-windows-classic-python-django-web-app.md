<properties
    pageTitle="Python webes Django az alkalmazás |} Microsoft Azure"
    description="Ebben az oktatóanyagban útmutatást ad meg a Azure egy Django-alapú webhely üzemeltetése klasszikus telepítési modellt használja a Windows Server 2012 R2 adatközponthoz virtuális gép használatával."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>A Windows Server virtuális Django Helló, világ webalkalmazás

> [AZURE.SELECTOR]
- [A Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [A Mac vagy Linux rendszerhez](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Ebben az oktatóanyagban ismerteti, hogyan szeretné üzemeltetni a Django-alapú webhelyén, a Microsoft Azure virtuális gép Windows Server használatával. Ebben az oktatóanyagban feltételezi, hogy nincs semmilyen előzetes használatában Azure. Ebben az oktatóanyagban befejeztével lehetősége van egy Django-alapú alkalmazás mentése és futtatása a felhőben.

Megtanulhatja, hogy miként:

* Állítsa be az Azure virtuális gépen host Django. Ebből az oktatóanyagból megtudhatja, hogyan ehhez a Windows Server, miközben azonos is lehet elvégezni a Linux virtuális Azure-ban is.
* Hozzon létre egy új Django alkalmazást a Windows.

Ebben az oktatóanyagban követve gyűjt a egy egyszerű Helló, világ webalkalmazást. Az alkalmazás fog tárolni az Azure virtuális gépen.

A kész alkalmazást listája Tovább gombra.

![A helló világ lap Megjelenítés Azure a böngészőablakban][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Létrehozása és konfigurálása az Azure virtuális gépen host Django

1. Kövesse az utasításokat a megadott [Itt](virtual-machines-windows-classic-tutorial.md) hozhat létre az Azure virtuális gép a Windows Server 2012 R2 adatközponthoz eloszlás.

1. Kérje meg a Azure irányítsa át a port 80-alapú forgalmat a webről 80-as port a virtuális gépen:
 - Nyissa meg az újonnan létrehozott virtuális gép az Azure klasszikus portálon, majd kattintson a **VÉGPONTOK** fülre.
 - Kattintson a képernyő alján a **hozzáadása** gombra.
    ![végpont hozzáadása](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Nyissa meg a **TCP** protokoll adatait a **80-as nyilvános PORT** , **személyes PORT 80**.
![][port80]
1. Az **IRÁNYÍTÓPULT** lapon kattintson a **Csatlakozás** **Távoli asztali** segítségével távolról jelentkezzen be az újonnan létrehozott Azure virtuális gép elemre.  

**Fontos megjegyzés:** Minden, az alábbi utasításokat feltételezik jelentkezett be a virtuális gép helyesen, és a helyi számítógép zónában, hanem parancsok ott vannak kiállító.

## <a id="setup"> </a>Python, Django, WFastCGI telepítése

**Megjegyzés:** Töltse le az ESC Internet Explorer beállításainak konfigurálása is az Internet Explorer használata érdekében (kezdő/felügyeleti eszközök/Manager/helyi kiszolgáló, kattintson az **Internet Explorer fokozott biztonság beállításait**, kapcsolva).

1. Telepítse a legújabb Python 2.7 vagy 3.4 [python.org][].
1. Telepítse a wfastcgi és django csomagok mezőpont használatával.

    Python 2.7 a következő paranccsal jeleníthető meg.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Python 3.4 a következő paranccsal jeleníthető meg.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>IIS telepítése az FastCGI

1. Telepítse az IIS FastCGI támogatásával.  Ez eltarthat néhány percig, amíg végre.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Egy új Django-alkalmazás létrehozása

1.  A *C:\inetpub\wwwroot*adja meg a következő parancsot Django új projekt létrehozása:

    Python 2.7 a következő paranccsal jeleníthető meg.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Python 3.4 a következő paranccsal jeleníthető meg.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Az eredmény a New-AzureService parancs](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  A **django-rendszergazdai** parancs hoz létre egy alapszerkezetét Django-alapú webhelyek:

  -   **helloworld\manage.PY** segít szolgáltatója indítása és leállítása Django-alapú aktuális webhelyének üzemeltetését a
  -   **helloworld\helloworld\settings.PY** Django beállításokat, az alkalmazás tartalmazza.
  -   **helloworld\helloworld\urls.PY** egyes URL-cím és a nézet közötti hozzárendelés kódot tartalmazza.

1.  **Views.py** a *C:\inetpub\wwwroot\helloworld\helloworld* címtárban nevű új fájl létrehozása. Ez a nézet, amely a "Helló, világ" lap-alapú tartalmaz. A szerkesztő indítása, és adja meg az alábbiakat:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Cserélje le a urls.py fájl tartalmát a következő.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>IIS beállítása

1. A globális következő kezelők szakaszának zárolásának feloldása  Ezzel engedélyezi a python kezelő a Web.config használatát.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Engedélyezze a WFastCGI.  Ezzel hozzáadja az alkalmazás a globális következő a végrehajtható Python értelmező és a wfastcgi.py parancsfájl hivatkozó.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Hozzon létre egy fájlt a *C:\inetpub\wwwroot\helloworld*.  Értékét a `scriptProcessor` attribútum egyeznie kell az előző lépésben kimenetét.  Nézze meg a lapot [wfastcgi][] pypi további be wfastcgi beállítások.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Frissítse az IIS alapértelmezett webhely mutasson a django projekt mappa helyét.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Végezetül töltse be a lap a böngészőben.

![A helló világ lap Megjelenítés Azure a böngészőablakban][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Az Azure virtuális gép leállítása

Amikor elkészült, az Ez az oktatóanyag, állítsa le, illetve távolítsa el az újonnan létrehozott Azure virtuális gép szabadítson fel erőforrásokat a többi oktatóprogramjaihoz és felmerülése Azure használati költségek elkerülésére.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
