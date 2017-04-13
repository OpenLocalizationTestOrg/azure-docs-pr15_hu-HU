<properties
    pageTitle="Azure API-kezelés virtuális Magánhálózati kapcsolat beállítása"
    description="Megtudhatja, hogy miként beállítása a virtuális Magánhálózati kapcsolat az Azure API-kezelési és az access web services keresztül."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Azure API-kezelés virtuális Magánhálózati kapcsolat beállítása

API-kezelés VPN-támogatás lehetővé teszi, hogy csatlakozzon a API adatkezelési átjáró Azure virtuális hálózathoz (klasszikus). Ebben a csoportban adhatja API-kezelés az ügyfelek számára, hogy biztonságosan csatlakoztathassa a kódmentes webes szolgáltatásokhoz, amelyek a helyszíni, vagy nem érhetők el más módon nyilvános internetkapcsolat.

>[AZURE.NOTE] Azure API-kezelési algoritmusával klasszikus VNETs. A klasszikus VNET létrehozásával kapcsolatos további tudnivalókért lásd: a [Create (klasszikus), az Azure Portal segítségével virtuális hálózat](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). A klasszikus VNETs kapcsolódás ARM VNETS további tudnivalókért lásd [az új VNets a csatlakozás a klasszikus VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Engedélyezése virtuális Magánhálózati kapcsolat

>Virtuális Magánhálózati kapcsolatot a **prémium** és **fejlesztői** rétegek csak érhető el. Átválthat arra, hogy az [Azure klasszikus portálon][] nyissa meg a API alkalmazáskezelési szolgáltatás, és nyissa meg a **mérete** fülre. Az **Általános** területen jelölje be a prémium réteg, és kattintson a Mentés gombra.

Ahhoz, hogy a virtuális Magánhálózati kapcsolatot, az [Azure klasszikus portálon][] nyissa meg a API alkalmazáskezelési szolgáltatás, és kattintson a **beállítás** fülre. 

A virtuális Magánhálózati csoportban váltson **virtuális Magánhálózati kapcsolatot** **a**.

![Lap API-kezelési-példányok konfigurálása][api-management-setup-vpn-configure]

Most már láthatja a lista összes régiók, ahol a API alkalmazáskezelési szolgáltatás már kiépítve.

Jelölje ki a virtuális Magánhálózati és alhálózat minden területhez tartozik. A lista VPN adatai van kitöltve alapján a virtuális Azure-előfizetése rendelkezésre álló hálózatok beállítása a régióban állítja be.

![Jelölje ki a virtuális Magánhálózati][api-management-setup-vpn-select]

Kattintson a **Mentés** elemre a képernyő alján. Nem fogja tudni közben frissítése az API-kezelés szolgáltatása egyéb műveletek elvégzésére az Azure klasszikus portálról. A szolgáltatás átjáró továbbra is elérhető, és nem lesz hatással futtatókörnyezet hívásokat.

Figyelje meg, hogy az átjáró virtuális címének módosítása-e minden alkalommal, amikor VPN engedélyezve van-e.

## <a name="connect-vpn"> </a>Kapcsolódás virtuális Magánhálózati mögött webszolgáltatás

Után a VPN Használatát a API-kezelési szolgáltatáshoz csatlakozik, a virtuális hálózaton belül webszolgáltatásokhoz elérése jelentkezés nem különbözik nyilvános szolgáltatások eléréséhez szükséges. Csak írja be a helyi címet vagy a webszolgáltatás állomásnév (Ha a DNS-kiszolgáló az Azure virtuális hálózat lett beállítva) **webes szolgáltatás URL-cím** mezőjébe egy új API létrehozásakor vagy Szerkesztés egy meglévőt.

![Virtuális Magánhálózati API hozzáadása][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Szükséges portokat API-kezelés VPN ikon

API Management szolgáltatás példány egy VNET helyezkedik el, amikor a rendszer az alábbi táblázat a portokat használja. Ha az alábbi portokat blokkolt, előfordulhat, hogy a szolgáltatás nem működik megfelelően. Egy vagy több letiltott portokhoz, akkor a leggyakoribb helytelen konfigurálása probléma egy VNET az API-kezelés használatakor.

| Port                      | Szövegirány        | Protokoll | Cél                                                          | Forrás / cél              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443-as                      | Bejövő          | TCP                | Ügyfél kapcsolati API-kezelés                           | INTERNETES / VIRTUAL_NETWORK        |
| 80,443                       | Kimenő         | TCP                | Azure tárolására és Azure Service Bus API Management függés | VIRTUAL_NETWORK VAGY INTERNET        |
| 1433                         | Kimenő         | TCP                | SQL API-kezelés függőségek                               | VIRTUAL_NETWORK VAGY INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Kimenő         | TCP                | A szolgáltatás Bus API-kezelés függőségek                       | VIRTUAL_NETWORK VAGY INTERNET        |
| 5671                         | Kimenő         | AMQP               | Esemény központi házirend napló API-kezelés függőség            | VIRTUAL_NETWORK VAGY INTERNET        |
| 6381, 6382, 6383             | Bejövő/kimenő | UDP                | A gyorsítótár vgx.dll API-kezelés függőségek                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Kimenő         | TCP                | Mely Számjegy az Azure fájlmegosztáson API Management típusú függés            | VIRTUAL_NETWORK VAGY INTERNET        |

## <a name="custom-dns"> </a>Egyéni DNS server telepítése

API-kezelés Azure szolgáltatások számos függ. Amikor egy API Management szolgáltatás példány egy VNET, ahol az egyéni DNS-kiszolgáló használják a üzemelteti, kell Azure szolgáltatások állomásnevekké megoldható. Kérjük, kövesse [az](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) útmutató egyéni DNS-beállítási.  

## <a name="related-content"> </a>Kapcsolódó tartalom


* [Hozzon létre egy virtuális hálózati hely közötti virtuális Magánhálózati kapcsolatot az Azure klasszikus portálon][]
* [Azure API-kezelés hívások nyomon követheti az API megtekintő használatáról][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure klasszikus portál]: https://manage.windowsazure.com/

[Hozzon létre egy virtuális hálózati hely közötti virtuális Magánhálózati kapcsolatot az Azure klasszikus portálon]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Azure API-kezelés hívások nyomon követheti az API megtekintő használatáról]: api-management-howto-api-inspector.md
