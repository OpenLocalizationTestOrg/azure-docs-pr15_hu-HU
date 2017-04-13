<properties
    pageTitle="A sínek webhelyén, a egy Linux virtuális fonetikus szolgáltató |} Microsoft Azure"
    description="Állítsa be, és a sínek-alapú webhelyén, a Azure virtuális Linux gép használatával fonetikus szolgáltató."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>A sínek webalkalmazás-Azure virtuális fonetikus

Ebben az oktatóanyagban szemlélteti, hogyan lehet tárolni a sínek webhelyén, a Azure fonetikus Linux virtuális gép használatával.  

Ebben az oktatóanyagban ellenőrzése Ubuntu Server 14.04 LTS használatával. Ha egy másik Linux terjesztési használja, előfordulhat, hogy módosítania kell a lépések követésével telepítse sínek.

> [AZURE.IMPORTANT] Azure van két különböző telepítési modellekkel létrehozásáról és használatáról az erőforrások: [az erőforrás-kezelő és klasszikus](../../../resource-manager-deployment-model.md).  Ez a cikk bemutatja, hogy a klasszikus telepítési modellt használja. Azt javasoljuk, hogy a legtöbb új telepítések az erőforrás-kezelő modell használata.

## <a name="create-an-azure-vm"></a>Hozzon létre egy Azure virtuális

Indítsa el az Azure virtuális létrehozásával Linux képével.

A virtuális létrehozásához használhatja az Azure klasszikus portálra vagy az Azure parancssor CLI ().

### <a name="azure-management-portal"></a>Azure Kezelőportálja segítségével

1. Jelentkezzen be az [Azure klasszikus portál](http://manage.windowsazure.com)
2. Kattintson az **Új** > **kiszámítania** > **virtuális gép** > **gyors létrehozásához**. Jelölje ki a Linux képet.
3. Írjon be egy jelszót.

Miután a virtuális már kiépítve, kattintson a virtuális nevére, és kattintson az **Irányítópult**. Keresse meg az SSH végpontot, **SSH részletek**alatt.

### <a name="azure-cli"></a>Azure CLI

Kövesse a [Create a virtuális gépen futó Linux][vm-instructions].

A virtuális már kiépítve, miután a SSH végpont elérheti a következő parancs futtatásával:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Telepítse a fonetikus sínen

1. A virtuális használata SSH.

2. A SSH munkamenetből használja a következő parancsokat a virtuális fonetikus telepítése:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    A telepítés néhány percig tart. Ha elkészült, a következő paranccsal ellenőrizheti, hogy telepítve van-e a fonetikus:

        ruby -v

    Ez a fonetikus telepített verziója adja eredményül.

3. A következő paranccsal sínek telepítése:

        sudo gem install rails --no-rdoc --no-ri -V

    Használja a – nem-rdoc és – nem – j jelölők ugorja át a gyorsabb dokumentációjában telepítése.
    Ez a parancs hozzáadása a -V szolgáltatást úgy jelennek meg a telepítés folyamatának információt, valószínűleg végrehajtani, hosszú időt vesz igénybe.

## <a name="create-and-run-an-app"></a>Létrehozása és futtatása alkalmazás

Bejelentkezve továbbra is SSH keresztül, futtassa az alábbi parancsokat:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Az [Új](http://guides.rubyonrails.org/command_line.html#rails-new) parancs új sínek alkalmazás hoz létre. A [kiszolgálóról](http://guides.rubyonrails.org/command_line.html#rails-server) parancs elindítja a WEBrick webkiszolgáló sínek épített. (Gyártási használatra volna valószínűleg használni kívánt másik kiszolgálót, például Unicorn vagy személy.)

Ekkor megjelennek a kimeneti az alábbihoz hasonló.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Zárólap hozzáadása

1. Nyissa meg az [Azure klasszikus portál] [ management-portal] , és válassza ki a virtuális.

    ![virtuális gép lista][vmlist]

2. Jelölje ki a **VÉGPONTOK** a lap tetején, és kattintson a **+ VÉGPONT hozzáadása** az oldal alján.

    ![végpontok lapon][endpoints]

3. **VÉGPONT hozzáadása** párbeszédpanelen jelölje be a "Hozzáadása egy különálló végpontot", és kattintson a **következő** nyílra.

    ![az új végpont párbeszédpanel][new-endpoint1]

3. Párbeszédpanelen a következő lapon adja meg a következő adatokat:

    * **Név**: HTTP

    * **Protocol (protokoll)**: TCP

    * **Nyilvános PORT**: 80

    * **MAGÁNJELLEGŰ PORT**: 3000

    Ez hoz létre, amely fog irányítja a forgalmat a magánjellegű portja 3000, ahol a sínek kiszolgáló figyel 80 egy nyilvános portot.

    ![az új végpont párbeszédpanel][new-endpoint]

4. Kattintson a jelölőnégyzet be van jelölve a végpont menteni.

5. Egy üzenet arról, hogy **A folyamatban lévő frissítés**jelenjenek meg. Ez az üzenet eltűnik, miután a végpont tevékenykedik. Az alkalmazás letöltése a virtuális gép DNS-nevének navigálással előfordulhat, hogy tesztelése. A webhely jelenjen meg az alábbihoz hasonló:

    ![alapértelmezett sínek lap][default-rails-cloud]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban elvégzett lépéseket a legtöbb manuálisan. Üzemi környezetben azt, akinek fejlesztési gépre írja be az alkalmazás és a Azure virtuális kell üzembe. A legtöbb gyártási környezetekben állomása is, például Apache vagy NginX, mely fogópontok kérése a továbbítás sínek alkalmazása több példányával, és a statikus erőforrások felszolgálásához egy másik kiszolgáló folyamat együtt sínek alkalmazása. További tudnivalókért olvassa el a http://rubyonrails.org/deploy/ című témakört.

Többet szeretne tudni a fonetikus sínen, látogasson el a [sínek útmutatók a fonetikus][rails-guides].

A fonetikus levelezőprogramból Azure szolgáltatást használ, lásd:

* [Strukturálatlan adattárolásra BLOB használatával][blobs]

* [Tár kulcs/érték párokká táblázatok használata][tables]

* [A tartalom kézbesítési hálózati sávszélesség magas tartalmat][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
