<properties
    pageTitle="Az SQL Server virtuális gép IPython jegyzetfüzet kiszolgáló beállítása |} Microsoft Azure"
    description="Állítsa be adatokat tudományos virtuális gép SQL Server és a IPython kiszolgálóval."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev" 
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>A fejlett analitikai IPython jegyzetfüzet kiszolgáló-Azure SQL Server virtuális gép beállítása

Ez a témakör bemutatja, hogyan kell rendelkezni, és állítsa be az SQL Server virtuális gép egy felhőalapú adatok tudományos környezet részeként használható. A Windows virtuális gép azokat az eszközöket, például a jegyzetfüzet IPython, Azure tároló Explorer és AzCopy, valamint egyéb hasznos tudományos projektekhez segédprogramot igazoló van beállítva. Azure tároló Explorer és AzCopy, például adja meg a helyi gépről Azure blob-tárolóhoz feltölteni az adatokat, vagy töltse le a helyi számítógép zónában blob-tárolóból kényelmes lehetőségeket.

Az Azure virtuális gépek galéria, amelyeknél a Microsoft SQL Server több kép tartalmazza. Jelölje ki, az adatok igényeinek megfelelő SQL Server virtuális képet. Ajánlott képeket a következők:

- Az SQL Server 2012 SP2 Enterprise kis méretű közepes adatokkal és
- Az SQL Server 2012 SP2 vállalati optimalizálva DataWarehousing Munkaterhelésekből adatok nagy ahhoz, hogy nagyon nagy méretű

 > [AZURE.NOTE] Az SQL Server 2012 SP2 Enterprise a kép **nem tartalmaz adatokat lemezen**. Meg kell hozzáadása és/vagy csatolni egy vagy több virtuális merevlemezen, hogy tárolja az adatokat. Amikor létrehoz egy Azure virtuális gép, rajta egy az operációs rendszer, a C meghajtó megfeleltetve és a D meghajtó rendelve egy ideiglenes lemezzel. Ne használja a D meghajtó adatait tárolja. Foglalja, csak az ideiglenes tárolási biztosít. Is kínál nincs redundancia vagy a biztonsági másolat, mert azt Azure tároló nem találhatók.


##<a name="Provision"></a>Az Azure klasszikus portál csatlakozzon, és az SQL Server virtuális gép kiépítése

1.  Az [Azure klasszikus portált](http://manage.windowsazure.com/) a fiók használatával jelentkezzen be.
    Azure fiókja nem rendelkezik, látogasson el a [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

2.  Az Azure klasszikus portálon, a weblap bal alsó válassza a **+ Új**, **számítja ki**, kattintson a **virtuális gép**, gombmenü **FROM GYŰJTEMÉNY**.

3.  A **virtuális gép létrehozása** lapon válassza ki a virtuális gép képet tartalmazó SQL Server-adatok szükségletek, és kattintson az oldal alsó sarkában a következő nyílra. A támogatott SQL Server Azure képeket a legfrissebb információkért témakörben [Az Azure virtuális gépeken futó SQL Server – első lépések](http://go.microsoft.com/fwlink/p/?LinkId=294720) [Az Azure virtuális gépeken futó SQL Server](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentációjában beállítása.

    ![Az SQL Server virtuális][1]

4.  Az első **Virtuális gép beállítása** lapon adja meg a következő adatokat:

    -   Adja meg a **virtuális számítógép nevét**.
    -   **Új felhasználónév** mezőbe írja be a virtuális helyi rendszergazdafiók egyedi felhasználónevét.
    -   Az **Új jelszó** mezőbe írja be erős jelszóval. További tudnivalókért olvassa el a [Erős jelszavak](http://msdn.microsoft.com/library/ms161962.aspx)című témakört.
    -   A **Jelszó megerősítése** mezőbe írja be újra a jelszót.
    -   A legördülő listában válassza ki a megfelelő **MÉRETET** .

     > [AZURE.NOTE] A virtuális gép méretének során kiépítési megadott: A2 mérete legkisebb ajánlott a gyártási munkaterhelésekből. A minimális ajánlott virtuális géphez mérete A3 SQL Server Enterprise Edition használata esetén. Jelölje be a A3 vagy újabb, SQL Server Enterprise Edition használata esetén. Jelölje ki a A4 használatakor az SQL Server 2012 és 2014-es vállalati optimalizált tranzakció alapú Munkaterhelésekből képekhez.
Jelölje ki a A7 használatakor az SQL Server 2012 és 2014-es vállalati optimalizált az adatok raktározással Munkaterhelésekből képek. A kijelölt vonatkozó adatok lemezt beállíthatja számát. A rendelkezésre álló virtuális számítógép méretű és a szám, amely egy virtuális géphez csatolhat adatok lemezt a legfrissebb tudnivalókért lásd [Az Azure virtuális gép méretű](http://msdn.microsoft.com/library/azure/dn197896.aspx). Árinformációkat, [Virtuális Macines árak](https://azure.microsoft.com/pricing/details/virtual-machines/)talál.

    Kattintson a jobb alsó továbbra is tovább nyílra.

    ![Virtuális konfigurálása][2]

5.  A második oldalon **virtuális gép beállításainak** konfigurálása a hálózat, tárolására és elérhetőség erőforrások:

    -   A **Felhőbeli szolgáltatástól** mezőben válassza a **Létrehozás egy új, felhőalapú szolgáltatásba**lehetőséget.
    -   A **Felhőalapú szolgáltatás DNS neve** mezőben adja meg az lehetőség, a DNS-név első részét, úgy, hogy egy nevet a formátumban **TESTNAME.cloudapp.net** befejezése
    -   A **Terület és AFFINITÁS csoport/virtuális hálózati** mezőbe egy területet, ahol kell elhelyezni a virtuális kép kijelölése
    -   Jelöljön ki egy meglévő tárterület-fiókot a **Tárterület-fiókot**, vagy jelölje ki az automatikusan generált.
    -   A **ELÉRHETŐSÉGÉNEK beállítása** párbeszédpanelen jelölje be a **(nincs)**.
    -   Olvassa el és fogadja el a árak információkat.

6.  A **VÉGPONTOK** csoportban kattintson az üres legördülő menü a **név**mezőben, és válassza a **MSSQL** , majd írja be a port száma a adatbázismotort (**1433** az alapértelmezett példány) példányának.

7.  Az SQL Server virtuális is szolgálhat egy IPython jegyzetfüzet kiszolgálóhoz, amely a leendő konfigurálja a rendszer.
    Adja hozzá az új végpont megadása a IPython jegyzetfüzet kiszolgáló használni a port. Írjon be egy nevet a **név** oszlopban, jelölje be a nyilvános port választási, és a személyes port 9999 port száma.

    Kattintson a jobb alsó továbbra is tovább nyílra.

    ![Jelölje ki a MSSQL és IPython-portok][3]

8.  Fogadja el az alapértelmezett **virtuális telepítése ügynök** beállítás bejelölve, és kattintson a jobb alsó sarkában lévő a varázsló a virtuális kiépítési folyamat befejezéséhez kattintson a pipára.

    `![Virtuális végleges beállításai][4]

9.  Várakozás közben Azure előkészíti a virtuális gépet. Haladjon végig a virtuális gép állapotot várt:

    -   Kezdési (kiépítési)
    -   Leállítva
    -   Kezdési (kiépítési)
    -   Fut (kiépítési)
    -   Fut


##<a name="RemoteDesktop"></a>Nyissa meg a távoli asztali és a telepítés befejezéséhez virtuális gépen

1.  Befejeztével kiépítési kattintson a nevére a virtuális gépen lépjen az IRÁNYÍTÓPULT lapra. A lap alján kattintson a **Csatlakozás**gombra.

2.  Nyissa meg a használatával a Windows távoli asztali program rpd fájlt választva (`%windir%\system32\mstsc.exe`).

3.  A **Windows biztonsági** párbeszédpanelen a szükséges az előző lépésben megadott a helyi rendszergazdafiók jelszavát.
    (Előfordulhat, hogy rákérdez a hitelesítő adatok a virtuális gép ellenőrzésére.)

4.  Az első alkalommal jelentkezik be a virtuális számítógéphez több folyamatok szükség lehet elvégezni, beleértve az asztal, a Windows-frissítések és a Befejezés Windows kezdeti konfigurációs feladatokat (sysprep) beállítása. Windows sysprep befejeződése után az SQL Server telepítése után konfigurációs feladatokat. Az alábbi műveleteket közben befejezésekor jelenhet meg néhány perc késés. `SELECT @@SERVERNAME`Előfordulhat, hogy adnak a helyes nevet mindaddig, amíg az SQL Server telepítése befejeződik, és az SQL Server Management Studio alkalmazásban nem lehet visable kezdőlapján.

Windows távoli asztali virtuális készülék való csatlakozás után működik a virtuális gép bármely más számítógép hasonlóan. Csatlakozás SQL Server az SQL Server Management Studio (a virtuális gépen fut) az alapértelmezett példány a szokásos módon.


##<a name="InstallIPython"></a>Jegyzetfüzet IPython és más támogató eszközök telepítése

Az új SQL Server virtuális egy IPython jegyzetfüzet kiszolgáló lesz, és telepítse a további támogató eszközök, ilyen AzCopy, Azure tároló Intéző, hasznos adatok tudományos Python csomagok és mások konfigurálásához speciális testreszabási parancsfájl kapja meg azt. Telepítése:

- Kattintson a jobb gombbal a **Windows Start** ikonra, majd a **parancssor (rendszergazda)**
- Másolja a következő parancsokat, és illessze be a parancssorablakban.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Amikor a rendszer kéri, adja meg a jegyzetfüzet IPython kiszolgáló lehetőség jelszó.
- A testreszabási parancsfájl automatizálja több telepítés utáni eljárás, amely tartalmazza:
    + Telepítés és beállítás IPython jegyzetfüzet kiszolgáló
    + A Windows tűzfal a korábban létrehozott végpontok megnyitásakor a TCP-portokat:
    + Az SQL Server remote connectivity
    + A jegyzetfüzet IPython kiszolgálói távoli kapcsolat
    + Minta IPython jegyzetfüzetek és a SQL-parancsfájlok beolvasása
    + Le, és hasznos adatok tudományos Python csomagok telepítése
    + A letöltése és telepítése az Azure eszközöket, például a AzCopy és Azure tároló Explorer  
<br>
- Előfordulhat, hogy elérése és IPython jegyzetfüzet futtatása bármilyen böngészőből helyi vagy távoli egy URL-cím használatával `https://<virtual_machine_DNS_name>:<port>`, ahol a virtuális gép kiépítése során a kijelölt IPython nyilvános portot.
- IPython jegyzetfüzet kiszolgáló háttér szolgáltatásként fut, és újraindul automatikusan a virtuális gép újraindítása után.

##<a name="Optional"></a>Adatok lemez csatolni, szükség szerint

Ha a virtuális kép nem tartalmaz adatokat lemezt, azaz kívül a C meghajtó (OS lemez) és (ideiglenes lemezre), a D meghajtó lemezt kell hozzáadnia egy vagy több adatok lemezre tárolja az adatokat. A virtuális képet az SQL Server 2012 SP2 vállalati optimalizálva DataWarehousing Munkaterhelésekből megtalálható az SQL Server-adatok, és jelentkezzen be fájlok további lemezen előre beállított.

 > [AZURE.NOTE] Ne használja a D meghajtó adatait tárolja. Foglalja, csak az ideiglenes tárolási biztosít. Is kínál nincs redundancia vagy a biztonsági másolat, mert azt Azure tároló nem találhatók.

További adatok lemezt csatolásához [csatolása adatok lemezen a Windows virtuális gép hogyan](virtual-machines-windows-classic-attach-disk.md), amely végigvezeti Önt keresztül ismertetett lépésekkel:

1. A virtuális gép korábbi lépésben kiépítve üres lemez(ek) csatolása
2. Az új lemez(ek) a virtuális gépen inicializálni


##<a name="SSMS"></a>Csatlakozás SQL Server Management Studio és a vegyes módú hitelesítés engedélyezése

Az SQL Server adatbázismotor nem használhatja a Windows-hitelesítés nélkül tartomány környezetben. A adatbázismotor csatlakoztatása egy másik számítógépről, SQL Server-vegyes módú hitelesítés konfigurálása. Vegyes módú hitelesítés lehetővé teszi az SQL Server-hitelesítés és a Windows-hitelesítés használatával. SQL-hitelesítési módban való adatokat közvetlenül az SQL Server virtuális adatbázisok modulról az adatok importálása az [Azure gépi tanulási Studio](https://studio.azureml.net) ingest szükséges.

1.  Távoli asztali használatával csatlakozik a virtuális gép, miközben a Windows **Search** ablaktáblán, és írja be az **SQL Server Management Studio** (SMSS). Ide kattintva elindíthatja az SQL Server Management Studio (SSMS). Előfordulhat, hogy szeretne egy helyi SSMS későbbi felhasználás céljából az asztalon.

    ![Indítsa el a SSMS][5]

    Nyissa meg a Management Studio először, létre kell hoznia a felhasználók Management Studio környezetben. Ez eltarthat egy darabig.

2.  A megnyitásakor, Management Studio jeleníti meg a **Csatlakozás a kiszolgálóhoz** párbeszédpanelen. A **kiszolgáló neve** mezőbe írja be a virtuális gépen való csatlakozáshoz a adatbázismotor az objektum Intézővel nevét.
    (A virtuális számítógépnév helyett is használhatja **(helyi)** vagy egy vesszőt a **kiszolgáló nevét**. Jelölje be a **Windows-hitelesítés**, és hagyja ** *a\_virtuális\_neve*\\a\_helyi\_rendszergazda** a **felhasználónév** mezőbe. Kattintson a **Csatlakozás**gombra.

    ![Csatlakozás a kiszolgálóhoz][6]

    <br>

     > [AZURE.TIP] Az SQL Server hitelesítési mód rendszerleíró kulcs megváltoztatása Windows vagy az SQL Server Management Studio segítségével módosíthatja. Hitelesítési mód használata a beállításjegyzék kulcs módosítása módosításához indítsa el az **Új lekérdezést** , és hajtsa végre a következő parancsfájlt:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    A hitelesítési módot az SQL Server management Studio segítségével módosítása:

3.  Az **SQL Server Management Studio objektum Explorer**kattintson a jobb gombbal az SQL Server (a virtuális számítógépnév)-példány nevét, és válassza a **Tulajdonságok parancsot**.

    ![Kiszolgáló tulajdonságai][7]

4.  A **Biztonság** lapon **kiszolgálói hitelesítési**csoportban jelölje be **az SQL Server és a Windows-hitelesítési módban**, és kattintson **az OK**gombra.

    ![Jelölje ki a hitelesítési mód][8]

5.  **SQL Server Management Studio** párbeszédpanelen kattintson az **OK gombra** , elismerjük az a követelmény, indítsa újra az SQL Server.

6.  Az **Objektum Explorer**kattintson a jobb gombbal a kiszolgáló, és kattintson az **Újraindítás**gombra. (Ha az SQL Server Agent fut, akkor is újra kell indítani.)

    ![Indítsa újra a][9]

7.  Az **SQL Server Management Studio** párbeszédpanel azzal, hogy szeretné-e az SQL Server újraindítani az **Igen** gombra.

##<a name="Logins"></a>SQL Server-hitelesítés bejelentkezési létrehozása

Csatlakozás a adatbázismotor másik számítógépről, létre kell hoznia legalább egy SQL Server-hitelesítés bejelentkezési.  

Új SQL Server bejelentkezési programozás útján is létrehozása és az SQL Server Management Studio használatával. Hozzon létre egy új rendszergazdák felhasználót tartalmazó SQL-hitelesítés a programozás útján, indítsa el az **Új lekérdezést** , és hajtsa végre a következő parancsfájlt. Csere < Új felhasználó nevét\> és < új jelszót\> a választható *felhasználónevet* és *jelszót*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


(A minta kód kikapcsolja azokat a házirend ellenőrzése és a jelszó lejáratának) szükség szerint módosítsa a a házirendet. Többet szeretne tudni az SQL Server bejelentkezések a [bejelentkezési létrehozása](http://msdn.microsoft.com/library/aa337562.aspx)című témakör tartalmaz.  

Az SQL Server Management Studio segítségével új SQL Server bejelentkezési létrehozása:

1.  Az **SQL Server Management Studio objektum Explorer**bontsa ki a mappát, amelyben létre szeretne hozni az új bejelentkezési server-példány.

2.  Kattintson a jobb gombbal a **biztonsági** mappát, mutasson az **Új**, és válassza a **Bejelentkezés...**.

    ![Új bejelentkezési][10]

3.  Írja be a **bejelentkezési név** mezőbe az új felhasználó nevét a **bejelentkezési – új** párbeszédpanel **Általános** lapján.

4.  Jelölje ki az **SQL Server-hitelesítés**.

5.  A **jelszó** mezőbe írjon be egy jelszót az új felhasználó. Adja meg, hogy a jelszó ismét a **Jelszó megerősítése** mezőbe.

6.  A hivatkozási jelszó házirend-beállítások összetettsége és végrehajtására, jelölje be a **érvényesítése jelszóházirend** (ajánlott). Ez a beállítás alapértelmezett SQL Server-hitelesítés ki van jelölve.

7.  A hivatkozási jelszólejárati házirend jelszóbeállításokat, jelölje be a **érvényesítése jelszólejárati** (ajánlott). A hivatkozási jelszóházirend meg kell adni ahhoz, hogy ezt a jelölőnégyzetet. Ez a beállítás alapértelmezett SQL Server-hitelesítés ki van jelölve.

8.  Kényszeríthet ki a felhasználót, hogy hozzon létre egy új jelszót az első alkalommal használja a bejelentkezés után, jelölje be a **felhasználónak meg kell változtatnia a következő bejelentkezéskor jelszó** (Ha a bejelentkezési másvalaki nevében használata ajánlott. Ha a bejelentkezési Ön tervezi használni, nem ezzel a beállítással.) A hivatkozási jelszólejárati meg kell adni ahhoz, hogy ezt a jelölőnégyzetet. Ez a beállítás alapértelmezett SQL Server-hitelesítés ki van jelölve.

9.  **Alapértelmezett adatbázis** listában jelölje ki a bejelentkezés egy alapértelmezett adatbázist. **fő** az alapértelmezett érték lehetőséget. Ha még nem hozott létre felhasználói adatbázis, hagyja meg a **fő**.

10. Az **alapértelmezett nyelv** listában hagyja **alapértelmezett** értékként.

    ![Bejelentkezés tulajdonságai][11]

11. Ha az első bejelentkezés hoz létre, érdemes a bejelentkezési kijelöl egy SQL Server-rendszergazdaként. Ha igen, a **Kiszolgálói szerepkörök** lapon a **rendszergazdák**ellenőrzése.

    > [AZURE.IMPORTANT] A rendszergazdák rögzített kiszolgálói szerepkör tagjai a adatbázismotor teljes hozzáféréssel rendelkezik. Biztonsági okokból gondosan korlátozni a szerepkör tagsága.

    ![Rendszergazdák][12]

12. Kattintson az OK gombra.

##<a name="DNS"></a>A virtuális gép DNS nevének megállapításához

Az SQL Server adatbázismotor csatlakoztatása egy másik számítógépről, ismernie kell a virtuális gép tartománynév (DNS) nevét.

(Ez az a név, az interneten azonosítja a virtuális gépet használ. Az IP-címét is használhatja, de amikor Azure helyezi a redundancia vagy karbantartásához erőforrások megváltozik az IP-címét. A tartománynév lesz stabil, mert egy új IP-cím átirányítható.)

1.  A klasszikus Azure-portálon (vagy az előző lépést) jelölje ki a **virtuális gépeken FUTÓ**.

2.  A **DNS-név** oszlopban a **Virtuális gép példányok** lapon keresse meg és másolja a virtuális gép, amely akkor jelenik meg a **http://**előzi DNS-nevét. (A felhasználói felület esetleg jelennek meg a teljes nevét, de kattintson rá a jobb gombbal, és válassza a másolás.)

##<a name="cde"></a>Csatlakozás a adatbázismotor másik számítógépről

1.  A számítógép csatlakozik az internethez nyissa meg az SQL Server Management Studio eszközben.

2.  **Csatlakozás a kiszolgálóhoz** vagy **adatbázismotort csatlakozás** párbeszédpanel **kiszolgáló neve** mezőjében adja meg a virtuális gép (határozza meg az előző tevékenységre) és a nyilvános végpont port száma formátumban *Dns_név, port_száma* például **tutorialtestVM.cloudapp.net,57500**DNS-nevét.

3.  A **hitelesítés** mezőben jelölje ki az **SQL Server-hitelesítés**.

4.  A **bejelentkezési** mezőbe írja be egy Ön által létrehozott korábbi tevékenység bejelentkezési nevét.

5.  A **jelszó** mezőbe írja be a bejelentkezési korábbi tevékenység létrehozott jelszót.

6.  Kattintson a **Csatlakozás**gombra.

##<a name="amlconnect"></a>Kapcsolódás a adatbázismotor a Azure gépi tanulási

A csoportwebhely adatok tudományos folyamat lépései későbbi fogja használni a [Azure gépi tanulási Studio](https://studio.azureml.net) létre és helyezhetnek üzembe a gépi tanulási modellek. Az SQL Server virtuális adatbázisból származó adatok ingest közvetlenül Azure gépi tanulási oktatás vagy pontozási, amellyel az **Adatok importálása** modul egy új [Azure gépi tanulási Studio](https://studio.azureml.net) kísérlet. Ez a témakör foglalt további részleteket a csapat adatok tudományos folyamat útmutató-kapcsolaton keresztül. Egy – Bevezetés című [Mi az Azure gépi tanulási Studio?](machine-learning-what-is-ml-studio.md).

2.  Az [adatok importálása modul](https://msdn.microsoft.com/library/azure/dn905997.aspx) **Tulajdonságok** ablakában jelölje ki **Azure SQL-adatbázissal** az **Adatforrás** legördülő listából.

3.  **Adatbázis-kiszolgáló neve** mezőbe írja be a`tcp:<DNS name of your virtual machine>,1433`

4.  Írja be az SQL-felhasználó nevét a **kiszolgáló felhasználói fiók neve** mezőbe.

5.  A **kiszolgáló felhasználói fiók jelszavának** szövegmezőben adja meg az sql-felhasználó jelszavát.

    ![Azure Machine Learning adatimportálás][13]

##<a name="shutdown"></a>Leállítás és virtuális gép használaton felszabadítása

Azure virtuális gépeken futó árú szerint **fizet csak akkor használja**. Annak érdekében, hogy, amelyek nem számlázott a virtuális gép nem használatakor, akkor nem lehet **Leállítva (Deallocated)** állapotú.

> [AZURE.NOTE] A virtuális gép leállítása belül (Windows power beállítások használata), a virtuális van állítva, de továbbra is kiosztva. Győződjön meg arról, hogy nem számlázott, mindig leállítása virtuális gépeken futó az [Azure klasszikus portálon](http://manage.windowsazure.com/). A virtuális Powershellen keresztül meg is szüntetheti, hívja fel a "PostShutdownAction" ShutdownRoleOperation "StoppedDeallocated" egyenlő.

Le és felszabadítása a virtuális gépen:

1. Az [Azure klasszikus portált](http://manage.windowsazure.com/) a fiók használatával jelentkezzen be.  

2. Jelölje ki a **virtuális gépeken FUTÓ** a bal oldali navigációs sávján.

3. Virtuális gépeken futó listájában kattintson a virtuális számítógép nevére, majd válassza az **IRÁNYÍTÓPULT** oldalát.

4. A lap alján kattintson a **LEÁLLÍTÁS**gombra.

![Virtuális leállítása][15]

A virtuális gép lesz felszabadítása, de nem törlődnek. Az Azure klasszikus portálról bármikor előfordulhat, hogy indítsa újra a virtuális gépet.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Az Azure SQL Server virtuális használatára: következő lépések?

A virtuális gép ettől kezdve készen áll az adatok tudományos gyakorlatok használni. A virtuális gép egyben Azure gépi tanulási és a csoport adatok tudományos folyamat (TDSP) feltárása és feldolgozás, az adatok és más feladatok együtt egy IPython jegyzetfüzet kiszolgálójával használatra kész.

Az adatok tudományos folyamat a következő lépésekkel a [Csapat adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) van megfeleltetve, és a lépések, amelyek az adatok áthelyezése HDInsight, folyamat és minta ott tanulási az adatok az Azure gépi tanulási előkészítése az alábbiakat tartalmazhatják.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
