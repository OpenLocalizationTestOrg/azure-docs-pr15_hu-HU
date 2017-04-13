<properties
    pageTitle="A környezet a szolgáltatás háló megoldásban napló Analytics optimalizálása |} Microsoft Azure"
    description="Mérje fel, hogy a kockázatok és a szolgáltatás alkalmazások, micro-szolgáltatások, csomópontok és fürt állapotának használhatja a szolgáltatás háló megoldás."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Log Analytics szolgáltatás háló megoldás

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](log-analytics-service-fabric-azure-resource-manager.md)
- [A PowerShell](log-analytics-service-fabric.md)

Ez a cikk ismerteti, hogyan napló Analytics a szolgáltatás háló megoldást segítségével azonosításához és kapcsolatos problémák megoldása a szolgáltatás háló fürt keresztül.

A szolgáltatás háló megoldást adatain Azure diagnosztika a szolgáltatás háló VMs úgy, hogy az adatok gyűjtése a ÜVEGVATTA Azure-táblából. Log Analytics majd olvassa be a szolgáltatás háló keretrendszer eseményeket, például a **Megbízható szolgáltatás eseményeket**, **Szereplő eseményeket**, **Működési események**és **Egyéni esemény-nyomkövetés eseményeket**. A megoldás irányítópult, az Ön jelenítenie főbb problémák és fontos eseményeket a szolgáltatás háló környezetben.

Első lépések a megoldást, szüksége lesz a szolgáltatás háló fürt csatlakozhat napló Analytics munkaterület. Az alábbi három forgatókönyv is célszerű figyelembe venni:

1. Ha nem telepítette a szolgáltatás háló fürt, ismertetett lépésekkel hozhatja létre ***Deploy szolgáltatás háló fürt csatlakozik a napló Analytics munkaterület*** új fürt telepíthető, és azt beállítania, hogy a napló Analytics-jelentés.

2. Ha teljesítmény számláló gyűjt a hosts más MOBILE megoldást, például biztonsági használatáról a szolgáltatás háló fürt van szüksége, kövesse a ***szolgáltatás háló fürt telepítve virtuális kiterjesztésű csatlakozik egy MOBILE munkaterület Deploy.***

3. Ha már telepítette a szolgáltatás háló fürt és a kívánt napló Analytics csatlakozni, kövesse a ***tároló meglévő fiók felvétele a napló Analytics.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Telepítse a szolgáltatás háló fürtre napló Analytics munkaterület csatlakozik.
Ez a sablon az alábbi műveleteket végzi el:


1. Üzembe helyezése egy Azure Service háló fürthöz napló Analytics munkaterület már csatlakozott. Új munkaterület létrehozása a sablon telepítésekor lehetősége van, vagy a szövegbeviteli egy már meglévő napló Analytics-munkaterület nevét.
2. A diagnosztikai tárterület-fiók felvétele a napló Analytics munkaterület.
3. Lehetővé teszi, hogy a szolgáltatás háló megoldást a napló Analytics-munkaterületen.

[![Azure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Miután kiválasztotta a központi telepítés gombot, akkor az Azure portálon, kijelölt szerkesztése paramétereinek érkezik. Új erőforráscsoport készíteni, ha egy új nevet a napló Analytics-munkaterület: ![szolgáltatás háló](./media/log-analytics-service-fabric/2.png)

![Szolgáltatás háló](./media/log-analytics-service-fabric/3.png)

Fogadja el a jogi szerződést, majd kattintson a "Létrehozása" a telepítés elindításához. A telepítés befejezése után meg kell jelennie az új munkaterület és létrehozott csoportját, és a WADServiceFabric * hozzáadott esemény, WADWindowsEventLogs és WADETWEvent táblák:

![Szolgáltatás háló](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>A szolgáltatás háló fürtre csatlakozik egy MOBILE munkaterület telepített virtuális kiterjesztésű üzembe.
Ez a sablon az alábbi műveleteket végzi el:

1. Üzembe helyezése egy Azure Service háló fürthöz napló Analytics munkaterület már csatlakozott. Új munkaterület létrehozása, vagy egy meglévőt használ.
2. A diagnosztikai tároló fiókok felvétele a napló Analytics-munkaterületre.
3. Lehetővé teszi, hogy a szolgáltatás háló megoldást a napló Analytics munkaterületen.
4. Minden virtuális-skála megadása a szolgáltatás háló fürt a MMA ügynök bővítmény telepítése A telepített MMA ügynökkel tudni megtekinteni a csomópontok kapcsolatos teljesítménymutatók áll.


[![Azure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


Ezeket a lépéseket a fenti a szükséges paramétereket bemeneti, és a telepítés elindításához. Még egyszer az új munkaterület, fürt és az összes létrehozott ÜVEGVATTA táblák kell megjelennie:

![Szolgáltatás háló](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>A teljesítményadatok megtekintése

Perf adatainak megtekintése a csomópontjának:
</br>
- Indítsa el a naplót Analytics munkaterület az Azure portálról.

![Szolgáltatás háló](./media/log-analytics-service-fabric/6.png)

- A bal oldali ablaktáblában válassza a beállítások, és válassza az adatok >> Windows teljesítmény számláló >> "Hozzáadása a kijelölt teljesítmény számláló": ![szolgáltatás háló](./media/log-analytics-service-fabric/7.png)

- Log keresés a következő lekérdezések használatával kapcsolatban a csomópontok kulcs mértékek elmélyedhet:
</br>

    egy. Összehasonlíthatók az átlagos Processzor kihasználtsági a csomópontjait az utolsó egy órával a csomópontok problémákat, és milyen időközönként csomópont volt a Nyárs megtekintéséhez:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Szolgáltatás háló](./media/log-analytics-service-fabric/10.png)


    b. A rendelkezésre álló memória hasonló vonaldiagramok megtekintése az alábbi lekérdezés mindegyik csomóponton:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    A lekérdezés használatával a csomópontjait, rajta a pontos átlagos értéke a rendelkezésre álló memória csomópontok, listájának megjelenítése:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Szolgáltatás háló](./media/log-analytics-service-fabric/11.png)


    c billentyűkombinációt. Abban az esetben, hogy részletesen megjelenítsen egy adott csomópont vizsgálata az óránkénti átlag, minimum, a legnagyobb és 75-PERCENTILIS processzorhasználata alapján szeretné Ha tud ehhez a lekérdezés (csere számítógép-mező):

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Szolgáltatás háló](./media/log-analytics-service-fabric/12.png)

    Olvassa el a további információt a teljesítménymutatók napló Analytics [itt]. (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Log Analytics tároló meglévő fiók felvétele

Ez a sablon egy új vagy meglévő napló Analytics-munkaterület egyszerűen ad a meglévő tárterület-fiókok.
</br>

[![Azure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] Erőforráscsoport kijelölése dolgozik egy már meglévő napló Analytics-munkaterület, ha kijelölése "Használata meglévő" és a Keresés az erőforráscsoport tartalmazó a MOBILE munkaterület Hozzon létre egy új egy ha egyébként.
![Szolgáltatás háló](./media/log-analytics-service-fabric/8.png)

Ezzel a sablonnal telepítését követően lesz a napló Analytics-munkaterületre csatlakoztatott tároló megjelenik. Ebben az esetben lehet további tárterületet fiókok lehet az előbb létrehozott Exchange-munkaterületre kerül.
![Szolgáltatás háló](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Szolgáltatás háló események megtekintése

Ha a telepítés befejezésekor és engedélyezve van a szolgáltatás háló megoldást a munkaterületen kattintson a **Szolgáltatás háló** csempére a napló Analytics-portálon meg a szolgáltatás háló irányítópult. Az irányítópult az oszlopot az alábbi táblázat tartalmazza. Minden egyes oszlopát a felső tíz események száma a megadott tartomány adott oszlop feltételnek megfelelő sorolja fel. A napló keresés, ahol a teljes listát, kattintson a jobb alsó részén minden egyes oszlopát **látható a teljes** , vagy az oszlop fejlécére kattintva futtathatja.

| **Szolgáltatás háló esemény** | **Leírás** |
| --- | --- |
| Főbb kapcsolatos problémák | Problémák, például RunAsyncFailures RunAsynCancellations és csomópont szűrőlisták megjelenítése beállításra. |
| Műveleti események | Főbb műveleti eseményeket, például alkalmazás frissítése és telepítések. |
| Megbízható szolgáltatás események | Főbb megbízható szolgáltatás események egy ilyen Runasyncinvocations. |
| Szereplő események | Főbb szereplő események generálja a micro szolgáltatásokat, például a kivételek által egy szereplő módszer, szereplő aktiválásra és deactivations és így tovább. |
| Alkalmazás események | Az összes egyéni esemény-nyomkövetés események az alkalmazások által generált. |

![Szolgáltatás háló irányítópult](./media/log-analytics-service-fabric/sf3.png)

![Szolgáltatás háló irányítópult](./media/log-analytics-service-fabric/sf4.png)


A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan kell felvenni az adatokat a szolgáltatás háló más adatait.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![nem](./media/log-analytics-malware/oms-bullet-red.png)|![nem](./media/log-analytics-malware/oms-bullet-red.png)| ![igen](./media/log-analytics-malware/oms-bullet-green.png)|            ![nem](./media/log-analytics-malware/oms-bullet-red.png)|![nem](./media/log-analytics-malware/oms-bullet-red.png)|10 perc |


>[AZURE.NOTE] A szolgáltatás háló megoldás az események körének módosíthatja az **adatok alapján utóbbi 7 napból** tetején látható az irányítópult elemre kattintva. Az utóbbi 7 napból 1 nap vagy hat órán belül generált események is megjelenítheti. Vagy válassza a **saját** egyéni dátumtartományra vonatkozóan megadásához.


## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes szolgáltatás háló esemény adatait.
