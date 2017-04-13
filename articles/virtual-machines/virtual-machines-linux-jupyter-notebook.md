<properties
    pageTitle="Hozzon létre egy Jupyter/IPython jegyzetfüzet |} Microsoft Azure"
    description="További információ a Jupyter/IPython jegyzetfüzetet az erőforrás manager telepítési modell Azure-ban készült Linux virtuális gépre telepítéséhez."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Azure Jupyter jegyzetfüzet

A [Projekt Jupyter](http://jupyter.org), korábban a [IPython projekt](http://ipython.org)ad az eszközök gyűjteménye ed hatékony interaktív ismertetése, amely élő számítási dokumentum létrehozása a kód végrehajtás kombinálása használatával. A jegyzetfüzet fájlok is tartalmazhatnak, tetszőleges szöveget, matematikai képletek, beviteli kódot, eredmények, grafikus elemekkel, videók és más típusú médiafájlokat, hogy a modern webböngészőben megjelenítésére alkalmas. Teljes mértékben ismerkedik Python, és megtudhatja, hogy szórakoztató, interaktív környezetben, és végezze el az egyes komoly párhuzamos vagy műszaki számítások szeretné, hogy Jupyter jegyzetfüzet-e kiváló választás.

![Képernyőkép](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) elemzése a hangrögzítés felépítésének használata SciPy és Matplotlib csomagok.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter két módja: Azure jegyzetfüzetek vagy egyéni telepítési
Azure biztosít, amelyek segítségével [gyorsan használatbavétele Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx)szolgáltatás.  Az Azure jegyzetfüzet szolgáltatás használatával, könnyen hozzáférhet Jupyter a webhelyen elérhető felületet méretezhető számítási erőforrások Python, a power és annak számos tárak.  A telepítés kezeli a szolgáltatási, mivel a felhasználók hozzá tudnak férni ezek az erőforrások felügyelete és konfigurálása a felhasználó által meg nélkül.

Ha a jegyzetfüzet szolgáltatás nem működik a levelezés folytassa Ez a cikk, amely fog megtudhatja, hogy miként bevezetését tervezi a Microsoft Azure-Jupyter jegyzetfüzet Linux virtuális gépeken futó (VMs) használatával.

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Létrehozása és konfigurálása a virtuális Azure

Az első lépésként futó Azure virtuális gép (virtuális) létrehozása.
A virtuális egy teljes operációs rendszer a felhőben, és futtassa a Jupyter jegyzetfüzet szolgálnak. Azure is alkalmas rendszert futtató Linux és a Windows virtuális gépeken futó, és témákat vesszük sorra virtuális gépeken futó mindkét fajta Jupyter beállítását.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Hozzon létre egy Linux virtuális, és olyan portot Jupyter megnyitása

Kövesse az utasításokat a megadott [Itt] [ portal-vm-linux] a *Ubuntu* eloszlás virtuális gép létrehozása. Ebben az oktatóanyagban Ubuntu Server 14.04 LTS használ. A felhasználó neve *azureuser*fogja feltételezzük.

Miután a virtuális gép üzembe helyezése kattintva nyissa meg a hálózati biztonsági csoport a biztonsági szabály szükséges.  Az Azure portálról nyissa meg a **Hálózati biztonsági csoportokat** , és nyissa meg a lapot a megfelelő a virtuális biztonsági csoport. Be kell állítania egy bejövő biztonsági szabályt az alábbi beállításokkal: **TCP** -protokoll esetében **\*** a forrásportot (nyilvános) és a **9999** -(személyes) célportjáéval.

![Képernyőkép](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

A hálózati biztonsági csoport a **Hálózati** kapcsolaton kattintson, és jegyezze fel a **Nyilvános IP-cím** , a következő lépés a virtuális csatlakozhat lesz szükség szerint.

## <a name="install-required-software-on-the-vm"></a>A virtuális szükséges szoftver telepítése

A virtuális futtatásához a Jupyter jegyzetfüzetet, azt előbb telepítenie kell Jupyter és függőségét. Kapcsolódás a linux virtuális ssh és a felhasználónevével és jelszavával, párosítás választotta, a virtuális létrehozásakor. Ebben az oktatóanyagban fog gitt és használjuk csatlakozás Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Ubuntu Jupyter telepítése
Telepítse a Anaconda, a népszerű adatok tudományos python eloszlás használatával a megadott [Alakíthatnak ki olyan Analytics](https://www.continuum.io/downloads)a hivatkozások egyikére.  Kezdve az írás a dokumentum, a lenti hivatkozásokkal vannak a legtöbb dátum verziók felfelé.

#### <a name="anaconda-installs-for-linux"></a>A Linux anaconda telepítések
<table>
  <th>Python 3.4.</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bites</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bites</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Telepítés Anaconda3 2.3.0 Ubuntu 64 bites
Példaként ez hogyan telepíthető Anaconda Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Képernyőkép](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter konfigurálása és az SSL használatával
Telepítése után szükség szánjon egy kis időt Jupyter a konfigurációs fájl beállítását. Ha a Jupyter beállítása során fellépő problémák merülnek tekintse meg [a jegyzetfüzet Servert futtató Jupyter dokumentációjában](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)hasznos lehet.

Következő azt `cd` az SSL-tanúsítvány létrehozása és szerkesztése a profilok konfigurációs fájl a profil címtárhoz.

Linux a következő parancsot használja.

    cd ~/.jupyter

A következő parancsot használja az SSL-tanúsítvány (Linux és Windows) létrehozása.

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Megjegyzendő, hogy mivel önaláírt SSL-tanúsítvány, ha a böngészőben a jegyzetfüzetet kapcsolódás képet ad a biztonsági figyelmeztetés.  Hosszú távú termelési használatra fogja használni kívánt szervezete társított megfelelően aláírt tanúsítvány.  Mivel ez a bemutató túlra tanúsítványkezelés, hogy fog szigorúan önaláírt tanúsítvány most.

A tanúsítvány, mellett jelszóval védheti a jegyzetfüzet a jogosulatlan is meg kell adnia.  Biztonsági okokból Jupyter használja titkosított jelszót a konfigurációs fájl, először titkosítása jelszavát kell.  IPython itt megteheti; segédprogram a parancssorba futtassa a következő parancsot.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Kérni fogja a jelszót, és a megerősítést kérő, és ezután nyomtatja ki a jelszót. Megjegyzés: Ez a következő lépésben.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Ezután a profil konfigurációs fájl, amely azt szerkeszteni a `jupyter_notebook_config.py` a címtárban a fájlt.  Megjegyzés:, hogy a fájl nem létezik – csak a Ha ebben az esetben hozzon létre.  Ez a fájl tartalmaz számos mező, és alapértelmezés szerint minden vannak megjegyzéseket fűzhet.  Megnyithatja a fájlt, és tetszőlegesen a szövegszerkesztőben, és győződjön meg arról, hogy rendelkezik legalább az alábbi tartalom. **A Config és az előző lépésben a sha1 a c.NotebookApp.password helyére ne felejtse el**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>A Jupyter jegyzetfüzet futtatása

Ezen a ponton vagyunk készen áll a kezdésre a Jupyter jegyzetfüzetet. Ehhez nyissa meg a könyvtár szeretne a jegyzetfüzetek tárolására és a jegyzetfüzet Jupyter kiszolgáló kezdődhet a következő parancsot.

    /anaconda3/bin/jupyter-notebook

Most már hozzáférhet a címen Jupyter jegyzetfüzetébe `https://[PUBLIC-IP-ADDRESS]:9999`.

Amikor először nyitja meg a jegyzetfüzetét, a bejelentkezési lapja megkérdezi, hogy a jelszót. És amikor bejelentkezik az, látni fogja a "Jupyter jegyzetfüzet irányítópult", amely olyan, az összes jegyzetfüzet-műveletek az vezérlőkarakterek központban.  Ezen a lapon hozzon létre új jegyzetfüzetet, és nyissa meg a meglévőket.

![Képernyőkép](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>A Jupyter Jegyzetfüzet használata

Ha az **Új** gombra kattint, látni fogja a következő lap megnyitása.

![Képernyőkép](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

A terület jelölt egy `In []:` kérése a szövegbeviteli területre, és ide írhat be, bármely érvényes Python kódot és azt hajtja végre, amikor gombra kattint `Shift-Enter` vagy a "Lejátszás" ikonra (a jobbra mutató háromszög eszköztár) gombra.

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>A hatékony mintákat: live számítási dokumentumok a multimédiás tartalmak használatára

A jegyzetfüzetet önmagában kell úgy érzi nagyon természetes bárki megtekintheti, aki használta Python és egy szövegszerkesztőben, mert a bemutatják, hogyan vegyesen is: Python kód szövegblokkokat hajthat végre, de azt is, hogy a jegyzeteket és más egyéb szöveg "Markdown" a "Kódból" cella stílusának módosításával a legördülő menü használatával eszköztár.

Jupyter sokkal több, mint egy szövegszerkesztőt, lehetővé teszi, hogy a számítási és a multimédiás tartalom (szöveg, képek, video- és gyakorlatilag bármilyen modern webböngészőben megjeleníthető) keverése. Akkor is keverjen ki, szöveg, kódot, videók és egyebek!

![Képernyőkép](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

És a power Python meg számos kitűnő tárak tudományos és technikai a számítások, az alábbi képernyőképen egy egyszerű számítás elvégezhető a egy összetett hálózat-elemzés egy környezetben az összes, azonos könnyű.

Ez az élő kiszámítása a teljesítmény a régi webhely keverése mintákat sok lehetőségeket kínál, és kiválóan alkalmas az a felhő; a jegyzetfüzet használható:

* A számítási firkatömb felderítő felvenni, a probléma működik.

* Eredmények a munkatársaival is megoszthatja, a "élő" számítási űrlap vagy a Nyomtatott képernyőmásolat menüpontot formats (HTML, PDF-fájl).

* Terjesztése, és ossza meg, amely magában foglalja a számítást, így diákok azonnal nyugodtan kísérletezhet a valós kódot élő tananyag, módosítsa, majd újra hajtsa végre interaktív módon.

* "Végrehajtható tanulmányt", amely a találatokat a kutatás oly módon, hogy is azonnal reprodukálható, érvényesített és meghosszabbított mások számára.

* Egy olyan együttműködési számítások platform mint: több felhasználó ugyanarra a jegyzetfüzet-kiszolgálóra élő számítási munkamenet megosztása jelentkezhetnek be.


Ha megnyitja a IPython kód [tárházba][], töltse le, és kattintson a saját Azure Jupyter virtuális kísérletezés a jegyzetfüzet példák egy teljes címtárban találhatja meg.  Egyszerűen csak töltse le a `.ipynb` a webhelyen lévő fájlok, és töltse fel őket a jegyzetfüzet Azure virtuális az irányítópulton alakzatot (vagy töltse le őket közvetlenül a virtuális).

## <a name="conclusion"></a>Elfogadásáról

A Jupyter jegyzetfüzet eléréséhez interaktív a power Azure a Python ökológiai hatékony felületet biztosít.  Bemutatja, hogy az esetek használatát, ideértve a egyszerű feltárására és Python, adatelemzés és a megjelenítés, szimulációk és a párhuzamos számítások tanulási széles köre. Az eredményül kapott jegyzetfüzet dokumentumok tartalmazzák a számítások előfordulnak és Jupyter másokkal megosztható teljes rögzíteni.  A Jupyter jegyzetfüzet egy helyi alkalmazást is alkalmazható, de azt kiválóan alkalmas Azure a felhőben telepítésekhez

Jupyter alapvető funkcióját belül Visual Studio [Python Tools for Visual Studio][] (PTVS) keresztül is érhetők el. PTVS egy ingyenes és Megnyitás forrás beépülő modult a Microsofttól, amely a Visual Studio változik egy speciális Python fejlesztői környezet, amely tartalmazza az IntelliSense használatával, a hibakereséshez, egy speciális szerkesztő profilkészítési és a párhuzamos számítások integráció.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Python Developer Center](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[tárházba]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
