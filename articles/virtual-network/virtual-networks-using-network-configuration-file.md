<properties 
    pageTitle="A hálózati konfigurációs fájl segítségével virtuális hálózat konfigurálása" 
    description="Utasítások az Azure adatkezelési portálra létrehozása vagy módosítása a virtuális hálózatokat abból a hálózati konfigurációs fájl importálásához és exportálásához. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>A hálózati konfigurációs fájl segítségével virtuális hálózat konfigurálása

Egy virtuális hálózati (VNet) konfigurálhatja az Azure adatkezelési portál használatával, illetve egy hálózati konfigurációs fájl használatával.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Létrehozó és módosító felhasználói hálózati konfigurációs fájl 
A tartalom-előállítás hálózati konfigurációs fájl legegyszerűbben a hálózati beállítások exportálása egy meglévő virtuális hálózat konfigurálása a fájl, amely tartalmazhat be szeretné állítani a virtuális hálózat beállításainak módosításával.

Ha szerkeszteni szeretné a hálózati konfigurációs fájl, egyszerűen nyissa meg a fájlt programot, végezze el a szükséges módosításokat, majd a mentse a fájlt. Ha módosítani szeretné a hálózati konfigurációs fájl bármely *XML-* szerkesztő is használhatja. 

Az útmutató a [hálózati konfigurációs fájl séma](https://msdn.microsoft.com/library/azure/jj157100.aspx)szorosan célszerű követnie. 

Azure, amely tartalmaz valamit, **használja**a rendszerbe állított alhálózat számításba veszi. Alhálózat használatban van, nem módosítható. Módosítása, előtt helyezze mindent, egy másik alhálózathoz nem éppen módosított az alhálózathoz telepítette.   Lásd: a [virtuális vagy egy másik alhálózat szerepkör-példányt áthelyezése](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Az adatkezelési portálon virtuális hálózati beállítások exportálása és importálása  
Importálása, és a hálózati konfigurációs fájl található PowerShell- vagy az adatkezelési portál használatával hálózati kereséskonfigurációs beállítások exportálása. Az alábbi útmutatást segítséget exportálása és importálása az adatkezelési portálon. 

### <a name="to-export-your-network-settings"></a>A hálózat beállításainak exportálása
Amikor, minden beállítás a virtuális hálózatokhoz az előfizetés írandó .xml fájl. 

1. Jelentkezzen be az **adatkezelési portál**.
2. Az adatkezelési portálon a **hálózatok** lap alján kattintson az **Exportálás**parancsra. 
3. A **hálózati konfigurálásról exportálása** ablakban ellenőrizze, hogy ki van kijelölve az előfizetést, amelyhez a megadott hálózati beállítások exportálni kívánt. Kattintson a jobb alsó sarkában kattintson a bejelölést. 
4. Amikor a program kéri, mentse *NetworkConfig.xml* a megfelelő helyre.


### <a name="to-import-your-network-settings"></a>A hálózati beállítások importálása

1. Az **Adatkezelési portál**bal alsó sarokban, kattintson a navigációs ablakban kattintson az **Új**gombra.
2. Kattintson a **hálózati szolgáltatások** -> **virtuális hálózati** -> **konfiguráció importálása**.
3. **A hálózati konfigurációs fájl importálása** lapon tallózással keresse meg a hálózati konfigurációs fájl, és kattintson a **következő** mutató nyílra.
4. **A hálózat létrehozása** lapon megjelenik a bemutató mely részei a hálózati konfigurálásról lesz módosítható vagy létrehozott képernyőn megjelenő információkat. A változtatások úgy néznek ki Önnek megfelelő, kattintson a frissíteni, vagy hozzon létre virtuális hálózatához folytatni a bejelölést. 