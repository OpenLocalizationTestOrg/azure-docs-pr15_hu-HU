<properties
    pageTitle="Hozzon létre egy Linux virtuális az Azure-portálon |} Microsoft Azure"
    description="Hozzon létre egy Linux virtuális az Azure-portálon."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Hozzon létre egy Linux virtuális Azure a portálon


Ez a cikk bemutatja, hogyan Linux virtuális gép létrehozása az [Azure portal](https://portal.azure.com/) segítségével.

A követelmények a következők:

- [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)

- [SSH nyilvános és titkos kulcs fájlok](virtual-machines-linux-mac-create-ssh-keys.md)


1. Jelentkezett be az Azure-fiók személyazonosságát az Azure portált, kattintson a **+ Új** bal felső sarokban lévő:

    ![képernyő1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Kattintson a **piactér** **virtuális gépeken futó** , majd a **Ubuntu kiszolgáló 14.04 LTS** a **Kiemelt alkalmazások** képek listában.  Ellenőrizze a képernyő alján, hogy van-e a telepítési modell `Resource Manager` , és kattintson a **Létrehozás**gombra.

    ![képernyő2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. **Alapvető tudnivalók** lapon adja meg:
    - a virtuális nevét
    - a felügyeleti felhasználó felhasználónév
    - a **nyilvános kulcshoz SSH** beállítása a hitelesítés típusa
    - a SSH nyilvános kulcs karakterláncként (a a `~/.ssh/` címtár)
    - erőforrás neve és a csoport jelölje ki a Communicator alkalmazás

    Kattintson **az OK gombra** a folytatáshoz, és válassza ki a virtuális méretét; azt hasonlóan kell kinéznie a következőket:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Válassza a **DS1** méretét, a prémium SSD Ubuntu telepíti, és kattintson a **Válassza a** beállítások konfigurálása.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. A **Beállítások**területen hagyja az alapértelmezett értékeket tároló és a hálózati az értékek, és kattintson **az OK** gombra az összefoglalás megtekintéséhez.  Értesítés a lemez típusának kiválasztása a DS1 van beállítva prémium SSD, az **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Erősítse meg a beállításokat az új Ubuntu virtuális, és kattintson az **OK gombra**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Nyissa meg a portálon irányítópult és a **hálózati kapcsolatok** válassza a hálózati kártya

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Nyissa meg a nyilvános IP-címek menüt a hálózati kártya beállításai

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. A nyilvános IP-a SSH nyilvános kulccsal be SSH

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Következő lépések

Most már létrehozott egy Linux virtuális gyorsan célokra használandó vizsgálat vagy a bemutató. Hozzon létre egy Linux virtuális szabta a infrastruktúra, követheti, ezek a cikkek közül.

- [Hozzon létre egy Linux virtuális Azure sablonok használata](virtual-machines-linux-cli-deploy-templates.md)
- [Hozzon létre egy SSH védett Linux virtuális Azure sablonok használata](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Hozzon létre egy Linux virtuális az Azure CLI használatával](virtual-machines-linux-create-cli-complete.md)
