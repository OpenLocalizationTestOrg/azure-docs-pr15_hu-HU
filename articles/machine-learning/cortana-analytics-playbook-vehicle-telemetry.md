<properties 
    pageTitle="Gépjármű telemetriai analytics megoldás playbook |} Microsoft Azure" 
    description="A Cortana intelligenciával kapcsolatos funkcióinak használata valós idejű és a prediktív háttérismeretek gépjármű állapot és a vezetői eléréséhez szokások." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Gépjármű telemetriai analytics megoldás playbook

Ez a **menüben** a a fejezetekben az e playbook mutató hivatkozásokat tartalmaz. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>– Áttekintés
Felügyelő számítógépek át lett helyezve a labor ki, és most már vannak leparkolni a garázs! Ezek a legmodernebb automobiles érzékelők, biztosítva azok nyomon követése és a Lync-események milliónyi minden második számtalan tartalmazzák. Megakadályozási, hogy 2020, amelyet a legtöbb e autós fog van már csatlakozik az internethez. Képzelje el, koppintás a különböző adatok megadására a legjobb osztály biztonsági, megbízhatóságának és vezetői be! A Microsoft ezt a Cortana intelligenciával formákat megváltozz tett.

A Microsoft üzletiintelligencia-Cortana pedig egy teljes körű felügyelt nagy adatok fejlett analitikai csomagja, amely lehetővé teszi az adatok intelligens művelet alakíthat ki. A Cortana intelligencia gépjármű Telemetriai Analytics megoldás sablon megismerteti szeretnénk. Ez a megoldás bemutatja, hogyan autós kereskedők rakodótere gyártók és biztosítási cégek használatával a Cortana intelligenciával kapcsolatos funkcióinak nyereség valós idejű és gépjármű állapot és a vezetői prediktív háttérismeretek szokások. 

A megoldás alkalmazása [lambda architektúra minta](https://en.wikipedia.org/wiki/Lambda_architecture) a teljes lehetséges a Cortana üzletiintelligencia-Platform for megjelenítő valós idejű és parancsfájl. A megoldást: 

- Itt egy gépjármű telematika simulatort használja.
- Esemény hubok használja az szimulált gépjármű telemetriai események milliónyi ingesting az Azure 
- Értékáram-elemzés használja a valós idejű háttérismeretek gépjármű egészségügyi eléréséhez
-  továbbra is fennáll az adatok magasabb szintű köteg elemzéséhez hosszú távú tárolóba. 
- Gépi tanulási előnyeit rendellenességet kimutatására a valós idejű tart, és a köteg prediktív háttérismeretek eléréséhez feldolgozása.
- használja a HDInsight átalakításához skála adatait és adatok gyári üzletifolyamat-tervező, ütemezés, az erőforrás-kezelés és nyomon követése a parancsfájl folyamat kezelése 
- valós idejű adatok és a prediktív analytics megjelenítések a Power BI gazdag irányítópult ad a megoldás

## <a name="architecture"></a>Architektúra

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*1 – gépjármű Telemetriai Analytics megoldási ábra*

Ez a megoldás a következő **Cortana üzletiintelligencia-összetevők** tartalmazza, és a végpontok közötti integráció bővíthető


- **Esemény hubok** gépjármű telemetriai események milliónyi ingesting az Azure.
- Valós idejű háttérismeretek gépjármű egészségügyi elveszti **Értékáram-elemzés** és továbbra is fennáll, hogy az adatok magasabb szintű köteg elemzéséhez hosszú távú tárolóba.
- Valós idejű rendellenességet észlelési **Gépi tanulási** és a prediktív háttérismeretek eléréséhez parancsfájl.
- **HDInsight** kapcsolatos van károkozásra az adatokat a skála
- **Adatok gyári** üzletifolyamat-tervező, kezeli az erőforrás-kezelés ütemezése, és a parancsfájl folyamat ellenőrzésére.
- **A Power BI** gazdag irányítópult ad a megoldás valós idejű adatok és a prediktív analytics megjelenítések.

Ez a megoldás két különböző **adatforrásokat**éri el: 

- **Szimulált gépjármű jelek és diagnosztika**: bocsát ki diagnosztikai adatok és a gépjármű és a vezetői minta egy adott pontján állapotát, egymásnak megfelelő jelek egy gépjármű telematika simulatort használja az idő. 
- **Gépjármű katalógus**: a modell hozzárendelése egy VIN tartalmazó hivatkozás adatkészletet.
