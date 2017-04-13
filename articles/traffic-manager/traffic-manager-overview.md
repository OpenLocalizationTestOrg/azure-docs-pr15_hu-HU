<properties
    pageTitle="Adatforgalom Manager |} Microsoft Azure"
    description="Ez a cikk segít megérteni forgalom Manager van, és hogy-e a megfelelő forgalom útválasztási választás az alkalmazás"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Az adatforgalom-kezelő áttekintése

Microsoft Azure forgalom elemre kattintva állítsa be a felhasználó forgalom szolgáltatás végpontok különböző adatközpont esetén eloszlása teszi lehetővé. Forgalom Manager által támogatott szolgáltatás végpontok Azure VMs Web Apps, hozzáadni, majd a cloud services. Adatforgalom Manager rendelkező külső, nem Azure végpontokat is használhatja.

Forgalom Manager a tartomány (DNS) használ, irányítsa át az ügyfél kérelmek a legmegfelelőbb végpont [forgalom útválasztás módszer](traffic-manager-routing-methods.md) a és a végpontok állapotának alapján. Adatforgalom-kezelővel forgalom útválasztás módszerek az igényeinek, másik alkalmazás igényekkel, a végpontok állapot [ellenőrzése](traffic-manager-monitoring.md)és a automatikusan átveszi cellatartományban. Adatforgalom felügyelője rugalmassá a hiba, többek között a teljes Azure-terület hibát.

## <a name="traffic-manager-benefits"></a>Adatforgalom Manager legfontosabb előny

Adatforgalom Manager segíthetnek:

- **Elérhetőség kritikus alkalmazások javítása**

    Forgalom Manager biztosítja a magas elérhetőség az alkalmazások figyelése a végpontok, valamint képzések automatikusan átveszi, ha megszakad a végpontok.

- **Nagy teljesítményű alkalmazások válaszidő javítása**

    Azure lehetővé teszi, hogy futtassa a felhőszolgáltatások és a webhelyek, amely a világ adatközpont esetén. Forgalom Manager alkalmazás válaszidő javítja a úgy irányítja a forgalmat a végpont a legalacsonyabb hálózati késés és az ügyfél számára.

- **Szolgáltatás karbantartásához legrövidebb leállás nélkül**

    Az alkalmazások legrövidebb leállás nélkül a tervezett karbantartás műveleteket végezheti el. Forgalom Manager alternatív végpontok forgalom arra utasítja, miközben folyamatban van a karbantartást.

- **A helyszíni és az alkalmazások felhőalapú kombinálása**

    Forgalom Manager támogatja a külső, nem Azure végpontok, amely lehetővé teszi a hibrid használható a felhő és a helyszíni telepítésekről, így a "burst-a-felhőbe," "áttelepítése-a-felhőbe," és "feladatátvevő felhő" esetek.

- **A forgalom nagy, összetett telepítések terjesztése**

    [Beágyazott forgalom Manager profilok](traffic-manager-nested-profiles.md)használ, forgalom útválasztás módszerek kombinálásának támogatja az egyéni igényeknek nagyobb, összetett telepítések összetett és rugalmas szabályok létrehozásához.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [forgalom Manager működése](traffic-manager-how-traffic-manager-works.md).

- Megtudhatja, hogy miként használja a [forgalom Manager végpont figyelése](traffic-manager-monitoring.md)magas rendelkezésre állás alkalmazások fejlesztéséhez.

- További tudnivalók a [forgalom útválasztás módszerek](traffic-manager-routing-methods.md) forgalom Manager által támogatott.

- [A forgalom Manager profil létrehozása](traffic-manager-manage-profiles.md).

