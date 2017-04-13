<properties 
   pageTitle="Az erőforrás-kezelő az előnézeti portál használatával NSGs kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként kezelheti a exising NSGs az erőforrás-kezelő az előnézeti portál használatával"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Az előnézet portálon NSGs kezelése

> [AZURE.SELECTOR]
- [Portál](virtual-network-manage-nsg-arm-portal.md)
- [A PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Adatok beolvasása

A meglévő NSGs megtekintése, szabályok lekérése egy meglévő NSG, és megtudhatja, milyen erőforrások egy NSG társítva.

### <a name="view-existing-nsgs"></a>Meglévő NSGs megtekintése
Az előfizetés minden meglévő NSGs megtekintéséhez kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson a **Tallózás >** > **hálózati biztonsági csoportokat**.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Jelölje be a NSGs listáját a **hálózati biztonsági csoportoknak** lap.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

A **RG-NSG** erőforráscsoport NSGs listájának megtekintéséhez kövesse az alábbi lépéseket. 

1. Kattintson a **erőforrás csoportok >** > **RG-NSG** > **…**.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Erőforrások listája keresse meg az elemek megjelenítése a NSG ikonra, az **erőforrások** lap alább látható módon.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>A lista összes szabály egy NSG

Egy névvel ellátott **NSG-FrontEnd**NSG szabályok megtekintéséhez kövesse az alábbi lépéseket. 

1. A **hálózati biztonsági csoportok** lap, vagy az **erőforrások** lap alább látható kattintson a **NSG-FrontEnd**.
2. A **Beállítások** lapon kattintson a **Bejövő szabályok**elemre.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. A **Bejövő szabályok biztonsági** lap alább látható módon jelenik meg.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. A **Beállítások** lapon kattintson a **kimenő üzenetek biztonsági szabályok** olvassa el a kimenő szabályok elemre.

>[AZURE.NOTE] Alapértelmezett szabályok megtekintéséhez kattintson az **alapértelmezett szabályok** ikon, amely a szabályokat jelenít meg a lap tetején.

### <a name="view-nsgs-associations"></a>NSGs társítások megtekintése

Ha szeretné megtekinteni, milyen erőforrások a **NSG-FrontEnd** NSG társítani, kövesse az alábbi lépéseket.

1. A **hálózati biztonsági csoportok** lap, vagy az **erőforrások** lap alább látható kattintson a **NSG-FrontEnd**.
2. A **Beállítások** lapon kattintson a **alhálózat** milyen alhálózat hozzárendelve a NSG megtekintéséhez.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. A **Beállítások** lapon kattintson a **hálózati kapcsolatok** megtekintése NIC Mik a NSG van társítva.

## <a name="manage-rules"></a>Szabályok kezelése

Szabályok hozzáadása egy meglévő NSG, meglévő szabályok szerkesztése és eltávolítása a szabályok.

### <a name="add-a-rule"></a>A szabály hozzáadása

**Bejövő** forgalmat a bármely számítógép **NSG-FrontEnd** NSG akiknek engedélyezi a **443-as** port szabály hozzáadásához kövesse az alábbi lépéseket.

1. A **hálózati biztonsági csoportok** lap, vagy az **erőforrások** lap alább látható kattintson a **NSG-FrontEnd**.
2. A **Beállítások** lapon kattintson a **Bejövő szabályok**elemre.
3. A **Bejövő szabályok biztonsági** lap kattintson a **Hozzáadás**gombra. Ezután a **bejövő biztonsági szabály hozzáadása elemre** a lap, az alább látható módon, töltse ki az értékeket, és kattintson **az OK**gombra.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Néhány másodperc után az új szabály a **Bejövő szabályok** lap a látható.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>A szabály módosítása

Ha módosítani szeretné a bejövő forgalom engedélyezése az **Internet** csak az előbb létrehozott szabályt, kövesse az alábbi lépéseket.

1. A **hálózati biztonsági csoportok** lap, vagy az **erőforrások** lap alább látható kattintson a **NSG-FrontEnd**.
2. A **Beállítások** lapon kattintson az előbb létrehozott szabályt.
3. A **engedélyezése https** lap az **adatforrás** tulajdonság módosítása az alább látható módon, és kattintson a **Mentés**.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Szabály törlése

Az előbb létrehozott szabály törléséhez kövesse az alábbi lépéseket.

1. A **hálózati biztonsági csoportok** lap, vagy az **erőforrások** lap alább látható kattintson a **NSG-FrontEnd**.
2. A **Beállítások** lapon kattintson az előbb létrehozott szabályt.
3. A **https lehetővé teszi** a lap kattintson a **Törlés**gombra, és majd az **Igen**gombra.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Társítások kezelése

Az alhálózathoz NSG és NIC társíthat. Akkor is is elkülöníteni egy NSG bármely erőforrás van rendelve.

### <a name="associate-an-nsg-to-a-nic"></a>Egy NSG, hogy egy hálózati hozzárendelése

**NSG-FrontEnd** NSG **TestNICWeb1** hálózati kártya szeretne társítani, kövesse az alábbi lépéseket.

1. A **hálózati biztonsági csoportok** lap, vagy az **erőforrások** lap alább látható kattintson a **NSG-FrontEnd**.
2. A **Beállítások** lapon kattintson a **hálózati kapcsolatok** > **társítása** > **TestNICWeb1**.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Elkülöníteni egy NSG a egy hálózati kártya

**NSG-FrontEnd** a **TestNICWeb1** hálózati kártya NSG elkülöníteni, kövesse az alábbi lépéseket.

1. Kattintson az Azure portálról **erőforrás csoportok >** > **RG-NSG** > **…**  >  **TestNICWeb1**.
2. Kattintson a **TestNICWeb1** lap **módosítása biztonsági...**  > **None**.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Ez a lap segítségével a hálózati bármelyik meglévő NSG szeretne társítani.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Az alhálózathoz egy NSG elkülöníteni

A **NSG-FrontEnd** NSG elkülöníteni a **FrontEnd** alhálózat, kövesse az alábbi lépéseket.

1. Kattintson az Azure portálról **erőforrás csoportok >** > **RG-NSG** > **…**  >  **TestVNet**.
2. Kattintson a **Beállítások** lap **alhálózat** > **FrontEnd** > **hálózati biztonsági csoport** > **nincs**.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. A **FrontEnd** lap kattintson a **Mentés**gombra.

![Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Egy olyan alhálózathoz NSG hozzárendelése

Újra társítani a **NSG-FrontEnd** NSG **FronEnd** az alhálózathoz, kövesse az alábbi lépéseket.

1. Kattintson az Azure portálról **erőforrás csoportok >** > **RG-NSG** > **…**  >  **TestVNet**.
2. Kattintson a **Beállítások** lap **alhálózat** > **FrontEnd** > **hálózati biztonsági csoport** > **NSG-FrontEnd**.
3. A **FrontEnd** lap kattintson a **Mentés**gombra.

>[AZURE.NOTE] A thh NSG a **Beállítások** lap is társítása egy NSG alhálózat.

## <a name="delete-an-nsg"></a>Egy NSG törlése

Ha nem tartozik minden erőforrás egy NSG csak törölheti. Ha törölni szeretne egy NSG, kövesse az alábbi lépéseket.

1. Kattintson az Azure portálról **erőforrás csoportok >** > **RG-NSG** > **…**  >  **NSG-FrontEnd**.
2. Kattintson a **Beállítások** lap, a **hálózati kapcsolatok**.
3. Ha bármelyik NIC szerepel a listában, kattintson a hálózati, és hajtsa végre a [Dissociate egy NSG egy hálózati kártya a](#Dissociate-an-NSG-from-a-NIC)lépés: 2.
4. Ismételje meg a 3-as az egyes adaptert
5. Kattintson a **Beállítások** lap **alhálózat**.
6. Ha bármelyik alhálózat szerepel a listában, kattintson a alhálózat és 2 és 3 [-alhálózat a NSG Dissociate](#Dissociate-an-NSG-from-a-subnet)a lépések.
7. Görgetés balra az **NSG-FrontEnd** lap, majd kattintson a **Törlés** > **Igen**.

[Azure portál - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Következő lépések

- A [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) NSGs.
