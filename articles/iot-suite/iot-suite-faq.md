<properties
  pageTitle="Gyakori kérdések Azure IoT csomagja |} Microsoft Azure"
  description="Gyakori kérdések IoT csomagja"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Gyakori kérdések IoT csomagja

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Mi a különbség az Azure-portálon erőforráscsoport törlése és azureiotsuite.com az előre beállított megoldást a Törlés elemre kattintással között?

- Ha törli a [azureiotsuite.com]az előre beállított megoldást[lnk-azureiotsuite], az előre beállított megoldás; létrehozásakor voltak kiépítve összes erőforrás törlése Ha további erőforrások hozzá az erőforráscsoport, ezek is törlődnek. 

- Ha törli az erőforráscsoport az [Azure portál][lnk-azure-portal], csak törölje az erőforrások erőforráscsoport; akkor is törölnie kell az Azure Active Directory-alkalmazás az előre beállított megoldás az [Azure klasszikus portál]társított[lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hány IoT központi példánya is kiépítése az előfizetésem? 

Tíz. Létrehozhat egy [Azure támogatási jegyek] [ link-azuresupportticket] tipp ezt a korlátot, de alapértelmezés szerint akkor is csak kiépítése tíz IoT hubok előfizetésenként, [Azure előfizetés korlátai]című témakörben ismertetett módon[link-azuresublimits]. Eredményt adja mivel minden előre beállított megoldás rendelkezések egy új IoT hubon keresztül csatlakozott, akkor is csak kiépítése legfeljebb 10 előre definiált megoldások az adott előfizetés. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Hány DocumentDB példánya is kiépítése az előfizetésem?

Ötven. Létrehozhat egy [Azure támogatási jegyek] [ link-azuresupportticket] tipp ezt a korlátot, de alapértelmezés szerint kiépítése csak az ötven DocumentDB példányok száma előfizetésenként. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hány ingyenes a Bing Maps API-khoz is kiépítése az előfizetésem?

Két. Csak két belső tranzakciók szint 1 a Bing Maps nagyvállalati csomagokhoz hozhat létre az Azure-előfizetésben. A távoli felügyeleti megoldást a belső tranzakciók szint 1. csomaggal alapértelmezés szerint már kiépítve. Emiatt csak is kiépítése legfeljebb két távoli felügyeleti megoldások az előfizetés módosítás nélkül.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Statikus térképen a távoli felügyeleti megoldást üzembe, hogy hogyan adhatok hozzá egy interaktív Bing-térkép? 
1. A Bing Maps API beszerzése vállalati QueryKey [Azure]portálról[lnk-azure-portal]: 
 1. Nyissa meg azt a hol van a vállalati a Bing Maps API az [Azure portál]erőforráscsoport[lnk-azure-portal].
 2. Kattintson az összes beállítást, majd a kulcskezelő. 
 3. Észre fogja venni két kulcsok: főkulcsos és QueryKey. Másolja az az érték QueryKey.

     > [AZURE.NOTE] Nem rendelkezik a Bing Maps API Enterprise-fiók? Hozzon létre egyet az [Azure portál] [ lnk-azure-portal] szerint gombra kattintva + új, a Bing Maps API a vállalati keresése és útmutatást követve hozzon létre.

2. A legújabb kódot lehúzható [Azure-IoT-távoli-ellenőrző][lnk-remote-monitoring-github].

3. A helyi vagy az a felhő telepítési parancssori telepítési útmutatásának megfelelően, az a tár /docs/ mappában. 

4. Cloud példányban jelenik meg a legfelső szintű mappában található vagy helyi már futtatása után a *. a telepítés során létre user.config fájlt. Nyissa meg ezt a fájlt egy szövegszerkesztőben. 

5. Ha meg szeretné jeleníteni a QueryKey kimásolt értékét a következő sort módosítása: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Létrehozhatok előre beállított megoldást, ha van Microsoft Azure-DreamSpark?
Jelenleg nem olyan megoldást hoz létre előre a a [Microsoft Azure-DreamSpark] [ lnk-dreamspark] fiókot. Jó helyen jár, létrehozhat egy [ingyenes próbaverzió Azure-fiók] [ lnk-30daytrial] csak néhány percet, amely lehetővé teszi az előre beállított megoldás létrehozása.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hogyan törölhetek egy AAD-ös bérlői webhelyet?

Lásd: Eric Golpe [törlése egy Azure AD-bérlő áttekintése a következő]blogbejegyzésből[lnk-delete-aad-tennant].

## <a name="next-steps"></a>Következő lépések

Egyéb funkciók közül egyesek és funkciók előre IoT csomagja megoldást is feltárása:

- [Előre beállított prediktív karbantartási megoldás – áttekintés][lnk-predictive-overview]
- [IoT biztonsági alapoktól][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
