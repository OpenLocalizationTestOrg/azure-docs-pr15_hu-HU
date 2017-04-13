<properties
    pageTitle="Adatok tudományos virtuális gépeken futó Azure-ban |} Microsoft Azure"
    description="Állítsa be adatokat tudományos virtuális gépen"
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
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Adatok tudományos virtuális gépeken futó Azure-ban

Utasításokat, amelyek bemutatják, hogyan IPython jegyzetfüzet kiszolgálók az Azure virtuális és az SQL-szolgáltatás az Azure virtuális beállítása itt találhatók. A Windows virtuális gép azokat az eszközöket, például a jegyzetfüzet IPython, Azure tároló Explorer és AzCopy, valamint egyéb hasznos tudományos projektekhez segédprogramot igazoló van beállítva. Azure tároló Explorer és AzCopy, például adja meg a helyi gépről Azure tárolóhoz feltölteni az adatokat, vagy töltse le a helyi számítógép zónában tárhelyről kényelmes lehetőségeket. 

Ez a menüben, amelyek bemutatják, hogy miként állíthatja be a különböző adatok tudományos környezetekben a [Csapat adatok tudományos folyamat (TDSP)](data-science-process-overview.md)által használt témakörökre mutató hivatkozásokat tartalmaz.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Azure virtuális gépeken futó különböző típusú is kiépítéstől, és úgy beállítva, hogy egy felhőalapú adatok tudományos környezet részeként használható. A melyik változat kapcsolatos virtuális gép használandó döntéshez és gépi tanulási a felhőben adatához célhely modellezni a típus és az adatok mennyiségét. 

* Ha ennek eldöntése, fontolja meg a kérdések megválaszolása című témakörben olvashat [Megtervezése az Azure gépi tanulási adatok tudományos környezetben](machine-learning-data-science-plan-your-environment.md). 
* Katalógus néhány fejlett analitikai során előforduló esetet tárgyal olvassa el a [Speciális Analytics folyamat és technológia Azure gépi tanulási esetek](machine-learning-data-science-plan-sample-scenarios.md) című témakört.

Két adathalmaz utasítások találhatók:

* [Állítsa be az Azure virtuális gép az fejlett analitikai IPython jegyzetfüzet kiszolgáló](machine-learning-data-science-setup-virtual-machine.md) szemlélteti, hogyan hozhatók létre az Azure virtuális gép IPython jegyzetfüzet és egyéb eszközök segítségével végezze el az adatok tudományos azokra az esetekre, amelyben eltérő SQL Azure tárolási forma használható tárolja az adatokat.

* [Állítsa be az Azure SQL Server virtuális gépen van, mint egy IPython jegyzetfüzet kiszolgáló fejlett analitikai](machine-learning-data-science-setup-sql-server-virtual-machine.md) szemlélteti, hogyan hozhatók létre az Azure SQL Server virtuális gép IPython jegyzetfüzet és egyéb eszközök segítségével végezze el az adatok tudományos azokra az esetekre, amelyben egy SQL-adatbázis használható tárolja az adatokat.

Miután kiépített és konfigurált, ezek virtuális gépeken futó használatra IPython jegyzetfüzet kiszolgálók a feltárás és az adatok feldolgozásának és a más Azure gépi tanulási és a csoport adatok tudományos folyamat (TDSP) együtt szükséges feladatokat. Az adatok tudományos folyamat a következő lépésekkel [TDSP tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) van megfeleltetve, és a lépések, amelyek az adatok áthelyezése SQL Server- vagy HDInsight, folyamat, és ott tanulási az adatok az Azure gépi tanulási előkészítése mintát tartalmazhatnak.


> [AZURE.NOTE] Azure virtuális gépeken futó árú szerint **fizet csak akkor használja**. Győződjön meg arról, hogy, amelyek nem számlázott a virtuális gép nem használatakor, akkor tartalmaz az [Azure klasszikus portál](http://manage.windowsazure.com/) **Leállítva (Deallocated)** állapotban. A lépésenkénti útmutatást, vagy hogyan felszabadításához, akkor a virtuális gép, olvassa el [leállás és virtuális gép használaton felszabadítása](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
