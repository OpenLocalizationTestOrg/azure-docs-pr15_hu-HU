<properties
    pageTitle="MongoDB telepítése a Windows virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként MongoDB telepítése egy fut a Windows Server 2012 R2 az erőforrás-kezelő telepítési modell készült Azure virtuális."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Telepítse és állítsa be a MongoDB kattintson egy Windows virtuális Azure-ban
[MongoDB](http://www.mongodb.org) népszerű Megnyitás-forrást, a nagy teljesítményű NoSQL adatbázis. Ez a cikk végigvezeti való telepítéséről és konfigurálásáról MongoDB Windows Server 2012 R2 virtuális gépen (virtuális) Azure-ban. Akkor is [telepítse az Azure-ban egy Linux virtuális a MongoDB](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Előfeltételek

Mielőtt telepítése és konfigurálása MongoDB, kell hozzon létre egy virtuális, és ideális esetben adatok lemezen hozzáadása azt. A virtuális létrehozása és adatok lemezen hozzáadása az alábbi cikkekben talál:

- [A Windows Server virtuális létrehozása az Azure portál használatával](virtual-machines-windows-hero-tutorial.md) , vagy [Hozzon létre egy Windows Server virtuális Azure PowerShell használatával](virtual-machines-windows-ps-create.md)
- [A Windows Server virtuális adatok lemezre csatolása az Azure portál használatával](virtual-machines-windows-attach-disk-portal.md) , vagy [a Windows Server virtuális adatok lemezre csatolása Azure PowerShell használatával](https://msdn.microsoft.com/library/mt603673.aspx)
    
[Jelentkezzen be a Windows Server virtuális](virtual-machines-windows-connect-logon.md) távoli asztali telepítése és beállítása MongoDB megkezdéséhez.


## <a name="install-mongodb"></a>MongoDB telepítése

> [AZURE.IMPORTANT] MongoDB biztonsági funkciókat, például a hitelesítésről és az IP-cím kötés, alapértelmezés szerint nincs engedélyezve vannak. Mielőtt MongoDB munkakörnyezetben engedélyezni kell biztonsági funkciókat. További tudnivalókért olvassa el a [MongoDB biztonsági és hitelesítési](http://www.mongodb.org/display/DOCS/Security+and+Authentication)című témakört.

1. Miután felvette a virtuális távoli asztali változatában, nyissa meg az Internet Explorer a virtuális **Start** menüjében.

2. Válassza a **használata ajánlott a biztonság, adatvédelem és kompatibilitási beállítások** , amikor először nyitja meg az Internet Explorerben, és kattintson az **OK gombra**.

3. Az Internet Explorer fokozott biztonság beállításai alapértelmezés szerint engedélyezve van. A MongoDB webhelyen engedélyezett webhelyek listájának felvétele:

    - Válassza az **eszközök** ikonra a jobb felső sarokban.
    - Az **Internetbeállítások**párbeszédpanelen kattintson a **Biztonság** fülre, és válassza ki a **Megbízható helyek** ikonra.
    - Kattintson a **helyek** gombra. Adja hozzá _https://\*. mongodb.org_ a megbízható helyek, és kattintson az párbeszédpanel bezárása listájára.

    ![Az Internet Explorer biztonsági beállításainak konfigurálása](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Tallózással keresse meg a [Letöltések MongoDB -](http://www.mongodb.org/downloads) lapon (http://www.mongodb.org/downloads).

5. Alapértelmezés szerint akkor jelölje be a **Közösségi kiszolgáló** edition és a legújabb stabil kiadásban a Windows Server 2008 R2 64 bites és a későbbi. A telepítő letöltéséhez kattintson a **letöltése (msi)**elemre.

    ![MongoDB installer letöltése](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    A letöltés befejeződése után a telepítő futtatása

6. Olvassa el és fogadja el a licencszerződést. Amikor a rendszer kéri, kattintson a **kész** telepítés.

7. Az utolsó képernyő kattintson a **telepítés**gombra.


## <a name="configure-the-vm-and-mongodb"></a>A virtuális és MongoDB konfigurálása

1. Az elérési út változók nem frissülnek a MongoDB Installer. A MongoDB nélkül `bin` helyre a path változóban, meg kell adni a teljes elérési útját minden alkalommal, amikor egy MongoDB végrehajtható használja. A hely hozzáadása a path változó:

    - Kattintson a jobb gombbal a **Start** menü, és válassza a **rendszer**.
    - Kattintson a **Speciális rendszer beállításai**elemre, és kattintson a **Környezeti változók**.
    - **Rendszer változói**csoportban jelölje ki az **elérési utat**, és kattintson a **Szerkesztés**gombra.

    ![Elérési út változók konfigurálása](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Az elérési út hozzáadása a MongoDB `bin` mappát. MongoDB általában telepítve van a `C:\Program Files\MongoDB`. Ellenőrizze a telepítés elérési út a virtuális. Az alábbi példa összeadja az alapértelmezett MongoDB telepítése helyét a `PATH` változó:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Vegye fel a vezető pontosvesszőt (`;`) jelzi, hogy hová szeretné hozzáadni a `PATH` változó.

2. Az adatok lemezen MongoDB adatokat, és jelentkezzen be könyvtárak létrehozása A **Start** menüből válassza a **parancssor parancsot**. Az alábbi példák létrehozása a könyvtárak meghajtón f

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Egy MongoDB példány kezdetben a következő parancsot, és az adatok elérési útja beállítása, és könyvtárak megfelelően jelentkezzen:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Eltarthat néhány percig, amíg a napló fájlokat lefoglalhat és figyeli a kapcsolatok MongoDB. Log üzenetekhez irányítja át a *F:\MongoLogs\mongolog.log* fájlt `mongod.exe` kiszolgáló elindul, és lapjának fájlok osztja ki.

    > [AZURE.NOTE] A parancssor ezt a feladatot a fókuszban lévő maradjon, a MongoDB példány futása közben. Hagyja nyitva, MongoDB folytatja a parancssorablakba. Vagy MongoDB telepítése szolgáltatásként, a következő lépésben ismertetett módon.

4. Megbízhatóbb MongoDB változat, telepítse a `mongod.exe` szolgáltatásként. Szolgáltatás létrehozása azt jelenti, hogy nem kell minden alkalommal, amikor MongoDB használni kívánt futó parancssor hagyja. Hozzon létre a szolgáltatást az alábbiak szerint elérési útját az adatokat, és jelentkezzen be könyvtárak megfelelően beállítása:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Az előző parancs szolgáltatást hoz létre MongoDB, nevű "Mongo DB" leírását. A következő paramétereket is meg vannak adva:

    - A `--dbpath` lehetőséget, adja meg a adatkönyvtárának helyét.
    - A `--logpath` beállítást kell használni adjon meg egy naplófájlban, mivel a futó szolgáltatás nem egy parancsablakban kimeneti megjelenítéséhez.
    - A `--logappend` beállítással megadhatja, hogy újra kell indítani a szolgáltatás megjeleníti a kimeneti hozzáfűzése a meglévő naplófájl.

  A MongoDB szolgáltatásba kezdéséhez futtassa a következő parancsot:

    ```
    net start MongoDB
    ```

    A MongoDB szolgáltatás létrehozásával kapcsolatos további tudnivalókért olvassa el a [MongoDB Windows szolgáltatás beállítása](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service)című témakört.

## <a name="test-the-mongodb-instance"></a>A MongoDB példányának tesztelése

Egyetlen példánya fut, vagy a szolgáltatásként telepített MongoDB most indíthat létrehozásával és az adatbázisok használatával. Indítsa el a MongoDB felügyeleti rendszerhéj, nyisson meg egy másik parancssorablakot a **Start** menüből, és írja be a következő parancsot:

```
mongo  
```

Jeleníthet meg az adatbázis a `db` parancsot. Bizonyos adatok szúrja be a következőképpen:

```
db.foo.insert( { a : 1 } )
```

Adatok keresése az alábbi képlettel történik:

```
db.foo.find()
```

A kimenet szövege a következőhöz hasonló:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Kilépés a `mongo` konzol, az alábbi képlettel történik:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Állítsa be a tűzfalat, és a hálózati biztonsági csoport szabályai
Most, hogy telepítve van, és olyan portot fut, nyissa meg a Windows tűzfal MongoDB így távolról csatlakozhat MongoDB. Olyan portot 27017 engedélyezése új bejövő szabályt szeretne létrehozni, nyissa meg a felügyeleti PowerShell-parancssorában, és írja be a következő parancsot:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

A **Fokozott biztonságú Windows tűzfal** grafikus felügyeleti eszköz használatával is létrehozhat a szabályt. Engedélyezi a TCP-port 27017 bejövő új szabályt létrehozni.

Ha szükséges, a meglévő Azure virtuális hálózati alhálózat kívül a MongoDB való hozzáférés engedélyezése hálózati biztonsági csoport szabály létrehozása. A hálózati biztonsági csoport szabályokat az [Azure portálja](virtual-machines-windows-nsg-quickstart-portal.md) vagy az [Azure PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md)használatával hozhat létre. A Windows tűzfal szabályokat a TCP-porthoz 27017 a virtuális hálózati felületén a MongoDB virtuális engedélyezése

> [AZURE.NOTE] TCP 27017 portja MongoDB által használt alapértelmezett portja. A port használatával módosíthatja a `--port` paraméter indításakor `mongod.exe` manuálisan, vagy egy szolgáltatást. Ha módosítja a port, feltétlenül frissítse az előző lépéseket a Windows tűzfal és a hálózati biztonsági csoport szabályokat.


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban telepítése és beállítása a Windows virtuális MongoDB megtanulta azt. Most már hozzáférhet MongoDB meg a Windows virtuális követve a Speciális témakörök [MongoDB dokumentációt](https://docs.mongodb.com/manual/).
