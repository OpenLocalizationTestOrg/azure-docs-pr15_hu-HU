<properties
    pageTitle="A külső Azure service költségek megértéséhez |} Microsoft Azure"
    description="Tudjon meg többet a számlázás a külső szolgáltatások – korábbi neve piactér, díjak Azure-ban."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>

# <a name="understand-your-azure-external-service-charges"></a>A külső Azure service költségek ismertetése

Ez a cikk ismerteti a számlázás a külső szolgáltatások Azure-ban. Külső szolgáltatások hívható piactér rendelések szolgál. Külső szolgáltatások szolgáltatás független gyártók által nyújtott, de Azure ökológiai belül teljes mértékben integrálva. Megtudhatja, hogy miként:

- Külső szolgáltatások azonosítása
- Megértéséhez, hogy miben különbözik a számlázási Azure-más forrásokból
- Megtekintése és nyomon követni a külső szolgáltatások használatát, felmerülés költségek
- Külső szolgáltatásban rendelés, és hogyan fizet értük kezelése

## <a name="what-are-azure-external-services"></a>Mik azok a Azure külső szolgáltatások?

Külső szolgáltatások használatával hívható Azure piactérről. Általánosságban elmondható harmadik fél érhető el az Azure közzétett szolgáltatásokat is legyenek. Például az ClearDB és SendGrid az Azure-ban vásárolhat, de nem a Microsoft által közzétett külső szolgáltatások is.

### <a name="identify-external-services"></a>Külső szolgáltatások azonosítása

Amikor, létrehozhatnak egy új külső szolgáltatás vagy az erőforrás, figyelmeztetés jelenik meg:

![Piactér beszerzési figyelmeztetés](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Külső szolgáltatások, amelyek nem Microsoft cégek által közzétett, de néha Microsoft-termékek is kategóriába sorolt külső szolgáltatások.

### <a name="external-services-are-billed-separately"></a>Külső szolgáltatások vannak számlát kapni külön-külön

Külső szolgáltatások belül az Azure előfizetés egyes rendelések számít. Az egyes szolgáltatásokhoz a számlázási időszak legördülő listában válassza a szolgáltatás beszerzésekor. Nem tud összezavarodik, a számlázási időszak az előfizetés, amelyek alapján vásárolta. Külön számlák is tartalmaz, és a hitelkártya külön-külön terheli.

### <a name="each-external-service-has-a-different-billing-model"></a>Egyes külső szolgáltatás rendelkezik-e egy másik számlázási modell

Egyes szolgáltatások számlát befizetések módon kapni, miközben mások modell használata olyan havi fizetési alapján. Hitelkártyát van szüksége az Azure külső szolgáltatások, a számla fizetési külső szolgáltatások nem lehet vásárolni.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>Havi ingyenes credits nem használható a külső szolgáltatások

Ha egy [ingyenes credits](https://azure.microsoft.com/pricing/spending-limits/)tartalmazó Azure-előfizetést használ, azok külső szolgáltatás számlák nem alkalmazható. Külső szolgáltatások vásárlása hitelkártya használata.

## <a name="view-external-service-spending-and-history"></a>Nézet külső szolgáltatás kiadások és előzmények

Az egyes előfizetések belül az [Azure portált](https://portal.azure.com/)a külső szolgáltatásokat listájának megtekintése: 

1. Jelentkezzen be az [Azure portálját](https://portal.azure.com/) , és [Nyissa meg azt a **Számlázás** lap](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![A központi menüben válassza a számlázás](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. Az **előfizetés költségek** szakaszban jelölje be az előfizetést, és meg szeretné tekinteni. 
   
    ![Jelöljön ki egy előfizetést, az a számlázási lap](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Kattintson a **külső szolgáltatások**elemre.

    ![Külső szolgáltatások kattintson az előfizetés lap](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. Meg kell jelennie a külső szolgáltatásban rendelés, a publisher nevet, szolgáltatási réteg vásárolta, az erőforrás, és a rendelés állapota rendelkezésére bocsátotta mindegyike. Jelölje ki a múltbeli számlák látni szeretné egy külső szolgáltatás.

    ![Jelöljön ki egy külső szolgáltatást](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. Innen kiindulva megtekintése múltbeli számla összegek, beleértve a adó bontásban.

    ![Külső szolgáltatások számlázási előzmények megtekintése](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Külső szolgáltatásban rendelés fizetési módokat kezelése

Frissítse a fizetési módokat külső szolgáltatásban rendelés a [Fiók központ](https://account.windowsazure.com/).

> [AZURE.NOTE] Ha az előfizetését, munkahelyi vagy iskolai fiókkal kell, [lépjen kapcsolatba a támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) módosíthatja a fizetési mód.

1. Jelentkezzen be a [Fiók Center](https://account.windowsazure.com/) , és [Nyissa meg azt a **piactér** lap](https://account.windowsazure.com/Store)

    ![Jelölje ki a fiók központ piactér](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Jelölje be a kezelni kívánt külső szolgáltatás

    ![Jelölje be a kezelni kívánt külső szolgáltatás](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. A lap jobb oldalán kattintson a **Módosítás fizetési módot** . Ezt a hivatkozást egy másik portálon kezelheti a fizetési mód életre.
    
    ![Rendelések összesítése](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. **Információ a Szerkesztés** gombra, és kövesse a képernyőn megjelenő utasításokat követve frissítse fizetési adatait.

    ![Jelölje ki az adatok szerkesztése](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>Egy külső szolgáltatásban rendelés visszavonása

Ha meg szeretné a külső szolgáltatásban rendelés, az erőforrás az [Azure portál](https://portal.azure.com)törölni szeretne.

![Erőforrás törlése](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Segítségre van szüksége? Forduljon az ügyfélszolgálathoz.

Ha továbbra is további kérdései vannak, kérjük, a [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a problémát megoldódott gyorsan.
