<properties 
    pageTitle="A beállítási lépések gépjármű telemetriai analytics megoldás sablon PowerBI irányítópult |} Microsoft Azure" 
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


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Gépjármű telemetriai analytics megoldás sablon PowerBI irányítópult beállítására vonatkozó utasítások

Ez a **menüben** a a fejezetekben az e playbook mutató hivatkozásokat tartalmaz. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


A gépjármű Telemetriai Analytics megoldást hogyan autós kereskedők, rakodótere gyártók és biztosítási cégek is kihasználhatja a Cortana intelligenciával kapcsolatos funkcióinak valós idejű és a prediktív háttérismeretek gépjármű egészségügyi eléréséhez, és a meghajtó javítása területén vevő vezetői szokások fordul elő, K + F és marketingkampányok bővíthető. A dokumentum hogyan konfigurálható a PowerBI jelentések és irányítópultok után a megoldást telepíti az előfizetésben lépésenkénti útmutatást tartalmaz. 


## <a name="prerequisites"></a>Előfeltételek
1.  A gépjármű Telemetriai Analytics-megoldást üzembe helyez a [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3) való navigálással  
2.  [Telepítse a Microsoft Power BI-asztal](http://www.microsoft.com/download/details.aspx?id=45331)
3.  [Azure előfizetés](https://azure.microsoft.com/pricing/free-trial/). Ha nem rendelkezik az Azure előfizetéssel, Azure ingyenes verzióra szóló előfizetés első lépések
4.  A PowerBI Microsoft-fiók
    

## <a name="cortana-intelligence-suite-components"></a>Üzletiintelligencia-csomagja Cortana összetevői
A gépjármű Telemetriai Analytics megoldás sablon részeként a következő Cortana üzletiintelligencia-szolgáltatásokat az előfizetése van telepítve.

- **Esemény hubok** gépjármű telemetriai események milliónyi ingesting az Azure.
- Valós idejű háttérismeretek gépjármű egészségügyi elveszti **Adatfolyam analitikus**s és továbbra is fennáll, hogy az adatok magasabb szintű köteg elemzéséhez hosszú távú tárolóba.
- Valós idejű rendellenességet észlelési **Gépi tanulási** és a prediktív háttérismeretek eléréséhez parancsfájl.
- **HDInsight** kapcsolatos van károkozásra az adatokat a skála
- **Adatok gyári** üzletifolyamat-tervező, kezeli az erőforrás-kezelés ütemezése, és a parancsfájl folyamat ellenőrzésére.

**A Power BI** gazdag irányítópult ad a megoldás valós idejű adatok és a prediktív analytics megjelenítések. 

A megoldás használja a két különböző forrásokból: **szimulált gépjármű jelek és diagnosztikai adatkészlet** és **gépjármű katalógus**.

Egy gépjármű telematika simulatort használja az részeként ebben a megoldást. Diagnosztikai adatok bocsát ki és a megfelelő a gépjármű állapotát és minta ösztönzése egy adott pontján időben jeleket. 

A gépjármű katalógus egy hivatkozás adatkészlet tartalmazó VIN a modell hozzárendelése


## <a name="powerbi-dashboard-preparation"></a>PowerBI irányítópult előkészítése

### <a name="deployment"></a>Telepítési

A telepítés befejezése után megjelenik a következő diagramon az összes alábbi összetevők zöld megjelölve. 

- A zöld csomópontok jobb felső sarkában lévő nyílra kattintva nyissa meg a megfelelő szolgáltatások ellenőrzése, hogy ezek közül az összes telepített sikeresen.
- Az adatok simulatort használja csomag letöltéséhez kattintson a jobb oldali **Gépjármű telematika simulatort használja** a csomóponton felső nyílra. Mentse, és bontsa ki a fájlokat a számítógépen helyben. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Ezután készen áll a széles tárházát valós idejű eléréséhez állítsa be a PowerBI-irányítópult, gépjármű állapot és a vezetői prediktív háttérismeretek szokások. Minden jelentések létrehozása és konfigurálása az irányítópulton órává körülbelül 45 percet vesz igénybe. 


### <a name="setup-power-bi-real-time-dashboard"></a>Valós idejű irányítópult a Power BI beállítása

**Szimulált adatok létrehozása**

1. A helyi számítógépen nyissa meg a mappát, ahová a gépjármű telematika simulatort használja csomag![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Az alkalmazás ***CarEventGenerator.exe***végrehajtása.
3.  Diagnosztikai adatok bocsát ki és a megfelelő a gépjármű állapotát és minta ösztönzése egy adott pontján időben jeleket. Ez a központi telepítés részeként konfigurált Azure esemény központi példány van közzétéve.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**A valós idejű irányítópult-alkalmazás indítása**

A megoldást hoz létre egy valós idejű irányítópult PowerBI alkalmazást tartalmazza. Ez az alkalmazás központi esemény példányhoz, amelyből Értékáram-elemzés közzé, az események folyamatosan figyeli. Minden esetben ez az alkalmazás kapja feldolgozásával az adatokat egy gépi tanulási kérés-válasz pontozási végpontot. Az eredményül kapott adatkészlet a PowerBI leküldéses API-khoz megjelenítő elem van közzétéve. 

Az alkalmazás letöltése:

1.  Kattintson a diagram nézetben a PowerBI csomópontra, és kattintson a **Letöltés valós idejű irányítópult alkalmazás**"hivatkozás, kattintson a Tulajdonságok ablaktáblában.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Kibontása és a helyi meghajtóra az alkalmazás mentése![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Az alkalmazás **RealtimeDashboardApp.exe** végrehajtása
4.  Érvényes Power BI hitelesítő adatait, jelentkezzen be, és kattintson az **Elfogadás** gombra
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>Állítsa be a PowerBI-jelentések
A valós idejű jelentéseket és az irányítópult hogy körülbelül 30-45 perc befejezéséhez. Nyissa meg a [http://powerbi.com](http://powerbi.com) , és jelentkezzen be.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

A Power BI új adatkészletet jön létre. Kattintson a **ConnectedCarsRealtime** adatkészlet.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Mentse a üres jelentést, használja a **Ctrl + s billentyűkombinációt**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Adja meg a *jelentések gépjármű Telemetriai Analytics valós idejű -*jelentés nevét.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Valós idejű jelentések
Ez a megoldás három valós idejű jelentések van:

1.  A művelet járműveket
2.  Karbantartási igénylő járműveket
3.  Járműveket állapot statisztika

Megadhatja, hogy az összes három valós idejű jelentésének beállítása vagy leállítása minden szakasz után, és folytassa a következő szakaszban a köteg jelentések beállításainak megadása. Azt javasoljuk, hogy a teljes az összefüggéseket a valós idejű elérési útját a megoldást ábrázolásához összes három jelentéseket készíthet.  

### <a name="1-vehicles-in-operation"></a>1. a művelet járműveket
  
Kattintson duplán az **oldal-1** , és nevezze át a "Művelet járműveket"  
    ![Csatlakoztatott autók - járműveket művelet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Jelölje ki a **vin** mezőt a **mezők** , és válassza a **"Kártya"**képi megjelenítés típusú.  

Kártyás megjelenítés ábrán látható módon jön létre.  
    ![Csatlakoztatott autók - választó vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Kattintson az új képi megjelenítés hozzáadása üres területre.  

Válassza ki **a város** és **vin** mezőket. **"**Térképre" képi megjelenítésének módosítása. Húzza a **vin** az értékek területre. Húzza **a város** mezőkből származó **Jelmagyarázat** területén.   
    ![Autók - kártyás megjelenítés csatlakozik.](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
**Formátuma** szakaszban válassza a **képi megjelenítések**, kattintson a **cím** és a **szöveg** módosítása **"Művelethez település szerint járműveket"**.  
    ![Csatlakoztatott autók - járműveket műveletben település szerint](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Végleges képi megjelenése ábrán látható módon.    
    ![Csatlakoztatott autók - végleges képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Kattintson az új képi megjelenítés hozzáadása üres területre.  

Jelölje ki **a város** **vin**, majd **Csoportosított oszlop típusú diagram**képi megjelenítési típus módosítása. A **tengely területre** , és **vin** **érték** területén a **Város** mező biztosítása  

**"Vin számát"** diagram rendezéséhez  
    ![Csatlakoztatott autók - vin száma](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

**Diagramcím** módosítása **"Művelethez település szerint járműveket"**  

Kattintson a **Formátum** szakasz, majd válassza a **Adatok színek**, a **"A"** **Minden látszik** gombra.  
    ![Csatlakoztatott autók – az összes adat szín megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Egyes város színének módosításához kattintson a szín ikonra.  
    ![Autók - más színek csatlakozik.](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Kattintson az új képi megjelenítés hozzáadása üres területre.  

**Csoportosított oszlop típusú diagram** képi megjelenítések jelölje ki, húzza a **Város** mező a **tengely** területet, a **modell** a **Jelmagyarázat** területén és **vin** **érték** területen.  
    ![Csatlakoztatott autók – csoportosított oszlop típusú diagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Csatlakoztatott autók - leképezés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Ezen a lapon minden képi megjelenítés átrendezése a ábrán látható módon.  
    ![Csatlakoztatott autók - megjelenítések](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Sikerült beállítani a "Művelet járműveket" valós idejű jelentést. Folytathatja a következő valós idejű jelentés létrehozása vagy leállítása itt, és állítsa be az irányítópulton. 

### <a name="2-vehicles-requiring-maintenance"></a>2. a járműveket karbantartási megkövetelése
  
Kattintson a ![hozzáadása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) új jelentés hozzáadásához nevezze **"járműveket igénylő karbantartási"**

![Csatlakoztatott autók - karbantartási igénylő járműveket](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Jelölje ki a **vin** mező, és a képi megjelenítési típus módosítása a **kártyára**.  
    ![Csatlakoztatott autók - Vin kártyás megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Van még egy az adatkészlet "MaintenanceLabel" nevű mezőt. Ez a mező is kell egy értéknek "0" vagy "1"." Az Azure gépi tanulási modellenként részeként megoldás kiépítése és a valós idejű elérési integrálva van állítva. "1" érték azt jelzi, hogy egy gépjármű karbantartást igényel. 

Karbantartási megköveteli járműveket adatok megjelenítésére szolgáló **Lapszintű** szűrő felvétele: 

1. Húzza az **"MaintenanceLabel"** mező **szint**szűrőknek.  
![Csatlakoztatott autók - lapszintű szűrők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. **Az alapvető szűrés** menü alsó részén MaintenanceLabel lap szint szűrő bemutató parancsra.  
![Autók - csatlakozik az alapvető szűrés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Állítsa a szűrő értékét 1- **re**    
![Csatlakoztatott autók - szűrési érték](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Kattintson az új képi megjelenítés hozzáadása üres területre.  

A képi megjelenítések jelölje ki a **Csoportosított oszlop típusú diagram**  
![Csatlakoztatott autók - Vind kártyás megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Csatlakoztatott autók – csoportosított oszlop típusú diagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Húzza a **modell** mezőt a **tengely** területre, **Vin** **érték** területre. Képi megjelenítés szerint **vin számát**a rendezéshez.  **Diagramcím** **"Járműveket modellenként karbantartási igénylő"** módosítása  

**Vin** mezőket húzni a **Színtelítettség** **mezők** jelen ![mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) lap **Megjelenítés** csoportjában  
![Csatlakoztatott autók - színtelítettség](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

**Adatok** színének **formátuma** szakaszban a képi megjelenítések  
A Minimum színének módosítása: **F2C812**  
A Maximum színének módosítása: **FF6300**  
![Autók - színe megváltozik a csatlakoztatott](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Autók – új képi megjelenítés színek csatlakoztatott](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Kattintson az új képi megjelenítés hozzáadása üres területre.  

Jelölje ki a **csoportosított oszlop típusú diagram** a képi megjelenítéseket, **vin** mező **értéke** területre húzni, húzza **Város** mezőt a **tengely** területre. **"Vin számát"**diagram rendezéséhez. **Diagramcím** **"Járműveket település szerint karbantartási igénylő"** módosítása   
![Csatlakoztatott autók - járműveket igénylő karbantartási település szerint](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Kattintson az új képi megjelenítés hozzáadása üres területre.  

Jelölje ki a **Többsoros kártyás** megjelenítés a képi megjelenítéseket, **az adatmodelleket** és **vin** húz a **mezők** területen.  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Újrarendezés, mind a megjelenítést, a végleges jelentést a következőképpen néz ki:  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Sikerült beállítani a "Járműveket igénylő karbantartás" valós idejű jelentést. Folytathatja a következő valós idejű jelentés létrehozása vagy leállítása itt, és állítsa be az irányítópulton. 

### <a name="3-vehicles-health-statistics"></a>3. a járműveket állapot statisztika
  
Kattintson a ![hozzáadása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) új jelentés hozzáadásához nevezze **"járműveket egészségügyi statisztikák"**  

**Nyomtáv** képi megjelenítések jelölje ki, és húzza a **sebesség** mező **érték, minimális érték, a maximális érték** területre.  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Módosítsa az alapértelmezett összesítés **sebesség** **érték** területen **átlagos** 

Az alapértelmezett összesítés **sebesség** **minimális** területen **minimálisra** módosítása

**Maximális** **sebesség** **maximális** területen az alapértelmezett összesítés módosítása

![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Nevezze át a **Nyomtáv cím** **"Átlagos sebesség"** 
 
![Csatlakoztatott autók - nyomtáv](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Kattintson az új képi megjelenítés hozzáadása üres területre.  

Hasonlóképpen adja hozzá az **átlagos motor olaj**, **átlagos üzemanyag**és **átlag motor mérsékelt** **űrszelvény** .  

Rakszelvénye módosítása **"Átlagos sebesség"** minden szembőségmérő fölött, egy mező az alapértelmezett összesítés lépéseit.

![Csatlakoztatott autók - szelvények](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Kattintson az új képi megjelenítés hozzáadása üres területre.

Válassza a **Vonal és csoportosított oszlopdiagram** a megjelenítést, majd húzza a **Megosztott tengely**, húzza a **sebessége**, **tirepressure és engineoil mezők** **Oszlopbeli értékek** területre, a **Város** mező átállítása összesítési típusukra **átlagos**. 

Húzza a **engineTemperature** mező **Sor értékek** területre, és módosítsa az összesítést típus **átlagos**. 

![Csatlakoztatott autók - megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

A diagram **címének** módosítása **"átlagos sebessége, autógumit nyomása, motor olaj**és motor hőmérsékleti".  

![Csatlakoztatott autók - megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Kattintson az új képi megjelenítés hozzáadása üres területre.

**Fatérkép** képi megjelenítések jelölje ki, húzza a **minta** mezőben a **csoport** területre, és a **MaintenanceProbability** mezőt az **értékek** területre.

A diagram **címének** módosítása **"gépjármű**levő karbantartási igénylő".

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Kattintson az új képi megjelenítés hozzáadása üres területre.

Válassza ki **100 %-ig halmozott sáv diagram** képi megjelenítés, húzza a **Város** mezőt a **tengely** területre, és húzza a **MaintenanceProbability** **RecallProbability** mezők **értékét** területére.

![Csatlakoztatott autók – új képi megjelenítés hozzáadása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Kattintson a **Formátum**gombra, jelölje be az **Adatok szín**és a **MaintenanceProbability** beállítani a **"F2C80F"**értékre.

A diagram **címének** módosítása **"valószínűség a gépjármű karbantartási & visszahívása által város"**.

![Csatlakoztatott autók – új képi megjelenítés hozzáadása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Kattintson az új képi megjelenítés hozzáadása üres területre.

Jelölje ki a képi megjelenítések képi megjelenítés **Területdiagram** , húzza a **modell** mezőt a **tengely** területre, és húzza a **engineOil, tirepressure, sebességétől és MaintenanceProbability** mezőket az **értékek** területre. Saját összesítő típusának módosítása **"Átlagos"**. 

![Csatlakoztatott autók - összesítési típusának módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

A diagram címének módosítása **"Motor olaj átlagos, nyomása, sebességétől és karbantartás valószínűség modellenként megunja"**.

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Kattintson az üres területre, új képi megjelenítés hozzáadása:

1. Jelölje ki **Pont diagram** képi megjelenítések.
2. A **Részletek** és a **Jelmagyarázat** területén húzza a **minta** mezőben.
3. Húzza a **üzemanyag** mező az **x tengely** területre, és módosítsa az összesítést **átlagos**.
4. **EngineTemparature** húzza az **y tengely**területre, majd az összesítés **átlagos**
5. Húzza a **vin** mező **méret** területére.


![Csatlakoztatott autók – új képi megjelenítés hozzáadása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Módosítsa **a diagramcím** **"Üzemanyag átlagokat, motor hőmérsékleti modellenként"**.

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

A végleges jelentést fog kinézni, alább látható módon.

![Csatlakoztatott autók végleges jelentést](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>A valós idejű irányítópult a jelentéseket a PIN-kód megjelenítések
  
Hozzon létre egy üres dashboard irányítópultok mellett pluszjel ikonra kattintva. "Telemetriai Analytics műszerfal" is elnevezése

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

A megjelenítés, a fenti jelentésekből az irányítópult PIN kódot. 
 
![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Az irányítópult következőképpen kell kinéznie, amikor a három jelentések jönnek létre, és a megfelelő megjelenítések rögzített vannak az irányítópulton. Ha az összes jelentés nem hozott létre, az irányítópult ábrája különböző. 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gratulálok! A valós idejű irányítópult sikeresen létrehozott. Miközben még tovább CarEventGenerator.exe és RealtimeDashboardApp.exe végre, látnia kell élő frissítések az irányítópulton. Az alábbi lépések elvégzéséhez körülbelül 10 – 15 perccel kell vennie.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>A Power BI köteg feldolgozás irányítópult beállítása

>[AZURE.NOTE] Az adatvégrehajtás Befejezés és generált adatok értékű év folyamat végpont parancsfájl folyamat körülbelül két óra (a sikeres befejezésétől helyezési) vesz igénybe. Így megvárja, amíg a feldolgozása előtt hajtsa végre a következő lépésekkel befejezéséhez. 

**A Lekérdezéstervező PowerBI-fájl letöltése**

-   Egy előre konfigurált PowerBI Lekérdezéstervező fájl a telepítés része
-   Kattintson a diagram nézetben a PowerBI csomópontra, és kattintson a Tulajdonságok ablaktábla hivatkozásra **a PowerBI Lekérdezéstervező fájl letöltése**![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Mentés helyi meghajtóra

**Állítsa be a PowerBI-jelentések**

-   Nyissa meg a Lekérdezéstervező fájlt "VehicleTelemetryAnalytics - asztali Report.pbix" PowerBI asztali változatában. Ha Ön nem rendelkezik, telepítse a PowerBI asztali [PowerBI asztali telepítése](http://www.microsoft.com/download/details.aspx?id=45331). 

-   A **lekérdezés szerkesztése**gombra.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Kattintson duplán a **forrás**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Frissítse a kiszolgáló kapcsolati karakterláncot a telepítés részeként használ kiépítve Azure SQL-kiszolgálóval. Kattintson a diagram az Azure SQL csomópontra, és megtekintheti a Tulajdonságok ablaktábla a kiszolgáló nevét.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- **Adatbázis** *connectedcar*hagyja.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Kattintson az **OK gombra**.
- Ekkor megjelenik a **Windows hitelesítő adatait** lapon által választott alapértelmezett, módosíthatja az adatbázis **hitelesítő adatok** **adatbázis** lap jobb oldali gombjával.
- Adja meg a **felhasználónevét** és a **jelszót** az Azure SQL-adatbázis megadott a telepítés során.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Kattintson a **Csatlakozás**
- Ismételje meg a fenti lépéseket a jelen jobb oldali három hátralévő lekérdezésnél, és frissítse a az adatforrás részletei kapcsolat.
- Kattintson a **Bezárás gombra, és töltse be**. A Power BI Desktop fájl adatkészleteket SQL Azure-adatbázistáblák kapcsolódik.
- **Bezárása** A Power BI Desktop fájlt.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Kattintson a **Mentés** gombra a módosítások mentéséhez. 
 
Beállította, hogy a köteg feldolgozás út a megoldást megfelelő jelentéseihez. 


## <a name="upload-to-powerbicom"></a>Töltse fel a *powerbi.com*
 
1.  Nyissa meg a PowerBI webhely portál http://powerbi.com, és jelentkezzen be.
2.  Kattintson az **adatok beolvasása**  
3.  Töltse fel a Power BI Desktop fájlt.  
4.  Ha szeretne, kattintson **fájlok Get-adatok beolvasása > -> helyi fájl**  
5.  Nyissa meg azt a **"VehicleTelemetryAnalytics – asztali Report.pbix"**  
6.  Miután a fájl feltöltésekor, átláthassák visszatér a Power BI-munkaterület.  

Egy adathalmaz, jelentés és egy üres dashboard létrejön meg.  
 

A meglévő irányítópult **A Power BI** **Telemetriai Analytics műszerfal** PIN diagramok. Az előbb létrehozott üres irányítópult elemre, és nyissa meg a **jelentések** csoportban kattintson az imént feltöltött jelentés.  

![Gépjármű Telemetriai PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Megjegyzés: a jelentés hat lapok foglalja magában:**  
Az oldal-1: Gépjármű önéletrajzba:  
Lap 2: Valós idejű gépjármű állapota  
Lap 3: Újrahasznosítását alapú járműveket   
Oldal 4: Járműveket idézni  
Oldal 5: Hatékony alapú járműveket üzemanyag  
Lap 6: Contoso embléma  

![Csatlakoztatott autók PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Lap 3**PIN-kódot a következőket:  

1.  VIN száma  
    ![Csatlakoztatott autók PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Újrahasznosítását alapú járműveket modellenként – vízesés diagram  
    ![Gépjármű Telemetriai - 4-es PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Weblap-5**PIN-kódot a következő: 
 
1.  Vin száma    
    ![Gépjármű Telemetriai - 5 PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Üzemanyag-hatékony járműveket modellenként: csoportosított oszlop típusú diagram  
    ![Gépjármű Telemetriai - PIN-kód diagramok 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Lap 4**, a PIN-kódot a következő:  

1.  Vin száma  
    ![Gépjármű Telemetriai – 7 PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Település szerint, visszahívott járműveket modell: Fatérkép  
    ![Gépjármű Telemetriai - 8 PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Lap 6**PIN-kódot a következőket:  

1.  A Contoso motorok embléma  
    ![Gépjármű Telemetriai – 9-es PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Az irányítópult rendszerezése**  

1.  Nyissa meg azt az irányítópulton
2.  Mutasson a minden diagram, és nevezze át az alábbi képen a teljes irányítópult megadott elnevezési alapját. Is mozgás a diagramok megjelenésével megegyező kialakítással szeretné a lenti irányítópult.  
    ![Gépjármű Telemetriai - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![Gépjármű Telemetriai PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Ha létrehozott az összes jelentés említett a dokumentumban, a végleges elkészült irányítópult az alábbi ábrán így néz. 

![Gépjármű Telemetriai - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gratulálok! Sikeresen létrehozta a jelentések és a valós idejű, a cserélendő nyereség és köteg gépjármű állapot és a vezetői háttérismeretek az irányítópulton szokások.  
