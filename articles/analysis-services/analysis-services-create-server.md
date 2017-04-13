<properties
   pageTitle="Az Analysis Services-kiszolgáló létrehozása az Azure-ban |} Microsoft Azure"
   description="További információ az Analysis Services-kiszolgálói példány létrehozása az Azure-ban."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Hozzon létre egy Analysis Services-kiszolgáló
Ez a cikk végigvezeti új Analysis Services-kiszolgáló erőforrás létrehozása az Azure-előfizetésben.

## <a name="before-you-begin"></a>Első lépések
Első lépések a következők szükségesek:

- **Azure előfizetés**: látogatás [Azure ingyenes próbaverziót](https://azure.microsoft.com/offers/ms-azr-0044p/) hozhat létre fiókot.
- **Erőforráscsoport**: egy erőforráscsoport már van, vagy [Hozzon létre egy újat](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Az Analysis Services-kiszolgáló létrehozása új számlázható szolgáltatás eredményezhet. További tudnivalókért olvassa el a Analysis Services árak című témakört.

## <a name="create-an-analysis-services-server"></a>Hozzon létre egy Analysis Services-kiszolgáló

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. Kattintson a **+ Új** > **üzletiintelligencia- + analytics** > **Analysis Services**.

3. Az **Analysis Services** lap töltse ki a szükséges mezőket, és nyomja le az **létrehozása**.

    ![Kiszolgáló létrehozása](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Kiszolgáló neve**: írja be egy egyedi nevet a kiszolgáló hivatkozni.

    - **Előfizetés**: Jelölje ki azt az előfizetést, ezen a kiszolgálón való váltók stb.

    - **Erőforráscsoport**: ezek a tárolók segítséget nyújt az Azure erőforrások kezelésében. További tudnivalókért lásd: az [erőforrás csoportok](../resource-group-overview.md).

    - **Hely**: Ez Azure adatközponthoz helyen tárolja a kiszolgálón. Válasszon egy helyet a legnagyobb felhasználói alap legközelebbi.

    - **Árak réteg**: jelölje be a árak réteg. Táblázatos modellek legfeljebb 100 GB támogatottak. A árak réteg később bármikor módosíthatja.

4. Kattintson a **létrehozása**gombra.

Hozzon létre egy perc; általában tesz gyakran pár másodpercre. Ha **portálra hozzáadása**lehetőséget választotta, nyissa meg az új kiszolgáló megjelenítéséhez a portálon. Másik lehetőségként nyissa meg azt a **További szolgáltatások** > **Analysis Services** tekintheti meg, ha készen áll-e a kiszolgáló. Ha nem látszik, frissítse a listát.

 ![Irányítópult](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Következő lépések
Miután létrehozta a kiszolgáló, azt is megteheti [rendszerbe állítják a modellt](analysis-services-deploy.md) , SSDT használatával vagy SSMS.

Ha a modellt, a kiszolgáló beállítaná a helyszíni adatforrások csatlakozik, kell egy [helyszíni adatok átjáró](analysis-services-gateway.md) telepíteni egy olyan számítógépen, a hálózaton.
