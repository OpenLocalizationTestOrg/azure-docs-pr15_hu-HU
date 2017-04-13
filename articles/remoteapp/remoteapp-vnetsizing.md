
<properties
    pageTitle="Az Azure RemoteApp egy VNET adatainak méretező |} Microsoft Azure"
    description="További tudnivalók: Azure RemoteApp egy VNET rendszerrel IP-cím követelményei"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Az Azure RemoteApp egy VNET adatainak méretezése

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp virtuális hálózattal (VNET) használatakor a RemoteApp használja az alhálózathoz belüli IP-címek. A RemoteApp szolgáltatásban beosztásának alapján, kell gondoskodjon arról, hogy alhálózathoz elég IP-címek RemoteApp virtuális gépeken futó érhető el. Miközben az egyik útmutató nincs tökéletes hogyan RemoteApp dinamikusan forog be- és virtuális gépeken futó a gyűjteményben lefelé forog adni, azt segít becslése alhálózat a tartomány. Ez különösen fontos, miután RemoteApp szolgáltatás egy VNET kerül, nem tudja-e alhálózat méretének növelése RemoteApp törlése nélkül.

Az egyes RemoteApp gyűjteményre szeretné futtatni maximális kapacitással rendelkeznie kell a rendelkezésre álló 100 IP-címek. Ha van egy RemoteApp gyűjtemény a szabványos csomagban, és azt szeretné, hogy a felhasználók maximális száma 500, például kell 100 IP-címeket, hogy a webhelycsoport van. Hasonlóképpen szükséges 100 IP-címek RemoteApp gyűjteménye, amelynek 800 felhasználók egyszerű tervben. Ha kevesebb felhasználók is (kisebb, mint a maximális), a webhelycsoport használati szükséges IP-címek csökkentheti. A minimális alhálózat méret megkövetelésének 30 IP-címek (/ 27).

Olvassa el az alábbi információkra győződjön meg arról, hogy a VNET beállítva, és a munkaidő-propertly:

- [Az Azure VNET egy személyes VNET áttelepítése](remoteapp-migratevnet.md)
- [Azure RemoteApp használata az Azure VNET ellenőrzése](remoteapp-vnet.md)
