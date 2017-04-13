<properties
pageTitle="A megosztott Access aláírások használata adatok HDInsight hozzáférés korlátozása"
description="Megtudhatja, hogy miként megosztott Access-aláírások használata tároló Azure BLOB tárolt adatok HDInsight-elérésének korlátozására."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Azure tároló megosztott Access aláírások segítségével HDInsight az adatokhoz való hozzáférés korlátozása

Adattárolás tároló Azure BLOB HDInsight használja. HDInsight az alapértelmezett tárolására szolgál a fürt blob teljes hozzáféréssel kell rendelkeznie, míg más BLOB a fürt által használt tárolt adatok engedélyekkel korlátozhatja. Például érdemes, hogy bizonyos adatok, csak olvasható. Ezt megteheti a megosztott Access aláírások használata.

Megosztott Access aláírások (Társítások) Azure tároló fiókok szolgáltatása, amely lehetővé teszi, hogy az adatokhoz való hozzáférés korlátozása. Ha például nyújtó csak olvasható hozzáférés az adatokhoz. A dokumentumban megtanulhatja, hogyan Társítások segítségével olvasási és csak lista hozzáférés engedélyezése blob-tárolóhoz hdinsight.

##<a name="requirements"></a>Követelmények

* Az Azure előfizetéssel

* C# vagy Python. Példa C# kód Visual Studio megoldásként áll rendelkezésre.

    * Visual Studio 2013 vagy a Skype 2015 verzióját kell lennie.
    
    * Python 2.7 vagy újabb verzióját kell lennie.

* Egy Linux-alapú HDInsight fürt vagy [Azure PowerShell] [ powershell] – Ha egy meglévő Linux-alapú fürthöz, használhatja Ambari megosztott Access aláírás hozzáadása a fürt. Ha nem, Azure PowerShell használatával új fürt aláírás létrehozása és hozzáadása egy megosztott Access fürt létrehozása során.

* Példa fájljait [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Ez a tár magában a következőket:

    * Visual Studio projekt, a tárhely tároló tárolt házirend és Társítások hozhat létre, HDInsight való használatra
    
    * Egy Python parancsfájlt, amely a tárhely tároló tárolt házirend és Társítások hozhat létre, HDInsight való használatra
    
    * Egy PowerShell-parancsprogramot, amely a hozzon létre egy új HDInsight fürt és konfigurálja úgy, hogy a Társítások használja.

##<a name="shared-access-signatures"></a>Megosztott Access aláírások

Van két űrlap az Access aláírások megosztott:

* Alkalmi: A kezdési időpontot, lejárati idő és a Társítások engedélyeinek olyan minden megadott az Társítások URI (vagy hallgatólagos, abban az esetben, ha elhagyják a kezdő időpont).

* Hozzáférési házirend tárolt: tárolt hozzáférési házirend - blob-tárolóhoz, tábla, várólista vagy fájlmegosztás - erőforrás tároló meg, és egy vagy több átengedése aláírások korlátozásokkal kezelésére használható. A Társítások társítása egy tárolt hozzáférési házirendet, a Társítások örökölnek kényszerek – a kezdési időpontot, a lejárati idő és a engedélyek – a tárolt hozzáférési házirend.

A két űrlapok közötti különbség egy fő forgatókönyvet fontos: visszavonási. A Társítások URL-címet,, hogy bárki, aki beolvassa a Társítások felhasználhassa, függetlenül attól, akik mintáját rajta. Ha egy Társítások nyilvánosságra, bárki a világon a használható. A felosztott Társítások nem érvényes, amíg a négy dolog egyike történik:

1. A lejárati idő a Társítások megadott eléréséig.

2. A lejárati idő a tárolt hozzáférési házirend a Társítások által hivatkozott megadott elérésekor (Ha a tárolt hozzáférési házirend hivatkozott, és adja meg a lejárati idő). Ez akkor fordulhat vagy elő, mert az időtartam, vagy módosította az tárolt hozzáférési házirendet, hogy egy lejárati idő múltbeli, amelynek vissza szeretné vonni a Társítások egy lehetőség van.

3. A tárolt hozzáférési házirend a Társítások által hivatkozott törölni, amely olyan, ha vissza szeretné vonni a Társítások úgy. Figyelje meg, hogy akkor hozza létre újból a tárolt hozzáférési házirend pontosan azonos nevű, ha minden meglévő Társítások tokenek újra lesz érvényes szerint (feltételezve, hogy, amely a lejárati idő a Társítások nem ment) tárolt access házirendhez társított engedélyek. Szándékozik visszavonása a Társítások, ne használjon másik nevet, ha a hozzáférési házirend-lejárati idő a jövőben hozza létre újból a.

4. A fiókkulcs a Társítások létrehozása használt jön. Figyelje meg, hogy ennek hatására az összes alkalmazás-összetevők, hogy a fiókkulcs használatával nem sikerül hitelesíteni, amíg nem használja az egyéb érvényes fiókkulcs vagy az újonnan regenerált fiókkulcs frissítve lesznek.

> [AZURE.IMPORTANT] Megosztott access aláírás URI társítva a fiókkulcs az aláírás létrehozásához használt, és a társított tárolt hozzáférési házirend (ha vannak ilyenek). Ha nincs tárolt hozzáférési házirend van megadva, csak úgy lehet visszavonni átengedése aláírás a fiókkulcs módosítása. 

Ajánlott mindig használata az access tárolt házirendek, így aláírások visszavonása vagy kiterjesztése a lejárati dátum, szükség szerint. A dokumentumok használata című témakör lépéseit a Társítások létrehozása access-házirendek tárolja.

További információt a megosztott Access aláírások olvassa el a [a Társítások modell ismertetése](../storage/storage-dotnet-shared-access-signature-part-1.md)című témakört.

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Hozzon létre egy tárolt házirendet, és a Társítások létrehozása

Jelenleg a tárolt házirendet programozás útján kell létrehoznia. A C# és hozzon létre egy tárolt házirendek és Társítások [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)Python példát talál.

###<a name="create-a-stored-policy-and-sas-using-c"></a>Tárolt házirend és C használatával Társítások létrehozása\#

1. A Visual Studio alkalmazásban nyissa meg azt a megoldást.

2. A megoldás Intézőben kattintson a jobb gombbal a __SASToken__ projekten, és válassza a __Tulajdonságok parancsot__.

3. Válassza a __Beállítások__ , és adja hozzá a következő tételek értékeket:

    * StorageConnectionString: A kapcsolati karakterlánc a tárterület-fiókom tárolt házirend és Társítások a létrehozni kívánt. A következő formátumban kell megadni `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` hol `myaccount` a tárterület-fiók neve és `mykey` kulcsa a tárterület-fiókot.
    
    * ContainerName: A tárterület-fiókot való hozzáférés korlátozása a tároló.
    
    * SASPolicyName: A nevet, létrejön tárolt házirend.
    
    * FileToUpload: A tároló feltöltendő fájl elérési.
    
4. A projekt futtatásához. Ekkor megjelenik egy konzol ablak, és a következőhöz hasonló adatokat fog megjelenni, miután a Társítások hozott létre:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Mentse a Társítások házirend jogkivonat, lesz szükség szerint meg, amikor a tárterület-fiók társítása a HDInsight fürt. A tárterület-fiók nevét, és a tároló nevét is lesz szüksége.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Tárolt házirend és Python használatával Társítások létrehozása

1. Nyissa meg a SASToken.py fájlt, és módosítsa az alábbi értékeket:

    * házirend\_neve: létrejön tárolt házirend használni kívánt nevet.
    
    * tárterület\_fiók\_neve: a tárterület-fiókja nevét.
    
    * tárterület\_fiók\_kulcs: a tárterület-fiókom a billentyűt.
    
    * tárterület\_tároló\_neve: a tárterület-fiókot való hozzáférés korlátozása a tároló.
    
    * Példa\_fájl\_elérési útja: a tároló feltöltendő fájl elérési útja

2. Futtassa a. A parancsprogram befejeztével jeleníti meg a biztonsági jogkivonat, az alábbihoz hasonló:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Mentse a Társítások házirend jogkivonat, lesz szükség szerint meg, amikor a tárterület-fiók társítása a HDInsight fürt. A tárterület-fiók nevét, és a tároló nevét is lesz szüksége.
    
##<a name="use-the-sas-with-hdinsight"></a>A Társítások használata hdinsight szolgáltatáshoz

Egy HDInsight fürthöz létrehozásakor meg kell adnia egy elsődleges tárterület-fiókot, és tetszés szerint adjon meg további tárterület-fiókok. Mindkét módszer tárhely hozzáadása a teljes hozzáférés a tárterület-fiókok és tárolók használt szükség.

A megosztott Access aláírás használatához tároló való hozzáférés korlátozása, hozzá kell adnia egy egyéni tétel az __alapvető-webhely__ konfigurálása a fürt.

* A __Windows-alapú__ vagy __Linux-alapú__ HDInsight fürt ezt megteheti a PowerShell használatával fürt létrehozása során.

* __Linux-alapú__ HDInsight fürtre vonatkozóan a konfiguráció használata Ambari fürt létrehozását követően módosíthatja.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Új fürt, amely használja a Társítások létrehozása

Példa egy HDInsight fürt, amely használja a Társítások létrehozása szerepel a `CreateCluster` a tárat a címtár. Ahhoz, hogy használhassa, kövesse az alábbi lépéseket:

1. Nyissa meg a `CreateCluster\HDInsightSAS.ps1` fájl egyszerű szövegszerkesztőben, és módosítsa az alábbi értékeket, a dokumentum elejére.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Például módosítása `'mycluster'` szeretne létrehozni a csoport nevére. A tárhely és a biztonsági jogkivonat létrehozásakor Társítások értékeket egyeznie kell az előző lépéseket értékeit.
    
    Miután módosította az értékeket, akkor mentse a fájlt.

1. Nyissa meg egy új Azure PowerShell-parancssorában. Ha nem ismeri Azure PowerShell, vagy azt nem telepítette, olvassa el a [Telepítse és állítsa be a Azure PowerShell][powershell].

2. A parancssorból a következő segítségével Azure-előfizetéséhez hitelesíteni:

        Login-AzureRmAccount
    
    Amikor a rendszer kéri, jelentkezzen be a fiók Azure előfizetéshez tartozó.
    
    Ha több Azure előfizetéssel társított bejelentkezési adatait, szükség lehet használni `Select-AzureRmSubscription` jelölje ki a használni kívánt előfizetést.

2. A parancssorból könyvtárak módosítása a `CreateCluster` HDInsightSAS.ps1 fájlt tartalmazó címtár. A következő használatával futtatása
        
        .\HDInsightSAS.ps1
    
    A parancsfájl fut, mint azt fogja jelentkezzen kimeneti PowerShell-parancssorában csoport és a tárterület-fiókok létrehozása az erőforrás szerint. Ezután kérni fogja a HTTP-felhasználó megadása a HDInsight fürt. Ez az a felhasználói fiókot, amellyel biztonságossá a HTTP/s hozzáférést a fürthöz.
    
    A Linux-alapú fürtre hoz létre, ha Ön is kéri egy SSH felhasználói fiók nevét és jelszavát. Ez a fürt távolról bejelentkezési szolgál.
    
    > [AZURE.IMPORTANT] Amikor a rendszer kéri a HTTP/s vagy SSH felhasználónevét és jelszavát, meg kell adnia egy jelszót, amely megfelel az alábbi feltételek:
    >
    > - Legalább 10 karakter hosszú kell lennie.
    > - Tartalmaznia kell legalább egy számjegyet
    > - Tartalmaznia kell legalább egy nem alfanumerikus karaktert
    > - Tartalmaznia kell legalább egy felső vagy kis


Megnyílik egy ideig tartson, általában körülbelül 15 percet Ez parancsfájl. A parancsprogram befejeztével hibák nélkül a fürt létrehozva.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Frissítés a Társítások használni egy meglévő fürthöz

Ha egy meglévő Linux-alapú fürthöz, az alábbi lépésekkel adhat a Társítások a __fő-webhely__ konfigurálása:

1. Nyissa meg a fürt felhasználói felület Ambari webhelyet. Ez a lap címét https://YOURCLUSTERNAME.azurehdinsight.net. Amikor a rendszer kéri, hitelesíteni a fürthöz a rendszergazda nevét (rendszergazda), és a csoport létrehozásakor használt jelszót használja.

2. A felhasználói felület Ambari webes bal oldalán kattintson a __Fájlrendszerhez__ , és válassza a a __konfigurációk__ fülre a lap közepén.

3. Kattintson a __Speciális__ fülre, és majd görgessen, amíg meg nem találja az __egyéni core-webhelyhez__ szakaszban.

4. Bontsa ki az __egyéni core-webhely__ csoportban görgessen le a befejezési és elemet, majd a __... tulajdonság hozzáadása__ hivatkozásra. A __kulcs__ és __érték__ mezők a következő értékeket használja:

    * __Kulcs__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Érték__: A Társítások az előzőleg futtatta C# vagy Python alkalmazás által visszaadott
    
    __CONTAINERNAME__ cserélje le a C# vagy Társítások alkalmazást használta tároló nevére. Cserélje le __STORAGEACCOUNTNAME__ használt tárterület fiók nevére.

5. Kattintson a kulcs és érték mentése a __Hozzáadás__ gombra, majd a __Mentés__ gombra kattintva mentse a módosításokat. Amikor a rendszer kéri, adjon meg egy leírást a változás ("hozzáadása Társítások tároló access" például), és kattintson a __Mentés__gombra.

    A módosítások elvégzése után kattintson az __OK gombra__ .

    > [AZURE.IMPORTANT] Ezzel menti a módosításokat, de mielőtt a módosítás életbe léptetéséhez újra kell indítani több szolgáltatásokat.

6. A felhasználói felület Ambari webes __Fájlrendszerhez__ a bal oldali listában válassza ki, és válassza a __Indítsa újra az összes__ lehetőséget a __Szolgáltatás műveletek__ legördülő listában a jobb oldalon. Amikor a rendszer kéri, jelölje be a __karbantartási mód bekapcsolása__ és majd jelölje ki __Conform indítsa újra az összes".

    Ismételje meg ezt a folyamatot a MapReduce2 és fonal bejegyzések a listából, kattintson a lap bal szélén.

7. Ezek újra kell indítani, jelölje ki egyenként, és tiltsa le a __Szolgáltatás műveletek__ a karbantartási mód legördülő listája.

##<a name="test-restricted-access"></a>A hozzáférés-korlátozás tesztelése

Ha ellenőrizni szeretné, hogy korlátozta a hozzáférést, tegye az alábbiakat:

* A __Windows-alapú__ HDInsight fürt távoli asztali segítségével csatlakozzon a fürthöz. Lásd: [tudott kapcsolatot létesíteni a HDInsight RDP használatáról](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) további információt.

    Miután létrejött, használatával a __Hadoop parancssor__ ikonra az asztal nyisson meg egy parancssort.

* A HDInsight fürt __Linux-alapú__ SSH segítségével csatlakozzon a fürthöz. SSH használata Linux-alapú fürt látható információt az alábbiak egyikét:

    * [A HDInsight Linux OS X és Unix Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Miután létrejött a fürthöz, kövesse az alábbi lépéseket, ellenőrizze, hogy csak olvasható és a lista elemet a Társítások tároló fiók is:

1. A megjelenő kérdésnél írja be a következőt. __SASCONTAINER__ cserélje le a tároló a Társítások tároló fiók létrehozásához nevét. __SASACCOUNTNAME__ cserélje le a Társítások használt tárterület-fiók nevére:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Ez a felsorolásban, amelyben szerepelnie kell a fájl feltöltése a tároló tartalmát a tároló és Társítások létrehozásának ideje.
    
2. A következő segítségével ellenőrizze, hogy erről a fájl tartalmát. Ahogy az előző lépésben a __SASCONTAINER__ és __SASACCOUNTNAME__ cseréje __Fájlnév__ cserélje ki az előző parancs jelenik meg a fájl neve:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Ez a fájl tartalmát a felsorolásban.
    
3. A következő segítségével töltse le a fájlt a helyi fájlrendszerben:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    A fájl letöltése __Példa.txt__nevű helyi fájlként.

4. A következő segítségével a helyi fájl feltöltése __testupload.txt__ a Társítások tárolón nevű új fájl:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Az alábbihoz hasonló üzenetet kap:
    
        put: java.io.IOException
        
    Ez a hiba oka az, hogy a tárolási helye olvasási + csak lista. Használja a következő szeretné elhelyezni az adatokat a fürt írható alapértelmezett tárolón:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Ebben az esetben a művelet sikeresen befejeződik.
    
##<a name="troubleshooting"></a>Hibaelhárítás

###<a name="a-task-was-canceled"></a>Tevékenység megszakították.

__A jelenség__: segítségével a PowerShell-parancsprogramot fürt létrehozásakor az alábbi hibaüzenet jelenhet:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__OK__: Ez a hiba akkor fordulhat elő, ha a rendszergazda/HTTP-felhasználó, a fürt, illetve (Linux-alapú fürt,) használ egy jelszót a SSH felhasználó.

__Megoldás__: a következő feltételeknek megfelelő jelszót használja:

- Legalább 10 karakter hosszú kell lennie.
- Tartalmaznia kell legalább egy számjegyet
- Tartalmaznia kell legalább egy nem alfanumerikus karaktert
- Tartalmaznia kell legalább egy felső vagy kis

##<a name="next-steps"></a>Következő lépések

Most, hogy a korlátozott hozzáférésű tárhely hozzáadása a HDInsight fürt van megtanulta, további adatokkal végzett munkához a fürt egyéb módjai:

* [HDInsight struktúra használata](hdinsight-use-hive.md)

* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)

* [HDInsight MapReduce használata](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
