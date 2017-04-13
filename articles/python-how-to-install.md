<properties
    pageTitle="Python és a SDK - Azure telepítése"
    description="Megtudhatja, hogyan telepítheti az Azure használata Python és a SDK csomagjában talál."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Python és a SDK telepítése

Python egyszerű beállítás a Windows és a [Bash a Windows](https://msdn.microsoft.com/commandline/wsl/about), Mac és Linux előre telepített megtalálható. Ez az útmutató végigvezeti telepítési és a Felkészülés a számítógép használatát az Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Mit tartalmaz a Python Azure SDK?

Az Azure SDK Python összetevői fejlesztése, telepítése, és az Azure Python alkalmazások kezelése teszi lehetővé. Az Azure SDK Python kifejezetten, az alábbiakat tartalmazza:

* **Adatkezelési tárak**. Ezek osztálytár Azure erőforrások, például a tárterület-fiók esetén virtuális gépeken futó kezelése felületet biztosít.

* **Futási idejű tárak**. Ezek osztálytár felületet nyújt Azure funkciókat, például a tárhely és a szolgáltatás bus elérése.

## <a name="which-python-and-which-version-to-use"></a>Melyik verziót szeretné használni, és milyen Python

Több változatok Python tolmácsoknak áll rendelkezésre, – többek között:

* CPython – a szokásos és a leggyakrabban használt Python értelmező
* PyPy - CPython gyors, kompatibilis alternatív végrehajtása
* IronPython - .net/CLR futó Python értelmező
* Jython – a Java virtuális gépen futó Python értelmező

**CPython** v2.7 vagy v3.3 + PyPy 5.4.0 tesztelése és az Python Azure SDK támogatott.

## <a name="where-to-get-python"></a>Hol találja Python?

Többféle módon CPython első:

* Közvetlenül a [www.python.org][]
* Az egy megbízható distro például [www.continuum.io][], [www.enthought.com][] vagy [www.activestate.com][]
* A forrásból származó összeállítása!

Csak az adott szükség van, javasoljuk, hogy az első két lehetőség közül választhat.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK telepítése a Windows, Linux és MacOS (csak a tárak ügyfél)

Ha már telepítve Python, a mezőpont ügyfél-tárakat a köteget telepítheti a meglévő Python 2.7 vagy a Python 3.3 + környezetben is használhatja. A csomagok letöltése a [Python csomag Index][] (PyPI).

Előfordulhat, hogy a rendszergazdai jogok:

- Linux és MacOS, használja a `sudo` parancs: `sudo pip install azure-mgmt-compute`.
- Windows: rendszergazdaként nyissa meg a PowerShell-/ parancssor

Telepíthetők külön-külön minden egyes Azure szolgáltatás tár:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Előzetes csomagok segítségével kell telepíteni a `--pre` jelző:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Egy egyetlen sort a Azure tárak halmazának is telepítheti a `azure` metaadat-csomagot. A metaadat-csomag nem az összes csomagok közzététele stabil, még, óta a `azure` metaadat-csomag előzetes van. Azonban core csomagok, a kód minőség/teljesség perspektívák tekinthető "stabil" megoldása
- azt hivatalos neve ilyenként szinkronizálja más nyelvek minél korábban beállítást. Azt nem tervezi, továbbá fő módosításokat addig.

Mivel ez éppen a előzetes verzióval, akkor használja a `--pre` jelző:

```console
   $ pip install --pre azure
```
   
akár közvetlenül

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Az első további csomagok

A [Python csomag Index][] (PyPI) Python tárak gazdag a kijelölt tartalmaz.  Ha azt választotta, telepítheti egy Distro, már fognak rendelkezésére állni leggyakrabban a technikai számítások webes fejlesztése különböző felhasználási területei érdekes eltolással.


## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio

[Python Tools for Visual Studio][] (PTVS), a szabad/OSS bővítmény a Microsoft, és kapcsolja be a teljes értékű Python IDE:

![How-to-telepítés-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

PTVS használata nem kötelező, de cserébe Python és a webes projekt/megoldás támogatása, hibakeresése során, adatgyűjtés, interaktív ablak sablon szerkesztése és az Intellisense, ajánljuk.

PTVS is egyszerűen [Felhőszolgáltatások][] és a [webhelyek][]üzembe támogatása a Microsoft Azure-telepítéshez használni.

PTVS működik-e a meglévő Visual Studio 2013 vagy a Skype 2015 telepítés.  Dokumentáció, letöltések és beszélgetések című témakörben talál [Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Linux és MacOS Azure felhasználási területei

Linux vagy MacOS, fő Azure forgatókönyvet támogatja:

1. Az ügyfél-tárak használatával Python igénybe Azure-szolgáltatások

2. Az alkalmazás egy Linux virtuális fut

3. Fejlesztése, és mely számjegy használatával Azure webhelyek közzétételéhez

Az első eset kihasználhatja az Azure PaaS például [blob-tárolóhoz][], [várólista-tároló][], [táblatároló][] stb Pythonic csomagolást keresztül az Azure REST API-khoz gazdag webalkalmazások Szerző teszi lehetővé. Ezek a munkák azonos a Windows, Mac és Linux rendszerhez.  A helyi fejlesztési gépi vagy egy Linux virtuális Azure futó ügyfél tárakban is használhatja.

A virtuális eset egyszerűen indítsa el a Linux virtuális (Ubuntu, CentOS, Suse) lehetőség, és Futtatás/kezelése, amely tetszik.  Példaként [IPython][] replikációs/jegyzetfüzet futtassa a Windows és Mac vagy Linux számítógépre, és a böngésző mutasson egy Linux vagy a Windows több feldolgozású virtuális IPython motor futó Azure. Lásd: további információt az [Azure IPython jegyzetfüzet][] oktatóprogram.

Kapcsolatos tudnivalókat az egy Linux virtuális beállításához tanulmányozza az [létrehozása egy virtuális gépen futó Linux][] oktatóprogram.

Mely számjegy-környezetben használja, Python webalkalmazás fejlesztése, és közzéteheti minden olyan operációs rendszer egy Azure webhelyre.  A tárházba való Azure megnyomása automatikusan hoz létre virtuális környezetet, és mezőpont a szükséges csomagok telepítése.

További tájékoztatást nyújt, és a közzétételi webhelyek Azure lásd: az oktatóanyag [Webhelyek létrehozásáról a Django][], a [Webhelyek létrehozásáról a palack][]és a [Webhelyek létrehozása a lombikot][]. Bármely WSGI-kompatibilis keretrendszer használatával kapcsolatos általános tudnivalókért lásd [Azure-webhely konfigurálása Python][].


## <a name="additional-software-and-resources"></a>Külön szoftver és erőforrások:

* [Azure SDK Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Hivatalos Azure minták Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Alakíthatnak ki olyan Analytics Python ki.][]
* [Enthought Python ki.][]
* [ActiveState Python ki.][]
* [SciPy - tudományos Python tárak együttese][]
* [NumPy - Python numerics tárról][]
* [Django projekt - elért webes keretrendszer/CMS][]
* [IPython - Python egy speciális replikációs/jegyzetfüzet][]
* [Azure IPython jegyzetfüzet][]
* [Python Tools for Visual Studio a GitHub][]
* [Python Developer Center](/develop/python/)

[Alakíthatnak ki olyan Analytics Python ki.]: http://continuum.io
[Enthought Python ki.]: http://www.enthought.com
[ActiveState Python ki.]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.ActiveState.com]: http://www.activestate.com
[SciPy - tudományos Python tárak együttese]: http://www.scipy.org
[NumPy - Python numerics tárról]: http://www.numpy.org
[Django projekt - elért webes keretrendszer/CMS]: http://www.djangoproject.com
[IPython - Python egy speciális replikációs/jegyzetfüzet]: http://ipython.org
[IPython]: http://ipython.org
[Azure IPython jegyzetfüzet]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Webhelyek]: web-sites-python-ptvs-django-mysql.md
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio a GitHub]: https://github.com/microsoft/ptvs
[Python csomag Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Linux operációs rendszert futtató virtuális gép létrehozása]: virtual-machines-linux-quick-create-cli.md
[Webhelyek létrehozása Django]: web-sites-python-create-deploy-django-app.md
[Palack webhelyek létrehozása]: web-sites-python-create-deploy-bottle-app.md
[Webhelyek létrehozása lombikot]: web-sites-python-create-deploy-flask-app.md
[Azure-webhely konfigurálása Python]: web-sites-python-configure.md
[táblatároló]: storage-python-how-to-use-table-storage.md
[a várakozási tárhely]: storage-python-how-to-use-queue-storage.md
[BLOB-tárolóhoz]: storage-python-how-to-use-blob-storage.md
