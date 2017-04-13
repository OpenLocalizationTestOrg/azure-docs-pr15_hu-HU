<properties
   pageTitle="Állítsa be az Azure felhőalapú szolgáltatás Project Visual Studio |} Microsoft Azure"
   description="Megtudhatja, hogy miként projektben Azure felhőalapú szolgáltatás konfigurálása a Visual Studióban, attól függően, hogy a projekt követelményei."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Állítsa be az Azure felhőalapú szolgáltatás Project Visual Studio

Beállíthatja az Azure felhőalapú szolgáltatás project attól függően, hogy a projekt az igényeknek megfelelően alakíthatja. Beállíthatja, hogy a projekt, a következő kategóriák tulajdonságai:

- **Azure közzététele egy felhőalapú szolgáltatásba**

  Győződjön meg arról, hogy a program nem törli véletlenül egy meglévő felhőalapú szolgáltatás üzembe Azure tulajdonság beállítása

- **Hibakeresési egy felhőalapú szolgáltatásba, a helyi számítógépen, vagy az**

  A szolgáltatás konfiguráció, és jelzi, hogy szeretné-e az Azure tároló irányító indítása kijelölhet.

- **Egy felhőalapú szolgáltatás csomagot az érvényesítés után**

  Eldöntheti, hogy a hibaként figyelmeztetéseket, így biztos lehet, hogy a felhőalapú szolgáltatás csomag telepítésének nélkül kapcsolatos problémák megoldásához. Ez csökkenti a várakozási időt, ha telepítése, és kattintson a hiba, hogy felfedezése.

Az alábbi ábrán egy konfiguráció használata futtatása és a felhőalapú szolgáltatást helyileg hibakeresési kijelölése. Beállíthatja, hogy az ablakból igénylő projekt tulajdonságainak a képen látható módon.

![A Microsoft Azure-projekt beállítása](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Az Azure felhőalapú szolgáltatás project konfigurálása

1. Egy felhőalapú szolgáltatás project **Megoldás**Intézőből konfigurálása, nyissa meg a helyi menü a felhőalapú szolgáltatás projekt, és válassza a **Tulajdonságok parancsot**.

  A Visual Studio-szerkesztő megjelenik egy lapon írja be az a felhő szolgáltatási projekt nevét.

1. Kattintson a **fejlesztés** fülre.

1. Győződjön meg arról, hogy véletlenül ne törölje az üzenet egy meglévő telepítési lista törlése előtt az Azure, a meglévő telepítés válasszon **Igaz**.

1. Jelölje be a szolgáltatás beállításai használata futtatása és hibakeresési helyi meghajtóra, a felhőalapú szolgáltatás a **szolgáltatás konfigurálása** lista kívánt válassza a szolgáltatás beállításai.

  >[AZURE.NOTE] Ha szeretné-e használni, akkor olvassa el a szolgáltatás konfiguráció létrehozása hogyan: kezelése szolgáltatás beállításokat és a profilok. A szerepkör a szolgáltatás konfiguráció módosítani szeretné, ha [a szerepkörök, a Visual Studio az Azure felhőalapú szolgáltatás konfigurálása](vs-azure-tools-configure-roles-for-cloud-service.md)tájékozódhat.

1. Indítsa el az Azure tároló irányító futtatása, vagy a helyi meghajtóra, a felhőalapú szolgáltatás hibakeresési az **Azure indítása tároló irányító**, válassza a **Igaz**.

1. Győződjön meg arról, hogy nem teheti közzé, ha vannak csomag érvényesítési hibákat, **Treat figyelmeztetés hibaként**, válassza a **Igaz**.

1. Győződjön meg arról, hogy a webes szerepkör ugyanazt a portot használja a helyi meghajtóra IIS Express **használják a webhelyen a project portokat**minden indításakor válassza **Igaz**. Egy adott webes projekt egy adott portot használja, nyissa meg a helyi menü a webes projekt, válassza a **Tulajdonságok** lapon, kattintson a **webhely** fülre, és módosítása a **Project URL-címe** beállítással **IIS Express** szakaszában a portszámot. Ha például írja be `http://localhost:14020` , a project URL-CÍMÉT.

1. A felhőalapú szolgáltatás projekt tulajdonságainak végrehajtott módosítások mentéséhez, válassza az eszköztáron a **Mentés** gombra.

## <a name="next-steps"></a>Következő lépések

További Azure felhőalapú szolgáltatás projektek beállításáról a Visual Studióban, olvassa el a [több szolgáltatás konfiguráció használata az Azure beállítása a project](vs-azure-tools-multiple-services-project-configurations.md)című témakört.
