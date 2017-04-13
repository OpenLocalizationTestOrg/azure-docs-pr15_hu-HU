<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Hálózati irányelvek |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások hálózati megfontolandó szempontok"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások hálózati megfontolandó szempontok

## <a name="how-to-select-an-azure-virtual-network"></a>Az Azure virtuális hálózat kijelölése
Az alábbi útmutatásokat segítséget jelöljön ki egy virtuális hálózatot Azure Active Directory tartományi szolgáltatások használatára.

### <a name="type-of-azure-virtual-network"></a>Azure virtuális hálózati típusa

- Azure Active Directory tartományi szolgáltatások klasszikus Azure virtuális hálózatban engedélyezheti.

- Azure az Active Directory tartományi szolgáltatások is a **nem engedélyezhető az Azure erőforrás-kezelő használatával létrehozott virtuális hálózatok**.

- Erőforrás-kezelő-alapú virtuális hálózati csatlakozhat, amelyben engedélyezve van az Azure Active Directory tartományi szolgáltatások klasszikus virtuális hálózat. Ezt követően a virtuális erőforrás-kezelő-alapú hálózati Azure Active Directory tartományi szolgáltatások is használhatja. További tudnivalókért lásd: a [hálózati kapcsolat](active-directory-ds-networking.md#network-connectivity) szakaszban.

- **Területi virtuális hálózatok**: Ha egy meglévő virtuális hálózathoz használni kívánja, győződjön meg arról, hogy az területi virtuális hálózat.

    - A régi affinitás csoportok mechanizmusa használó virtuális hálózatok az Azure Active Directory tartományi szolgáltatások nem használhatók.

    - Azure Active Directory tartományi szolgáltatások, [a területi virtuális hálózatokhoz régebbi virtuális hálózatokat](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)használni.


### <a name="azure-region-for-the-virtual-network"></a>A virtuális hálózat Azure terület

- A felügyelt tartomány telepítve van a virtuális hálózat azonos Azure régióban Azure Active Directory tartományi szolgáltatások úgy dönt, hogy a szolgáltatás engedélyezése.

- Azure Active Directory tartományi szolgáltatások által támogatott Azure területen jelölje ki a virtuális hálózat.

- Lásd: a [régió szerint Azure szolgáltatások](https://azure.microsoft.com/regions/#services/) lap tudnivalók a Azure régiók, amelyben Azure Active Directory tartományi szolgáltatások érhető el.


### <a name="requirements-for-the-virtual-network"></a>A virtuális hálózat vonatkozó követelmények

- **Az Azure munkaterhelésekből közelében**: válassza ki a virtuális hálózat, amely jelenleg tárolja/fog tárolni a virtuális gépeken futó, amely az Azure Active Directory tartományi szolgáltatások hozzáféréssel kell rendelkeznie.

- **Egyéni/hozása-a-saját DNS-kiszolgálók**: győződni arról, hogy nincsenek-e nincs egyéni DNS-kiszolgálók be van állítva a virtuális hálózat.

- **Meglévő tartományok azonos nevű tartomány**: Győződjön meg arról, hogy ilyen nevű tartományban érhető el, hogy virtuális hálózaton meglévő tartomány nem rendelkezik. Tegyük fel például, már elérhető "contoso.com" nevű a a kijelölt virtuális hálózat tartományt. Később megpróbálja engedélyezni az Azure Active Directory tartományi szolgáltatások felügyelt tartomány azonos nevű tartományban (azaz "a contoso.com), hogy virtuális hálózaton. Hibát tapasztal meg az Azure Active Directory tartományi szolgáltatások. Ez a hiba oka az, hogy a tartomány nevét a virtuális hálózat az ütközések feloldása. Ebben az esetben az Azure Active Directory tartományi szolgáltatások felügyelt tartomány beállításának kell használnia egy másik nevet. Másik megoldás, vonja kiépítése a meglévő tartomány, és ezután folytassa Azure Active Directory tartományi szolgáltatások engedélyezése.

> [AZURE.WARNING] Miután engedélyezte a szolgáltatás nem aktiválhatók tartományi szolgáltatások virtuális másik hálózathoz.


## <a name="network-security-groups-and-subnet-design"></a>Hálózati biztonsági csoportokat és a alhálózat Tervező
A [Hálózati biztonsági csoport (NSG)](../virtual-network/virtual-networks-nsg.md) , amely lehetővé teszi, vagy megtagadja a hálózati forgalmat a virtuális példányaiban virtuális hálózatban Access vezérlőelem-lista (vezérlés) szabályok listáját tartalmazza. Lehet, hogy NSGs alhálózat vagy a alhálózat egyes virtuális példányokat társított. Ha egy NSG alhálózat társítva, vezérlés szabályokat alkalmazni alhálózat a virtuális példányok. Ezeken kívül az egyes virtuális forgalom korlátozva lehet egy NSG közvetlenül az, hogy virtuális társításával további.

![Ajánlott alhálózat Tervező](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Gyakorlati tanácsok a alhálózat kiválasztása
- Telepítse az Azure Active Directory tartományi szolgáltatások **külön-külön alhálózathoz** a Azure virtuális hálózaton belül.

- A felügyelt tartomány dedikált alhálózathoz NSGs nem vonatkoznak. Ha NSGs dedikált alhálózathoz kell alkalmazni, győződjön meg arról, **ne tiltsa le a portokat, szolgáltatási és kezeli a tartomány szükséges**.

- Nem túl korlátozzák IP-címek érhető el a felügyelt tartománya a dedikált alhálózat számát. Ezt a korlátozást megakadályozza, hogy a szolgáltatás két tartomány vezérlő érhető el a felügyelt tartomány.

- **Az átjáró alhálózat az Azure Active Directory tartományi szolgáltatások nem engedélyezi** a virtuális hálózat.


> [AZURE.WARNING] Amikor társíthatja az Azure Active Directory tartományi szolgáltatások, amelyben alhálózat egy NSG engedélyezve van, e, előfordulhat, hogy a Microsoft azt jelenti, hogy a szolgáltatás, és kezelheti a tartomány zavarhatják. Emellett a Azure AD-bérlő és a felügyelt tartomány közötti szinkronizálás megszakadt. **A SLA környezetekben, ahol egy NSG alkalmazása, amely letiltja a Azure Active Directory tartományi szolgáltatások frissítése és kezelése a tartomány nem vonatkozik.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Az Azure Active Directory tartományi szolgáltatások szükséges portok
Az alábbi portokat az Azure Active Directory tartományi szolgáltatások szolgáltatáshoz szükségesek, és a felügyelt tartomány karbantartása. Győződjön meg arról, hogy a következő portokat nem blokkolja a alhálózat, amelyben engedélyezte a felügyelt tartománya.

| Port száma | Cél |
|---|---|
| 443-as | A Azure AD-bérlő való szinkronizálás |
| 3389 | A tartomány kezelésére |
| 5986 | A tartomány kezelésére |
| 636 | A felügyelt tartomány biztonságos hozzáférés LDAP (LDAPS) |



## <a name="network-connectivity"></a>A hálózati kapcsolat
Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány engedélyezhető csak egy egyetlen klasszikus Azure virtuális hálózaton belül. Erőforrás-kezelő Azure alapján létre virtuális hálózatok nem támogatottak.


### <a name="scenarios-for-connecting-azure-networks"></a>Azure hálózatok összekapcsolásának felhasználási területei
Csatlakozás Azure virtuális hálózatok bármelyik az alábbi telepítési jelenik meg a felügyelt tartomány használatára:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>A felügyelt tartomány használata az egynél több Azure klasszikus virtuális hálózati
Más klasszikus Azure virtuális hálózatok csatlakozhat, amelyben az Azure Active Directory tartományi szolgáltatások engedélyezte Azure klasszikus virtuális hálózat. A virtuális Magánhálózati kapcsolat lehetővé teszi a felügyelt tartomány használata a munkaterhelésekből más virtuális hálózatok rendszerbe.

![Klasszikus virtuális a hálózati kapcsolat](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>A felügyelt tartomány használata az erőforrás-kezelő-alapú virtuális hálózati
Erőforrás-kezelő-alapú virtuális hálózati csatlakozhat, amelyben az Azure Active Directory tartományi szolgáltatások engedélyezte Azure klasszikus virtuális hálózat. A kapcsolat lehetővé teszi a felügyelt tartomány használata a munkaterhelésekből, az erőforrás-kezelő-alapú virtuális hálózatban rendszerbe.

![Erőforrás-kezelő klasszikus virtuális a hálózati kapcsolat](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Hálózati kapcsolat beállításai

- **Webhely virtuális Magánhálózati kapcsolat útján VNet-VNet kapcsolatok**: virtuális hálózat egy másik virtuális hálózathoz csatlakozik (VNet VNet) hasonlít egy helyszíni hely kapcsolódás virtuális hálózat. Mindkét kapcsolat típusát a virtuális Magánhálózati átjáró segítségével adja meg a biztonságos alagutas szolgáltatással IPsec /.

    ![Virtuális Magánhálózati átjáró segítségével virtuális a hálózati kapcsolat](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [További információ - virtuális Magánhálózati átjáró segítségével virtuális hálózatok csatlakoztatása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **Virtuális hálózati peering VNet-VNet hibák**: virtuális hálózati peering egy mechanizmusa ugyanabban a régióban két virtuális hálózatokhoz kapcsolódó Azure Gerinchez hálózaton keresztül. Miután peered, a két virtuális hálózat egyik minden kapcsolat célra jelennek meg. Továbbra is külön erőforrásként kezelése, de saját IP-címek segítségével közvetlenül az alábbi virtuális hálózatok virtuális gépeken futó kommunikálhat egymással.

    ![Virtuális hálózati kapcsolat peering használatával](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [További információ – ezek olyan virtuális hálózati peering](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Kapcsolódó tartalom

- [Azure virtuális hálózati peering](../virtual-network/virtual-network-peering-overview.md)

- [A klasszikus telepítési modell VNet-VNet kapcsolat beállítása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)
