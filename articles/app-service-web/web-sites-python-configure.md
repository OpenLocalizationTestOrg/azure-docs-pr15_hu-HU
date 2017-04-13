<properties 
    pageTitle="Azure alkalmazás szolgáltatás Web Apps alkalmazások Python konfigurálása" 
    description="Ebben az oktatóanyagban a szerzői és az átjáró felülete (WSGI) kompatibilis Python alkalmazást egyszerű webkiszolgálóra konfigurálása webes szolgáltatás Azure alkalmazásindítónalkalmazások ismerteti." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Azure alkalmazás szolgáltatás Web Apps alkalmazások Python konfigurálása

Ebben az oktatóanyagban létrehozása és konfigurálása egy egyszerű webhely kiszolgálói átjáró felület (WSGI) kompatibilis Python alkalmazást [Web Service Azure](http://go.microsoft.com/fwlink/?LinkId=529714)alkalmazásindítónalkalmazások lehetőségeket ismerteti.

Azt ismerteti, hogy mely számjegy telepítési, például a virtuális környezet és a requirements.txt használatával csomag telepítése további szolgáltatásait.


## <a name="bottle-django-or-flask"></a>Palack, Django vagy lombikba?

A Microsoft Azure piactéren a palack, Django és lombikot keretek sablonokat tartalmazza. Ha Azure App szolgáltatásban az első webalkalmazást fejlesztéséhez, vagy Ön nem ismeri a mely számjegy, ha azt ajánljuk, hogy oktatóanyagokban, amelyek lehetnek a lépésenkénti útmutatást adunk a munkaidő-alkalmazások a gyűjteményből használata Windows vagy Mac mely számjegy telepítési épület közül kövesse:

- [Palack web Apps alkalmazások létrehozása](web-sites-python-create-deploy-bottle-app.md)
- [Django web Apps alkalmazások létrehozása](web-sites-python-create-deploy-django-app.md)
- [Lombikot web Apps alkalmazások létrehozása](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Azure portálon webhely-alkalmazás létrehozása

Ebben az oktatóanyagban feltételezi, hogy egy meglévő Azure előfizetés, és az access az Azure-portálra.

Ha egy meglévő web App alkalmazásban nem rendelkezik, létrehozhat egyet az [Azure-portálon](https://portal.azure.com).  Kattintson az új gombra, a bal felső sarokban, majd válassza a **webhely + Mobile** > **Web App alkalmazásban**.

## <a name="git-publishing"></a>Mely számjegy-közzététel

[Helyi mely számjegy telepítési Azure alkalmazás szolgáltatás](app-service-deploy-local-git.md)a megjelenő utasításokat követve állítsa be a mely számjegy közzétételi az újonnan létrehozott webhely számára. Ebben az oktatóanyagban mely számjegy elkészítheti, kezelheti és közzététele a Python webalkalmazás Azure alkalmazás szolgáltatást használja.

Miután mely számjegy-közzétételt be van állítva, mely számjegy összegyűjti létrehozott, és a web app társított. A tárházba URL-cím jelenik meg, és ezentúl adatok leküldéses helyi fejlesztői környezetéből a felhőbe használható. Mely számjegy keresztül alkalmazások közzététele, győződjön meg arról, hogy mely számjegy ügyfél is telepítve van, és használja a web app tartalom nyomni Azure alkalmazás szolgáltatás a megjelenő utasításokat.


## <a name="application-overview"></a>Alkalmazás – áttekintés

A következő szakaszokban a következő fájlok hozhatók létre. A legfelső szintű a mely számjegy tárházba kerüljön.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI eseménykezelő

WSGI Python szabványos leírt [EGP 3333](http://www.python.org/dev/peps/pep-3333/) definiáló felületet, a webkiszolgáló és Python között. Egy szabványos felületet biztosít írása az egyes webalkalmazások és keretek Python használatával. Népszerű Python webes keretek ma használata WSGI. Azure alkalmazás szolgáltatás Web Apps alkalmazások ad meg ilyen keretek; támogatása Ezenkívül a haladó felhasználók is készíthet saját mindaddig, amíg az egyéni kezelő követi az WSGI specifikációja útmutatásokat.

Íme egy példa egy `app.py` , amely meghatározza, hogy egy egyéni kezelő:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Ehhez az alkalmazáshoz helyileg futtatását is lehetővé teszi `python app.py`, majd tallózással `http://localhost:5555` a böngészőben.


## <a name="virtual-environment"></a>Virtuális környezet

Bár a fenti példa alkalmazás használatához nincs szükség minden olyan külső csomagokat, akkor valószínű, hogy az alkalmazás esetén kell néhány.

Azure mely számjegy telepítési kezelésének egyszerűsítése érdekében külső csomag függőségek, támogatja a virtuális környezetek létrehozását.

Amikor Azure egy requirements.txt a legfelső szintű a tárházba azt észleli, automatikusan létrehoz egy virtuális környezet nevű `env`. Ez csak akkor fordul elő, az első példányban, vagy bármely telepítése után a kijelölt Python során futtatókörnyezet megváltozott.

Valószínűleg szeretné helyileg fejlesztési virtuális környezet kialakítása, de nem adja hozzá a mely számjegy tárházba.


## <a name="package-management"></a>Csomag kezelése

A felsorolt requirements.txt csomagok automatikusan települ mezőpont használatáról virtuális környezetben. Minden példányban ez történik, de mezőpont kihagyja telepítési, ha egy csomag már telepítve van.

Példa `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python verziója

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Példa `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Adja meg, hogyan kell kezelni a kiszolgáló kéréseket web.config fájl létrehozása kell.

Megjegyzendő, hogy a tárban tárolnak, ahol x megegyezik a kijelölt Python futtatókörnyezet, majd az Azure automatikusan bemásolja a megfelelő fájlt web.config web.x.y.config fájl esetén.

Az alábbi web.config példák virtuális környezet proxy parancsfájl, a következő szakaszban ismertetett támaszkodhat.  Azok a példában használt WSGI kezelő használata `app.py` felett.

Példa `web.config` Python 2.7 esetében:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Példa `web.config` Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statikus fájlok kezelje a rendszer a webkiszolgáló közvetlenül, anélkül, hogy Python kódját teljesítménynövekedés keresztül.

A fenti példa a lemezen statikus fájlokat helyének meg kell egyeznie a hely URL-címét. Ez azt jelenti, hogy egy kérelemre `http://pythonapp.azurewebsites.net/static/site.css` fog szolgálni, a fájl a lemezen `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`van, ahol megadhatja a WSGI kezelő. A fenti példákban van `app.wsgi_app` mivel a kezelő nevű függvény `wsgi_app` a `app.py` gyökérmappájában.

`PYTHONPATH`testre szabható, de ha minden függőség a virtuális környezetben a requirements.txt megadásával, ne módosítani szeretné azt.


## <a name="virtual-environment-proxy"></a>Virtuális környezet Proxy

A következő parancsfájl beolvasni a WSGI kezelőt, a virtuális környezetben, és jelentkezzen be a hibák aktiválása használják. Általános és módosítások nélkül használt szolgál.

Tartalmának `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Mely számjegy telepítési testreszabása

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Telepítési hibáinak elhárítása - csomag

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Hibaelhárítás – ezek olyan virtuális környezet

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Python Developer Center](/develop/python/).

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Útmutató a módosítása a webhelyekre alkalmazás szolgáltatás című: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)





 
