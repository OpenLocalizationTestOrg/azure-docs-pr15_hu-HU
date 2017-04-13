<properties 
    pageTitle="Naplózás a gépi tanulási webszolgáltatásokhoz |} Microsoft Azure" 
    description="Megtudhatja, hogy miként gépi tanulási webszolgáltatásokhoz naplózás engedélyezéséhez. Naplózás az API-k hibaelhárítási további információt tartalmaz." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Gépi tanulási webszolgáltatásokhoz naplózás engedélyezése  

A dokumentum klasszikus webszolgáltatásokhoz naplózás képességeinek ismertetése. A naplózás engedélyezése a Web services további információkat, egy hibaszám és, üzenet, amely segíthet a hívások átirányítása a gépi tanulási API-k elhárításában túl tartalmaz.  

Az Azure klasszikus portálon webszolgáltatásokhoz a naplózás engedélyezése:   

1.  Jelentkezzen be az [Azure klasszikus](https://manage.windowsazure.com/) portálra
2.  Kattintson a bal oldali szolgáltatásai oszlopnak a **Gépi TANULÁSI**.
3.  Kattintson a munkaterület, majd a **WEBES szolgáltatások**.
4.  A webes szolgáltatások listában kattintson a webszolgáltatás nevére.
5.  A végpontok listában kattintson a végpontjának neve.
6.  Kattintson a **KONFIGURÁLÁS**gombra.
7.  **Diagnosztikai NYOMKÖVETÉSI** szintet *hiba* vagy az *összes*, majd kattintson a **MENTÉS**gombra.

Az Azure gépi tanulási Web Services portál naplózás engedélyezése.

1. Jelentkezzen be az [Azure gépi tanulási Web Services](https://services.azureml.net) portál.
2. Kattintson a klasszikus webszolgáltatásokhoz.
3.  A webes szolgáltatások listában kattintson a webszolgáltatás nevére.
4.  A végpontok listában kattintson a végpontjának neve.
5.  Kattintson a **konfigurálása**.
6.  Állítsa be a **naplózás** *hiba* vagy az *összes*, majd kattintson a **MENTÉS**gombra.

## <a name="the-effects-of-enabling-logging"></a>A naplózás engedélyezése hatásai

Naplózás engedélyezve van, amikor a felhasználó munkaterület kapcsolódik az összes a diagnosztikai és a kijelölt végpont eredő hibák Azure tárterület-fiókba jelentkezett. Megtekintheti a tárterület-fiók az Azure klasszikus portálon irányítópult megtekintése (a Quick Glance szakasz alján) a munkaterületet.  

A naplók megtekintheti a különböző eszközök "feltárása" Azure tárterület-fiók használata. A legegyszerűbb lehet egyszerűen keresse meg az Azure klasszikus portálon tárterület-fiókjába, és kattintson a **TÁROLÓK**. Látni szeretné majd **Machine Learning diagnosztikáról**nevű tároló. Tároló a tárterület-fiókhoz társított összes munkaterületek minden web service végpontok diagnosztikai információkat tartalmazza. 
 
## <a name="log-blob-detail-information"></a>Log blob részletes adatai

A tárolóban lévő minden blob adatait diagnosztika pontosan végre a megfelelő műveletet:

-   A köteg-végrehajtási mód egy végrehajtását  
-   A kérés-válasz metódus egy végrehajtása  
-   Kérés-válasz tároló inicializálni
  
Minden egyes blob nevét az alábbi képernyő előtaggal foglalja magában: 

{Munkaterület azonosító}-{webszolgáltatás azonosító}-{végpont azonosítója} / {típusú naplózása}  

Naplófájl típusa esetén az alábbi értékek közül:  

- Köteg  
- pontszámhoz/kérések  
- pontszámhoz/init  