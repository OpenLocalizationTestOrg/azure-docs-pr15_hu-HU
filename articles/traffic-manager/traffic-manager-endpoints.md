<properties
   pageTitle="Azure forgalom Manager végpontok kezelése |} Microsoft Azure"
   description="Ez a cikk segít hozzáadása, eltávolítása, engedélyezése és letiltása a végpontok Azure forgalom Manager alkalmazásból."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Adja hozzá, letiltása, engedélyezése és törlése a végpontok

A Web Apps alkalmazások Azure alkalmazás szolgáltatás már szolgáltatása feladatátvétel és a webhelyek belül egy adatközpont, függetlenül a webhely mód ciklikus forgalom útválasztási funkciókat. Azure forgalom Manager feladatátvétel és ciklikus forgalom továbbítása különböző adatközpont esetén webhelyekre és felhőbeli szolgáltatásokhoz megadását teszi lehetővé. Az első lépés biztosítani, hogy funkció hozzáadása a felhőalapú szolgáltatás vagy webhely végpont forgalom Manager.

>[AZURE.NOTE] Külső helyekre vagy forgalom Manager profilok nem adhatja hozzá, a végpontokat az Azure klasszikus portálon. A REST API- [Definíció létrehozása](http://go.microsoft.com/fwlink/p/?LinkId=400772) vagy a Windows PowerShell [Hozzáadása-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774)kell használnia.

A forgalom Manager profilok részét képező egyéni végpontok le is tilthatja. Végpontokat bele a felhőszolgáltatások és a webhelyek. A profil részeként zárólap letiltása hagyja, de a profil működik, mintha a végpont nem szerepel az azt. Ez a művelet nagyon hasznos, ha ideiglenesen eltávolítása, amely a karbantartási mód, vagy éppen újra nem telepítik zárólap. Miután a végpont lépéseket ismét, engedélyezhető.

>[AZURE.NOTE] Zárólap letiltása amelyen semmi sem a teendő az Azure környezetben állapotába. A megfelelő végpont és érkeznek a forgalmat, akkor is, ha az adatforgalom-kezelőben letiltva marad. Ezenkívül egy profil zárólap letiltása nem befolyásolja állapota egy másik profilhoz.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Egy felhőalapú szolgáltatás vagy webhely végpont hozzáadása


1. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
2. A lap tetején kattintson a **végpontjait** megtekintheti, hogy már a konfigurációban a végpontok.
3. A lap alján kattintson a **Hozzáadás** a **Szolgáltatás végpontok hozzáadása** lap eléréséhez. Alapértelmezés szerint a lapon a kívánt felhőszolgáltatásba **Szolgáltatási végpontok**csoportban találhatók.
4. Felhőbeli szolgáltatásokhoz jelölje ki a listában, a végpontokat profil ahhoz a felhőszolgáltatások. Törölje a jelet a felhőalapú szolgáltatás neve eltávolítja a listáról, a végpontok.
5. A webhelyeket kattintson a **Szolgáltatás típusa** legördülő listára, és válassza a **Web App alkalmazásban**.
6. Jelölje ki a listában, és vegye fel őket a profil végpontok, a webhelyek. Törölje a jelet a webhely neve eltávolítja a listáról, a végpontok. Figyelje meg, hogy ki, akkor csak egyetlen webhelye per Azure adatközponthoz (más néven régió). Ha egy adatközponthoz, akkor jelölje be az első webhely több webhelyek üzemeltető webhelye lehetőséget választja, mások ugyanazt a adatközpontban elérhetetlenné kijelölés. Tartsa szem előtt, hogy csak a szokásos webhelyek szerepelnek.
7. A profil a végpontok kijelölése, után kattintson a jobb alsó sarkában, a módosítások mentéséhez kattintson a bejelölést.

>[AZURE.NOTE] *Feladatátvevő* forgalom jóváhagyási módszert, miután szeretne hozzáadni vagy eltávolítani zárólap használja, ne módosítsa a feladatátvevő prioritás listában, az beállítása lapon az Ön konfigurációjának kívánt feladatátvevő sorrendjének megfelelően. További tudnivalókért lásd: a [konfigurálása feladatátvevő forgalom útválasztás](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Zárólap letiltása

1. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
2. A lap tetején kattintson az Ön konfigurációjának szereplő végpontjait megtekintheti a **Végpontok** .
3. Kattintson az végpontot, amelyet le szeretne tiltani, és kattintson a **letiltása** az oldal alján.
4. Adatforgalom leáll halad az végpontot, a DNS Time-to-Live (TTL) be van állítva a forgalom Manager tartománynév alapján. A TTL (élettartam) módosíthatja a forgalom Manager profil konfigurációs lapról.

## <a name="to-enable-an-endpoint"></a>Zárólap engedélyezése

1. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
2. A lap tetején kattintson az Ön konfigurációjának szereplő végpontjait megtekintheti a **Végpontok** .
3. Kattintson az engedélyezni kívánt végpontot, és kattintson a **engedélyezése** az oldal alján.
4. Adatforgalom indul el a szolgáltatás újra, mint a profil diktált halad.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Egy felhőalapú szolgáltatás vagy webhely végpontot törlése


1. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
2. A lap tetején kattintson a **végpontjait** megtekintheti, hogy már a konfigurációban a végpontok.
3. A végpontok lapon kattintson a nevére az végpontot, amelyet a profilt törölni szeretné.
4. A lap alján kattintson a **Törlés**gombra.

>[AZURE.NOTE] Külső helyekre vagy forgalom Manager profilok nem törölhető, a végpontokat az Azure klasszikus portálon. A Windows PowerShell kell használnia. További tudnivalókért lásd: az [Eltávolítás-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Következő lépések

- [Feladatátvevő útválasztási mód konfigurálása](traffic-manager-configure-failover-routing-method.md)
- [Ciklikus útválasztási mód konfigurálása](traffic-manager-configure-round-robin-routing-method.md)
- [Teljesítmény útválasztási mód konfigurálása](traffic-manager-configure-performance-routing-method.md)
- [Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)
- [Műveletek a forgalom Manager (REST API-referencia)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
