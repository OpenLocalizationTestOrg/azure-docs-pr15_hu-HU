<properties
   pageTitle="Hogyan kell tartani egy felhőalapú szolgáltatásba állandó virtuális IP-cím |} Microsoft Azure"
   description="Megtudhatja, hogy miként győződjön meg arról, hogy virtuális IP-címe (virtuális) az Azure felhőalapú szolgáltatás nem változtatja meg a."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Hogyan kell tartani egy felhőalapú szolgáltatásba állandó virtuális IP-cím

Ha egy felhőalapú szolgáltatásba, Azure-ban tárolt, akkor szükség lehet annak érdekében, hogy a virtuális IP-cím (virtuális) a szolgáltatás nem változik. Sok kezelési szolgáltatáshoz regisztrálása a tartománynevek használata a tartomány (DNS). A DNS működését, csak akkor, ha a virtuális változatlan marad. Azure eszközök a **Közzététel varázsló** használatával biztosítani, hogy az a felhő szolgáltatás virtuális frissítésekor, nem változik. DNS tartománykezelés cloud Services használatáról további információt talál [az Azure felhőszolgáltatásában egyéni tartománynév beállítása](./cloud-services/cloud-services-custom-domain-name.md)

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Közzététel egy felhőalapú szolgáltatásba a virtuális módosítása nélkül

A virtuális egy felhőalapú szolgáltatásba, akkor történik, amikor először beállítaná őket Azure egy adott környezetben, például az éles üzemi környezetben. A virtuális nem változtatja meg, kivéve, ha a telepítő kifejezetten törlése vagy implicit módon törölte a telepítési frissítési folyamat. A virtuális megőrzéséhez nem törölnie kell a üzembe, és kell gondoskodnia arról is, hogy a Visual Studio nem automatikusan törli a üzembe. Megadhatja, hogy a vezérlő viselkedése a **Varázsló közzététele**, amely támogatja a több telepítési lehetőségek telepítési beállítások megadásával. Megadhat egy friss telepítés vagy a frissítés példányban lehet egyedi vagy egyidejű, és mindkét fajta frissítés telepítések megőrzése a virtuális. A különböző típusú telepítési meghatározását [Azure alkalmazás varázsló közzététele](vs-azure-tools-publish-azure-application-wizard.md)című témakör tartalmaz.  Ezeken kívül szabályozhatja, hogy egy felhőalapú szolgáltatásba az előző telepítését törlődik, hiba történik, ha. A virtuális váratlanul megváltozhatnak, ha ezt a beállítást nincs megfelelően beállítva.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Egy felhőalapú szolgáltatásba frissítése a virtuális módosítása nélkül

1. A felhőalapú szolgáltatás legalább egyszer telepítését követően a Azure projekt csomópontját helyi menüjének megnyitása, és válassza a **Közzététel**. A **Közzététel Azure** varázsló jelenik meg.

1. Az előfizetések listájában válassza ki a telepítéshez használni kívánt időszintet, és kattintson a **Tovább** gombra. A **Beállítások** lap, a varázsló jelenik meg.

1. Az **Általános beállítások** lapon ellenőrizze, hogy minden helyesen-e a felhőbeli szolgáltatástól, amelyhez esetén központi telepítését, a **környezetben**, a **Konfigurációs összeállítása**és a **Szolgáltatás konfigurálása** nevét.

1. A **Speciális beállítások** lapon győződjön meg róla, hogy a tárterület-fiók és a telepítés címke helyes, hogy a **telepítési hiba esetén törlése** jelölőnégyzet nincs bejelölve, és hogy a **telepítési** frissítés jelölőnégyzetet. Jelölje ki a **telepítési** frissítés jelölőnégyzetet, akkor győződjön meg arról, hogy a telepítő nem törölhető, és a virtuális nem vesznek el, amikor az alkalmazás tegye közzé újra. Jelölésének törlésével **törlése telepítési hiba jelölőnégyzetet**, akkor győződjön meg arról, hogy a virtuális nem vesznek el, ha hiba történik a telepítés során.

1. További megadhatja, hogyan szeretné frissíteni kell a szerepkörök, válassza a **Beállítások** hivatkozásra a **telepítés frissítése** mező melletti, és válassza a növekményes frissítés vagy az egyidejű frissítése lehetőséget a **telepítés frissítése** beállításai párbeszédpanelen. Ha úgy dönt, hogy a frissítés egyre növekvő tendenciát mutat, az egyes példányok frissül egymás után, így mindig érhető el az alkalmazást. Ha úgy dönt, hogy egyidejű frissítése, minden esetben frissülnek, egy időben. Egyidejű frissítése gyorsabb, de a szolgáltatás nem érhetők el a frissítési folyamat során.

1. Ha elégedett a beállításokkal, válassza a **Tovább** gombra.

1. A varázsló **Összegzés** lapon ellenőrizze a beállításait, és kattintson a **Közzététel** gombra.

  >[AZURE.WARNING] Ha a telepítés meghiúsul, cím, hogy miért nem sikerült, és telepítsen újra haladéktalanul, a felhőalapú szolgáltatás elhagyása sérült állapotban elkerülése érdekében.

## <a name="next-steps"></a>Következő lépések

További információt a közzététel Azure Visual Studio, témakörben [közzététele Azure alkalmazás varázsló](vs-azure-tools-publish-azure-application-wizard.md).
