<properties
   pageTitle="Megtekintése a portálon üzembe helyezési műveleteket |} Microsoft Azure"
   description="Az Azure portal segítségével az erőforrás-kezelő-telepítési hibák feltárása ismerteti."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Az Azure Portal telepítési műveletek megtekintése

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [A PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-VAL](resource-manager-troubleshoot-deployments-rest.md)

A műveletek telepítés az Azure portálon keresztül is megtekintheti. Előfordulhat, hogy tervezheti meg megtekintése a műveletek, ha kapott hiba a telepítés során, ez a cikk koncentrál műveleteket, amelyek nem megtekintése. A portál felületet, amely lehetővé teszi, hogy megkeresését a hibákat és lehetséges javítások határozza meg.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Üzembe helyezési műveleteket használatával kapcsolatos hibák elhárítása

A telepítési műveletek megtekintéséhez kövesse az alábbi lépéseket:

1. A telepítési részt erőforráscsoport figyelje meg az utolsó telepítési állapotának. Megadhatja, hogy ezt az állapotot, további részleteket.

    ![telepítési állapota](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Ekkor megjelenik a legutóbbi telepítési előzményeket. Jelölje ki a környezetben, amely nem sikerült.

    ![telepítési állapota](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Jelölje ki a **sikertelen volt. Részletekért kattintson ide** miért nem sikerült a telepítési leírása megjelenítéséhez. Az alábbi képen a DNS-rekord nem egyedi.  

    ![a telepítés nem sikerült megtekintése](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Ez a hibaüzenet, hogy hibaelhárítási lépések elég kell lennie. Ha további információra kíváncsi kapcsolatos mely feladatok lettek befejeződött, tekinthet meg a műveletek, ahogy az alábbi lépéseket.

4. A **telepítés** lap a megtekintheti a telepítési műveleteket. Válassza a minden művelet kapcsolatban további részletekre kíváncsi.

    ![műveletek megtekintése](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    Ebben az esetben láthatja, hogy a tárterület-fiókot, virtuális hálózati és elérhetőségének beállítása sikeresen létrehozva. A nyilvános IP-cím sikertelen, és más erőforrások: a nem kísérlet történt.

5. Megtekintheti a telepítésének eseményeinek **események**kiválasztásával.

    ![események megtekintése](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Lásd: a telepítéshez eseményeket, és jelölje ki bármelyik további információt.

    ![Lásd: az események](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Használja a naplókat kapcsolatos hibák elhárítása

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Telepítés hibák megtekintéséhez kövesse az alábbi lépéseket:

1. Megtekintheti a naplókat erőforráscsoport **Naplózási naplók**kiválasztásával.

    ![Jelölje ki a naplók](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. Az a **Naplókat** lap jelenít meg az összes erőforrás csoportot a legutóbbi műveleteinek összegzését előfizetéséhez. Grafikus ábrázolása az időt és a tevékenységek állapotát és a tevékenységek listáját tartalmazza.

    ![műveletek megjelenítése](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Az adott feltételeknek kiemelése a naplókat nézetének is szűrheti. Válassza a **szűrő** a **naplókat** a lap tetején.

    ![szűrő naplók](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. A **szűrő** lap kattintson a feltételek a naplókat nézet korlátozására csak a megtekinteni kívánt műveleteket. Például szűrheti a műveleteket csak a hibák az erőforráscsoport megjelenítéséhez.

    ![szűrési beállítások megadása](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Műveletek további időtartam megadásával is szűrheti. Az alábbi képen az egy adott 20 perc időszak nézet szűrők.

    ![idő beállítása](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. A műveletek közül választhat a listában. Válassza ki a műveletet, amely tartalmazza a kívánt kutatási hiba.

    ![művelet kiválasztása](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Ekkor megjelenik az adott művelet eseményeket. Figyelje meg a **Korrelációs Azonosítót** a összesítése. Az azonosító kapcsolódó események nyomon követéséhez használják. Azt is lehet hasznos, ha használata a technikai támogatási elhárítani a problémát. Az esemény részleteit esemény közül választhat.

    ![Jelölje be az esemény](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Ekkor megjelenik az esemény részleteit. Mindenekelőtt figyelje meg a **Tulajdonságok** információt a hiba.

    ![naplózási napló részletek megjelenítése](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

A napló szűrve a következő alkalommal, amikor megtekinti azt, így előfordulhat, hogy ezeket az értékeket a nézet a műveletek bővítéséhez módosítása tárolja.

## <a name="next-steps"></a>Következő lépések

- Az adott telepítési hibák megoldása című témakörben kaphat segítséget [Azure Azure erőforrás-kezelővel erőforrások telepítésekor a gyakori hibák megoldásához](resource-manager-common-deployment-errors.md).
- Műveletek más típusú figyelheti a naplókat használatával kapcsolatos további tudnivalókért lásd: [az erőforrás-kezelő műveletek naplózási](resource-group-audit.md).
- A telepítési azt végrehajtása előtt ellenőrizendő olvassa el a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy.md)című témakört.
