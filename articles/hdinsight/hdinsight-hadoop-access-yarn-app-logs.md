<properties
    pageTitle="Access Hadoop fonal alkalmazás naplózza programozás útján |} Microsoft Azure"
    description="Access-alkalmazás programozás útján HDInsight a Hadoop fürt naplót."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Access fonal alkalmazás bejelentkezik a Windows-alapú hdinsight szolgáltatáshoz

Ebből a témakörből megtudhatja, hogyan érhető el a naplók befejezése után az Azure hdinsight szolgáltatáshoz Hadoop fürtre fonal (még egy másik erőforrás-egyeztető) alkalmazáshoz

> [AZURE.NOTE] A dokumentumban szereplő információk csak a Windows-alapú HDInsight fürt vonatkozik. FONAL eléréséről Linux-alapú HDInsight fürt bejelentkezik, lásd [a HDInsight Linux-alapú Hadoop bejelentkezik fonal Access-alkalmazás](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Előfeltételek

- A Windows-alapú HDInsight fürtre.  Lásd: [létrehozása a Windows-alapú Hadoop fürt a hdinsight szolgáltatásból lehetőségre](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>FONAL ütemterv kiszolgáló

A <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Fonal ütemterv kiszolgáló</a> általános információt nyújt kitöltött kérelmeket, valamint a keretrendszer-specifikus alkalmazás adatokat, két különböző felületeken keresztül. Kifejezetten:

* Tárterület és a HDInsight fürt általános alkalmazás információt a lekérés lett engedélyezett 3.1.1.374 verzióval vagy újabb verziójában.
* Az ütemterv kiszolgáló keretrendszer-specifikus alkalmazás adatra jelenleg nem áll rendelkezésre a HDInsight fürt.


Alkalmazások általános információkat tartalmazza a következő adatok rendezése:

* Az azonosítója, az alkalmazások egyedi azonosítója
* A felhasználó, aki elindította az alkalmazás
* Az alkalmazás befejezéséhez kísérletek információk
* Az adott alkalmazás kísérel meg által használt tárolók

A HDInsight fürt meg ezt az információt szeretne tárolni Azure-kezelő által az alapértelmezett tárolóban alapértelmezett Azure tárterület-fiókját az előzmények tárolóval. Az általános adatok kész alkalmazások tudja visszaszerezni REST API-val keresztül:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>FONAL alkalmazások és a naplók

FONAL több programozási modellek (MapReduce alatt az egyiket) támogatja az erőforrás-kezelés az alkalmazások figyelése/ütemezési szétválasztás. Ez történik át egy általános *erőforrás-kezelő* (erőforrás-kezelő), a használati dolgozó-csomópont- *NodeManagers* (NMs), és a alkalmazásonként *ApplicationMasters* (AMs). Az alkalmazás AM egyezteti erőforrások (Processzor, memóriát, szabad, hálózati) az alkalmazás futtatásához a RM. együtt Az erőforrás-kezelő működik-e adni, ezek az erőforrások, mint *tárolók*megadó NMs. A AM a felelős a a tárolók kiosztotta a RM. végrehajtásának nyomon követése Az alkalmazások számos tárolók attól függően, hogy az alkalmazás jellegét lehet szükség.

Ezenkívül minden alkalmazás állhat, több *alkalmazás megpróbál* jelenlétében összeomlik, vagy egy óra közötti kommunikáció elvesztése miatt befejezéséhez és egy RM. Tárolók emiatt a adott próbálkozásra egy alkalmazás kell megadni. Értelemben tároló biztosít az alapegység fonal alkalmazás által elvégzett munka környezetben, és a tároló környezetén belül végzett munkáját történik a egyetlen dolgozó csomópontot, amelyen a tároló van kiosztva. Lásd: [Fonal fogalmak] [ YARN-concepts] további hivatkozások.

Alkalmazás naplók (és a kapcsolódó tároló naplók) kritikus, a hibakereséshez hibás Hadoop-alkalmazásokat. FONAL gyűjteni, összesítése és tárolja a [Napló összesítést] az alkalmazás naplók szép keretet biztosít[ log-aggregation] szolgáltatás. A napló összesítési szolgáltatásokkal teszi elérésekor alkalmazás naplók további mérvadó, naplók összesítésének át az összes tárolók dolgozó csomóponton és egy összesített naplófájlt / dolgozó csomópontot a alapértelmezett fájlrendszerben tárolja őket az alkalmazás befejezése után. Az alkalmazás előfordulhat, hogy a száz vagy tárolók ezer használni, de fog egy fájlba, így egy naplófájlban / dolgozó csomópontot, az alkalmazás által használt mindig összesíti egyetlen dolgozó csomóponton futtassa az összes tárolók naplók. HDInsight fürt alapértelmezés szerint engedélyezve van a napló összesítési (3.0-s verziója és a fenti), és összesített naplók az alapértelmezett tárolóban a fürt a következő helyen található:

    wasbs:///app-logs/<user>/logs/<applicationId>

Ebben a helyet, *felhasználói* a felhasználó, aki elindította az alkalmazás nevére, akinek *applicationId* egyedi azonosítója kiosztotta a fonal RM. kérelmet.

Az összesített naplók nincsenek közvetlenül olvasható, mivel az egy [TFile]írták őket[T-file], [binárissá] [ binary-format] tároló által indexelt. FONAL eszközeivel CLI ezeket a naplókat kiírása for applications vagy az érdeklődésre számot tartó tárolók egyszerű szövegként. Ezeket a naplókat, egyszerű szöveg a következő fonal futtatásával parancs, közvetlenül a fürt csomópontok (után kapcsolódás RDP keresztül) tekintheti meg:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>FONAL erőforrás-kezelő felhasználói felület

FONAL erőforrás-kezelő a felhasználói felület a fürt headnode fut, és azok webböngészőn keresztül az Azure portál irányítópult: 

1. Jelentkezzen be az [Azure](https://portal.azure.com/)-portálra. 
2. A bal oldali menüben kattintson a **Tallózás gombra**, kattintson a **HDInsight fürt**, kattintson egy Windows-alapú fürtre a fonal alkalmazás naplók elérni kívánt.
3. A felső menüben kattintson az **Irányítópult**. Ekkor megjelenik a lapon a hívott **HDInsight lekérdezés konzol**nyit meg egy új böngészőablakban lapon.
4. A **HDInsight lekérdezés konzolban**kattintson **Fonal felhasználói felület**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
