<properties
   pageTitle="Adatforgalom Manager feladatátvevő forgalom útválasztási módszer beállítása |} Microsoft Azure"
   description="Ez a cikk segít feladatátvevő forgalom útválasztási módszer az adatforgalom-kezelőben konfigurálása"
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

# <a name="configure-failover-routing-method"></a>Feladatátvevő útválasztási mód konfigurálása

Egy szervezet gyakran szeretne szolgáltatásai megbízhatóságát a szükséges. Ezt nem, mert a biztonsági szolgáltatások abban az esetben, ha az elsődleges szolgáltatás megszakad. Szolgáltatás feladatátvételi közös mintát, hogy adja meg a szolgáltatások azonos, és egy elsődleges szolgáltatás forgalmat küldeni egy vagy több biztonsági szolgáltatások konfigurált listáját megőrzésével. Az alábbi eljárásokkal biztonsági másolatot az ilyen típusú Azure felhőszolgáltatások és a webhelyek adhatja meg.

Figyelje meg, hogy Azure webhelyek már biztosít a feladatátvevő forgalom útválasztás módszer használható funkciók körét, a webhelyeket belül egy adatközponthoz (más néven egy régió), függetlenül a webhely módot. Adatforgalom Manager különböző adatközpont esetén feladatátvevő forgalom útválasztási módszer a webhelyeket megadását teszi lehetővé.

## <a name="to-configure-failover-traffic-routing-method"></a>Feladatátvevő forgalom útválasztási módszer beállítása:

1. Az Azure klasszikus portálon, a bal oldali ablaktáblában kattintson a **Forgalom Manager** ikonra kattintva nyissa meg a forgalmat a kezelő ablaktáblában. Ha még nem hozott létre a forgalom Manager profil, [Forgalom Manager profilok kezelése](traffic-manager-manage-profiles.md) a lépések egy egyszerű forgalom Manager profil létrehozása című témakör tartalmaz.
2. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
3. A profil lapon kattintson a lap tetején a **Végpontok** , és ellenőrizze, hogy a mindkét cloud services és webhelyek (a végpontok) a konfigurációban felvenni kívánt találhatók. Lépések hozzáadása és eltávolítása a végpontok című témakörben találja [Az adatforgalom-kezelőben végpontok kezelése](traffic-manager-endpoints.md).
4. A profil lapon kattintson a **beállítás** gombra kattintva nyissa meg a konfigurációs lap tetején.
5. A **forgalom útválasztási módszer beállításait**ellenőrizze, hogy a forgalmat útválasztási módszer **feladatátvevő**. Ha nem, kattintson a **feladatátvevő** a legördülő listából.
6. **Feladatátvevő prioritás listában**állítsa át a feladatátvevő sorrendben, a végpontok. Ha bejelöli a **feladatátvevő** forgalom útválasztási módszer, a kijelölt végpontok sorrendjének ügyekben. Az elsődleges végpontot van az előtérben. A felfelé és lefelé mutató nyilakkal sorrendjének módosítása, szükség szerint. Feladatátvevő prioritásának beállítása a Windows PowerShell használatával kapcsolatos további tudnivalókért olvassa el a [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880)című témakört.
7. Győződjön meg arról, hogy a **Beállítások ellenőrzése** megfelelően vannak-e beállítva. Figyelés biztosítja, hogy az offline végpontokat nem kapnak forgalmat. Annak érdekében, hogy figyelheti a végpontok, meg kell adnia egy elérési utat és a fájlnevet. Megjegyzendő, hogy perjel "/" relatív elérési útja az érvényes bejegyzés és az azt jelenti, hogy a fájlt a legfelső szintű címtárban (alapértelmezett). Figyelés kapcsolatos további tudnivalókért olvassa el a [Forgalom Manager figyelés](traffic-manager-monitoring.md)című témakört.
8. A konfiguráció módosítások elvégzése után kattintson a **Mentés** elemre a lap alján.
9. Tesztelje a módosításokat a konfigurációban. [Adatforgalom kezelő beállításainak tesztelése](traffic-manager-testing-settings.md) talál további információt.
10. Miután a forgalom Manager profil beállítása és a munkát, a vállalat tartománynevét a forgalom Manager tartománynevére mutasson a mérvadó DNS-kiszolgáló a DNS-rekord szerkesztése Az ezzel kapcsolatos további tudnivalókért lásd: [internetes forgalmat Manager tartományhoz vállalati tartomány pont](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Következő lépések

[Internetes vállalati tartomány mutasson a forgalom Manager tartomány](traffic-manager-point-internet-domain.md)

[Adatforgalom Manager útválasztási módszerek](traffic-manager-routing-methods.md)

[Ciklikus útválasztási mód konfigurálása](traffic-manager-configure-round-robin-routing-method.md)

[Teljesítmény útválasztási mód konfigurálása](traffic-manager-configure-performance-routing-method.md)

[Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)

[Kezelő – letiltás engedélyezése forgalom profil vagy törlése](disable-enable-or-delete-a-profile.md)

[Adatforgalom Manager - letiltása vagy engedélyezése zárólap](disable-or-enable-an-endpoint.md)

