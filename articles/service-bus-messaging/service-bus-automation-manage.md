<properties
    pageTitle="Azure Service Bus Azure automatizálási használatával kezelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure Service Bus kezelése az Azure automatizálási szolgáltatás használatával."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Azure Service Bus Azure automatizálási használatával kezelése

Ez az útmutató fog megismerteti a Azure automatizálási szolgáltatásai számára, és hogyan használható Azure Service Bus kezelésének egyszerűsítése érdekében.

## <a name="what-is-azure-automation"></a>Mi az Azure automatizálási?

[Azure automatizálási](../automation/automation-intro.md) az Azure szolgáltatásainak egyszerűsítése a felhőbeli adatkezelési folyamat-automatizálása és a kívánt állapotot konfigurációs. Azure automatizálási kézi, használatával ismételni, hosszabb ideig futó és hiba jobban tevékenységek automatizálható növeléséhez megbízhatóság, a hatékonyság és a time to érték a szervezet számára.

Azure automatizálási tartalmaz egy nagymértékben megbízható, könnyen hozzáférhető munkafolyamat végrehajtása motor, amely az igényeknek méretezze át. Az Azure automatizálás folyamatok is lehet megrúgni manuálisan, 3rd külső rendszerek, illetve rendszeres időközönként, hogy a tevékenységek fordulhat elő, szükség esetén pontosan.

Csökkentse a terhelést műveleti és szabadítson fel informatikai és DevOps oktatói szűkítheti az üzleti értéknövelő áthelyezésével, a felhőbeli adatkezelési feladatok Azure automatizálási automatikusan futtatható munkát.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Hogyan segíthet az Azure automatizálási Azure Service Bus kezelése?

A [Szolgáltatás Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx)segítségével szolgáltatás Bus Azure automatizálást kezelheti. Azure automatizálási belül futtathatja a PowerShell-parancsfájlokat számos a REST API szolgáltatás Bus tevékenységek végrehajtásához. Akkor is párosítás ezek REST API hívásokat az Azure automatizálás a parancsmagokat többi Azure szolgáltatás, összetett feladatok automatizálásához Azure szolgáltatásokat és a 3 külső rendszerek is.

Íme néhány példa a Azure Service Bus kezelése a PowerShell használatá:

* [Egyéni PowerShell-parancsmagok Azure Service Bus sorok kezelése](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Hogyan hozhat létre a szolgáltatás Bus sorban várakozó, témák és előfizetésekből egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hozzon létre Azure Service Bus névtér PowerShell használatával](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

A PowerShell-modult a Azure service bus található automatizálás runbooks letölthető a [PowerShell gyűjtemény](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Következő lépések

Most, hogy már megtanulta Azure automatizálási, és hogyan használható Azure Service Bus kezelése alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure automatizálási.

* Lásd az Azure automatizálási [oktatóanyag első lépések](../automation/automation-first-runbook-graphical.md)
* Lásd: [A PowerShell szolgáltatás Bus](service-bus-powershell-how-to-provision.md) kezelése
