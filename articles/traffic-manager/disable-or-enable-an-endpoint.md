<properties
   pageTitle="A forgalom Manager végpont engedélyezése vagy letiltása |} Microsoft Azure"
   description="Ez a cikk segítséget nyújt a forgalom Manager profil végpontok engedélyezése vagy letiltása."
   services="traffic-manager"
   documentationCenter="na"
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>A forgalom Manager végpont engedélyezése vagy letiltása

A forgalom Manager profilok részét képező egyéni végpontok le is tilthatja. Végpontokat bele a felhőszolgáltatások és a webhelyek. A profil részeként zárólap letiltása hagyja, de a profil működik, mintha a végpont nem szerepel az azt. Ez a művelet nagyon hasznos, ha ideiglenesen eltávolítása, amely a karbantartási mód, vagy éppen újra nem telepítik zárólap. Miután a végpont lépéseket ismét, hogy engedélyezése

>[AZURE.NOTE] **Zárólap letiltása amelyen semmi sem a teendő az Azure környezetben állapotába. A megfelelő végpont és érkeznek a forgalmat, akkor is, ha az adatforgalom-kezelőben letiltva marad. Ezenkívül egy profil zárólap letiltása nem befolyásolja állapota egy másik profilhoz.**

## <a name="to-disable-an-endpoint"></a>Zárólap letiltása

1. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
1. A lap tetején kattintson az Ön konfigurációjának szereplő végpontjait megtekintheti a **Végpontok** .
1. Kattintson az végpontot, amelyet le szeretne tiltani, és kattintson a **letiltása** az oldal alján.
1. Adatforgalom leáll halad az végpontot, a DNS Time-to-Live (TTL) be van állítva a forgalom Manager tartománynév alapján. A TTL (élettartam) módosíthatja a forgalom Manager profil konfigurációs lapról.

## <a name="to-enable-an-endpoint"></a>Zárólap engedélyezése


1. Az Azure klasszikus portál forgalom kezelő munkaablakban keresse meg a módosítani kívánt végpont beállításokat tartalmazó forgalom Manager profil, és válassza a profil nevétől jobbra található nyílra. Ekkor megnyílik a beállítások lap a profilhoz.
1. A lap tetején kattintson az Ön konfigurációjának szereplő végpontjait megtekintheti a **Végpontok** .
1. Kattintson az engedélyezni kívánt végpontot, és kattintson a **engedélyezése** az oldal alján.
1. Adatforgalom indul el a szolgáltatás újra, mint a profil diktált halad.

## <a name="next-steps"></a>Következő lépések

[Kezelő – letiltás engedélyezése forgalom profil vagy törlése](disable-enable-or-delete-a-profile.md)

[Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)

[Adatforgalom Manager teljesítménnyel kapcsolatos szempontok](traffic-manager-performance-considerations.md)