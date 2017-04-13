<properties
   pageTitle="Tiltsa le, engedélyezése és törlése a forgalom Manager profilok |} Microsoft Azure"
   description="Ez a cikk segít a forgalom Manager profilok kezelése."
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

# <a name="disable-enable-or-delete-a-profile"></a>Tiltsa le, engedélyezése és profil törlése


Egy meglévő forgalom Manager profil letilthatja, hogy azt nem hivatkozik felhasználói kérések konfigurált végpontját. Amikor letiltja a forgalom Manager profilok, a saját profil és a profil tárolt adatok érintetlen marad, és a forgalom kezelő felület szerkesztheti. Ha újra be kell kapcsolnia a profillal, könnyen végrehajtható, az Azure-ban a portál és -átirányítások folytatódik. A forgalom Manager profilok az Azure-portálon létrehozásakor automatikusan engedélyezve van.

## <a name="to-disable-a-profile"></a>A profil letiltása

1. Módosítsa a DNS-erőforrásrekord megfelelő rekordtípust és egy másik nevet vagy a IP-címe adott pontjára mutató használja az interneten az Internet a DNS-kiszolgálóra. A DNS-erőforrásrekord az Internet a DNS-kiszolgálóra más szóval, módosíthatja, hogy már nem használja egy CNAME rekord, amely a tartománynév a forgalom Manager profil mutat.
1. Adatforgalom leáll éppen irányítja a forgalmat Manager profilbeállítások keresztül a végpontok.
1. Jelölje be a letiltani kívánt profilt. Jelölje ki a profil lapon a forgalmat, jelölje ki a profilnév melletti oszlopban kattintson a profil. Nem válassza a: a profil vagy a neve melletti nyílra, ezzel eljut a beállítások lap a profil.
1. A profil megadása után kattintson a lap alján letiltása.

## <a name="to-enable-a-profile"></a>Ahhoz, hogy a profil

1. Jelölje ki a profillal, amely esetén engedélyezni szeretné. Jelölje ki a profil lapon a forgalmat, jelölje ki a profilnév melletti oszlopban kattintson a profil. Nem válassza a: a profil vagy a neve melletti nyílra, ezzel eljut a beállítások lap a profil.
1. A profil kiválasztása után kattintson az engedélyezés, az oldal alján.
1. A DNS-erőforrásrekord az Internet a DNS-kiszolgálóra használni a CNAME rekord típusát, amely a vállalat tartománynevét megfelelteti a tartománynév a forgalom Manager profil módosítása. További tudnivalókért olvassa el a [pont internetes forgalmat Manager tartományhoz vállalati tartomány](traffic-manager-point-internet-domain.md)című témakört.
1. Adatforgalom elkezdenek át a végpontok újra.

## <a name="delete-a-profile"></a>Profil törlése


1. Győződjön meg arról, hogy a DNS-erőforrásrekord az Internet a DNS-kiszolgálóra nem használja egy CNAME rekord, amely a tartománynév a forgalom Manager profil mutat.
1. Jelölje ki a törölni kívánt profilt. Jelölje ki a profil lapon a forgalmat, jelölje ki a profil
1. Kattintson a profil melletti oszlopban. Nem válassza a: a profil vagy a neve melletti nyílra, ezzel eljut a beállítások lap a profil.
1. A profil megadása után kattintson a Törlés az oldal alján.

## <a name="next-steps"></a>Következő lépések

[Adatforgalom Manager - letiltása vagy engedélyezése zárólap](disable-or-enable-an-endpoint.md)

[Feladatátvevő útválasztási mód konfigurálása](traffic-manager-configure-failover-routing-method.md)

[Ciklikus útválasztási mód konfigurálása](traffic-manager-configure-round-robin-routing-method.md)

[Teljesítmény útválasztási mód konfigurálása](traffic-manager-configure-performance-routing-method.md)

[Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)

