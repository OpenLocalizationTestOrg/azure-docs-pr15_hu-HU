<properties
    pageTitle="R Server (előzetes verzió) HDInsight RStudio telepíteni |} Microsoft Azure"
    description="Hogyan telepítheti az RStudio R kiszolgálóval HDInsight (előzetes verzió)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>RStudio telepítése az R kiszolgálón HDInsight (előzetes verzió)

Nincsenek több integrált fejlesztői környezet (IDE) R ma, többek között a legutóbb a Microsoft bejelentve [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), az asztal- és eszközök [RStudio](https://www.rstudio.com/products/rstudio-server/)vagy Walware's Holdas-alapú [StatET](http://www.walware.de/goto/statet)egy család. A legnépszerűbb Linux között van [RStudio kiszolgáló](https://www.rstudio.com/products/rstudio-server/) , amely tartalmaz egy böngészőalapú IDE távoli ügyfelek számára.  RStudio Server telepítése HDInsight prémium fürt él csomóponton Ez a témakör a teljes IDE élmény a fejlesztés és R parancsfájlok végrehajtása R-kiszolgálóval a fürt és lehet jelentős mértékben hatékonyabb, mint R konzol alapértelmezett használata.

Ez a cikk megtanulhatja, hogyan telepítheti a közösségi (ingyenes) verziójának RStudio fürt él csomóponton egyéni parancsfájl használatával. Ha azt szeretné, hogy akár kereskedelmi célra is licencelt Pro verziójának RStudio, [RStudio](https://www.rstudio.com/products/rstudio/download-server/)kiszolgálóról hajtsa végre a telepítési utasításokat.

> [AZURE.NOTE] A lépéseket a jelen dokumentum HDInsight fürt-R-kiszolgálón szükséges, és nem működnek megfelelően, ha egy HDInsight fürthöz használja, ahol k telepítve volt az [R parancsfájl művelet telepítése](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Előfeltételek

* Egy Azure hdinsight szolgáltatáshoz fürthöz telepített R-kiszolgálóval. Útmutatásért lásd: az [első lépések a HDInsight fürt R-kiszolgálón](hdinsight-hadoop-r-server-get-started.md).
* Egy SSH ügyfél. Linux és Unix terjesztését vagy Macintosh-OS X az `ssh` parancs hiányzik az operációs rendszerrel. A Windows azt javasoljuk [Cygwin](http://www.redhat.com/services/custom/cygwin/) [gitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)vagy [OpenSSH lehetőséget](https://www.youtube.com/watch?v=CwYSvvGaiWU).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Az egyéni parancsfájl használatával fürt RStudio telepítése

1. Azonosítsa a fürt a szegély csomópontot. Egy HDInsight fürt R-kiszolgálóval az alábbiakban a használni kívánt névformátumot fő csomópont és él csomópontot.

    * Címsor csomópont-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Biztonsági a csomópont-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. A szegély csomópontot a fürt a fenti elnevezési minta használatával történő SSH. 
 
    * Ha egy Linux ügyfélprogramból csatlakozik, olvassa el a [Csatlakozás a Linux-alapú HDInsight fürtre](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Ha a Windows ügyfélről csatlakozik, olvassa el a [Csatlakozás a Linux-alapú HDInsight fürtre gitt használatával](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Miután csatlakozott, automatikusan a fürt felhasználó legfelső szintű. A következő parancsot használja a SSH munkamenetben.

        sudo su -

4. Töltse le az egyéni parancsfájl RStudio telepítéséhez. A következő parancsot használja.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Módosíthatom az engedélyeket a az egyéni parancsfájl, és futtassa a. A következő parancsokat használhatja.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Ha egy SSH jelszót használt R-kiszolgálóval egy HDInsight fürthöz létrehozásakor, ugorja át ezt a lépést, és folytassa a következő. Ha korábban egy SSH billentyű helyett a fürt létrehozásához, be kell SSH felhasználói jelszót. RStudio való csatlakozáskor szüksége lesz a jelszót. A következő parancsokat. Amikor a rendszer kéri **aktuális Kerberos jelszó**, csak nyomja le az **ENTER BILLENTYŰT**.  Megjegyzés:, ezt kell lecserélnie `USERNAME` a HDInsight fürt egy SSH felhasználóval.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Ha sikeresen beállította a jelszavát, meg kell jelennie egy üzenet jelenik meg.

        passwd: password updated successfully


    Lépjen ki a SSH munkamenetet.

7. Hozzon létre egy SSH alagutas a fürthöz való megfeleltetésével `localhost:8787` a HDInsight fürt az ügyfélgép a. Új böngésző-munkamenetet megnyitása előtt, létre kell hoznia egy SSH alagutas.

    * Egy Linux ügyfél vagy egy Windows ügyfél [Cygwin](http://www.redhat.com/services/custom/cygwin/) , majd nyissa meg a munkamenet, és használja a következő parancsot.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        **Felhasználónév** helyére egy SSH a HDInsight fürt felhasználó, és a csere **CLUSTERNAME** a HDInsight fürt nevű is használhatja a jelszót, hanem egy SSH kulcs hozzáadásával`-i id_rsa_key`     

    * Ha a Windows-ügyfél használata gitt majd

        1.  Nyissa meg a gitt, és adja meg a kapcsolat adatait. Ha nem ismeri a gitt, [Használjon SSH a Linux-alapú Hadoop a HDInsight a Windows](hdinsight-hadoop-linux-use-ssh-windows.md) kapcsolatos információk találhatók használatához a hdinsight szolgáltatásból lehetőségre.
        2.  A párbeszédpanel bal oldalán a **kategória** csoportban bontsa ki a **kapcsolatot**, bontsa ki a **SSH**, és válassza a **alagutak**parancsra.
        3.  Adja meg a **SSH port átirányítás ellenőrzése beállítások** képernyőn az alábbi adatokat:

            * **Forrásport** - továbbítani szeretné az ügyfél portjához. Ha például **8787**.
            * **Címzett** – a cél, amely a helyi ügyfélszámítógépre kell rendelni. Ha például **localhost:8787**.

            ![Egy SSH alagutas létrehozása] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Egy SSH alagutas létrehozása")

        4. Kattintson a **Hozzáadás gombra** , a beállítások gombra, és kattintson a **megnyitni** egy SSH kapcsolat megnyitásához.
        5. Amikor a rendszer kéri, jelentkezzen be a kiszolgáló. Ez egy SSH munkamenetet, és a alagutas engedélyezése.

8. Nyisson meg egy webböngészőt, és írja be a port a alagutas megadott alapján alábbi URL-címet.

        http://localhost:8787/ 

9. Írja be a SSH felhasználónév és jelszó való csatlakozáshoz a fürt kéri. Ha egy SSH kulcsot a fürt létrehozása közben használt, meg kell adnia a a fenti 5 lépésben létrehozott jelszót.

    ![Csatlakozás R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Egy SSH alagutas létrehozása")

10. Ellenőrzéséhez RStudio telepítése sikerült, futtatását is lehetővé teszi a vizsgálat parancsfájlt, R végrehajtja a fürt MapReduce és a külső feladatok alapján. Térjen vissza a SSH konzol, és írja be a töltheti le a próba parancsfájl RStudio futtatásához a következő parancsokat.

    * Ha a Hadoop fürtre R hozta létre, a paranccsal.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Ha egy külső fürt R hozta létre, a paranccsal.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. RStudio látni fogja a próba parancsfájl letöltött. Kattintson duplán a fájlra, nyissa meg, jelölje be a fájl tartalmát, akkor kattintson a **Futtatás**gombra kattintva. Meg kell jelennie a kimenet **konzol** ablakban.
 
    ![A telepítés tesztelése] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "A telepítés tesztelése")

Másik lehetőségként az írja be a lenne `source(testhdi.r)` vagy `source(testhdi_spark.r)` a parancsfájl végrehajtásához.

## <a name="see-also"></a>Lásd még:

- [R-kiszolgáló beállításai helyi HDInsight fürt kiszámítása](hdinsight-hadoop-r-server-compute-contexts.md)

- [HDInsight prémium R Server Azure tárhely beállításai](hdinsight-hadoop-r-server-storage.md)


 
