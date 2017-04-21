#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Az Azure parancssori eszközök használatáról a Mac- és Linux

Ez az útmutató ismerteti a Mac- és Linux az Azure parancssori eszközök segítségével létrehozása és kezelése, szolgáltatások Azure-ban. Az érintett esetek **eszközök telepítése**, **a közzétételi beállítások importálása**, **létrehozása és kezelése az Azure-webhelyekre**és **hozhat létre és kezelhet Azure virtuális gépeken futó**. Teljes hivatkozás dokumentációjának, lásd: [a Mac- és Linux dokumentáció Azure parancssori eszköz][reference-docs]. 

##<a name="table-of-contents"></a>A tartalomjegyzék
* [Mik azok az Azure parancssori eszközök a Mac- és Linux](#Overview)
* [Az Azure parancssori eszközök telepítése a Mac- és Linux](#Download)
* [Azure-fiók létrehozása](#CreateAccount)
* [Hogyan töltheti le és a közzétételi beállításokat importálása](#Account)
* [Hogyan hozhat létre, és az Azure-webhely kezelése](#WebSites)
* [Hogyan hozhat létre és kezelhet az Azure virtuális gépen](#VMs)


<h2><a id="Overview"></a>Mik azok az Azure parancssori eszközök a Mac- és Linux</h2>

A Mac- és Linux az Azure parancssori eszközök telepítéséhez és Azure szolgáltatások kezelése parancssori eszközök.
 
A támogatott tevékenységek a következők:

* Közzétételi beállítások importálása.
* Hozzon létre és Azure webhelyek kezelése.
* Létrehozhatja és kezelheti az Azure virtuális gépeken futó.

Írja be a teljes listáját a támogatott parancsok, `azure -help` a eszközök telepítése után a parancssorból vagy a [hivatkozás dokumentáció][reference-docs].

<h2><a id="Download">Az Azure parancssori eszközök telepítése a Mac- és Linux</a></h2>

Az alábbi lista tartalmazza az operációs rendszertől függően a parancssori eszközök telepítésével kapcsolatos információk:

* **MAC**: az [Azure SDK telepítő]letöltése[mac-installer]. Nyissa meg a letöltött .pkg fájlt, és hajtsa végre a telepítést, a program kéri.

* **Linux**: [Node.js] legújabb verziójának telepítéséhez[ nodejs-org] (lásd: [Telepítse Node.js keresztül csomag Manager][install-node-linux]), majd futtassa a következő parancsot:

        npm install azure-cli -g

    **Megjegyzés**: a parancsot, emelt jogosultságokkal rendelkező szüksége lehet:

        sudo npm install azure-cli -g

* **A Windows**: futtassa a Winows installer (MSI-fájl), amely érhető el az alábbi: [Azure parancssori eszközök][windows-installer].


A telepítés tesztelésére, írja be a `azure` parancsot. Ha a telepítés sikeres volt, látni fogja az összes rendelkezésre álló lista `azure` parancsokat.

<h2><a id="CreateAccount"></a>Azure-fiók létrehozása</h2>

A Mac- és Linux az Azure parancssori eszközök használatához szüksége lesz az Azure-fiók.

Nyisson meg egy webböngészőt, és keresse meg [http://www.windowsazure.com] [ windowsazuredotcom] , és kattintson a **próba-előfizetésre** jobb felső sarokban.

![Azure webhelyén][Azure Web Site]

Kövesse az utasításokat a fiók létrehozásához.

<h2><a id="Account"></a>Hogyan töltheti le és a közzétételi beállításokat importálása</h2>

Első lépésként meg kell először töltse le és importálja a közzétételi beállításokat. Ez lehetővé teszi, hogy az eszközök segítségével létrehozása és kezelése az Azure-szolgáltatások. Letöltése a közzétételi beállításokat, használja a `account download` parancsot:

    azure account download

Ez az alapértelmezett böngésző Nyissa meg, és kérdezze meg, hogy jelentkezzen be az adatkezelési portálra. Bejelentkezve, a `.publishsettings` fájl letöltődik. Jegyezze fel a fájlt tároló.

Következő lépésként importálja a `.publishsettings` fájl a következő parancs futtatásával cseréje `{path to .publishsettings file}` útvonala a `.publishsettings` fájl:

    azure account import {path to .publishsettings file}

Eltávolíthatja az összes tárolt adatokat a <code>import</code> parancs használatával a <code>account clear</code> parancsot:

    azure account clear

Kapcsolódó lehetőségek listájának `account` parancsokat, és használja a `-help` beállítást:

    azure account -help

Importálás után a közzétételi beállításokat, törölnie kell a `.publishsettings` biztonsági okokból fájlt.

> [AZURE.NOTE] Importálásakor közzétételi beállításokat, az Azure előfizetés be tartozó hitelesítő adatokat tárolja belül a `user` mappát. A `user` mappa védi az operációs rendszer. Azonban javasoljuk, hogy az Ön által további lépésekre titkosítása a `user` mappát. Ezt az alábbi módon teheti meg:    
> 
> - A Windows a mappa tulajdonságainak módosítása vagy BitLocker használja.
> - A Mac rendszeren kapcsolja be a FileVault a mappához.
> - A Ubuntu a funkcióval titkosított kezdőlap címtár. Más Linux terjesztését egyenértékű funkciókat kínál.

Most már készen áll Azure webhelyekre és Azure virtuális gépeken futó kezelése és létrehozásának.  

<h2><a id="WebSites"></a>Hogyan hozhat létre, és az Azure-webhely kezelése</h2>

###<a name="create-a-website"></a>Webhely létrehozása

Egy Azure webhelyet, először hozzon létre egy üres könyvtár nevű `MySite` , és keresse meg a könyvtár.

Futtassa a következő parancsot:

    azure site create MySite --git

Ez a parancs kimenetét az újonnan létrehozott webhely alapértelmezett URL-CÍMÉT is tartalmaz. A `--git` beállítás lehetővé teszi, hogy mely számjegy segítségével mely számjegy tárházakban létrehozásával, mind a helyi alkalmazás könyvtárában, és a webhely adatközpont közzététele a webhelyen. Figyelje meg, hogy ha a helyi mappában már olyan mely számjegy adattárban, a parancs ad hozzá egy új távoli meglévő tárházba, mutasson a tárházba a webhely adatközpont.

Megjegyzendő, hogy hajtsa végre a `azure site create` parancsot a az alábbi lehetőségek közül:

* `--location [location name]`. Ezzel a beállítással megadhatja az adatközpont, amelybe a webhely létrehozása (például "nyugati US") helyét. Ez a beállítás nincs megadva, fogja promted kattintva válasszon egy helyet.
* `--hostname [custom host name]`. Ezzel a beállítással megadhatja, egy egyéni hostname (állomásnév) a webhelyhez.

Kattintson a webhely címtárban adhat tartalmat. A normál mely számjegy folyamat használata (`git add`, `git commit`) véglegesítse a tartalmakat. A következő mely számjegy paranccsal a webhely tartalma leküldéses az Azure: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>A GitHub történő közzététel beállítása

Folytonos közzététel GitHub összegyűjti beállításához használja a `--GitHub` lehetőséget, ha a webhely létrehozása:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Ha rendelkezik GitHub összegyűjti a helyi átirattal, vagy ha egy tárban tárolnak egy GitHub tárházba egyetlen távoli hivatkozással, ez a parancs automatikus közzététele kódot a GitHub tárat a webhelyre. Ettől kezdve a GitHub tárházba tolni módosításokat automatikusan közzéteszi a webhelyre.

Közzététel beállítása a GitHub, a használt alapértelmezett ág esetén a fő ág. Egy másik ág megadásához hajtsa végre a következő parancsot a helyi tárházba való:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Alkalmazás beállításainak konfigurálása

Alkalmazás beállításainak kulcs – érték párokká, az alkalmazás futásidőben a rendelkezésre álló. Ha a beállítás az Azure-webhelyhez, alkalmazás beállítás értékei felülírja az beállítások kulccsal határozzák meg a webhely-fájlt. Node.js és PHP-alkalmazásokhoz alkalmazás beállításainak érhetők el környezet változók. A következő példa bemutatja, hogyan kell beállítani a billentyű-érték pár:

    azure site config add <key>=<value> 

Megjelenítéséhez, az összes kulcs/érték párokká hajtsa végre a következőket:

    azure site config list 

Vagy ha, hogy a billentyűt, és szeretné, hogy az érték, akkor használhatja:

    azure site config get <key> 

Ha meg szeretné változtatni egy meglévő kulcs értékét, először törölje a meglévő billentyűt, és ezután állítsa be újra. A Törlés parancs a következő:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Lista és a Megjelenítés

A webhelyek listáját, használja az alábbi parancsot:

    azure site list

Egy webhely részletes információkat kaphat, használja a `site show` parancsot. A következő példa bemutatja a részletek `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Leállítása vagy és indítsa újra a webhely

Leállítása, indítása vagy a helyet, indítsa újra a `site stop`, `site start`, vagy `site restart` parancsokat:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Webhely törlése

Végül a helyet, törölheti a `site delete` parancsot:

    azure site delete MySite

Megjegyzendő, hogy ha a mappa adódott hol belül futnak bármelyik parancsok helye feletti `site create`, nem kell megadni a webhely neve `MySite` az utolsó paraméterként.

Egy teljes listájának megjelenítéséhez `site` parancsokat, és használja a `-help` beállítást:

    azure site -help 

<h2><a id="VMs"></a>Hogyan hozhat létre és kezelhet az Azure virtuális gépen</h2>

az Azure virtuális gép (.vhd fájl) Ön által vagy, amely érhető el a kép gyűjtemény virtuális gép képen lévő jön létre. Képeket szeretne látni a rendelkezésre álló, használja a `vm image list` parancsot:

    azure vm image list

Építse, és indítsa el a virtuális gép elérhető képekkel a `vm create` parancsot. A következő példa bemutatja, hogyan hozhat létre virtuális Linux gép (nevű `myVM`) a képet a képgyűjtemény (CentOS 6.2). A legfelső szintű felhasználónevet és jelszót a virtuális gép `myusername` és `Mypassw0rd` rendre. (Fontos, hogy a `--location` paraméterrel az adatközpont, amelyben a virtuális gép jön létre. Ha nincs megadva a `--location` paraméter kérni fogja válasszon egy helyet.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Átadása akkor fontolja meg a `--ssh` jelző (Linux) vagy `--rdp` flag kifejezéssel (Windows) `vm create` ahhoz, hogy az újonnan létrehozott virtuális géphez távkapcsolat.

Ha meg szeretné inkább kiépítése egyéni képen lévő virtuális géphez, a fájl az .vhd is létrehozhat egy képet a `vm image create` parancsot, majd használja a `vm create` hozhatók létre virtuális gépen parancsot. A következő példa bemutatja, hogyan hozhat létre egy Linux kép (nevű `myImage`) helyi .vhd fájlból. (A `--location` paraméter adja meg a kép adataihoz.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Helyett egy képet a egy helyi .vhd, létrehozhat egy képet az Azure Blob-tárolóhoz tárolt egy .vhd. Az ezt megteheti a `blob-url` paraméter:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Miután létrehozott egy képet, akkor is hozhatók létre virtuális gép a kép a `vm create`. Az alábbi parancsot létrehoz egy virtuális gép nevű `myVM` az előbb létrehozott képről (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Után egy virtuális számítógépre van kiépítve, érdemes létrehozása (például a) a virtuális gép távoli elérésének engedélyezése a végpontok. Az alábbi példában a `vm create endpoint` gépen való megnyitásához külső 22-es és helyi portot 22 parancs `myVM`:

    azure vm endpoint create myVM 22 22

(Beleértve az IP-cím, a DNS-neve és a végpont adatait) virtuális gép részletes információt a elérheti a `vm show` parancsot:

    azure vm show myVM

Le indítsa el, vagy indítsa újra a virtuális gép, használja az alábbi parancsok egyikét:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

És végül a virtuális törléséhez használja a `vm delete` parancsot:

    azure vm delete myVM

Parancsok létrehozásához és kezeléséhez virtuális gépeken futó listáját, használja a `-h` beállítást:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

