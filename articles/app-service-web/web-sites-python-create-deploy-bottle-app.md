<properties 
    pageTitle="Python web apps az Azure-ban palack" 
    description="Egy oktatóprogram, amely bemutatja a Python webalkalmazást az Azure alkalmazás szolgáltatás Web Apps alkalmazások futtatásához." 
    services="app-service\web" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-bottle-in-azure"></a>Web Apps alkalmazások létrehozása az Azure-ban palack

Ebből az oktatóanyagból megtudhatja, hogy miként Ismerkedés az Azure alkalmazás szolgáltatás Web Apps alkalmazások Python fut.. Web Apps alkalmazások korlátozott ingyenes szolgáltatója és a gyors telepítési biztosít, és használhatja a Python! Az alkalmazás növekedésével fizetett szolgáltatója válthat, és az összes többi Azure szolgáltatást is integrálhatja.

Létrehoz az palack webes keretrendszer használatával webalkalmazást (lásd: Ebben az oktatóanyagban változatainak [Django](web-sites-python-create-deploy-django-app.md) és [lombikot](web-sites-python-create-deploy-flask-app.md)). Lesz a web app létrehozása a Microsoft Azure piactéren lévő, mely számjegy telepítési beállítása és a tárházba helyileg klónozhatja. Ezután lesz a web app helyi futtatásához, módosítása, véglegesítése és [Azure alkalmazás szolgáltatás Web](http://go.microsoft.com/fwlink/?LinkId=529714)Apps alkalmazások leküldéses őket. Az oktatóprogram megtudhatja, hogy miként ehhez az Windows vagy Mac vagy Linux rendszerhez.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="prerequisites"></a>Előfeltételek

- A Windows, Mac vagy Linux rendszerhez
- Python 2.7 vagy 3.4.
- setuptools, mezőpont, virtualenv (csak Python 2.7.)
- Mely számjegy
- [Python Tools for Visual Studio 2.2][] (PTVS) - Megjegyzés: Ez a lépés nem kötelező

**Megjegyzés**: TFS közzétételi jelenleg nem támogatja a Python projektek.

### <a name="windows"></a>A Windows

Ha még nincs Python 2.7 vagy 3.4 telepített (32 bites), azt javasoljuk, [Python 2.7 Azure SDK] vagy használata a webes Platform telepítő [Azure SDK Python 3.4] telepítése. Ez az Python, setuptools, mezőpont, virtualenv stb (32 bites Python az Azure host számítógépén telepítve van) 32 bites verzióját telepíti. Másik lehetőségként, amely letölthető Python [python.org].

Mely számjegy javasoljuk, [Mely számjegy for Windows] vagy a [GitHub Windows]. Visual Studio használata esetén a beépített mely számjegy-támogatás is használhatja.

Ajánlott is [Python eszközök 2.2 for Visual Studio]telepítése. Ez a lépés nem kötelező, de a [Visual Studióban], beleértve a Visual Studio közösségi ingyenes 2013 vagy a webhelyen, a Visual Studio Express 2013 esetén kattintson a képet ad egy nagy Python IDE.

### <a name="maclinux"></a>A Mac vagy Linux rendszerhez

Érdemes a Python és mely számjegy már telepítve van, de győződjön meg arról, hogy Python 2.7 vagy a 3.4.


## <a name="web-app-creation-on-the-azure-portal"></a>Webhely-alkalmazás létrehozása az Azure-portálon

Az első lépés az alkalmazás létrehozása, ha a web app az [Azure-portálon](https://portal.azure.com)keresztül.  

1. Jelentkezzen be az Azure-portálra, és kattintson az **Új** gombra, a bal alsó sarokban. 
3. A Keresés mezőbe írja be a "python".
4. A keresési eredmények közt jelölje ki a **palack**, majd kattintson a **Létrehozás**gombra.
5. Állítsa be az új palack alkalmazást, például egy új alkalmazás szolgáltatás tervet, és új erőforráscsoport létrehozása. Kattintson a **Létrehozás**parancsra.
6. [Helyi mely számjegy telepítési Azure alkalmazás szolgáltatás](app-service-deploy-local-git.md)a megjelenő utasításokat követve állítsa be a mely számjegy közzétételi az újonnan létrehozott webhely számára.
 
## <a name="application-overview"></a>Alkalmazás – áttekintés

### <a name="git-repository-contents"></a>Mely számjegy tárházba tartalma

Az alábbiakban megtalálja azt a következő szakaszban fogja klónozhatja kezdeti mely számjegy adattárban fájlok áttekintése.

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

Az alkalmazás fő forrásainak. Diaminta-elrendezés-3 lap (index, a kapcsolattartó) áll.  Statikus tartalommá és a parancsfájlok tartalmaz a betöltés, jquery, modernizr és válasz.

    \app.py

Helyi fejlesztés kiszolgáló támogatása. Ezzel a helyi meghajtóra futtassa az alkalmazást.

    \BottleWebProject.pyproj
    \BottleWebProject.sln

Projektfájlok [Python Tools for Visual Studio]való használatra.

    \ptvs_virtualenv_proxy.py

IIS proxy virtuális környezetek és távoli támogatási hibakereséshez PTVS.

    \requirements.txt

Külső csomagok szükség szerint az alkalmazás. A telepítési parancsfájlt pip lesz a fájlban felsorolt csomagok telepítése.
 
    \web.2.7.config
    \web.3.4.config

IIS konfigurációs fájljait. A telepítési parancsfájlt a megfelelő web.x.y.config használja, és másolja a vágólapra, web.config.

### <a name="optional-files---customizing-deployment"></a>Nem kötelező fájlok - telepítési testreszabása

[AZURE.INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Nem kötelező fájlok - Python futtatókörnyezet

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>További fájlokat a kiszolgálón

Egyes fájlokat a kiszolgálón található, de nem kerülnek a mely számjegy tárházba. Ezek a telepítési parancsfájlt hozza létre.

    \web.config

IIS konfigurációs fájl. Minden példányban web.x.y.config készült.

    \env\

Python virtuális környezetben. A telepítés során létre Ha kompatibilis virtuális környezetet nem létezik a web App.  Requirements.txt szereplő csomagok telepítve mezőpont, de mezőpont kihagyja a telepítést, ha már telepítve van a csomagok.

A következő 3 szakaszok leírják, hogyan folytatja a web app fejlesztésének 3 különböző környezetekben csoportban:

- A Windows, és Python Tools for Visual Studio
- A Windows, és a parancssor
- A Mac vagy Linux, a parancssorból


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Web App – Windows - Python Fejlesztőeszközök for Visual Studio

### <a name="clone-the-repository"></a>A tár másolása

Első lépésként klónozhatja a tárat a megadott az Azure portál URL-cím használatával. További tudnivalókért lásd: [Helyi mely számjegy telepítési Azure alkalmazás szolgáltatásba](app-service-deploy-local-git.md).

Nyissa meg a legfelső szintű a tárházba megoldásfájlt (.sln).

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása

Most egy virtuális helyi fejlesztői környezet létrehoznia azt. Kattintson a jobb gombbal **Python környezetekben** választó **Hozzáadása virtuális környezet...**.

- Ellenőrizze, hogy a név, a környezet `env`.

- Jelölje ki az alap értelmező. Ellenőrizze, hogy verzióját szeretné használni az azonos Python, hogy be van jelölve a web App (a runtime.txt vagy az **Alkalmazás beállításai** lap a webalkalmazás az Azure-portálon).

- Ellenőrizze, hogy töltse le és telepítse a csomagok a beállítás be van jelölve.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

Kattintson a **létrehozása**gombra. Ezzel a virtuális környezet létrehozása, és requirements.txt felsorolt függőségek telepítése.

### <a name="run-using-development-server"></a>Futtassa a fejlesztés kiszolgáló használata esetén

Nyomja meg az F5 hibakeresés, és a webböngészőben automatikusan megnyíljon a helyben fut lapra.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

Beállíthatja, hogy töréspontok a forrásokban, használja a Megtekintés windows stb. Nézze meg az egyes szolgáltatások további információt a [Python Tools for Visual Studio dokumentációt] .

### <a name="make-changes"></a>Módosítása

Most már kísérletezhet, az alkalmazás források vagy a sablonok módosításával.

Után a változtatások tesztelt véglegesítse a mely számjegy tárházba őket:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a>További csomagok telepítése

Előfordulhat, hogy az alkalmazás függőségek Python és palack túl.

További csomagokat mezőpont használatával is telepítheti. Egy csomag telepítéséhez kattintson a jobb gombbal a virtuális környezet, és jelölje ki a **Python csomag telepítéséhez**.

Telepítse az Azure SDK Python, amely férhet hozzá Azure tárhely, a szolgáltatás bus és az egyéb Azure szolgáltatások, például adja meg a `azure`:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

Kattintson a jobb gombbal a virtuális környezet, és válassza a **Létrehozás requirements.txt** requirements.txt frissítéséhez.

Ezután hajtsa végre a módosításokat a mely számjegy tárházba requirements.txt.

### <a name="deploy-to-azure"></a>Azure telepítése

Indíthatja el a telepítést, kattintson a **szinkronizálás** vagy **leküldéses**. Szinkronizálási leküldéses és egy ki is tartalmaz.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

Az első telepítési időt vesz igénybe, mint hoz létre egy virtuális környezetben, telepítés csomagok stb.

Visual Studio nem jelenik meg a telepítési elért haladás. Ha azt szeretné, tekintse át az eredményt, akkor olvassa el a szakaszban a [Hibaelhárítás – telepítés](#troubleshooting-deployment).

Nyissa meg az Azure URL-címre kattintva megtekintheti a változtatásokat.


## <a name="web-app-development---windows---command-line"></a>Web app fejlesztési – Windows - parancs sor

### <a name="clone-the-repository"></a>Klónozhatja a tárházba

Első lépésként az URL-cím az Azure-portálon használja a tárházba klónozhatja, és adja hozzá az Azure tárházba távoli. További tudnivalókért lásd: [Helyi mely számjegy telepítési Azure alkalmazás szolgáltatásba](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása

Azt létre fogja hozni egy új virtuális környezet fejlesztési célokra (nem adja hozzá azt a tárat). A Python virtuális környezetek sem áthelyezhető, így minden dolgoznak az alkalmazás fejlesztője hoz létre saját helyi meghajtóra.

Ügyeljen arra, hogy a kijelölt Python azonos verzióját használja a web App (a runtime.txt vagy az alkalmazás beállításai lap a webalkalmazás az Azure-portálon)

A Python 2.7:

    c:\python27\python.exe -m virtualenv env

A Python 3.4:

    c:\python34\python.exe -m venv env

Minden külső csomagok követel meg az alkalmazás telepítése. A legfelső szintű a tárat a requirements.txt fájl segítségével a csomagok telepítése a virtuális környezetben:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Futtassa a fejlesztés kiszolgáló használata esetén

Az alkalmazás csoportban az alábbi paranccsal fejlesztési kiszolgáló indíthatja el:

    env\scripts\python app.py

A konzol jelennek meg az URL-címet, és figyeli a kiszolgáló port:

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

Ezután nyissa meg a webböngészőben, hogy az URL-címre.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a>Módosítása

Most már kísérletezhet, az alkalmazás források vagy a sablonok módosításával.

Után a változtatások tesztelt véglegesítse a mely számjegy tárházba őket:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése

Előfordulhat, hogy az alkalmazás függőségek Python és palack túl.

További csomagokat mezőpont használatával is telepítheti. Ha például telepítse az Azure SDK Python, amely férhet hozzá Azure tárolására, a szolgáltatás bus és az egyéb Azure szolgáltatás, írja be:

    env\scripts\pip install azure

Győződjön meg arról, requirements.txt frissítése:

    env\scripts\pip freeze > requirements.txt

A módosítások véglegesítése:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Azure telepítése

Indíthatja el a telepítést, a változtatások leküldéses az Azure:

    git push azure master

Ekkor megjelenik a telepítési parancsfájlt, beleértve a virtuális környezet létrehozása, web.config kibocsátása csomagok telepítése kimenetét.

Nyissa meg az Azure URL-címre kattintva megtekintheti a változtatásokat.


## <a name="web-app-development---maclinux---command-line"></a>Web app - Mac vagy Linux - fejlesztési parancssori

### <a name="clone-the-repository"></a>A tár másolása

Első lépésként az URL-cím az Azure-portálon használja a tárházba klónozhatja, és adja hozzá az Azure tárházba távoli. További tudnivalókért lásd: [Helyi mely számjegy telepítési Azure alkalmazás szolgáltatásba](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása

Azt létre fogja hozni egy új virtuális környezet fejlesztési célokra (nem adja hozzá azt a tárat). A Python virtuális környezetek sem áthelyezhető, így minden dolgoznak az alkalmazás fejlesztője hoz létre saját helyi meghajtóra.

Ellenőrizze, hogy verzióját szeretné használni az azonos Python, hogy be van jelölve a web App (a runtime.txt vagy az alkalmazás beállításai lap a webalkalmazás az Azure-portálon).

A Python 2.7:

    python -m virtualenv env

A Python 3.4:

    python -m venv env
vagy pyvenv boríték

Minden külső csomagok követel meg az alkalmazás telepítése. A legfelső szintű a tárat a requirements.txt fájl segítségével a csomagok telepítése a virtuális környezetben:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Futtassa a fejlesztés kiszolgáló használata esetén

Az alkalmazás csoportban az alábbi paranccsal fejlesztési kiszolgáló indíthatja el:

    env/bin/python app.py

A konzol jelennek meg az URL-címet, és figyeli a kiszolgáló port:

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

Ezután nyissa meg a webböngészőben, hogy az URL-címre.

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a>Módosítása

Most már kísérletezhet, az alkalmazás források vagy a sablonok módosításával.

Után a változtatások tesztelt véglegesítse a mely számjegy tárházba őket:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése

Előfordulhat, hogy az alkalmazás függőségek Python és palack túl.

További csomagokat mezőpont használatával is telepítheti. Ha például telepítse az Azure SDK Python, amely férhet hozzá Azure tárolására, a szolgáltatás bus és az egyéb Azure szolgáltatás, írja be:

    env/bin/pip install azure

Győződjön meg arról, requirements.txt frissítése:

    env/bin/pip freeze > requirements.txt

A módosítások véglegesítése:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Azure telepítése

Indíthatja el a telepítést, a változtatások leküldéses az Azure:

    git push azure master

Ekkor megjelenik a telepítési parancsfájlt, beleértve a virtuális környezet létrehozása, web.config kibocsátása csomagok telepítése kimenetét.

Nyissa meg az Azure URL-címre kattintva megtekintheti a változtatásokat.


## <a name="troubleshooting---package-installation"></a>Telepítési hibáinak elhárítása - csomag

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Hibaelhárítás – ezek olyan virtuális környezet

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="next-steps"></a>Következő lépések

Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni a palack és Python Tools for Visual Studio: 
 
- [Palack dokumentáció]
- [Python Tools for Visual Studio dokumentáció]

Azure Táblatárolóhoz és MongoDB használatáról bővebben:

- [Palack és a Python Tools for Visual Studio az Azure MongoDB]
- [Palack és a Python Tools for Visual Studio az Azure Azure Táblatárolóhoz]

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Palack és a Python Tools for Visual Studio az Azure MongoDB]: web-sites-python-ptvs-bottle-table-storage.md
[Palack és a Python Tools for Visual Studio az Azure Azure Táblatárolóhoz]: web-sites-python-ptvs-bottle-table-storage.md

<!--External Link references-->
[Azure SDK Python 2.7.]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK Python 3.4.]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[A Windows mely számjegy]: http://msysgit.github.io/
[A Windows GitHub]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python 2.2 Tools for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools for Visual Studio dokumentáció]: http://aka.ms/ptvsdocs 
[Palack dokumentáció]: http://bottlepy.org/docs/dev/index.html
 
