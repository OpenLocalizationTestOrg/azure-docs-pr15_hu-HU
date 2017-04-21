<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Azure CLI használatával

Az alábbi lépésekkel segítséget Azure CLI egyszerűen használata a legújabb verzió és a TNÉV előfizetést. Ha szeretne telepítse az Azure CLI és a fiókhoz csatlakozni, olvassa el a az [Azure parancssori kezelőfelületről Azure](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Lépés: 1: Frissítés Azure CLI verziója

Azure CLI szolgáltatás módjával feltétlenül szükséges parancsok használatához kell friss verziójával rendelkezik, ha lehetséges. Annak ellenőrzéséhez, azon verziójával, írja be a `azure --version`. Meg kell jelennie hasonló:

    $ azure --version
    0.8.17 (node: 0.10.25)

Ha azt szeretné, az Azure CLI verzióját, akkor olvassa el az [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Lépés: 2: Azure-fiók és előfizetés beállítása

Ha a használni kívánt fiókkal az Azure CLI csatlakozott, előfordulhat, hogy egynél több előfizetés. Tesz, érdemes áttekintenie, ha az előfizetésekhez érhető el a fiókjához beírásával `azure account list`, és válassza az előfizetés, írja be a használni kívánt `azure account set <subscription id or name> true` hol található a _Előfizetés azonosítója vagy nevét_ , hogy az előfizetés azonosítója vagy az előfizetés nevét, az aktuális munkamenetben dolgozni szeretne. Meg kell jelennie egy üzenetet, az alábbihoz hasonló:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Ha még nem rendelkezik az Azure-fiók, de az MSDN-előfizetésbe előfizetése van, ingyenes érheti el [az alábbi MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) –, vagy aktiválásával Azure credits ingyenes a számlát használhatja. Vagy az Azure access működni fog.
