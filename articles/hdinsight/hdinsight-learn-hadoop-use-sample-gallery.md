<properties
   pageTitle="További tudnivalók a HDInsight a minta gyűjteménnyel Hadoop |} Microsoft Azure"
   description="A HDInsight első lépések gyűjteményből minta alkalmazások futtatásával gyorsan megtudhatja, Hadoop. Használja a mintaadatokat, vagy adja meg a saját."
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
   ms.date="10/21/2016"
   ms.author="jgao"/>

# <a name="learn-hadoop-by-using-the-azure-hdinsight-getting-started-gallery"></a>További tudnivalók a Hadoop az Azure hdinsight szolgáltatáshoz első lépések gyűjtemény használatával

Az első lépések gyűjtemény csak érhető el a Windows-alapú HDInsight fürtre vonatkozóan. A gyűjtemény tartalmaz egy könnyen és gyorsan megtudhatja Hadoop minta alkalmazások futtatásával hdinsight szolgáltatásból lehetőségre. A minták részét származnak, a mintaadatokat tartalmazó. A maradék mintáknál a saját adatain adhat meg. Vannak jelenleg a következő hat minták (további érkező):

- Az Azure adatok megoldások
    - Microsoft Azure webhely naplófájl elemzése
    - Microsoft Azure tároló analytics
- A mintaadatokat tartalmazó megoldások
    - Érzékelő adatelemzés
    - Twitter trend-elemzés
    - Webhely naplófájl elemzése
    - Mahout film ajánlási

![HDInsight Hadoop vihar és HBase első lépések gyűjtemény megoldásairól mintaadatokat.][hdinsight.sample.gallery]

Az alábbi videó bemutatja, hogyan tehető függővé a a Twitteren trend analysis minta:

<center><a href="https://www.youtube.com/embed/7ePbHot1SN4">https://www.YouTube.com/EMBED/7ePbHot1SN4></a></center>

Az irányítópult http:// között is elérhető<YourHDInsightClusterName>.azurehdinsight.net/ vagy az Azure portálról.

**Minta futtatásához a gyűjteményből az első lépések**

1. Jelentkezzen be az [Azure portál][azure.portal].
2. Kattintson a **Tallózás gombra** a bal oldali menüben kattintson a **HDInsight fürt**, és kattintson a csoport nevére.
3. A felső menüből az **Irányítópult** elemre.
4. Írja be a felhasználónevét és jelszavát a HTTP-felhasználó (más néven fürt felhasználó).
6. Kattintson az **Első lépések gyűjtemény** a lap tetején.
7. Kattintson a minta. Minden egyes minta a lépések részletes leírását futtatásához adja vissza. Az alábbi képen látható, a Twitteren trend analysis minta:

    ![HDInsight Twitter trend analysis minta][hdinsight.twitter.sample]

## <a name="next-steps"></a>Következő lépések
Ha többet szeretne tudni a HDInsight más módokon a következők:

- [HDInsight tanulási térkép][hdinsight.learn.map]
- [HDInsight infographic][hdinsight.infographic]

<!--Image references-->
[hdinsight.sample.gallery]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Getting-Started-Gallery.png
[hdinsight.twitter.sample]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Twitter-Trend-Analysis-sample.png

<!--Link references-->
[hdinsight.learn.map]: https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/
[hdinsight.infographic]: http://go.microsoft.com/fwlink/?linkid=523960
[azure.portal]:https://portal.azure.com
