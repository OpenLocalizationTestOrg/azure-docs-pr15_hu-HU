<properties
   pageTitle="Állítsa be a forgalom Manager kerek Ágnes forgalom útválasztási módszer |} Microsoft Azure"
   description="Ez a cikk segít a forgalom Manager végpontok ciklikus terheléselosztási konfigurálása."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-round-robin-routing-method"></a>Ciklikus konfigurálása útválasztási módszer

Közös forgalom útválasztási módszer mintát, hogy egy sor olyan azonos végpontok, amely tartalmazza a felhőszolgáltatások és a webhelyek, adja meg, és küldjön minden ciklikus módon. Az alábbi lépésekkel szerkezeti Manager forgalom konfigurálása annak érdekében, hogy hajtsa végre a megfelelő forgalom útválasztási módszer. Különböző forgalom útválasztási módszerekkel kapcsolatos további tudnivalókért lásd: a [forgalom Manager forgalom útválasztási módszereket](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure webhelyek már tartalmaz ciklikus terheléselosztási funkciók a webhelyeket belül egy adatközponthoz (más néven régió). Adatforgalom Manager különböző adatközpont esetén ciklikus forgalom útválasztási módszer a webhelyeket megadását teszi lehetővé.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Adatforgalom egyaránt (kerek Ágnes) útválasztási végpontok halmazának keresztül:

1. Az Azure klasszikus portálon, a bal oldali ablaktáblában kattintson a **Forgalom Manager** ikonra kattintva nyissa meg a forgalmat a kezelő ablaktáblában. Ha még nem hozott létre a forgalom Manager profil, [Forgalom Manager profilok kezelése](traffic-manager-manage-profiles.md) a lépések egy egyszerű forgalom Manager profil létrehozása című témakör tartalmaz.
2. Az Azure klasszikus portálon meg a forgalmat a kezelő ablakban keresse meg a forgalom Manager tartalmazó profilra, a módosítani kívánt beállításokat, és kattintson a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
3. A profil lapon kattintson a lap tetején a **Végpontok** , és győződjön meg róla, hogy az Ön konfigurációjának szerepeltetni kívánt szolgáltatás végpontokat bemutató. Lépések hozzáadása és eltávolítása a végpontok című témakörben találja [Az adatforgalom-kezelőben végpontok kezelése](traffic-manager-endpoints.md).
4. A profil lapon kattintson a **beállítás** gombra kattintva nyissa meg a konfigurációs lap tetején.
5. **Adatforgalom útválasztási mód beállításai**ellenőrizze, hogy a forgalmat útválasztási módszer **ciklikus**. Ha nem, kattintson a legördülő lista **ciklikus** .
6. Győződjön meg arról, hogy a **Beállítások ellenőrzése** megfelelően vannak-e beállítva. Figyelés biztosítja, hogy az offline végpontokat nem kapnak forgalmat. Annak érdekében, hogy figyelheti a végpontok, meg kell adnia egy elérési utat és a fájlnevet. Megjegyzendő, hogy perjel "/" relatív elérési útja az érvényes bejegyzés és az azt jelenti, hogy a fájlt a legfelső szintű címtárban (alapértelmezett). Figyelés kapcsolatos további tudnivalókért olvassa el a [Kapcsolatos forgalom Manager figyelése](traffic-manager-monitoring.md)című témakört.
7. A konfiguráció módosítások elvégzése után kattintson a **Mentés** elemre a lap alján.
8. Tesztelje a módosításokat a konfigurációban. További tudnivalókért lásd: a [Forgalom kezelő beállításainak tesztelése](traffic-manager-testing-settings.md).
9. Miután a forgalom Manager profil beállítása és a munkát, a vállalat tartománynevét a forgalom Manager tartománynevére mutasson a mérvadó DNS-kiszolgáló a DNS-rekord szerkesztése Az ezzel kapcsolatos további tudnivalókért lásd: [internetes forgalmat Manager tartományhoz vállalati tartomány pont](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Következő lépések


[Internetes vállalati tartomány mutasson a forgalom Manager tartomány](traffic-manager-point-internet-domain.md)

[Adatforgalom Manager útválasztási módszerek](traffic-manager-routing-methods.md)

[Feladatátvevő útválasztási mód konfigurálása](traffic-manager-configure-failover-routing-method.md)

[Teljesítmény útválasztási mód konfigurálása](traffic-manager-configure-performance-routing-method.md)

[Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)

[Kezelő – letiltás engedélyezése forgalom profil vagy törlése](disable-enable-or-delete-a-profile.md)

[Adatforgalom Manager - letiltása vagy engedélyezése zárólap](disable-or-enable-an-endpoint.md)

