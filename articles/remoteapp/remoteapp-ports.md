
<properties
    pageTitle="Lista portokat és az URL-címek whitelist az Azure RemoteApp rendszerbe a virtuális ügyfélhálózat |} Microsoft Azure"
    description="Megtudhatja, hogy mely portokat és az URL-címek kell konfigurálása az Azure RemoteApp keresztüli kommunikációt."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Engedélyezze a hozzáférést az Azure RemoteApp rendszerbe vevői virtuális hálózati portokat és URL-címek listája 

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A következő vonatkozik Azure RemoteApp felhő vagy hibrid gyűjtemény Ha telepít virtuális hálózatban (VNET). További információt a virtuális hálózatokon, olvassa el a [Virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md). Ha létrehozott egy hálózati biztonsági csoport (NSG) forgalmának korlátozása a virtuális hálózati erőforrásokat, amely Azure RemoteApp választotta, ellenőrizze, hogy az alábbiakban elérhető és engedélyezett a biztonsági házirendek a virtuális hálózaton keresztül. Hálózati biztonsági csoportok kapcsolatos további tudnivalókért olvassa el az [Mi az hálózati biztonsági csoport? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp alhálózat URL- és a végpontok hozzáférésre van szüksége: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Kimenő: TCP: 443, TCP: 10101-10175 
*    Nem kötelező – UDP: 10201-10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp ügyfelek hozzáférésre van szükségük a végpontok és URL-címek: 

Az ügyfelek az asztali számítógépek eszközökről stb, hogy a felhasználók az Azure RemoteApp gyűjteményben telepített alkalmazások csatlakozni lehet jelenti.

-  https://telemetry.RemoteApp.windowsazure.com  
-  (nem kötelező UDP-portok azok a cím) https://*.RemoteApp.windowsazure.com 
-  https://Login.Windows.NET  
-  https://Login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Kimenő: TCP: 443  
-  Választható - UDP: 3391 