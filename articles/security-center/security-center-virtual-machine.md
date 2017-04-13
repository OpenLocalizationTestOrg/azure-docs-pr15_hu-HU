<properties
   pageTitle="Azure biztonság otthon és Azure virtuális gépeken futó |} Microsoft Azure"
   description="A dokumentum segítségével hogyan Azure biztonság otthon van megóvása, Azure virtuális gépeken futó megértéséhez."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure biztonság otthon és Azure virtuális gépeken futó

[Azure biztonság otthon](https://azure.microsoft.com/services/security-center/) segítségével megakadályozhatja, hogy észleli és veszélyek válaszolni. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

Ebből a cikkből megtudhatja, hogyan biztonság otthon segíthetnek biztonságos az Azure virtuális gépeken futó (virtuális).

## <a name="why-use-security-center"></a>Miért érdemes használni a biztonság otthon?

Biztonság otthon segít Azure virtuális gép adatainak védelme betekintést kap abba, hogy a virtuális gép biztonsági beállítások megadásával. Biztonság otthon a VMs biztosítéki, ha az alábbi funkciók érhetők el:

- Operációs rendszer (operációs rendszerrel) biztonsági beállításokat, ajánlott konfiguráció szabályokkal
- Biztonsági és a fontos frissítések hiányzó
- Endpoint protection javaslatok
- Lemezen titkosítási érvényesítése
- Biztonsági értékelése és a remediation
- Veszélyt kimutatására

Az Azure VMs védelmének elősegítése, kívül biztonság otthon emellett biztonsági figyelő és felügyeleti Felhőszolgáltatások, alkalmazás szolgáltatások, a virtuális hálózatok és sok másra. 

>[AZURE.NOTE] Lásd: [Azure biztonság otthon bemutatása](security-center-intro.md) , ha többet szeretne tudni az Azure biztonság otthon.

## <a name="prerequisites"></a>Előfeltételek

Ismerkedés az Azure biztonság otthon, kell, hogy, és vegye figyelembe a következőket:

- Microsoft Azure-előfizetést kell rendelkeznie. Lásd: a [Biztonsági központ árak](https://azure.microsoft.com/pricing/details/security-center/) biztonság otthon szabad és normál rétegek további információt.
- A biztonság otthon elfogadása megtervezése című [Azure biztonság otthon tervezés és a műveletek útmutató](security-center-planning-and-operations-guide.md) megtudhatja, hogy tervezése és a műveletek kapcsolatos szempontjait.
- Operációs rendszer támogatási lehetőségek információért olvassa el a [Azure biztonság otthon gyakori kérdések](security-center-faq.md)című témakört. 

## <a name="set-security-policy"></a>Biztonsági szabály beállítása

Adatgyűjtés kell engedélyezni, hogy Azure biztonság otthon gyűjthet a javaslatok és létrehozott figyelmeztetéseket megadására szükséges információt alapján beállíthatja biztonsági házirend. Az alábbi ábrán láthatja, hogy **adatgyűjtési** volt **kapcsolva**.

Biztonsági házirendek meghatároz a vezérlők, amelyek a megadott előfizetés vagy erőforráscsoport erőforrások ajánlott. Biztonsági házirendje engedélyezése, előtt adatgyűjtés engedélyezve kell rendelkeznie, biztonság otthon adatait gyűjti össze a virtuális gépeken futó mérje fel, hogy a biztonsági állapotukban, adja meg a biztonsági javaslatok, és a kockázatok jelenít meg értesítést. Biztonság otthon, a házirendek az Azure előfizetések vagy az erőforrás-csoportok szerint a vállalat igényeinek, és milyen típusú alkalmazásokat vagy védelmi szintjének az adatokat az egyes előfizetések határozza meg. 

![Eszközbiztonsági házirend beállítása](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] További tudnivalók a minden **megelőzése házirend** érhető el, lásd: [biztonsági házirendek beállítása](security-center-policies.md) cikk.

## <a name="manage-security-recommendations"></a>Javaslatok biztonsági kezelése

Biztonság otthon elemzi a biztonsági állapot Azure erőforrásait. Ha a biztonság otthon azonosítja a potenciális biztonsági rés, javaslatok hoz létre. A javaslatok végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket.

Biztonsági házirendek beállítása, után a biztonság otthon elemzi az erőforrások azonosítása potenciális biztonsági biztonsági állapotát. Táblázatos formátumban, ahol minden sora egy adott ajánlási felel meg a javaslatok jelennek meg. Az alábbi táblázat néhány példával javaslatok Azure VMs és mindegyik fog mit alkalmazhat, ha a. Amikor kijelöl egy ajánlást, akkor nyújtanak információk arról, hogy miként ajánlása megvalósítását a biztonság otthon.

|Javaslat|Leírás|
|-----|-----|
|[Engedélyezése előfizetésben adatgyűjtés](security-center-enable-data-collection.md)|Javasolja, hogy bekapcsolja az egyes az előfizetések és az összes virtuális gépeken futó (VMs) a biztonsági házirendben adatgyűjtés az előfizetések.|
|[Operációs rendszer biztonsági ismételt](security-center-remediate-os-vulnerabilities.md)|Ajánlja igazíthatja a az operációs rendszer konfigurációk ajánlott konfiguráció szabályokkal, például nem engedélyezik a jelszavak szeretné menteni.|
|[Rendszer-frissítések telepítése](security-center-apply-system-updates.md)|Javasolja, hogy beállítaná hiányzó rendszer biztonsági és a fontos frissítések VMs.|
|[Indítsa újra a rendszer frissítés után](security-center-apply-system-updates.md#reboot-after-system-updates)|Javasolja, indítsa újra a virtuális alkalmazásának rendszerben frissítéseket a folyamat befejezéséhez.|
|[Az Endpoint Protection telepítése](security-center-install-endpoint-protection.md)|Javasolja, hogy a VMs (csak a Windows VMs) antimalware programok kiépítése.|
|[Az Endpoint Protection állapot riasztások feloldása](security-center-resolve-endpoint-protection-health-alerts.md)|Javasolja, hogy a endpoint protection hibák megoldásához.|
|[Virtuális ügynök engedélyezése](security-center-enable-vm-agent.md)|Lehetővé teszi, hogy mely VMs megkövetelése a virtuális Agent. A virtuális Agent kell telepíteni kell VMs kiépítése javítás eredeti a beolvasás és antimalware programok végzett vizsgálatot. A virtuális Agent alapértelmezés szerint a a Microsoft Azure piactéren lévő telepített VMs telepítve van. A [virtuális ügynök és bővítmények – 2 rész](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) a cikkben információt a virtuális Agent telepítési.|
| [Lemezen titkosítási alkalmazása](security-center-apply-disk-encryption.md) |Javasolja, hogy titkosítsa-e a virtuális merevlemezeken Azure lemez titkosítással (Windows és Linux VMs). Az operációs rendszer és az adatok kötet a virtuális a titkosítási ajánlott.|
| [Nincs telepítve a biztonsági értékelése](security-center-vulnerability-assessment-recommendations.md) | Javasolja, rés értékelési megoldás telepítése a virtuális. |
| [Biztonsági ismételt](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Lásd: a biztonsági értékelési telepítve van a virtuális a megoldás által talált rendszer és az alkalmazások biztonsági teszi lehetővé. |

>[AZURE.NOTE] További tudnivalók a javaslatok, lásd: [kezelése biztonsági ajánlások](security-center-recommendations.md) a cikk.

## <a name="monitor-security-health"></a>Monitor biztonsági állapota

Miután engedélyezte a [biztonsági házirendek](security-center-policies.md) egy előfizetés erőforrások, a biztonság otthon fog elemezheti az erőforrások azonosítása potenciális biztonsági biztonsága.  Az erőforrások együtt kapcsolatos problémák megoldásához, az **erőforrás biztonsági rendszerállapot** lap a biztonsági állapotát tekintheti meg. Ha **virtuális gépeken futó** **erőforrás biztonsági** állapot csempéjén gombra kattint, a **virtuális gépeken futó** lap nyílik a VMs javaslatok. 

![Biztonsági állapota](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Kezelése, és a biztonsági figyelmeztetések megválaszolása

Biztonság otthon automatikusan összegyűjti a elemzi és az Azure erőforrások, a hálózati és a csatlakoztatott partnermegoldások (például a tűzfalat, és végpont védelem megoldások), a naplóadatokat integrálja valós veszélyek észleli és csökkentése a téves. Szerint feljebb helyezése a különböző összesítése [észlelés](security-center-detection-capabilities.md), a biztonság otthon az elsőbbségi biztonsági figyelmeztetések segítségével gyorsan megtekintheti a probléma okát, és adja meg a javaslatok, hogyan lehet támadások ismételt létre tudja hozni.

![Biztonsági figyelmeztetések](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Jelölje be a biztonsági figyelmeztetés, ha többet szeretne megtudni az esemény, amelyen a értesítés és, ha bármelyik támadások ismételt még szükséges lépéseket. Biztonsági figyelmeztetések [típusa](security-center-alerts-type.md) és dátum szerint vannak csoportosítva.


## <a name="see-also"></a>Lásd még:

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
