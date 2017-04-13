<properties 
    pageTitle="Python webes Linux Django az alkalmazás |} Microsoft Azure" 
    description="Megtudhatja, hogyan szeretné üzemeltetni a Azure virtuális Linux gép használatával Django-alapú webes alkalmazáshoz." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Egy Linux virtuális Django Helló, világ webalkalmazás

> [AZURE.SELECTOR]
- [A Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [A Mac vagy Linux rendszerhez](virtual-machines-linux-python-django-web-app.md)

<br>

Ebben az oktatóanyagban ismerteti, hogyan szeretné üzemeltetni a Django-alapú webhelyén, a Microsoft Azure virtuális Linux gép használatával. Ebben az oktatóanyagban feltételezi, hogy nincs semmilyen előzetes használatában Azure. Ez az útmutató befejeztével lehetősége van egy Django-alapú alkalmazás mentése és futtatása a felhőben.

Megtanulhatja, hogy miként:

* A telepítő host Django az Azure virtuális gépen. Ebből az oktatóanyagból megtudhatja, hogyan ehhez a **Linux**, miközben azonos is lehet elvégezni a Windows Server virtuális Azure-ban is. 
* Linux Django új alkalmazás létrehozása

Ebben az oktatóanyagban követve Helló, világ egyszerű webalkalmazás rendszer generál. Az alkalmazás fog tárolni az Azure virtuális gépen.

Képernyőkép a kész alkalmazást nem éri el:

![A helló világ lap Megjelenítés Azure a böngészőablakban](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Létrehozása és konfigurálása az Azure virtuális gépen host Django

1. Kövesse az utasításokat a megadott [Itt](virtual-machines-linux-quick-create-portal.md) hozhat létre az Azure virtuális gép a *Ubuntu kiszolgáló 14.04 LTS* eloszlás.  Ha azt szeretné, válassza a jelszó-hitelesítést SSH nyilvános kulcs helyett.

1. Lehetővé teszi az útmutatást követve 80-as port bejövő HTTP-forgalmat a hálózati biztonsági csoport szerkesztése [az alábbi](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Alapértelmezés szerint új virtuális számítógépe nincs egy teljes tartománynevét (FQDN).  Létrehozhat egyet a képernyőn megjelenő utasításokat követve [Itt](virtual-machines-linux-portal-create-fqdn.md).  Ez a lépés nem kötelező, ebben az oktatóanyagban befejezéséhez.

## <a id="setup"> </a>a fejlesztői környezet beállítása

**Megjegyzés:** Ha Python telepítenie kell, vagy az ügyfél-tárak használni, című a [Python telepítési útmutatóját](../python-how-to-install.md).

A Ubuntu Linux virtuális már megtalálható előtelepítve Python 2.7, de nincsenek Apache vagy django telepítve.  Kövesse ezeket a lépéseket követve csatlakoztassa a virtuális, és Apache és django telepítése.

1.  Nyissa meg a **terminált** új ablakban.
    
1.  Írja be a következő parancsot a Azure virtuális csatlakozni.  Ha teljesen minősített tartománynév hozta létre, lehet csatlakozni a nyilvános IP-cím, a virtuális gép az Azure klasszikus portálon összefoglaló jelenik meg.

        $ ssh yourusername@yourVmUrl

1.  Írja be az alábbi parancsok django telepítése:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Írja be az való telepítéséhez Apache mod-wsgi a következő parancsot:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Egy új Django-alkalmazás létrehozása

1.  Nyissa meg a **terminált** ablak használta az előző szakasz ssh be a virtuális.
    
1.  Írja be az alábbi parancsok Django új projekt létrehozása:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    A **django-admin.py** parancsfájl hoz létre egy alapszerkezetét Django-alapú webhelyek:
    -   **helloworld/Manage.PY** segít szolgáltatója indítása és leállítása Django-alapú aktuális webhelyének üzemeltetését a
    -   **helloworld/helloworld/Settings.PY** Django beállításokat, az alkalmazás tartalmazza.
    -   **helloworld/helloworld/URLs.PY** egyes URL-cím és a nézet közötti hozzárendelés kódot tartalmazza.

1.  **Views.py** a **/var/www/helloworld/helloworld** címtárban nevű új fájl létrehozása. Ez a nézet, amely a "Helló, világ" lap-alapú tartalmaz. A szerkesztő indítása, és adja meg az alábbiakat:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Most már cserélje ki a **urls.py** fájl tartalmát a következő:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Apache beállítása

1.  Hozzon létre egy Apache virtuális host konfigurációs fájl **/etc/apache2/sites-available/helloworld.conf**. Tartalmát meg a következő, és cserélje le *yourVmName* tényleges neve áll a gépet használ (például *pyubuntu*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Engedélyezze a webhelyhez és a következő parancsot:

        $ sudo a2ensite helloworld

1.  Indítsa újra a Apache, az alábbi parancsot:

        $ sudo service apache2 reload

1.  Végezetül betöltése a lap a böngészőben:

    ![A helló világ lap Megjelenítés Azure a böngészőablakban](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Az Azure virtuális gép leállítása

Ha végzett a oktatóprogram, a Leállítás és/vagy a eltávolítása az újonnan létrehozott Azure virtuális gépen más oktatóanyagok erőforrásokat ingyenes, és nem a Azure használati költségek hozni.
