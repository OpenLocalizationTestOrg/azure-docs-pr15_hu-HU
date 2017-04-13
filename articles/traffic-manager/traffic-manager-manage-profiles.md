<properties
    pageTitle="Azure forgalom Manager profilok kezelése |} Microsoft Azure"
    description="Ez a cikk segít létrehozása, letiltása, engedélyezése, törlése és Azure forgalom Manager profil előzményeinek megtekintése."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Az Azure forgalom Manager profilok kezelése

Adatforgalom Manager profilok módszerekkel forgalom útválasztás szabályozhatja a forgalmat a felhőszolgáltatások és a webhely végpontok terjesztését. Ez a cikk ismerteti, hogyan hozhat létre, és ezek profilok kezelése.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Gyors létrehozása használatával forgalom Manager profil létrehozása

A forgalom Manager profilok gyors létrehozása az Azure klasszikus portal segítségével gyorsan létrehozhat. Gyors létrehozása lehetővé teszi az alapvető beállítások profilok létrehozása. Azonban nem használhat gyors létrehozása beállításokat is megadhat például végpontokat (a felhőszolgáltatások és a webhelyek), a feladatátvevő megrendelt feladatátvevő forgalom jóváhagyási módszert vagy a beállítások ellenőrzése a csoportját. Miután létrehozta a profilját, állítsa be ezeket a beállításokat az Azure klasszikus portálon. Adatforgalom Manager legfeljebb 200 végpontok egy profilt támogatja. Találatokkal használatát azonban csak néhány végpontok van szükség.

### <a name="to-create-a-traffic-manager-profile"></a>Adatforgalom Manager profil létrehozása

1. **A felhőszolgáltatások és a webhelyek üzembe a termelési környezetén.** Felhőbeli szolgáltatásokkal kapcsolatos további tudnivalókért olvassa el a [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074)című témakört. Webhelyek kapcsolatos további tudnivalókért olvassa el a [webhelyek](http://go.microsoft.com/fwlink/p/?LinkId=393327)című témakört.

2. **Jelentkezzen be az Azure klasszikus portálra.** **Új** a bal alsó sarkában az portált, kattintson a **hálózati szolgáltatások > forgalom Manager**, és kattintson a **Gyors létrehozása** a profil konfigurálásának megkezdéséhez.
3. **Állítsa be a DNS-előtagot.** A forgalom manager profil adjon meg egy egyedi DNS előtag nevét. Megadhatja, hogy csak az előtag forgalom Manager tartomány nevét.
4. **Jelölje ki azt az előfizetést.** Jelölje ki a megfelelő Azure előfizetést. Az egyes profilok társítva egyetlen előfizetést. Ha csak egy előfizetéssel rendelkeznek, ezt a lehetőséget akkor jelenik meg.
5. **Válassza ki a forgalom útválasztási módot.** Válassza ki a forgalom útválasztási módszert **házirend útválasztás forgalmat**. Forgalom útválasztási módszerekkel kapcsolatos további tudnivalókért olvassa el a [forgalom Manager forgalom útválasztási módszerek](traffic-manager-routing-methods.md)című témakört.
6. **Kattintson a profil létrehozása "létrehozása"**. A profil konfiguráció befejezése után a forgalmat a kezelő ablakban az Azure klasszikus portálon is keresse meg a profilját.
7. **Az Azure klasszikus portálon végpontok, figyelésére és a további beállítások konfigurálása** Csak gyors létrehozásához használja az egyszerű beállításainak konfigurálása. Érdemes további beállításokat, például a végpontok és végpont feladatátvevő sorrendjének listáját.


## <a name="disable-enable-or-delete-a-profile"></a>Tiltsa le, engedélyezése és profil törlése

Egy meglévő profilhoz úgy, hogy a forgalmat Manager nem hivatkozik felhasználói kérések konfigurált végpontjait tilthatja le. A forgalom Manager profilok letiltásakor a profil- és a profil tárolt adatok változatlanok maradnak, és szerkesztheti a forgalom kezelő felület.  Átirányítások folytathatja újra engedélyezheti a profilt. A forgalom Manager profilok az Azure klasszikus portálon létrehozásakor automatikusan engedélyezve van. Ha úgy dönt, hogy a profil már nem szükséges, törölheti azt.

### <a name="to-disable-a-profile"></a>A profil letiltása

1. Ha az egyéni tartománynév használata esetén módosítása az Internet a DNS-kiszolgálóra a CNAME rekord fölé már nem forgalom Manager profilját.
2. Adatforgalom leállítja az éppen irányítja a forgalmat Manager profilbeállítások keresztül a végpontok.
3. Jelölje be a letiltani kívánt profilt. A forgalom Manager lapon jelölje ki a profil az oszlopban, a profilnév melletti gombra kattintva. Megjegyzés: a profil vagy a neve melletti nyílra kattintva, megnyílik a beállítások lap a profilhoz.
4. A profil megadása után kattintson a **letiltása** az oldal alján.

### <a name="to-enable-a-profile"></a>Ahhoz, hogy a profil

1. Jelölje be a letiltani kívánt profilt. A forgalom Manager lapon jelölje ki a profil az oszlopban, a profilnév melletti gombra kattintva. Megjegyzés: a profil vagy a neve melletti nyílra kattintva, megnyílik a beállítások lap a profilhoz.
2. A profil megadása után kattintson a **engedélyezése** az oldal alján.
3. Ha egyéni tartománynevet használja, hozzon létre egy CNAME rekord az Internet a DNS-kiszolgálóra mutasson a forgalom Manager profil a tartománynév.
4. Adatforgalom ismét a végpontok irányítja.

### <a name="to-delete-a-profile"></a>A profil törlése

1. Győződjön meg arról, hogy a DNS-erőforrásrekord az Internet a DNS-kiszolgálóra már nem használja egy CNAME rekord, amely a tartománynév a forgalom Manager profil mutat.
2. Jelölje be a letiltani kívánt profilt. A forgalom Manager lapon jelölje ki a profil az oszlopban, a profilnév melletti gombra kattintva. Megjegyzés: a profil vagy a neve melletti nyílra kattintva, megnyílik a beállítások lap a profilhoz.
3. A profil megadása után kattintson a **Törlés** az oldal alján.

## <a name="view-traffic-manager-profile-change-history"></a>Nézet forgalom Manager profil módosítási előzmények

A módosítási előzmények megtekintheti az Azure klasszikus portálon szolgáltatások a forgalom Manager profilját.

### <a name="to-view-your-traffic-manager-change-history"></a>A forgalom Manager módosítási előzmények megtekintése

1. Az Azure klasszikus portál bal oldali ablaktáblájában kattintson a **Szolgáltatások**elemre.
2. A szolgáltatások kezelése lapon kattintson a **Művelet lehetőségre**.
3. A művelet naplók lapon szűrheti a forgalom Manager profil módosítási előzmények megtekintéséhez. A szűrési beállítások megadása után kattintson az eredmények megtekintéséhez a bejelölést.

   - A profil esetében a módosítások megtekintéséhez jelölje ki az előfizetést és időtartomány, és válassza a **Forgalom Manager** a **típus** helyi menüből.
   - Profilnév szerinti szűréshez a profil nevét írja be a **Szolgáltatás neve** mezőbe, vagy jelölje ki a helyi menüben.
   - Ha szeretné megtekinteni az egyes módosítások részleteit, jelölje ki a sort, amely meg szeretné tekinteni a módosítással, és kattintson a **Részletek** , a lap alján. A **Művelet részletei** ablakban megtekintheti az XML-megfeleltetése létrehozásakor vagy frissítésekor, a művelet részeként API-objektum.

## <a name="next-steps"></a>Következő lépések

- [Zárólap hozzáadása](traffic-manager-endpoints.md)
- [Feladatátvevő útválasztási mód konfigurálása](traffic-manager-configure-failover-routing-method.md)
- [Ciklikus útválasztási mód konfigurálása](traffic-manager-configure-round-robin-routing-method.md)
- [Teljesítmény útválasztási mód konfigurálása](traffic-manager-configure-performance-routing-method.md)
- [Internetes vállalati tartomány mutasson a forgalom Manager tartománynév](traffic-manager-point-internet-domain.md)
- [Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)