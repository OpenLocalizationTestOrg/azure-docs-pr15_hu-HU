<properties 
   pageTitle="Egy statikus, nyilvános IP-, erőforrás-kezelő az Azure portál használatával egy virtuális telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként üzembe VMs egy statikus, nyilvános IP-, az erőforrás-kezelő a zure portál használatával"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>A virtuális egy statikus, nyilvános IP-, az Azure portálon terjesztése

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Hozzon létre egy virtuális statikus nyilvános IP-cím 

Az Azure-portálon statikus nyilvános IP-cím egy virtuális létrehozásához kövesse az alábbi lépéseket.

1. Böngészőben, nyissa meg az [Azure portálra](https://portal.azure.com) , és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. A portál baloldali felső sarkában kattintson az **Új**>>**kiszámítania**>**Windows Server 2012 R2 adatközponthoz**.
3. A **telepítési modell kiválasztása** listában jelölje ki az **Erőforrás-kezelő** , és kattintson a **Létrehozás**gombra.
4. Az **alapvető tudnivalók** lap adja meg a virtuális alább látható módon, és kattintson **az OK**gombra.

    ![Azure portál – az alapok](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. A **Válassza ki a méret** lap az **A1 szabványos** kattintson alább látható módon, és kattintson a **Jelölje ki**.

    ![Azure portál - válassza ki a méretet](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. A **Beállítások** lap kattintson a **nyilvános IP-címet**, majd a **Létrehozás nyilvános IP-címét** a lap, a **hozzárendelés**, kattintson a **statikus** alább látható módon. És kattintson **az OK**gombra.

    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. A **Beállítások** lap kattintson **az OK gombra**.
8. Az **összefoglaló** lap, olvassa el alább látható módon, és kattintson **az OK**gombra.

    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Figyelje meg az új mozaik az irányítópulton.

    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Amikor a virtuális létrejött, a **Beállítások** lap jelenik meg alább látható módon

    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)