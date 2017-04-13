<properties
    pageTitle="Üres él csomópontok használata a HDInsight |} Microsoft Azure"
    description="Adja hozzá egy ampty él csomópontot HDInsight fürthöz, mint egy ügyfél használható hogyan, illetve hogyan vizsgálat/állomáshoz a HDInsight-alkalmazásokat."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Üres él csomópontok HDInsight használata

Megtudhatja, hogy miként egy üres él csomópont hozzáadása Linux-alapú HDInsight fürthöz. Egy üres él csomópont ügyfél eszközökkel telepítette és beállította a headnodes előtérbe hozza, de nem hadoop-szolgáltatást futtató Linux virtuális gép. A szegély csomópontot a fürt eléréséhez, tesztelje a ügyfélalkalmazások és a ügyfélalkalmazásokban szolgáltatója is használhatja. 

Egy üres él csomópont fűzheti hozzá egy meglévő HDInsight fürthöz új fürthöz a fürt létrehozásakor. Egy üres él csomópont hozzáadása befejeződött erőforrás-kezelő Azure-sablon segítségével.  A következő példa bemutatja, hogyan történik sablon használatával:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Ahogy a minta, igény szerint további konfigurációja, például a szegély csomópont [Apache szín](hdinsight-hadoop-hue-linux.md) telepítése végrehajtandó [művelet parancsfájl](hdinsight-hadoop-customize-cluster-linux.md) is felhívhatja.

Miután létrehozott egy él csomópontot, a szegély csomópont SSH használatával csatlakozhat, és futtassa a Hadoop-fürt HDInsight elérése az ügyféleszközök elől.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Egy él csomópont hozzáadása meglévő fürthöz

Ebben a részben, erőforrás-kezelő sablon segítségével adja hozzá egy meglévő HDInsight fürthöz egy él csomópontot.  Az erőforrás-kezelő sablon [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode)találhatók. Az erőforrás-kezelő sablon https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh található parancsfájl művelet parancsfájl hívások. A parancsprogram nem hajtja végre semmilyen műveletet.  Ez az erőforrás-kezelő sablonból hívó parancsfájl művelet bemutatására.

**Egy üres él csomópont hozzáadása meglévő fürthöz**

1. Hozzon létre egy HDInsight fürthöz, ha még nincs egyet.  Lásd: [Hadoop oktatóprogram: használatba Linux-alapú Hadoop a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kattintson az alábbi képen az Azure bejelentkezés, és nyissa meg az erőforrás-kezelő Azure-sablon az Azure-portálon. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Állítsa be a következő tulajdonságokat:

    - FÜRTNÉV: Adja meg a meglévő HDInsight fürthöz nevét.
    - EDGENODESIZE: Jelölje ki azt a virtuális méretét.
    - EDGENODEPREFIX: **Új**esetén az alapértelmezett érték.  Az alapértelmezett érték használ, biztonsági csomópont név **Új edgenode**.  Testre szabhatja az előtag a portálon. Is testre szabhatja a sablonból teljes nevét.


4. Kattintson az **OK gombra** a módosítások mentéséhez.
5. **Erőforráscsoport**jelöljön ki egy erőforrás-csoportot.
6. Kattintson a **Véleményezés jogi feltételek**gombra, és kattintson a **vásárlás**gombra.
7. Jelölje ki a **PIN-kód irányítópult**, és kattintson a **Létrehozás**gombra.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Adja hozzá egy él csomópontot, amikor fürt létrehozása

Ebben a részben, erőforrás-kezelő sablon segítségével HDInsight fürt hozzon létre egy él csomópontot.  Az erőforrás-kezelő sablon nyilvános [Azure Blob-tároló tárolóban](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json)található. Az erőforrás-kezelő sablon hívások egy parancsfájl művelet parancsfájl https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh helyen található. A parancsprogram nem hajtja végre semmilyen műveletet.  Ez az erőforrás-kezelő sablonból hívó parancsfájl művelet igazolni.

**Egy üres él csomópont hozzáadása meglévő fürthöz**

1. Hozzon létre egy HDInsight fürthöz, ha még nincs egyet.  Lásd: [Hadoop oktatóprogram: használatba Linux-alapú Hadoop a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kattintson az alábbi képen az Azure bejelentkezés, és nyissa meg az erőforrás-kezelő Azure-sablon az Azure-portálon. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Állítsa be a következő tulajdonságokat:
        
    - FÜRTNÉV: Adja meg, hozhat létre új fürt nevét.
    - CLUSTERLOGINUSERNAME: Adja meg a Hadoop HTTP felhasználó nevét.  Az alapértelmezett **rendszergazdai**neve.
    - CLUSTERLOGINPASSWORD: Adja meg a Hadoop HTTP felhasználó jelszavát.
    - SSHUSERNAME: Adja meg a SSH felhasználó nevét. Az alapértelmezett neve **sshuser**.
    - SSHPASSWORD: Adja meg a SSH felhasználó jelszavát.
    - HELY: Adja meg az erőforrás nevére, a fürt és az alapértelmezett tárterület-fiókot.
    - CLUSTERTYPE: Az alapértelmezett érték **hadoop**.
    - CLUSTERWORKERNODECOUNT: Az alapértelmezett értéke 2.
    - EDGENODESIZE: Jelölje ki azt a virtuális méretét.
    - EDGENODEPREFIX: **Új**esetén az alapértelmezett érték.  Az alapértelmezett érték, a szegély csomópont neve használata **Új edgenode**.  Testre szabhatja az előtag a portálon. Is testre szabhatja a sablonból teljes nevét.

4. Kattintson az **OK gombra** a módosítások mentéséhez.
5. **Erőforráscsoport**adjon meg egy új erőforráscsoport nevet.
6. Kattintson a **Véleményezés jogi feltételek**gombra, és kattintson a **vásárlás**gombra.
7. Jelölje ki a **PIN-kód irányítópult**, és kattintson a **Létrehozás**gombra. 


## <a name="access-an-edge-node"></a>Access-él csomópont

A szegély csomópont ssh végpont nem <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Ha például új-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

A szegély csomópontot a Azure portálon alkalmazásként jelenik meg.  A portál adja vissza az adatokat, a szegély csomópont SSH használatával eléréséhez.

**A szegély csomópont SSH végpont ellenőrzése**

1. Bejelentkezés az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a HDInsight fürt egy él csomópontot.
3. Kattintson a fürt lap **alkalmazásokat** . A szegély csomópont gondoskodik.  Az alapértelmezett nevet az **Új edgenode**.
4. Kattintson az él csomópontra. A SSH végpont gondoskodik.

**A szegély csomópontra struktúra használandó**

5. SSH használata a szegély csomópontot.  Lásd: [használata a Linux-alapú Hadoop a HDInsight Linux rendszerhez, a Unix, vagy az OS X SSH](hdinsight-hadoop-linux-use-ssh-unix.md) vagy [Linux-alapú Hadoop meg a Windows HDInsight a SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Miután csatlakozott a SSH használatával él csomópontot, használja a következő parancsot a struktúra konzol megnyitása:

        hive
7. A következő parancsot a fürt struktúra tábla megjelenítése:

        show tables;

## <a name="delete-an-edge-node"></a>Egy él csomópont törlése

Az Azure portálról törölhet is egy él csomópontot.

**Egy él csomópont eléréséhez**

1. Bejelentkezés az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a HDInsight fürt egy él csomópontot.
3. Kattintson a fürt lap **alkalmazásokat** . Meg kell él csomópontok listájának megtekintése.  
4. Kattintson a jobb gombbal a törölni kívánt él csomópontot, és kattintson a **Törlés**gombra.
5. Kattintson az **Igen gombra** kattintva erősítse meg.

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben van megtanulta, hogy miként vehet fel egy él csomópont és hogyan érhető el a szegély csomópontot. További tudnivalókért lásd: az alábbi cikkekben:

- [Alkalmazások telepítése HDInsight](hdinsight-apps-install-applications.md): megtudhatja, hogy miként a fürt HDInsight alkalmazás telepítése.
- [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): útmutató hdinsight szolgáltatáshoz közzé nem tett HDInsight alkalmazások telepítése.
- [Közzététel HDInsight-alkalmazások](hdinsight-apps-publish-applications.md): megtudhatja, hogy miként teheti közzé a Microsoft Azure piactéren egyéni HDInsight alkalmazások.
- [MSDN: egy HDInsight alkalmazás telepítése](https://msdn.microsoft.com/library/mt706515.aspx): megtudhatja, hogy miként definiálhat HDInsight-alkalmazások.
- [Parancsfájl művelettel testreszabása Linux-alapú HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogy miként parancsfájl művelettel további alkalmazások telepítéséhez.
- [Az erőforrás-kezelő sablonokkal HDInsight fürtök létrehozása Linux-alapú Hadoop](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogy miként hívja fel az erőforrás-kezelő sablonok HDInsight fürt létrehozásához.

