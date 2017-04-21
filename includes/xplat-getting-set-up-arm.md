<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Azure CLI az Azure erőforrás-kezelő használatával (ARM)

Mielőtt használatba vehetné a Azure CLI az erőforrás-kezelő parancsok és a sablonok Azure erőforrások és erőforrás-csoportok használatával munkaterhelésekből telepítését, fog-fiókra van szüksége az Azure (tanfolyam). Ha nem rendelkezik fiókkal, [Itt ingyenes Azure próbaverzió](https://azure.microsoft.com/pricing/free-trial/)elérheti.

> [AZURE.NOTE] Ha még nem rendelkezik az Azure-fiók, de az MSDN-előfizetésbe előfizetése van, ingyenes érheti el [az alábbi MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) –, vagy aktiválásával Azure credits ingyenes a számlát használhatja. Vagy az Azure access működni fog.

### <a name="step-1-verify-the-azure-cli-version"></a>Lépés: 1: Az Azure CLI verziójának ellenőrzése

Azure CLI feltétlenül szükséges parancsok és a sablonok ARM használatához szükséges, hogy legalább 0.8.17 verziója. Annak ellenőrzéséhez, azon verziójával, írja be a `azure --version`. Meg kell jelennie hasonló:

    $ azure --version
    0.8.17 (node: 0.10.25)

Ha módosítania kell a Azure CLI azon verziójával, olvassa el a [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Lépés: 2: Használja a munkahelyi vagy iskolai identitás és Azure ellenőrzése

Csak használhatja a ARM parancs mód, ha egy [bérlői Azure Active Directory](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) vagy a [Szolgáltatás egyszerű nevét](https://msdn.microsoft.com/library/azure/dn132633.aspx)használja. (Ezek is nevezik *szervezeti azonosítóját*.)

Szeretné látni, ha van, jelentkezzen be a gépelés `azure login` és használatáról a munkahelyi vagy iskolai felhasználónevét és jelszavát, amikor a rendszer kéri. Ha egy, meg kell jelennie egy üzenetet, az alábbihoz hasonló:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Ha nem jelenik meg egy új bérlő (vagy a szolgáltatás fő) a Microsoft-fiók identitással kell létrehoznia. (Ez általában a helyzet személyes MSDN-előfizetések vagy ingyenes próba-előfizetések.) Hozzon létre egy munkahelyi vagy iskolai azonosító egy Microsoft-azonosító készült Azure fiókjából, olvassa el a [társíthatja az Azure Active Directory Directory új Azure-előfizetéssel](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)című témakört. Ha úgy gondolja, hogy már a szervezeti azonosítóval kell rendelkeznie, előfordulhat, forduljon a személy, aki a fiók jött létre.

### <a name="step-3-choose-your-azure-subscription"></a>Lépés 3: Válassza az Azure előfizetés

Ha csak egy előfizetéssel Azure fiókban van, Azure CLI társít magát, hogy az előfizetés alapértelmezés szerint. Ha egynél több előfizetése van, akkor kell-e válassza azt az előfizetést, írja be a használni kívánt `azure account set <subscription id or name> true` hol található a _Előfizetés azonosítója vagy nevét_ , hogy az előfizetés azonosítója vagy az előfizetés nevét, az aktuális munkamenetben dolgozni szeretne.

Meg kell jelennie egy üzenetet, az alábbihoz hasonló:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Lépés: 4: Vigye az Azure CLI ARM módban

Az Azure erőforrás-kezelés (ARM) mód használata az Azure CLI, írja be a következőt `azure config mode arm`. Meg kell jelennie egy üzenetet, az alábbihoz hasonló:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Vissza az Azure szolgáltatás felügyeleti parancsaival beírásával válthat `azure config mode asm`.
