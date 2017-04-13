<properties
    pageTitle="Váltás az Azure előfizetés egy másik ajánlatra |} Microsoft Azure"
    description="Tudnivalók arról, hogy miként módosíthatja az Azure-előfizetése, és kattintson egy másik ajánlatot az előfizetés adatkezelési portál használatával"
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="genli"/>

# <a name="switch-your-azure-subscription-to-another-offer"></a>Az Azure előfizetés átválthat másik ajánlat

Egy [kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) vevőt tud váltani az Azure-előfizetése a [Fiók központ](https://account.windowsazure.com/Subscriptions)egy másik ajánlatra bizonyára. Ez a funkció használható például kihasználhatja a [havi credits Visual Studio előfizetők számára](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Ha a [Ingyenes próbaverziót](https://azure.microsoft.com/free/), ismerje meg, hogy miként [frissíthet a kirovó](billing-upgrade-azure-subscription.md).

#### <a name="whats-supported"></a>Mely szolgáltatások támogatottak:

| A                                                              | A                                                                                      |
|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [Kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Kirovó fejlesztők és tesztelése](https://azure.microsoft.com/offers/ms-azr-0023p/)              |
| [Kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          |
| [Kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio próba Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     |
| [Kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) | [MSDN platformok](https://azure.microsoft.com/offers/ms-azr-0062p/)                      |
| [Kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio vállalati](https://azure.microsoft.com/offers/ms-azr-0063p/)            |
| [Kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio vállalati (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [AZURE.NOTE] Más ajánlja fel a módosításokat, [lépjen kapcsolatba a támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
    
## <a name="switch-subscription-offer"></a>Váltás előfizetés ajánlat

> [AZURE.VIDEO switch-to-a-different-azure-offer]

1.  Jelentkezzen be a [Azure-fiók központ](https://account.windowsazure.com/Subscriptions).

2.  Válassza ki a befizetések előfizetést.

3.  Kattintson a **Váltás másik ajánlat**parancsra. A gomb csak akkor érhető el, ha kirovó és az első számlázási időszak a Kész gombra.

    ![Figyelje meg a kapcsoló ajánlat gombra a lap jobb oldalán](./media/billing-how-to-switch-azure-offer/switchbutton.png)
    
4.  **Jelölje ki a kívánt ajánlatot** ajánlatok a listában, és előfizetése bekapcsolható. Ebben a listában a fiókjához társított tagsági függ. Ha semmi nem érhető el, nézze meg a [rendelkezésre álló ajánlatok válthat listáját](#whats-supported) , és győződjön meg arról, hogy a megfelelő tagságot. 

    ![Jelölje ki, amelyet váltani szeretne felajánlás](./media/billing-how-to-switch-azure-offer/selectoffer.png)

5.  Attól függően, hogy az ajánlat vált a milyen következményekkel járnak váltás megjegyzést jelenhet meg. Gondosan hajtsa végre a listában, és kövesse az utasításokat, akkor a folytatás előtt.

    ![A jegyzetek véleményezése](./media/billing-how-to-switch-azure-offer/thingstonote.png)

6.  Az előfizetés átnevezheti. Alapértelmezés szerint akkor állítsa be az új ajánlatot nevet. Kattintson a **Váltás ajánlja fel** a folyamat befejezéséhez.

    ![Zöld gombra](./media/billing-how-to-switch-azure-offer/confirmpage.png)

7.  A siker! Az előfizetés ekkor az új ajánlatot lettek.

## <a name="why-cant-i-switch-offers"></a>Miért nem tudok ajánlatok váltani?

**Váltás másik ajánlatot** nem jelentkezik, ha:

- Ha nem a [kirovó](https://azure.microsoft.com/offers/ms-azr-0003p/). Jelenleg csak kirovó előfizetések egy másik ajánlatra lehet másikra váltani.

    - Ha a [Ingyenes próbaverziót](https://azure.microsoft.com/free/), ismerje meg, hogy miként [frissíthet a kirovó](billing-upgrade-azure-subscription.md).

    - Váltás másik előfizetésből, [lépjen kapcsolatba a támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)ajánlat.

- Továbbra is használja az első számlázási időszak; meg kell várnia, amíg a első számlázási időszak végén, mielőtt ajánlatok válthat.

Jelenhetnek meg **vannak nem érhető el az adott időben ország vagy terület ajánlatok** ha:

- Ha nem alkalmas bármilyen ajánlat kapcsolókat. Jelölje be a [rendelkezésre álló ajánlatok válthat listáját](#whats-supported).

## <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Mit jelent az Azure ajánlatok el a Váltás a szolgáltatást, és a számlázási?

A részletek, hogy mi történik, amikor vált az Azure-csomagok között a fiók-központban.

### <a name="access-to-services"></a>Az Access Services szolgáltatásban

Nincs szolgáltatás legrövidebb leállás bármely az előfizetéshez tartozó felhasználók számára nem. Az ajánlat vált azonban a korlátozások lehetnek. Például egyes ajánlatok tiltja az gyártási használatát, így kimutatásadatokat gyártási erőforrások áthelyezése egy másik előfizetést.

### <a name="billing"></a>A számlázás

Váltás nap számla összes nyitott díjak jön létre. Ezután az előfizetés egy új ajánlatot árak kifejezéseket számlát kapni. A dátum, amelyen ajánlatok módosította az előfizetés számlázási évforduló változik. Használati és a számlázási adatok előtt nem tartja meg az ajánlat módosítása, ezért azt javasoljuk, hogy előtte másolat letöltése.

> [AZURE.NOTE] A számlázással kapcsolatos korlátozások miatt ajánlat kapcsolók sem lehetséges előfizetés létrehozása után az első számlázási ciklusban.

## <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Is lehet áttelepítése kirovó [Felhőalapú megoldás szolgáltató](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) vagy [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

Jelenleg nem támogatott diagramtípusról Szolgáltatót, illetve EA ajánlat váltás fiókok közepén. A meglévő előfizetéshez tartozó áthelyezése EA, a regisztrációs rendszergazdai fiók hozzáadása az EA be van. Ezután a meghívó e-mailben kapja. Ha követi az utasításokat követve fogadja el a meghívót, az előfizetések automatikusan átkerülnek az Enterprise Agreement csoportban. CSP áttelepítéséhez, olvassa el az [Azure előfizetés áttelepítését a CSP](https://blogs.technet.microsoft.com/hybridcloudbp/2016/08/26/azure-subscription-migration-to-csp/).

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [rendszergazdai szerepkörök kezeléséhez](billing-add-change-azure-subscription-administrator.md) az előfizetéshez

- Nyomon követheti a használatát [használati adatainak és a számla](billing-download-azure-invoice-daily-usage-date.md) letöltésével

## <a name="need-help-contact-support"></a>Segítségre van szüksége? Forduljon az ügyfélszolgálathoz.

Ha továbbra is további kérdései vannak, kérjük, [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a problémát a rendszer gyorsan.
