<properties
    pageTitle="Terv létrehozása az Azure egymást fedő |} Microsoft Azure"
    description="Szolgáltatás rendszergazdái, amely lehetővé teszi, hogy a előfizetők rendelkezést virtuális gépeken futó terv létrehozása"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="create-a-plan-in-azure-stack"></a>Azure egymást fedő terv létrehozása

Csoportosítás egy vagy több szolgáltatás [csomagok](azure-stack-key-features.md#services-plans-offers-and-subscriptions) . Egy szolgáltatóként tervezi, hogy felajánlja a bérlők hozhat létre. A csomagok és szolgáltatások tartalmazzák a ajánlatok a bérlők fizessen elő. Ez a példa bemutatja, hogyan hozhat létre egy másik csomagra a számítási, a hálózati és a tárhely erőforrás szolgáltatók tartalmazza. Ez a terv előfizetők az azt jelenti, hogy virtuális gépeken futó kiépítése adja vissza.

1.  Böngészőben nyissa meg azt a https://portal.azurestack.local.

2.  [Jelentkezzen be](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator) az Azure Papírhalom portálra szolgáltatás rendszergazdái és a írja be a szolgáltatás rendszergazdai hitelesítő adatait (az Ön által létrehozott lépés 5 [futtassa a PowerShell](azure-stack-run-powershell-script.md) szakasz során fiók), és kattintson a **Bejelentkezés**gombra.

    Szolgáltatás-rendszergazdák a ajánlatok és csomagok hozhat létre, és a felhasználók kezelése.

3.  Terv és ajánlatot, amely a bérlők feliratkozhat létrehozni, kattintson az **Új** > **bérlői kínál + csomagok** > **csomagot**.

    ![](media/azure-stack-create-plan/image01.png)

4.  Az **Új terv** lap töltse ki a **Megjelenítendő név** és az **Erőforrás neve**. A megjelenítendő név a terv rövid nevet, amelyet a bérlők. Csak a rendszergazda láthatja, hogy az erőforrás nevét. A nevet, amelyet a rendszergazdák használata a terv Azure erőforrás-kezelő erőforrásként.

    ![](media/azure-stack-create-plan/image02.png)

5.  Hozzon létre egy új **Erőforráscsoport**, vagy jelöljön ki egy meglévőt elhelyezése a terv (például "OffersAndPlans")

    ![](media/azure-stack-create-plan/image02a.png)

6.  Kattintson a **szolgáltatások**, jelölje be a **Microsoft.Compute** **Microsoft.Network**és **Microsoft.Storage**, és **kattintson a**.

    ![](media/azure-stack-create-plan/image03.png)

7.  Kattintson a **kvóták**, **Microsoft.Storage (helyi)**, kattintson és majd jelölje ki az alapértelmezés szerinti kvóta vagy **létrehozása új kvóta** kvóta testreszabásához kattintson.

    ![](media/azure-stack-create-plan/image04.png)

8.  Írjon be egy nevet a kvóta, **Kvóta beállításai**parancsra, állítsa be a kvóta értékeket kattintson az **OK gombra**, és kattintson a **Létrehozás**gombra.

    ![](media/azure-stack-create-plan/image06.png)

9. Kattintson a **Microsoft.Network (helyi)**, majd jelölje ki az alapértelmezés szerinti kvóta, vagy kattintson a **Létrehozás új kvóta** kvóta testreszabása.

    ![](media/azure-stack-create-plan/image07.png)

10. Írjon be egy nevet a kvóta, **Kvóta beállításai**parancsra, állítsa be a kvóta értékeket kattintson az **OK gombra**, és kattintson a **Létrehozás**gombra.

    ![](media/azure-stack-create-plan/image08.png)

11. Kattintson a **Microsoft.Compute (helyi)**, majd jelölje ki az alapértelmezés szerinti kvóta, vagy kattintson a **Létrehozás új kvóta** kvóta testreszabása.

    ![](media/azure-stack-create-plan/image09.png)

12.  Írjon be egy nevet a kvóta, **Kvóta beállításai**parancsra, állítsa be a kvóta értékeket kattintson az **OK gombra**, és kattintson a **Létrehozás**gombra.

    ![](media/azure-stack-create-plan/image10.png)

13. A **kvóták** lap kattintson az **OK gombra**, és az **Új csomag** lap, kattintson a terv **létrehozása** .

    ![](media/azure-stack-create-plan/image11.png)

14. Jelenik meg az új csomag, kattintson az **összes erőforrás**, majd keresése a csomagot, és kattintson a nevére.

    ![](media/azure-stack-create-plan/image12.png)

## <a name="next-steps"></a>Következő lépések

[Az ajánlat létrehozása](azure-stack-create-offer.md)
