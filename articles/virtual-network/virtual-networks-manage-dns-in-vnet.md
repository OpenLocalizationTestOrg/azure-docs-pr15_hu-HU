<properties 
   pageTitle="A virtuális hálózati (VNet) által használt DNS-kiszolgálók kezelése"
   description="Megtudhatja, hogy miként vehet fel, és távolítsa el a DNS-kiszolgálók virtuális hálózatban (vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>A virtuális hálózati (VNet) által használt DNS-kiszolgálók kezelése

Az adatkezelési portálon vagy a hálózati konfigurációs fájl egy VNet használt DNS-kiszolgálók listájának kezelése Az egyes VNet 12 DNS-kiszolgálók is hozzáadhat. A DNS-kiszolgálók meghatározásakor fontos ellenőrizze, hogy a DNS-kiszolgálói környezetben a megfelelő sorrendben listára. Ciklikus DNS server listák nem működnek. Szolgálnak, hogy meghatározott sorrendben. Ha a lista első DNS-kiszolgáló tudja érhető el, akkor az ügyfél, hogy DNS-kiszolgálót, függetlenül attól, hogy a DNS-kiszolgáló működik megfelelően vagy nem fogja használni. A DNS-kiszolgáló virtuális hálózatához sorrendjének módosításához a DNS-kiszolgálók eltávolítása a listából, és felveheti azokat vissza a kívánt sorrendben.

>[AZURE.WARNING] Miután megtörtént a DNS-lista frissítése, újra kell indítani a virtuális gépeken futó található virtuális hálózatához, hogy az illető felveszi az új DNS-kiszolgáló beállításait. Virtuális gépeken futó folytatja az aktuális beállításra azok újraindításáig.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Az adatkezelési portálon virtuális hálózati DNS server lista szerkesztése

1. Jelentkezzen be az **adatkezelési portál**rendszerbe.

1. A navigációs ablakban kattintson a **hálózatokat**, és kattintson a **név** oszlopban a virtuális hálózat nevét.

1. Kattintson a **konfigurálása**.

1. A **DNS-kiszolgálók**adhatja meg a következőket:

    - **(Hozzáadás) egy új DNS-kiszolgáló – regisztrálása** Egyszerűen írja be a nevét és IP-címet a mezőbe. A DNS-kiszolgáló hozzáadja a virtuális hálózati DNS-kiszolgálók listában, és a is regisztrálja a DNS-kiszolgáló Azure.

    - **A DNS-kiszolgáló, amely korábban regisztrált – hozzáadása** Ha már bejegyezte DNS-kiszolgáló Azure, választhat az előre elhelyezett listából.

    - **Ha el szeretné távolítani a virtuális hálózatról – a DNS-kiszolgáló** Kattintson az eltávolítani kívánt kiszolgáló melletti X ikonra. Figyelje meg, hogy ezzel csak a művelettel eltávolítja a kiszolgáló a virtuális hálózati listából. A DNS-kiszolgáló használata a más virtuális hálózatokon az Azure-ban regisztrált marad. A DNS-kiszolgáló törlése az előfizetésből, lépjen a **hálózatok -> DNS-kiszolgálók** lapot.

    - **A DNS-kiszolgálók – más sorrendbe** Vissza a sorrendben, amelyben az összes jelennek meg, és adja hozzá őket a DNS-kiszolgálók eltávolítása. Ne feledje, hogy ez nem ciklikus DNS listáját.

    - **A DNS-kiszolgáló – átnevezése** Jelölje ki a DNS-kiszolgáló a listában, majd írja be az új nevet. Ez lesz egy új DNS-kiszolgáló regisztrálása az Azure, valamint vegye fel a DNS-kiszolgálók listában a virtuális hálózathoz. A régi DNS-kiszolgáló és IP-címének regisztrált az Azure marad. Törölheti azt a **DNS-kiszolgálók** lapon, ha nem használ, minden más virtuális hálózatokhoz.

1. Kattintson a **Mentés** a lap alján az új DNS-kiszolgálók konfiguráció mentéséhez.

1. Indítsa újra a virtuális gépeken futó szerezheti be az új DNS-beállítások engedélyezheti a virtuális hálózaton található.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Hálózati konfigurációs fájl használatával DNS server lista szerkesztése

Szerkesztéséhez DNS server hálózati konfigurációs fájl segítségével, először az adatkezelési portálról fogja exportálja a megadott beállításokat. Meg kell majd a hálózati konfigurációs fájl szerkesztése, és importálja a vissza az adatkezelési portálon keresztül. Az alábbi képen az Ez az eljárás végrehajtásához szükséges lépések magas szintű listáját.

1. A virtuális hálózat beállításainak exportálása egy hálózati konfigurációs fájl. És további információt a hálózat kereséskonfigurációs beállítások exportálása a lépéseket olvassa el a [Virtuális hálózat beállításainak exportálása egy hálózati konfigurációs fájl](virtual-networks-using-network-configuration-file.md)című témakört.

1. Adja meg a DNS-kiszolgáló adatait a virtuális hálózathoz. A DNS-kiszolgáló megadásáról további tudnivalókért lásd: [Adja meg a DNS-kiszolgáló hálózati virtuális konfigurációs fájl](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). További információt a hálózat konfigurációs fájl lásd: [Azure virtuális hálózati konfigurációja séma](https://msdn.microsoft.com/library/azure/jj157100.aspx) és [konfigurálása a virtuális hálózati használatával egy hálózati konfigurációs fájl](virtual-networks-using-network-configuration-file.md).

1. A hálózati konfigurációs fájl importálása. További információk és a hálózati konfigurációs fájl importálása a lépéseket olvassa el a [hálózati konfigurációs fájl importálása](virtual-networks-using-network-configuration-file.md)című témakört.

1. Indítsa újra a virtuális gépeken futó szerezheti be az új DNS-beállítások engedélyezheti a virtuális hálózaton található.
