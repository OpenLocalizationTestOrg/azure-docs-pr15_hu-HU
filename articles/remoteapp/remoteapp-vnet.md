
<properties
    pageTitle="Azure RemoteApp használata az Azure VNET érvényesítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként az Azure VNET Azure RemoteApp használata használatra kész állapotának ellenőrzése"
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



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Azure RemoteApp használata az Azure VNET ellenőrzése

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Mielőtt használni egy Azure VNET az Azure RemoteApp, érdemes lehet a VNET érvényesítéséhez. Ezzel az elemzéssel való kapcsolódási problémák megelőzése.

Az Azure VNET érvényesítésére, tegye a következőket:

1. Hozzon létre egy Azure virtuális gép az Azure RemoteApp használni kívánt Azure VNET, a alhálózat belül.

2. A **Csatlakozás** beállítás az adatkezelési portál használatával, hogy virtuális kapcsolatot.
3. Bekapcsolódás a virtuális gép Azure RemoteApp használni kívánt ugyanahhoz a tartományhoz. A helyszíni hálózaton kapcsolódó hibrid gyűjtemény hoz létre, ha a virtuális gép csatlakozik a helyi tartományhoz.

Ha sikeres, az Azure VNET készen áll a RemoteApp használata.

Többet szeretne tudni a végpontok közötti hibrid webhelycsoport munkafolyamatot a következő cikkekben talál:

- [A virtuális hálózat megtervezése Azure RemoteApp](remoteapp-planvnet.md)
- [A hibrid webhelycsoport létrehozása](remoteapp-create-hybrid-deployment.md)
- [Azure RemoteApp webhelycsoport telepíteni a Azure virtuális hálózaton (készült ExpressRoute támogatása)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
