<properties
    pageTitle="Azure API Management szolgáltatás példány több Azure területre telepítéséről"
    description="Megtudhatja, hogy miként példány Azure API Management szolgáltatás üzembe több Azure területre." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Azure API Management szolgáltatás példány több Azure területre telepítéséről

API-kezelés több területre telepítési, amely lehetővé teszi a kívánt Azure régiók tetszőleges számú egyetlen API management szolgáltatás elosztása API közzétevők támogatja. Ez csökkentheti a kérelem által észlelt késés földrajzilag elosztott API fogyasztói és is javítja a szolgáltatások elérhetőségét, ha egy terület offline állapotba kerül. 

Kezdetben létrehozás egy API alkalmazáskezelési szolgáltatás csak egy [egység][] tartalmaz, és egy egyetlen, az elsődleges régió kijelölt Azure területen található. További régiók egyszerűen felvehetők Azure klasszikus portálon keresztül. API adatkezelési átjáró server telepítve van az egyes régiók és hívás forgalom átirányítja a legközelebbi átjáró. A terület offline állapotba kerül, a forgalom esetén automatikusan újra irányított, az a következő legközelebbi átjáró. 

> [AZURE.IMPORTANT] Több elem régió telepítési a **[prémium][]** réteg csak érhető el.

## <a name="add-region"> </a>Példány API Management szolgáltatás üzembe új területére

Első lépésként kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ekkor megjelenik az API kezelése a publisher-portálra.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

Nyissa meg a API Management szolgáltatás példányának Azure klasszikus portálon **mérete** fülre. 

![Méret lap][api-management-scale-service]

Új területére üzembe helyezéséhez kattintson a legördülő listában, az elsődleges terület alatt, és válassza ki a listából egy területet.

![Régió hozzáadása][api-management-add-region]

Miután a régió ki van jelölve, válassza ki a mennyiségek az új terület a egység legördülő listából.

![Adja meg az erőforrás-mennyiség][api-management-select-units]

Miután a kívánt régiók és erőforrás-mennyiség van beállítva, kattintson a **Mentés**gombra.

## <a name="remove-region"> </a>Egy területről API Management szolgáltatás példány törlése

Példány API Management szolgáltatás eltávolítása a régió, nyissa meg a API Management szolgáltatás példányának Azure klasszikus portálon **mérete** fülre. 

![Méret lap][api-management-scale-service]

Kattintson az **X** távolítsa el a kívánt régió jobbra.  

![Terület eltávolítása][api-management-remove-region]

Miután a kívánt régiók törlődik, kattintson a **Mentés**gombra.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[Azure API-kezelés – első lépések]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[egység]: http://azure.microsoft.com/pricing/details/api-management/
[Támogatás]: http://azure.microsoft.com/pricing/details/api-management/

