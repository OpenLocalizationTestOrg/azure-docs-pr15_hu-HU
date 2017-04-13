<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Update DNS-beállításait az Azure virtuális hálózati |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások – első lépések"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure Active Directory tartományi szolgáltatások – az Azure virtuális hálózati Update DNS-beállításait

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Feladat 4: Frissítése az Azure virtuális hálózati DNS-beállításait
Előző konfigurációs feladatokat, sikeresen engedélyezte a címtárában az Azure Active Directory tartományi szolgáltatások. A következő tevékenység meggyőződni arról, hogy a virtuális hálózaton belül számítógépek kapcsolódni, és felhasználása az alábbi szolgáltatások. Frissítse a DNS-kiszolgáló beállításait a virtuális hálózat mutasson a két IP-címet, amelyre Azure Active Directory tartományi szolgáltatások érhető el a virtuális hálózaton.

> [AZURE.NOTE] Megjegyzés: az IP-címek lefelé az Azure Active Directory tartományi szolgáltatások, a könyvtárat **konfigurálása** lapon jelennek meg, miután engedélyezte a könyvtár Azure Active Directory tartományi szolgáltatások.

Hajtsa végre az alábbi konfigurálási lépéseket a DNS-kiszolgáló beállítása, amelyben az Azure Active Directory tartományi szolgáltatások engedélyezte a virtuális hálózat frissítése.

1. Nyissa meg az **Azure klasszikus portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Jelölje ki a bal oldali ablaktáblában a **hálózatok** csomópontot.

    ![Virtuális hálózatok csomópontot.](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. A **Virtuális hálózatok** lapon jelölje ki a virtuális hálózat, amelyben engedélyezett a Azure Active Directory tartományi szolgáltatások tulajdonságainak megjelenítéséhez.

4. Kattintson a **beállítás** fülre.

    ![Virtuális hálózatok csomópontot.](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. A **DNS-kiszolgálók** csoportban adja meg az Azure Active Directory tartományi szolgáltatások IP-címét.

6. Győződjön meg arról, hogy beírta a mind az IP-címek a címtárában **Tartományi szolgáltatások** szakaszában a **beállítás** lapon látható.

7. Szeretné menteni a DNS-kiszolgáló beállításait virtuális hálózat, kattintson a munkaablak, a lap alján kattintson a **Mentés** gombra.

   ![Frissítse a DNS-kiszolgáló beállításait a virtuális hálózat.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Miután frissítette a DNS-kiszolgáló beállításait a virtuális hálózat, a hálózaton, hogy a frissített DNS-konfigurációs első telhet, amíg az virtuális gépeken futó. Ha egy virtuális gép nem tudja elérni a tartományt, a DNS-gyorsítótár (pl. is ürítése. "ipconfig/flushdns") a virtuális gépen. Ez a parancs hatására a egy, a virtuális gépen a DNS-beállítások frissítése.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Tevékenység 5 – a jelszó-szinkronizálás engedélyezése az Azure Active Directory tartományi szolgáltatások
[Azure Active Directory tartományi szolgáltatások jelszó-szinkronizálás](active-directory-ds-getting-started-password-sync.md)engedélyezése, akkor a következő konfigurációs feladatot.
