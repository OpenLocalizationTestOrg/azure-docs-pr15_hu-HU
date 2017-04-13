<properties
    pageTitle="Automatikus méretezés egy felhőalapú szolgáltatásba a portálon |} Microsoft Azure"
    description="(klasszikus) Tudnivalók a klasszikus portál használatával állítsa be az automatikus méretezés szabályok egy felhőalapú szolgáltatás webes szerepkör vagy dolgozó szerepkör Azure-ban."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Hogyan kell egy felhőalapú szolgáltatásba automatikus méretezése

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-scale-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-scale.md)

Az Azure klasszikus portál skála lapon manuálisan átméretezheti a webes szerepkör vagy dolgozó szerepkör vagy engedélyezheti az automatikus méretezés Processzor betöltés vagy egy üzenet várólista alapján.

>[AZURE.NOTE] Ez a cikk a Felhőbeli szolgáltatástól webes és dolgozó szerepkörök koncentrál. Virtuális gép (klasszikus) közvetlenül létrehozásakor a egy felhőalapú szolgáltatás üzemelteti. Az adatok egy részét virtuális gépeken futó ilyen típusú vonatkozik. Egy virtuális gépeken futó elérhetősége halmazára méretezés valójában csak leállítása őket be- és kikapcsolása konfigurálnia skála alapján. További információt a virtuális gépeken futó és elérhetőségének beállítása a [virtuális gépeken futó elérhetőségének kezelése](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) című cikkben talál.

Az alkalmazás méretezési beállítása előtt vegye figyelembe az alábbi adatokat:

- Méretezés core használatát érinti. Nagyobb szerepkör-példányok további magmintákat használja. Az alkalmazás csak magmintákat belül az előfizetéshez tartozó méretezheti. Például ha előfizetése van húsz magmintákat, és az alkalmazások még a két közepes, melynek a mérete legfeljebb cloud services (négy magmintákat összesen), csak méretezheti be más felhőalapú szolgáltatás telepítés esetén az előfizetésben 16 magminták által. Lásd: a [Felhőalapú szolgáltatás méretű](cloud-services-sizes-specs.md) méretű kapcsolatban további tudnivalókat.

- Létre kell hoznia egy várólista és társítása szerepkör előtt méretezheti egy alkalmazást, egy üzenet küszöbérték alapján. További tudnivalókért lásd: [a várólista tároló szolgáltatás használata](../storage/storage-dotnet-how-to-use-queues.md).

- Úgy méretezheti információforrások, amelyek a felhőalapú szolgáltatás vannak csatolva. Erőforrások csatolása további tudnivalókért lásd: [hogyan: hivatkozás egy erőforrás egy felhőalapú szolgáltatásba](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Ahhoz, hogy az alkalmazás elérhetősége magas, győződjön meg a két vagy több szerepkör-példányok rendszerbe. További tudnivalókért olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.



## <a name="schedule-scaling"></a>Méretezés ütemezése

Alapértelmezés szerint az összes szerepkörének ne kövesse az adott ütemezett. Ezért bármely módosított beállítások alkalmazása minden időre és napok a évre mindenütt. Ha azt szeretné, telepítő a kézi és automatikus méretezés:

- Munkanapok száma
- Hétvégék
- A hét éjszakák
- A hét reggelente
- Adott dátumok
- Adott dátumtartományt.

Ebben az esetben az [Azure klasszikus portál](https://manage.windowsazure.com/) conigured a  
**Cloud Services** > **\[a felhőalapú szolgáltatás\]** > **skála** > **\[gyártási, vagy átmeneti\] ** lapot.

Kattintson a minden szerepkör meg szeretné változtatni **az időbeosztás beállítása** gombra.

![Felhőalapú szolgáltatást az automatikus méretezés alapuló ütemezés][scale_schedules]



## <a name="manual-scale"></a>Automatikus méretezés

A **méret** lapon manuálisan növelheti vagy csökkentheti hány példánya fut a egy felhőalapú szolgáltatásba. Ha létrehozott egy ütemtervet nem ez van beállítva minden egyes létrehozott ütemezés vagy mindig.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)kattintson a **Felhőszolgáltatások**, és kattintson a nevére kattintva nyissa meg az irányítópulton a felhőalapú szolgáltatás.

    > [AZURE.TIP] Ha nem látja a felhőalapú szolgáltatásba, előfordulhat, módosíthatja a **termelési** **előkészítése** és fordítva.

2. Kattintson a **méret**gombra.

3. Jelölje ki az ütemtervet meg szeretné változtatni a méretezési beállításokat. Az alapértelmezett *nem ütemezett időpontok* Ha nincs ütemezés van definiálva van.

4. Keresse meg a **metrikus skála** szakaszt, és válassza a **nincs**. Az összes szerepkörének alapértelmezett beállítása.

5. A felhőbeli szolgáltatástól minden szerepkörhöz egy csúszkájának használandó példány a számát.

    ![A felhőalapú szolgáltatások szerepkör manuális méretezése][manual_scale]

    Ha további előfordulásait, szükség lehet a [felhőalapú szolgáltatás virtuális gép méretének](cloud-services-sizes-specs.md)módosítása.

6. Kattintson a **Mentés**gombra.  
Szerepkör-példányok lesz, illetve el a megfelelő beállításokat a alapján.

>[AZURE.TIP] Amikor megjelenik ![][tip_icon] az egérmutató mozgatása, és milyen egy adott beállítás tartalmaz tájékoztatást kaphat.


## <a name="automatic-scale---cpu"></a>Automatikus méretezés - Processzor

Ez méretezze át, ha processzorhasználata hányada fölött vagy alatt megadott küszöbértékek; szerepkör-példányok létrehozott vagy törölt.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)kattintson a **Felhőszolgáltatások**, és kattintson a nevére kattintva nyissa meg az irányítópulton a felhőalapú szolgáltatás.

    > [AZURE.TIP] Ha nem látja a felhőalapú szolgáltatásba, előfordulhat, módosíthatja a **termelési** **előkészítése** és fordítva.

2. Kattintson a **méret**gombra.

3. Jelölje ki az ütemtervet meg szeretné változtatni a méretezési beállításokat. Az alapértelmezett *nem ütemezett időpontok* Ha nincs ütemezés van definiálva van.

4. Keresse meg a **metrikus skála** szakaszt, és válassza ki a **Processzor**.

5. Most már szerepkörök példányok, a cél processzorhasználata (felfelé skálával indítására) és a Ha át kívánja méretezni felfelé és lefelé szerint hány példánya minimális és maximális adattartomány is beállíthatja.

![Méretarányát egy felhőalapú szolgáltatás szerepkör betöltése processzor][cpu_scale]

>[AZURE.TIP] Amikor megjelenik ![][tip_icon] az egérmutató mozgatása, és milyen egy adott beállítás tartalmaz tájékoztatást kaphat.





## <a name="automatic-scale---queue"></a>Automatikus méretezés - várólista

Automatikusan ez méretezze át, ha az üzenetek számát fölött vagy alatt egy megadott küszöbértékét; szerepkör-példányok létrehozott vagy törölt.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)kattintson a **Felhőszolgáltatások**, és kattintson a nevére kattintva nyissa meg az irányítópulton a felhőalapú szolgáltatás.

    > [AZURE.TIP] Ha nem látja a felhőalapú szolgáltatásba, előfordulhat, hogy módosítania a **termelési** **előkészítése** és fordítva.

2. Kattintson a **méret**gombra.

3. Keresse meg a **metrikus skála** szakaszt, és válassza ki a **Processzor**.

4. Most már szerepkörök-példányok, a várólista és feldolgozása az egyes példányok, és ha át kívánja méretezni felfelé és lefelé szerint hány példánya üzenetek mennyiségét minimális és maximális adattartomány is beállíthatja.

![Egy üzenet várólista méretarányát egy felhőalapú szolgáltatás szerepkör][queue_scale]

>[AZURE.TIP] Amikor megjelenik ![][tip_icon] az egérmutató mozgatása, és milyen egy adott beállítás tartalmaz tájékoztatást kaphat.


## <a name="scale-linked-resources"></a>Csatolt erőforrások méretezése

Gyakran szerepkörbe méretezéséhez esetén hasznos, ha át kívánja méretezni, hogy az alkalmazás is használja az adatbázist. Ha az adatbázis csatolása a felhőbeli szolgáltatástól, elérheti a méretezési beállításokat az adott erőforrás a megfelelő hivatkozásra kattintva.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)kattintson a **Felhőszolgáltatások**, és kattintson a nevére kattintva nyissa meg az irányítópulton a felhőalapú szolgáltatás.

    > [AZURE.TIP] Ha nem látja a felhőalapú szolgáltatásba, előfordulhat, módosíthatja a **termelési** **előkészítése** és fordítva.

2. Kattintson a **méret**gombra.

3. Keresse meg a **csatolt erőforrások** szakaszt, és **az adatbázis-kezelés**skála kattintott.

    > [AZURE.NOTE] Ha egy **csatolt források** részben nem látható, valószínűleg nem rendelkezik olyan csatolt erőforrásokat.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
