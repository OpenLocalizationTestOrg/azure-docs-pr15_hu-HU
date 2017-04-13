<properties
    pageTitle="Azure forgalom Manager végpontok kezelése |} Microsoft Azure"
    description="Ez a cikk segít hozzáadása, eltávolítása, engedélyezése és letiltása a végpontok Azure forgalom Manager alkalmazásból."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Adja hozzá, letiltása, engedélyezése és törlése a végpontok

A Web Apps alkalmazások Azure alkalmazás szolgáltatás már szolgáltatása feladatátvétel és a webhelyek belül egy adatközpont, függetlenül a webhely mód ciklikus forgalom útválasztási funkciókat. Azure forgalom Manager feladatátvétel és ciklikus forgalom továbbítása különböző adatközpont esetén webhelyekre és felhőbeli szolgáltatásokhoz megadását teszi lehetővé. Az első lépés biztosítani, hogy funkció hozzáadása a felhőalapú szolgáltatás vagy webhely végpont forgalom Manager.

>[AZURE.NOTE]  Ez a cikk ismerteti, hogyan szeretné használni a klasszikus portált. Az Azure klasszikus portál csak akkor támogatja, a létrehozási és a hozzárendelés a felhőszolgáltatások és Web Apps alkalmazások, a végpontok. Az új [Azure portált](https://portal.azure.com) a használni kívánt felület.

A forgalom Manager profilok részét képező egyéni végpontok le is tilthatja. A profil részeként zárólap letiltása hagyja, de a profil működik, mintha a végpont nem szerepel az azt. Ez a művelet akkor lehet hasznos, amely a karbantartási mód, vagy éppen újra nem telepítik zárólap ideiglenes eltávolíthat. Miután a végpont lépéseket ismét, engedélyezhető.

>[AZURE.NOTE] Zárólap letiltása amelyen semmi sem a teendő az Azure környezetben állapotába. A megfelelő végpont és érkeznek a forgalmat, akkor is, ha az adatforgalom-kezelőben letiltva marad. Ezenkívül egy profil zárólap letiltása nem befolyásolja állapota egy másik profilhoz.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Egy felhőalapú szolgáltatás vagy webhely végpont hozzáadása

1. Az Azure klasszikus portálon a forgalom kezelő ablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil. A beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
2. A lap tetején kattintson a **végpontjait** megtekintheti, hogy már a konfigurációban a végpontok.
3. A lap alján kattintson a **Hozzáadás** a **Szolgáltatás végpontok hozzáadása** lap eléréséhez. Alapértelmezés szerint a lapon a kívánt felhőszolgáltatásba **Szolgáltatási végpontok**csoportban találhatók.
4. Cloud Services jelölje ki a listában, a végpontokat profil hozzáadni kívánt felhőszolgáltatásba. Törölje a jelet a felhőalapú szolgáltatás neve eltávolítja a listáról, a végpontok.
5. A webhelyeket kattintson a **Szolgáltatás típusa** legördülő listára, és válassza a **Web App alkalmazásban**.
6. Jelölje ki a listában, és vegye fel őket a profil végpontok, a webhelyek. Törölje a jelet a webhely neve eltávolítja a listáról, a végpontok. Megadhatja, hogy csak egy webhely használati Azure adatközponthoz (más néven régió). Jelölje ki az első webhelyet, ha a más webhelyek, a az azonos adatközponthoz elérhetetlenné kijelölése. Tartsa szem előtt, hogy csak a szokásos webhelyek szerepelnek.
7. A profil a végpontok kijelölése, után kattintson a jobb alsó sarkában, a módosítások mentéséhez kattintson a bejelölést.

>[AZURE.NOTE] Hozzáadása, vagy a forgalom útválasztási *feladatátvevő* módszerrel profil végpont eltávolítása után a feladatátvevő prioritás lista nem rendezhető azokat meg a kívánt módon. Beállíthatja, hogy a feladatátvevő prioritás listában az beállítása lapon sorrendjét. További tudnivalókért lásd: a [konfigurálása feladatátvevő forgalom útválasztás](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Zárólap letiltása

1. Az Azure klasszikus portálon a forgalom kezelő ablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil. A beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
2. A lap tetején kattintson az Ön konfigurációjának szereplő végpontjait megtekintheti a **Végpontok** .
3. Kattintson az végpontot, amelyet le szeretne tiltani, és kattintson a **letiltása** az oldal alján.
4. Ügyfelek forgalmat továbbra is a végpontra Time-to-Live (TTL) időtartamára. A TTL (élettartam), az beállítása lapon a forgalom Manager profil módosíthatja.

## <a name="to-enable-an-endpoint"></a>Zárólap engedélyezése

1. Az Azure klasszikus portálon a forgalom kezelő ablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil. A beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
2. A lap tetején kattintson az Ön konfigurációjának szereplő végpontjait megtekintheti a **Végpontok** .
3. Kattintson az engedélyezni kívánt végpontot, és kattintson a **engedélyezése** az oldal alján.
4. Ügyfelek irányítja át az engedélyezett végpontot, határozza meg a profilt.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Egy felhőalapú szolgáltatás vagy webhely végpontot törlése

1. Az Azure klasszikus portálon a forgalom kezelő ablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil. A beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
2. A lap tetején kattintson a **végpontjait** megtekintheti, hogy már a konfigurációban a végpontok.
3. A végpontok lapon kattintson a nevére az végpontot, amelyet a profilt törölni szeretné.
4. A lap alján kattintson a **Törlés**gombra.

## <a name="next-steps"></a>Következő lépések

* [Adatforgalom Manager profilok kezelése](traffic-manager-manage-profiles.md)
* [Útválasztási módszerek beállítása](traffic-manager-configure-routing-method.md)
* [Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)
* [Adatforgalom Manager teljesítménnyel kapcsolatos szempontok](traffic-manager-performance-considerations.md)
* [Műveletek a forgalom Manager (REST API-referencia)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
