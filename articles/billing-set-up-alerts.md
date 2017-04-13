<properties
    pageTitle="A Microsoft Azure-előfizetésben értesítéseket számlázási beállítása |} Microsoft Azure"
    description="Ismerteti, hogyan állíthat be értesítéseket a Azure számlán így elkerülheti a számlázási érheti."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>A Microsoft Azure-előfizetésekhez számlázási értesítések beállítása

Aggódnak mekkora az Azure előfizetéshez havonta esetén kiadások kapcsolatban? Ha a fiók rendszergazdája egy Azure-előfizetést, használhatja az Azure számlázási értesítési szolgáltatás hozzon létre testreszabott számlázási értesítések, amelyek segítségével figyelésére és számlázási tevékenység az Azure fiókok kezelése.

A szolgáltatás nem egy előnézeti szolgáltatás, így a legfontosabb dolog kell elvégeznie jelentkezzen be az azt. Keresse fel az Azure-fiók felügyeleti portál ahhoz, hogy ez a funkció [a betekintő-szolgáltatások lapon](https://account.windowsazure.com/PreviewFeatures) .

## <a name="set-the-alert-threshold-and-email-recipients"></a>A riasztások küszöb és e-mailek címzettjeinek beállítása

Miután értesítést kap az e-mailben a számlázási szolgáltatás be van kapcsolva az előfizetéshez, látogasson el a fiók portál [az előfizetések lapra](https://account.windowsazure.com/Subscriptions) . Kattintson a Lync-előfizetést, és válassza a **riasztások kategóriát**.

![][Image1]

Ezután kattintson az első szakasz **Hozzáadása riasztás** - állíthat be egy másik küszöbérték és legfeljebb két e-mail címzettnek meg minden értesítés előfizetésenként öt számlázási értesítések összesen.

![][Image2]

Értesítés hozzáadásakor, adjon meg egy egyedi nevet, válasszon egy kiadások küszöbérték, és válassza az e-mail címét, ahol riasztások küld. A küszöbértékét beállításakor a **Teljes számlázás** vagy **Pénzbeli hitelképesség** megadhatja a **Riasztási a** listából. A számlázási teljes jelzést küldi, amikor előfizetés kiadások meghaladja. Egy pénzbeli hitelképesség-értesítés küldi, amikor pénzbeli credits húzhatja a korlát alá. Pénzügyi credits általában ingyenes kísérletek és MSDN-fiókokkal társított előfizetések vonatkoznak.

![][Image3]

Azure támogatja a bármilyen e-mail cím, de nem ellenőrizze, hogy az e-mail cím együttműködik, győződjön meg róla a írta.

## <a name="check-on-your-alerts"></a>Az értesítések ellenőrzése

Értesítések beállítása után a fiók központ megjeleníti őket, és megtekintheti, hogy hány további állíthat be. Minden riasztás esetén lásd: a dátum és idő küldött, akár teljes számlázás vagy pénzbeli hitelképesség szóló értesítés, és a korlátot beállítása. A dátum és idő formátum 24 órás világidő koordinálása (UTC) szerint, és a dátumot ÉÉÉÉ-mm-dd-formátum. Kattintson a pluszjelre riasztás szerkesztésre a dokumentumtárból a listában, vagy kattintson a Kuka kattintva törölheti azt.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
