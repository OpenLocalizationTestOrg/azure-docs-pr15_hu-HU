<properties 
    pageTitle="A csoportwebhely adatok tudományos folyamat Hadoop fürt testreszabása |} Microsoft Azure" 
    description="Népszerű Python modulok egyéni Azure hdinsight szolgáltatáshoz Hadoop fürt elérhető."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>A csoportwebhely adatok tudományos folyamat Azure hdinsight szolgáltatáshoz Hadoop fürt testreszabása 

Ez a cikk ismerteti, hogyan egy HDInsight Hadoop fürthöz testreszabásához telepítése a 64 bites Anaconda (Python 2.7) minden csomóponton, amikor a fürt HDInsight szolgáltatásként már kiépítve. Azt is megtudhatja, hogy miként elérni az egyéni feladatok a fürthöz küldhetik headnode. A Testreszabás ellenőrzi az kényelmesen használható, amelyek feldolgozása struktúra rekordok a fürt felhasználó által definiált függvények (UDF) Anaconda szereplő számos népszerű Python modulokat. Ebben az esetben használt eljárásokról útmutatásért lásd: a [struktúra lekérdezések módját](machine-learning-data-science-move-hive-tables.md#submit).

A menü alatti, amelyek bemutatják, hogy miként állíthatja be a különböző adatok tudományos környezetekben a [Csapat adatok tudományos folyamat (TDSP)](data-science-process-overview.md)által használt témakörökre mutató hivatkozásokat.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Azure hdinsight szolgáltatáshoz Hadoop fürt testreszabása

Testre szabott HDInsight Hadoop fürt létrehozásához a felhasználóknak kell jelentkezzen be a [**Klasszikus Azure portálon**](https://manage.windowsazure.com/), a bal alsó sarkában kattintson az **Új** gombra, és majd jelölje ki a DATA SERVICES -> HDINSIGHT -> **Egyéni létrehozása** a **Fürt részletei** ablak megjelenítéséhez. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

A szövegbeviteli beállítása lapon 1 hozható létre a fürt nevét, és fogadja el a másik mező alapértelmezett értékének. Kattintson a nyílra kattintva nyissa meg a következő beállítása lapon. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

2 beállítása lapon bemeneti **Adatok csomópontok**számának, jelölje be a **Régió/virtuális hálózati**, és válassza ki a méreteket, a **Fej CSOMÓPONTOT** , és az **Adatok CSOMÓPONTOT**. Kattintson a nyílra kattintva nyissa meg a következő beállítása lapon.

>[AZURE.NOTE] A **Terület és virtuális hálózati** nem lehet ugyanaz, mint a tárterület-fiókot, amely a HDInsight Hadoop fürt használandó fog régiója. Egyéb esetben a negyedik beállítása lapon a tárterület-fiókot, amelyet használni szeretne a felhasználók nem jelennek meg a **Fiók neve**legördülő listája.

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

3 beállítása lapon adja meg a felhasználónevét és jelszavát a HDInsight Hadoop fürt. Az _ENTER billentyűt a struktúra/Oozie Metastore_ **nem** jelölje ki. Kattintson a nyílra kattintva nyissa meg a következő beállítása lapon. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

4 beállítása lapon adja meg a tárhely fiók nevét, az alapértelmezett tároló a HDInsight Hadoop fürt. Ha a felhasználók lehetőséget _létrehozni az alapértelmezett tárolót_ a **Alapértelmezett tároló** legördülő listában, a fürt azonos nevű tároló létrejön. Kattintson a nyílra kattintva Ugrás az utolsó beállítása lapon.

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Az utolsó **Parancsfájl-műveletek** beállítása lapon **parancsfájl művelet hozzáadása** gombra, és töltse ki a szöveg mezők az alábbi értékeket.
 
* **Neve** – olyan karakterlánc, ez a művelet parancsprogram neveként. 
* **Csomópont típusa** - választó **összes csomópontot**. 
* **PARANCSFÁJL URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* egy nyilvános tároló tárterület-fiókjában 
    * *getgoing* ahhoz, hogy PowerShell parancsprogramot fájlmegosztás megkönnyítése érdekében a felhasználók munka Azure-ban. 
* **Paraméterek** - (hagyja üresen)

Végül kattintson a jelölőnégyzet be van jelölve a indítsa el a testre szabott HDInsight Hadoop fürt létrehozását. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>A címsor csomópontot, Hadoop fürt elérése

Felhasználók előtt elérhetik a központi csomópontot a Hadoop fürt RDP keresztül engedélyezni kell a Hadoop fürt Azure-ban távoli eléréséhez. 

1. Jelentkezzen be az [**Azure klasszikus portál**](https://manage.windowsazure.com/), a bal oldalon válassza a **HDInsight** , válassza ki a Hadoop csoportját fürt listája a **beállítás** lapon kattintson, és kattintson a lap alján a **Távoli engedélyezése** ikonra.
    
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. A **Távoli asztali konfigurálása** ablakban írja be a FELHASZNÁLÓNEVÉT és JELSZAVÁT a mezőkben, és válassza ki a lejárati dátum távoli eléréséhez. Kattintson a jelölőnégyzet be van jelölve a távoli hozzáférést biztosít a Hadoop fürt központi csomópontot.

    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] A felhasználónevet és jelszót, a távoli elérhető sem a felhasználónevet és jelszót, amelyet a Hadoop fürt létrehozásakor használ. Ezek a hitelesítő adatok külön sorozatát. A lejárati idő a távoli hozzáférés szintén belül az aktuális dátumtól napján.

Miután távelérési engedélyezve van, kattintson a **Csatlakozás** távoli a lap alján az a központi csomópontot. Jelentkezzen be a központi csomópontot a Hadoop fürt a távelérési felhasználó, amelyet Ön korábban megadott hitelesítő adatok megadásával.

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

A fejlett analitikai folyamat a következő lépésekkel a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a van megfeleltetve, és a lépések, amelyek az adatok áthelyezése HDInsight, folyamat és minta ott tanulási az adatok az Azure gépi tanulási előkészítése az alábbiakat tartalmazhatják.

Megtudhatja, [hogy miként struktúra lekérdezések elküldése](machine-learning-data-science-move-hive-tables.md#submit) módjáról az Anaconda a központi csomópont a fürt a felhasználó által definiált függvények (UDF), a fürt tárolt struktúra bejegyzések feldolgozása használt szereplő Python modulokat eléréséhez.

 
