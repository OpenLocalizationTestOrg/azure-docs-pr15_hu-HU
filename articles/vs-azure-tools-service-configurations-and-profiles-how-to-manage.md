<properties
   pageTitle="Szolgáltatás-beállításokat és a profilok kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként dolgozhat a szolgáltatás beállításokat és a profilok konfigurációs fájlok |} amely tárolni a telepítési környezetekben beállításait, és tegye közzé a felhőalapú szolgáltatások beállításai."
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

# <a name="how-to-manage-service-configurations-and-profiles"></a>Szolgáltatás-beállításokat és a profilok kezelése

## <a name="overview"></a>– Áttekintés

Visual Studio történő közzétételekor egy felhőalapú szolgáltatásba, konfigurációs adatok kétféle konfigurációs fájlokat tárol: beállításokat és a profilok szolgáltatás. Szolgáltatás konfigurációk (.cscfg fájlok) tárolni a telepítési környezetben az Azure felhőalapú szolgáltatás beállításait. Azure használja a konfigurációs ezeket a fájlokat, amikor a felhőalapú szolgáltatások kezeli. Azonban profilok (.azurePubxml fájlok) áruházból közzététele felhőszolgáltatások beállításait. Ezek a beállítások milyen kiválasztani a közzétételi varázsló használja, és a Visual Studio helyileg használatosak felvételét találhatók. Ebből a témakörből megtudhatja, hogyan dolgozhat a konfigurációs fájl mindkét típusú.

## <a name="service-configurations"></a>Szolgáltatás-beállítások

A telepítési környezetekben használható több szolgáltatás konfigurációk hozhat létre. Például előfordulhat, hogy hozzon létre egy szolgáltatás konfigurálása a helyi környezethez használt futtatni, és tesztelje az Azure-alkalmazások és a gyártási környezetben egy másik szolgáltatás konfigurálása.

Hozzáadása, törlése, átnevezése és módosítása az igényeknek megfelelően alakítható szolgáltatás őket. A Visual Studióban, szolgáltatás őket kezelheti, az alábbi ábrán látható módon.

![Szolgáltatás-beállítások kezelése](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

A **Konfigurációk kezelése** párbeszédpanel a szerepkör tulajdonság lapról is megnyithatja. Nyissa meg a tulajdonságok egy szerepkör a Azure projektben, nyissa meg a helyi menü, az adott szerepkört, és válassza a **Tulajdonságok parancsot**. A **Beállítások** lapon a **Szolgáltatás konfigurálása** lista kibontása, és válassza a **Manage** a **Konfigurációk kezelése** párbeszédpanel megnyitásához.

### <a name="to-add-a-service-configuration"></a>A szolgáltatás konfiguráció hozzáadása

1. A megoldás Intézőben nyissa meg a helyi menü az Azure projekt, és válassza a **Beállítások kezelése**.

    A **Szolgáltatás konfigurációk kezelése** párbeszédpanel jelenik meg.

1. A szolgáltatás konfiguráció felvenni, létre kell hoznia egy meglévő konfigurációs másolatának. Ehhez válassza a használni kívánt másolja a listában, és válassza a **Create másolás**konfiguráció.

1. (Nem kötelező) A szolgáltatás beállításai egy másik nevet adhat, az új szolgáltatás konfiguráció választhat a listában, és válassza a **Nevezze át**. **A szöveg mezőbe** írja be a nevet, amelyet szeretne ebben a konfigurációban szolgáltatást használja, és kattintson **az OK**gombra.

    Új szolgáltatás konfigurációs fájl ServiceConfiguration nevű. [Új neve] .cscfg bekerül a Azure megoldást Explorer projekt.


### <a name="to-delete-a-service-configuration"></a>A szolgáltatás konfiguráció törlése

1. A megoldás Intézőben nyissa meg a helyi menü az Azure projekt, és válassza a **Beállítások kezelése**.

    A **Szolgáltatás konfigurációk kezelése** párbeszédpanel jelenik meg.

1. A szolgáltatás konfiguráció törléséhez válassza ki a használni kívánt törlése **a listában** , és válassza az **Eltávolítás**a konfiguráció. Ellenőrizze, hogy ez a beállítás a törölni kívánt megjelenik egy párbeszédpanel.

1. Válassza a **Törlés**.

     A szolgáltatás konfigurációs fájl a Azure megoldást Explorer projekt törlődik.


### <a name="to-rename-a-service-configuration"></a>A szolgáltatás konfiguráció átnevezése

1. A megoldás Intézőben nyissa meg a helyi menü az Azure projekt, és válassza a **Beállítások kezelése**.

    A **Szolgáltatás konfigurációk kezelése** párbeszédpanel jelenik meg.

1. A szolgáltatás konfiguráció átnevezéséhez az új szolgáltatás konfiguráció választhat **a listában** , és válassza a **Nevezze át**. A **név** mezőbe írja be a szolgáltatás konfigurálása a használni kívánt nevét, és kattintson **az OK**gombra.

    A szolgáltatás konfigurációs fájl nevét a projekt Azure megoldást Explorer megváltozik.

### <a name="to-change-a-service-configuration"></a>A szolgáltatás konfiguráció módosítása

- A szolgáltatás konfiguráció módosítani szeretné, ha meg szeretné változtatni az Azure projektben szerepkör helyi menüjének megnyitása, és válassza a **Tulajdonságok parancsot**. Lásd: [Útmutató: a szerepkörök konfigurálása az Azure felhőalapú szolgáltatás a Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) további információt.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Eltérő beállítás kombinációk módosíthat profilok

Profil használatával automatikusan kitöltheti a **Közzététel varázsló** beállítások különböző kombinációi, más célra. Például lehet hibakereséshez egy profilt, és egy másik kiadását hoz létre. Ebben az esetben a **hibakeresési** profil szeretné, hogy **IntelliTrace** engedélyezett és a kijelölt **hibakeresési** konfiguráció, és a **Megjelenés** profil szeretné, hogy **IntelliTrace** letiltott és a kijelölt **Megjelenés** konfiguráció. A különböző tárterület-fiókja szolgáltatás üzembe különböző profilok is használhatja.

A varázsló első futtatásakor alapértelmezett profil jön létre. Visual Studio tárolja a profil egy fájlt, amelynek .azurePubXml bővítmény bekerül az Azure project a **profilok** mappában találja. Különféle beállítási lehetőségekre húzva manuálisan a varázsló később futtatásakor megadása esetén automatikusan frissíti a fájlt. Az alábbi eljárás, futtatása előtt célszerű már közzétett a felhőalapú szolgáltatás legalább egyszer.

### <a name="to-add-a-profile"></a>Profil hozzáadása

1. Nyissa meg a helyi menü a Azure projekthez, és válassza a **Közzététel**gombra.

1. A **cél profil** listában válassza a **Profil mentése** gombra, az alábbi ábrán látható. Ezzel létrehoz egy profilt.

    ![Hozzon létre egy új profilt](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. A profil létrehozását követően jelölje ki a **cél profil** listában **<... kezelése >** .

    A **Profilok kezelése** párbeszédpanel jelenik meg, ahogy az alábbi ábrán látható.

    ![Profilok kezelése](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. A **név** listában válasszon egy profilt, és válassza a **Másolat létrehozása**.

1. Válassza a **Bezárás** gombra.

    Az új profil megjelenik a profilhoz céllistában.

1. A **Target profil** listában jelölje ki a profillal, amely az imént létrehozott. A varázsló Közzététel beállításai ki kell jelentkeznie a választási lehetőségek, a kijelölt profiljából.

1. Jelölje ki az **előző** és **következő** gombok a Közzététel varázsló minden egyes lap megjelenítéséhez, és kattintson a profil beállításainak testreszabása. További információkat talál az [Azure-alkalmazás varázsló közzététele](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. A beállításainak testreszabása után válassza a **Tovább gombra** kattintva visszatérhet a beállítások lap. A profil menti a program a szolgáltatás közzétételekor következő beállításokkal, vagy ha, válassza a **Mentés** mellett profilok listája.

### <a name="to-rename-or-delete-a-profile"></a>Átnevezésére és törlésére profil

1. Nyissa meg a helyi menü a Azure projekthez, és válassza a **Közzététel**gombra.

1. A **cél profil** listában válassza a **kezelés**.

1. **Profilok kezelése** párbeszédpanelen jelölje ki a törölni kívánt profilt, és válassza az **Eltávolítás**gombra.

1. A megjelenő megerősítést kérő párbeszédpanelen kattintson **az OK gombra**.

1. Válassza a **Bezárás gombra**.

### <a name="to-change-a-profile"></a>A profil módosítása

1. Nyissa meg a helyi menü a Azure projekthez, és válassza a **Közzététel**gombra.

1. A **Target profil** listában jelölje ki a profillal, amely a módosítani kívánt.

1. Jelölje ki az **előző** és **következő** gombok a Közzététel varázsló minden egyes lap megjelenítéséhez, és módosítsa a kívánt beállításokat. További információkat talál az [Azure-alkalmazás varázsló közzététele](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. A beállítások módosítása után válassza a **Tovább gombra** kattintva visszatérhet a **Beállítások** lap.

1. (Nem kötelező) válassza a **Közzététel** tegye közzé a felhőalapú szolgáltatást használ, az új beállítások. Ha nem közzé szeretné tenni a felhőalapú szolgáltatás adott időben, és bezárja a Közzététel varázslót, Visual Studio kéri, hogy szeretné-e a módosítások mentéséhez a profilhoz.

## <a name="next-steps"></a>Következő lépések

Visual Studio Azure projektjét más részeinek konfigurálásának ismertetése című témakörben talál [az Azure-projekt beállítása](http://go.microsoft.com/fwlink/p/?LinkID=623075)
