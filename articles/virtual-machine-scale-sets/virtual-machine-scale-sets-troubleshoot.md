<properties
    pageTitle="Virtuális gép skála értékkészletet Automatikus méretezéssel elhárítása |} Microsoft Azure"
    description="Hárítsa el a virtuális gép skála értékkészletet Automatikus méretezéssel. Ismerje meg tipikus problémáival és azok megoldását."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Automatikus méretezéssel bíró virtuális gép méretezés – hibaelhárítás

**A probléma** – létrehozott egy autoscaling infrastruktúra az Azure erőforrás-kezelő segítségével virtuális skála beállítása – például egy sablont, így üzembe helyezése: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – a meghatározott méretarány szabályok van, és azt nagyszerű, azzal a különbséggel, hogy mennyi helyezi el a VMs a betöltés, függetlenül Automatikus méretezéssel nem.

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

Megfontolandók a következők:

- Hány magmintákat minden virtuális rendelkezik, és minden egyes core betöltése?
 A fenti példa Azure quickstart útmutató sablon do_work.php parancsfájl, amelyek egyetlen alapszintű betölti van. A nagyobb, mint például Standard_A1 vagy D1 virtuális méret egyetlen alapszintű virtuális használata majd volna futtatásához szükséges a betöltés többször. Ellenőrizze, hogy hány cores a VMs megtekintésével [Windows méretű virtuális gépeken futó Azure-ban](../virtual-machines/virtual-machines-windows-sizes.md)

- A virtuális skála megadása a hány VMs módon, mindegyik munkát?

    Esemény ki skálával csak kerül sor, ha átlagát Processzor **összes** a VMs végig a skála megadása a küszöbértéket meghaladja időbeli a belső Automatikus méretezéssel szabályok definiált.

- DID kihagy skála eseményeket?

    Jelölje be a naplók az Azure-portálon skála eseményekre vonatkozóan. Esetleg volt skálával felfelé és lefelé, amely skálával elmulasztott. "Skála" szerint szűrhető.

    ![Naplókat][audit]

- A méretezés és a méretezési küszöbértékek eléggé másik vannak?

    Tegyük fel, ha át kívánja méretezni amikor átlagos Processzor értéke nagyobb, mint 50 %-kal több mint 5 percig tart, és a esetén 50 %-nál kisebb az átlag Processzor méretarányra szabály beállítása. Ez esetben problémához "flapping" esetén processzorhasználata közelébe ezt a küszöbértéket skála műveleteinek folyamatosan növeli, és a beállítás méretének csökkentése. Emiatt a Automatikus méretezéssel szolgáltatás próbálja megakadályozhatja, hogy a "flapping", amelyek szerint nem méretezés cikkét is. Ezért ellenőrizze, hogy a méretezési és skála küszöbértékek eléggé másik néhány SZÓKÖZ elhelyezésével méretezés engedélyezése.

- DID írása saját JSON sablon?

    Nagyon egyszerűen hibát követ, ezért készítéséhez sablont is, mint az egyik, amely fölött van igazolt dolgozhat, és a kis növekményes módosítása. 

- Akkor manuálisan méretezheti vagy kicsinyítés?

    Próbálja meg újbóli a virtuális skála megadása erőforrás módosítani szeretné a számát VMs manuálisan különböző "kapacitás" beállítást. Egy példa sablon ehhez itt van: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – szüksége lehet, hogy a gép azonos méretű amely használja a skála megadása a sablon szerkesztése. Ha VMs számának manuálisan sikeresen módosítása, majd tudja Automatikus méretezéssel okozza a problémát.

- Jelölje be a Microsoft.Compute/virtualMachineScaleSet, és az [Azure erőforrás Explorer](https://resources.azure.com/) Microsoft.Insights erőforrások

    Ez a egy nélkülözhetetlen hibaelhárító eszköz, amely jelzi, hogy az erőforrás-kezelő Azure erőforrások állapotát. Kattintson az előfizetés, és tekintse meg az erőforráscsoport hibaelhárítási. A számítási erőforrás-szolgáltató a tekintse meg a virtuális skála megadása létrehozott és a példány nézet, amely jelzi, hogy a telepítés állapotának. A virtuális skála megadása a VMs példány nézetének is ellenőrizhető. Ezután mappába érkeznek a Microsoft.Insights erőforrás-szolgáltató, és ellenőrizze, hogy az Automatikus méretezéssel szabályok megfelelőek.

- A diagnosztikai bővítmény dolgozik, és a teljesítményadatok vezérlés?

    __Frissítés:__ Azure Automatikus méretezéssel továbbfejlesztett egy alapú host mértékek folyamat, amely már nem kell telepíteni kell egy diagnosztika bővítmény használatára. Ez azt jelenti, hogy a következő néhány bekezdések már nem érinti hoz létre egy autoscaling alkalmazást az új folyamat. Azure sablonok használata a host folyamat konvertált példája: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Mérőszámok alapú host használatának Automatikus méretezéssel célszerűbb az alábbi okok miatt:

    - Kevesebb mozgó elemeket, nincs diagnosztika bővítmények telepítésére van szükség.
    - Egyszerűbb sablonok. Csak az összefüggéseket Automatikus méretezéssel szabályokat adni egy meglévő méretarányra beállított sablon.
    - Megbízhatóbb jelentése, és új VMs gyorsabb indítása.

    Érdemes lehet diagnosztikai bővítményében megőrzése a csak okok lenne, van-e szükség memória diagnosztika jelentéskészítés/méretezését. Mérőszámok alapú Host memória nem jelentést.

    Annak szem előtt csak ha, kövesse a cikk további részében diagnosztikai bővítmények továbbra is használja a autoscaling.

    Automatikus méretezéssel az Azure erőforrás-kezelő is dolgozhat (de nem rendelkezik) segítségével egy virtuális bővítmény nevű diagnosztika kiterjesztése. Azt a Teljesítményadatok megadása a sablonban tárterület-fiókhoz bocsát ki. Az adatok majd összesíti, a Azure Monitor szolgáltatás.

    Ha a hírcsatornájában szolgáltatás adatokat nem lehet beolvasni a VMs, küldjön e-mailben – például, mintha a VMs lefelé, ezért érdemes az (egy megadott létrehozása az Azure-fiók esetén) szolgáltatásbeli értendő.

    Léphet, és keresse meg az adatokat. Nézze meg a egy felhőalapú Intézővel Azure tárterület-fiókot. Példa a [Visual Studio felhő Intéző](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2)használatával jelentkezzen be, és válassza az Azure-előfizetést használ, és a diagnosztikai tároló fióknév hivatkozott diagnosztika bővítmény meghatározása a telepítési sablon.

    ![Felhőalapú Explorer][explorer]

    Ekkor megjelenik az alábbi kitöltenem, az egyes virtuális adatait tároló tábla. A legutóbbi sorok Linux és a Processzor mérőszám Példaként véve, nézze meg. A Visual Studio felhő Intéző támogatja a egy lekérdezési nyelv, így például a lekérdezést futtat "időbélyeg gt datetime'2016 – 02-02T21:20:00Z" ", hogy Ön a legutóbbi események (feltételezve, hogy ideje UTC szerint megadva). Nem látható nem felel meg a méretezés szabályokat állíthat be az adatokat? Az alábbi példában a gép 20 Processzor lépések 100 %-át az utolsó 5 perc növelése.

    ![Tárterület-táblázatokban][tables]

    Ha az adatok nem létezik, majd ezt azt jelenti, a probléma nem fut a VMs diagnosztikai kiterjesztésű. Ha az adatokat, akkor azt jelenti, vagy probléma a méretezés szabályokról, illetve a hírcsatornájában szolgáltatással van. [Azure állapotának](https://azure.microsoft.com/status/)ellenőrzése.

    Miután magára a fenti lépéseket, ha továbbra is problémákba Automatikus méretezéssel sikerült nézzen körül a fórumokban, [az MSDN webhelyen](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)vagy [egymásra halmozni túlcsordulás](http://stackoverflow.com/questions/tagged/azure), vagy jelentkezzen tanácsadás. A sablon és a teljesítményadatok nézetének megosztása lesz.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
