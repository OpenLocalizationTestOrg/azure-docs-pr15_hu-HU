<properties 
   pageTitle="NSGs létrehozása az Azure portálon ARM módban |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre, és a az Azure portálon ARM NSGs terjesztése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Az Azure portálon NSGs kezelése

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Is megtekintheti [a klasszikus telepítési modell NSGs létrehozása](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

A minta PowerShell-parancsok az alábbi fejlemények már létrehozott egy egyszerű környezet alapján az alkalmazási példát. Ha szeretne a dokumentumban megjelenített parancsokat, először hozza létre a tesztkörnyezetben úgy, hogy [Ez a sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd)üzembe helyezése, kattintson a **központi telepítés az Azure**, szükség esetén az alapértelmezett paraméterértékeket cseréje és kövesse az utasításokat a portálon. Az alábbi lépésekkel **RG-NSG** használja az erőforráscsoport telepítették a sablon nevét.

## <a name="create-the-nsg-frontend-nsg"></a>A NSG-FrontEnd NSG létrehozása

Ahogy azt az alkalmazási példát **NSG-FrontEnd** NSG létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson a **Tallózás >** > **hálózati biztonsági csoportokat**.

    ![Azure portál - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. A **hálózati biztonsági csoportok** lap kattintson a **Hozzáadás**gombra.
  
    ![Azure portál - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. A **hálózati biztonsági csoport létrehozása** lap hozzon létre egy *NSG-FrontEnd* nevű *RG-NSG* erőforrás csoportjának NSG, és kattintson a **Létrehozás**gombra.

    ![Azure portál - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Szabályok létrehozása egy meglévő NSG a

Egy meglévő NSG az Azure portálról szabályok létrehozásához kövesse az alábbi lépéseket.

2. Kattintson a **Tallózás >** > **hálózati biztonsági csoportokat**.

3. NSGs listájában kattintson a **NSG-FrontEnd** > **a bejövő szabályok**

    ![Azure portál - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. A **bejövő biztonsági szabályok**listáját kattintson a **Hozzáadás**gombra.

    ![Azure portál - szabály hozzáadása](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. A **bejövő biztonsági szabály hozzáadása** lap a *web-szabály* prioritásának *200* *TCP-n* keresztül hozzáférés engedélyezése a port *80* bármely virtuális gép bármilyen forrásból származó nevű szabály létrehozása, és kattintson **az OK**gombra. Figyelje meg, hogy ezek a beállítások a legtöbb már alapértelmezett értéket.

    ![Azure portál - szabály beállításai](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Néhány másodperc után jelenik meg az új szabály a NSG a.

    ![Azure portál – új szabály](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Ismételje meg a 6- *rdp-szabály* egy *250* , akiknek engedélyezi a hozzáférést a *TCP-n* keresztül port *3389* bármely virtuális gép bármilyen forrásból származó prioritását named bejövő szabály létrehozása.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>A NSG FrontEnd az alhálózathoz hozzárendelése

1. Kattintson a **Tallózás >** > **erőforráscsoport** > **RG-NSG**.
2. Kattintson a **RG-NSG** lap **…**  >  **TestVNet**.

    ![Azure portál - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Kattintson a **Beállítások** lap **alhálózat** > **FrontEnd** > **hálózati biztonsági csoport** > **NSG-FrontEnd**.

    ![Azure portál - alhálózat beállításai](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. A **FrontEnd** lap kattintson a **Mentés**gombra.

    ![Azure portál - alhálózat beállításai](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>A NSG-Kódmentes NSG létrehozása

A **NSG-Kódmentes** NSG létrehozásához, és azt a **Kódmentes** alhálózat társítani, kövesse az alábbi lépéseket.

1. Ismételje meg [a NSG-FrontEnd NSG létrehozása](#Create-the-NSG-FrontEnd-NSG) egy NSG nevű *NSG-fájlok* létrehozása
2. Ismételje meg a [szabályok létrehozása egy meglévő NSG a](#Create-rules-in-an-existing-NSG) **Bejövő** szabályok létrehozása az alábbi táblázatban.

  	|A bejövő szabályok|Kimenő szabály|
  	|---|---|
  	|![Azure portál – a bejövő szabályok](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure portál - kimenő szabály](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Ismételje meg [a NSG FrontEnd az alhálózathoz társítani](#Associate-the-NSG-to-the-FrontEnd-subnet) a **NSG-Kódmentes** NSG társíthatja az alhálózathoz **Kódmentes** című témakör lépéseit.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogy miként [kezelheti a meglévő NSGs](virtual-network-manage-nsg-arm-portal.md)
- A [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) NSGs.