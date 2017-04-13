<properties
    pageTitle="Automatikus méretezéssel a Microsoft Azure virtuális gépeken futó, a Felhőszolgáltatások és a Web Apps alkalmazások áttekintése |} Microsoft Azure"
    description="A Microsoft Azure Automatikus méretezéssel áttekintése. Virtuális gépeken futó, a Felhőszolgáltatások és a Web Apps alkalmazások vonatkozik."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Automatikus méretezéssel a Microsoft Azure virtuális gépeken futó, a Felhőszolgáltatások és a Web Apps alkalmazások áttekintése

A cikkből megtudhatja, milyen Microsoft Azure Automatikus méretezéssel előnyeit, és hogyan kezdjek hozzá az azt.  

Azure Monitor Automatikus méretezéssel csak [Virtuális gép skála beállítása](https://azure.microsoft.com/services/virtual-machine-scale-sets/), a [Cloud Services](https://azure.microsoft.com/services/cloud-services/)és az [Alkalmazás szolgáltatás – Web Apps alkalmazások](https://azure.microsoft.com/services/app-service/web/)érinti.

>[AZURE.NOTE] Azure kétféleképpen Automatikus méretezéssel tartalmaz. Az Automatikus méretezéssel egy régebbi verziójának virtuális gépeken futó (olyan elemhalmazokhoz elérhetőség) vonatkozik. Ez a funkció korlátozott támogatás, és azt javasoljuk, gyorsabb és megbízhatóbb Automatikus méretezéssel ügyfélszolgálatával virtuális skála csoportján áttelepítése. Ez a cikk a régebbi technológia használatát hivatkozás szerepel.  


## <a name="what-is-autoscale"></a>Mi az Automatikus méretezéssel?

Automatikus méretezéssel lehetővé teszi, hogy miként kezelje a betöltés az alkalmazást futtató erőforrások megfelelő mennyiségű. Lehetővé teszi erőforrások kezelésére nő a betöltés és is mentheti a pénzt erőforrások, amelyeket a rendszer ülő eltávolításával üresjárati. Akkor adja meg, hogy példányok futtatása és hozzáadása vagy eltávolítása a szabályhalmaz alapján automatikusan VMs minimális és maximális számát. Arról, hogy egy minimális gyártmányú kellene az alkalmazás mindig fut még nincs a terhelés alatt. A legnagyobb korlátozza a összköltségét lehetséges óránként. Automatikus méretezése hoz létre szabályok használatával e két érték között.

 ![Automatikus méretezéssel magyarázata. Hozzáadás és eltávolítás VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Ha szabály feltételek teljesülése esetén egy vagy több Automatikus méretezéssel műveletek induljanak. Hozzáadása és eltávolítása VMs, vagy egyéb műveletek elvégzését. A következő szemléltető ábra bemutatja ezt a folyamatot.  

 ![Magyarázó rész Automatikus méretezéssel munkafolyamat-Diagram](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Automatikus méretezéssel folyamat magyarázata
A következő magyarázatot alkalmazása az előző diagram részei.   

### <a name="resource-metrics"></a>Erőforrás-mértékek
Erőforrások elhelyezése a mértékek, amelyek később feldolgozása szabályokat. Mértékek származnak, különféle módon keresztül.
Virtuális skála beállítása Azure diagnosztika ügynökök telemetriai adatokat használ, mivel a Web Apps alkalmazások és a Cloud services telemetriai származik, közvetlenül az Azure infrastruktúra. A gyakran használt statisztikai adatokat processzorhasználata, a memóriahasználat, a szál száma, a várólista hossza és a lemezterület tartalmazza. Hogy milyen telemetriai adatokat is használhatja, című témakör [Automatikus méretezéssel közös mértékek](insights-autoscale-common-metrics.md).

### <a name="time"></a>Idő
Ütemezés szabályok UTC alapulnak. Be kell állítani az időzóna megfelelően a szabályok beállításakor.  

### <a name="rules"></a>Szabályok
A diagram csak egy Automatikus méretezéssel szabály látható, de többen is. Szükség szerint a helyzet az összetett egymást átfedő szabályok hozhat létre.  A szabály típusai  

 - **Metrikus-alapú** – például 50 %-nál processzorhasználata esetén végezze el ezt a műveletet.
 - **Időalapú** – például egy adott időzónájának a szombat minden 8 am webhook eseményindító.

Metrikus szabályok mérésére alkalmazás betöltése, és adja hozzá és VMs, hogy a betöltés alapján. Ütemezés szabályok engedélyezése, ha át kívánja méretezni, amikor idő mintázatok látható a betöltés, és szeretné méretezni előtt egy lehetséges betöltés növelése vagy csökkentése fordul elő.  


### <a name="actions-and-automation"></a>Műveletek telepített és automatizálási

Szabályok műveletek egy vagy több típusú válthat.

- **Méretezés** - skála VMs vagy kicsinyítés
- **E-mail** – Küldés e-mailt előfizetés rendszergazdák, a további-rendszergazdák és/vagy a megadott további e-mail címre
- **Via webhooks automatizálás** - hívás webhooks, amelyek összetett több művelet belülről és kívülről Azure válthat. Azure-belül elindíthatja az Azure automatizálási runbook, Azure függvény, vagy Azure logikájának alkalmazás. Példa kívüli Azure 3 fél URL-cím szolgáltatásai, például tartalékidő és Twilio tartalmazza.


## <a name="autoscale-settings"></a>Automatikus méretezéssel beállításai
Automatikus méretezéssel használja az alábbi kifejezések és a struktúra.

- Az **Automatikus méretezéssel beállítás** határozza meg, hogy ha át kívánja méretezni, felfelé vagy lefelé a Automatikus méretezéssel motor által olvasható. Egy vagy több profilok, a cél erőforrás, és az értesítési beállítások információt tartalmaz.
    - Az **Automatikus méretezéssel profil** beállítását, szabályokat, a eseményindítók és a profil és a megismétlődését skála tevékenységek kapacitása kombinációi. Beállíthatja, hogy több profil, amelyek lehetővé teszik a gondoskodik a különböző egymást átfedő követelményeket.
        - **Kapacitás beállítás** azt mutatja, a minimum, maximum és alapértelmezett értékének példányainak száma. [1 fügekrém használni a megfelelő helyre]
        - **Szabály** tartalmazza az eseményindító – metrikus az eseményindító vagy a időt az eseményindító – és a méretezés műveletet, jelezve, hogy Automatikus méretezéssel kell méretezni, felfelé vagy lefelé, ha ez a szabály teljesül.
        - **Ismétlődés** azt jelzi, hogy mikor Automatikus méretezéssel kell hatályba profil. Lehet különböző Automatikus méretezéssel profilok a különböző időpontot, vagy a hét, például.
- **Értesítés beállítása** azt határozza meg, milyen értesítések történjen az Automatikus méretezéssel beállítás profilok egyikének a feltételeknek eleget tevő alapján Automatikus méretezéssel esemény bekövetkezésekor. Automatikus méretezéssel is értesíteni kell egy vagy több e-mail címet, vagy hogy hívásokat kezdeményezzenek egy vagy több webhooks.

![Azure Automatikus méretezéssel beállítást, a profil és a szabály struktúra](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

A teljes listát konfigurálható mezők és leírását a [Automatikus méretezéssel REST API](https://msdn.microsoft.com/library/dn931928.aspx)érhető el.

Példák kódot.

* [Erőforrás-kezelő sablonok használata a virtuális skála eredménye a speciális Automatikus méretezéssel konfiguráció](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automatikus méretezéssel REST API-val](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Vízszintes és függőleges méretezése

Automatikus méretezéssel vízszintesen megnöveli csak értékekhez erőforrások, amely növekedéséhez (",") vagy a virtuális példányok száma ("a") csökkentése.  Beosztását, vízszintes rugalmasabb felhő helyzetben lehetővé teszi, hogy futtassa a potenciálisan terhelés kezelésére VMs ezer, amely. Függőleges méretezés különbözik. Továbbra is VMs ugyanannyi, de több ("felfelé") vagy annál kisebb ("lefelé") hatékony, lehetővé teszi a virtuális. A Power mértékegysége memóriahasználat, a Processzor sebességét, lemezterületet, stb.  Függőleges méretezés további korlátokkal rendelkezik. A rendelkezésre álló régió változhat, és gyorsan találatok nagyobb hardver- és a felső határ függő. Függőleges méretezés is általában virtuális leállítása és a szükséges lépések. További tudnivalókért olvassa el a [függőlegesen méretezni az Azure automatizálási készülék Azure virtuális](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md)című témakört.


## <a name="methods-of-access"></a>Az access módszerek
Beállíthatja a Automatikus méretezéssel keresztül

- [Azure portál](insights-how-to-scale.md)
- [A PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Platformok parancssor (CLI)](insights-cli-samples.md#autoscale )
- [Azure Monitor REST API-val](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Az Automatikus méretezéssel támogatott szolgáltatások


| Szolgáltatás                              | Séma és dokumentumok                                       |
|--------------------------------------|-----------------------------------------------------|
| Web Apps alkalmazások                             | [Méretezési Web Apps alkalmazások](insights-how-to-scale.md)              |
| Cloud Services                       | [Automatikus méretezéssel egy felhőalapú szolgáltatásba](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuális gépeken futó: klasszikus           | [Méretezési klasszikus virtuális gép elérhetőségének beállítása](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuális gépeken futó: Windows skála beállítása| [A Windows virtuális skála méretezési beállítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtuális gépeken futó: Linux skála állítja be.  | [Linux virtuális skála méretezési beállítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuális gépeken futó: Windows példa   | [Erőforrás-kezelő sablonok használata a virtuális skála eredménye a speciális Automatikus méretezéssel konfiguráció](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Következő lépések

Automatikus méretezéssel kapcsolatos további információért használata a Automatikus méretezéssel forgatókönyvek korábban már szerepel a listában, vagy olvassa el az alábbi forrásokat:

- [Azure Monitor Automatikus méretezéssel közös mérőszámok](insights-autoscale-common-metrics.md)
- [Gyakorlati tanácsok az Azure Monitor Automatikus méretezéssel](insights-autoscale-best-practices.md)
- [Küldés e-mailek és webhook értesítések Automatikus méretezéssel műveletek segítségével](insights-autoscale-to-webhook-email.md)
- [Automatikus méretezéssel REST API-val](https://msdn.microsoft.com/library/dn931953.aspx)
- [Hibaelhárítási virtuális gép skála készletek Automatikus méretezéssel](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
