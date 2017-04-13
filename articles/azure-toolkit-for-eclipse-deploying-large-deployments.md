<properties
    pageTitle="Nagy telepítések üzembe helyezése"
    description="További információ az Azure eszközkészlet használata Holdas nagy telepítések telepítése."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Nagy telepítések üzembe helyezése #

Ha a telepítő túl nagy ahhoz, hogy az alapértelmezett approot mappa található,-et is helyi tároló erőforrás, a telepítési gyökérmappa a JDK és alkalmazáskiszolgáló.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>A telepítési gyökérmappa nagy telepítésekhez helyi tároló erőforrás tartományként ##

1. Hozzon létre új helyi tároló erőforrás. Az erőforrás neve nem számít. A tároló erőforrásokat szerepkör szintre határozzák meg. A helyi tároló konfigurációs párbeszédpanel, amelyről úgy lehetett létrehozni egy új helyi tároló erőforrás eléréséhez a leggyorsabb módja van, az alábbi lépésekkel: kattintson a jobb gombbal a szerepkör a **Project Explorer** nézetben (bontsa ki a Azure projekt csomópontját, ha nem látja a szerepkör), kattintson az **Azure**, majd a **Helyi tároló**. **Helyi tároló** párbeszédpanelen kattintson az új helyi tároló erőforrás **hozzáadása** gombra.
1. Állítsa a kívánt méretet legalább 2048 MB (semmi kisebb okozhat ugyanannak a fájlnak méret problémák volna tapasztal, mint a approot).
1. Győződjön meg arról, hogy a **tartalmát az szerepkör-példányt a rendszer esetén a minden** be legyen jelölve; Ez segít a szerepkör-példány a rendszer esetén a már meglévő fájlokat az erőforrás az ütközések elindulásának megakadályozása az a telepítési indítási összefüggés.
1. Győződjön meg arról, hogy a **környezeti változó tárolásához a telepítés után az erőforrás elérési** értéke **DEPLOYROOT**karakterlánccal. A helyi tároló erőforrás párbeszédpanel az alábbihoz hasonlóan néznek ki.
    ![][ic667943]

Azt is megteheti Ha **DEPLOYROOT** használja a helyi erőforrás *nevét* , és hogy ne módosítsa az automatikusan generált környezeti változó neve (amely állítja be **DEPLOYROOT_PATH** ebben az esetben), amely működő az alkalmazáshoz.

Helyi tároló erőforrás létrehozásával kapcsolatban további információt a [helyi tároló tulajdonságainak][]találhatók.

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

[Az Azure eszközkészlet Holdas telepítése][] 

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[Helyi tárolási tulajdonságai]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
