<properties
    pageTitle="Közzététel HDInsight-alkalmazások |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre és HDInsight-alkalmazások közzététele."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>A Microsoft Azure piactéren HDInsight alkalmazások közzététele

A HDInsight alkalmazás be olyan alkalmazás, amely a Linux-alapú HDInsight fürthöz telepíthetik a felhasználók. Ezeket az alkalmazásokat a Microsoft, független gyártók (külső), illetve saját maga is készüljön. Ebben a cikkben megtanulhatja, hogy miként teheti közzé egy HDInsight-alkalmazást be a Microsoft Azure piactéren.  Közzétételi be a Microsoft Azure piactéren, általános információt talál [az Azure Piactérhez felajánlás közzététele](../marketplace-publishing/marketplace-publishing-getting-started.md)

HDInsight-alkalmazások használata a *Hozása: A saját licenc (BYOL)* modell, ahol az alkalmazás-szolgáltató a felelős a végfelhasználók számára az alkalmazás licencelési és végfelhasználók csak az előfizetést terhelő Azure által az általuk létrehozott, például a HDInsight fürt és a hozzájuk tartozó VMs csomópontokat erőforrások. Most magát az alkalmazás nem számlát Azure keresztül.

Másik HDInsight alkalmazás kapcsolódó témakör:

- [Alkalmazások telepítése HDInsight](hdinsight-apps-install-applications.md): megtudhatja, hogy miként a fürt HDInsight alkalmazás telepítése.
- [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogy miként telepíthet és tesztelhet az egyéni HDInsight-alkalmazásokat.

 
## <a name="prerequisites"></a>Előfeltételek

Annak érdekében, hogy az egyéni alkalmazás a piactér elküldéséhez kell létrehozta, és az egyéni alkalmazásban tesztelje. Az alábbi cikkekben talál:

- [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogy miként telepíthet és tesztelhet az egyéni HDInsight-alkalmazásokat.

Is van regisztrálnia kell a fejlesztői fiókjával. Lásd: [a Microsoft Azure piactéren található felajánlás közzététele](../marketplace-publishing/marketplace-publishing-getting-started.md) és [egy Microsoft Developer fiók létrehozása](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Alkalmazás meghatározása

Két lépésből áll érintett alkalmazásokat az Azure Piactérhez közzétételi.  Először határozza meg, hogy melyik fürt a alkalmazása kompatibilis; **createUiDef.json** fájl és kattintson a sablon az Azure portálról tegye közzé. Az alábbiakban createUiDef.json mintafájl.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|A mező  | Leírás   | A lehetséges értékek|
|-------|---------------|----------------|
|típusai  | A fürt típusok, az alkalmazás szolgáltatással kompatibilis.    |Hadoop HBase, vihar, külső (és ezek közül bármelyiket)|
|réteg  | A fürt rétegek, az alkalmazás szolgáltatással kompatibilis.    |Normál, prémium (vagy mindkettő)|
|verziók|  A HDInsight fürt típusok, az alkalmazás szolgáltatással kompatibilis.    |3.4.|

## <a name="package-application"></a>Csomag alkalmazás

A HDInsight-alkalmazások telepítéséhez szükséges összes fájlt tartalmazó zip-fájl létrehozása. A [Közzététel](#publish-application)alkalmazásban zip-fájl lesz szüksége.

- [createUiDefinition.json](#define-application).
- mainTemplate.json. [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md)a minta megjelenítéséhez.

    >[AZURE.IMPORTANT] Az alkalmazás telepítése parancsfájl nevek neve a következő formátumban egy adott fürt egyedinek kell lennie. Továbbá bármely telepítése és eltávolítása a parancsfájl-műveletek idempotent kell lennie, tehát a parancsfájlok hívható repeatly közben előállító ugyanazt az eredményt.
    
    >   név":" [összefűzés ("szín-telepítés-v0","-", uniquestring('applicationName')] "
        
    >Megjegyzés: a parancsfájl neve három részből:
        
    >   1. Parancsfájl név előtaggal, amely magában foglalja az alkalmazás nevét és a fontos az alkalmazás nevét.
    >   2. A "-" olvashatóság érdekében.
    >   3. Egyedi karakterlánc függvény és az alkalmazás nevét, mint a paraméter.

    >   Példa a fenti vége valamit be: szín-telepítés-v0-4wkahss55hlas az állandó parancsfájl művelet listában. Egy minta JSON tartalom [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json)című témakör tartalmaz.

- Parancsfájlok az összes szükséges.

> [AZURE.NOTE] Az alkalmazás fájljait (beleértve webalkalmazás fájljait, ha bármelyik) bármely nyilvánosan hozzáférhető végponton található.

## <a name="publish-application"></a>Alkalmazás közzététele

A következő lépésekkel HDInsight alkalmazás közzététele:

1. Jelentkezzen az [Azure közzétételi portál](https://publish.windowsazure.com/).
2. Kattintson a **megoldás sablonok** megoldás új sablon létrehozása a bal oldalról.
3. Írja be a címet, és kattintson a **megoldás új sablon létrehozása**gombra.
3. Kattintson a **Létrehozás fejlesztői központ fiókra, és csatlakozzon az Azure programhoz** regisztrálhatja a vállalat, ha még nem tette gombra.  Lásd: [egy Microsoft Developer fiók létrehozása](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Kattintson az **első néhány topológiák megadása**gombra. Megoldás sablon "parent" az összes annak topológiák. Több topológiák egy ajánlat/megoldás sablonba határozhatja meg. Felajánlás átmeneti tolódik, amikor azt az összes annak topológiák tolódik. 
4. Írjon be egy topológia nevet, és kattintson a pluszjelre.
5. Írja be az új verziót, és kattintson a pluszjelre.
6. A [csomag](#package-application)alkalmazásban készített zip-fájl feltöltése.  
7. Kattintson a **hitelesítő kérése**gombra. A Microsoft hitelesítő csapata tekintse át a fájlokat, és a tanúsítása a topológia.

## <a name="next-steps"></a>Következő lépések

- [Alkalmazások telepítése HDInsight](hdinsight-apps-install-applications.md): megtudhatja, hogy miként a fürt HDInsight alkalmazás telepítése.
- [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): útmutató hdinsight szolgáltatáshoz nem közzétett HDInsight alkalmazások telepítése.
- [Parancsfájl művelettel testreszabása Linux-alapú HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogy miként parancsfájl művelettel további alkalmazások telepítéséhez.
- [Hozzon létre Linux-alapú Hadoop fürtök sablonokkal az erőforrás-kezelő Azure hdinsight szolgáltatáshoz a](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogy miként hívja fel az erőforrás-kezelő sablonok HDInsight fürt létrehozásához.
- [Üres él csomópontok HDInsight használata](hdinsight-apps-use-edge-node.md): megtudhatja, hogy miként használni egy üres él csomópont fürt HDInsight elérése, a tesztelés HDInsight-alkalmazások és a szolgáltatója HDInsight-alkalmazásokat.

