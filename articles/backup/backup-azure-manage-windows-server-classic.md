<properties
    pageTitle="Azure biztonsági másolat tárolókban és kiszolgálók használata a klasszikus telepítési modell Azure kezelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti az Azure biztonsági másolat tárolókban és -kiszolgálók ebből az oktatóanyagból használata"
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jimpark;markgal"/>


# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Azure biztonsági másolat tárolókban és a klasszikus telepítési modell segítségével a kiszolgálókat kezelése

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](backup-azure-manage-windows-server.md)
- [Klasszikus](backup-azure-manage-windows-server-classic.md)

Ebben a cikkben megtalálja az Azure klasszikus portált, és a Microsoft Azure biztonsági agent keresztül érhető el a biztonságimásolat-kezelési-feladatok áttekintése.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erőforrás-kezelő telepítési modell.

## <a name="management-portal-tasks"></a>Adatkezelési portál feladatok
1. Jelentkezzen be az [adatkezelési portál](https://manage.windowsazure.com).

2. Kattintson a **Helyreállítás szolgáltatások**, majd kattintson a nevére az első lépések lap megtekintéséhez biztonsági tárolóból elemre.

    ![Helyreállítási szolgáltatások](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Jelölje ki az első lépések lap tetején a beállításokat, megjelenik a rendelkezésre álló engedélykezelési feladatai.

![Lapok kezelése](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Irányítópult
Válassza ki **az irányítópult** használatát a kiszolgáló áttekintéséhez. A **használati áttekintése** tartalmazza:

- A szám, a Windows-kiszolgálók regisztrált a felhőbe
- Azure virtuális gépeken futó felhőben védett száma
- A teljes tárterület felhasznált Azure-ban
- A legutóbbi feladatok állapotát

Az irányítópult alján, az alábbi műveleteket végezheti el:

- **Kezelés tanúsítvány** – Ha egy tanúsítványt használtak regisztrálni a kiszolgálót, akkor ennek használatával frissítheti a tanúsítvány. Ha tárolóra hitelesítő adatok használata esetén **kezelése tanúsítvány**nem használható.
- **Törlés** - törlése az aktuális biztonsági tárolóból elemre. Ha már nem használja egy biztonsági tárolóból elemre, törölheti, hogy a rendelkezésre álló tárterület méretének felszabadítani. **Törlés** csak akkor engedélyezett a tárolóból elemre az összes kiszolgálón a regisztrált törlése után.

![Biztonsági másolat irányítópult-feladatok](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Regisztrált cikkek
Válassza a **Registered elemek** megtekintése a tárolóból elemre kattintva regisztrált a kiszolgáló nevét.

![Regisztrált cikkek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

**A szűrő** Azure virtuális gép alapértelmezés szerint. Megtekintheti a tárolóból elemre kattintva regisztrált a kiszolgáló nevét, jelölje be **a Windows server** a legördülő menüből.

További lehetőségek hajtsa végre az alábbi műveleteket:

- **Engedélyezés Re regisztráció** – Ez a beállítás ki van jelölve a kiszolgálón is használhatja a **Regisztrációs varázsló** a helyszíni Microsoft Azure biztonsági Agent másodszori regisztrálhatja a kiszolgáló és a biztonsági másolat tárolóból elemre. A tanúsítvány hiba miatt újraregisztrálása kell, vagy ha egy kiszolgáló kellett az újonnan létrehozott kell.
- **Törlés** - kiszolgáló törlése a a biztonsági másolat tárolóból elemre. A kiszolgáló társított tárolt adatok azonnal törlődik.

    ![Regisztrált cikk feladatok](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Védett elemek
Jelölje be a **Védett elemek** megtekintése, az elemeket, amelyeket a kiszolgálókról mentésben.

![Védett elemek](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurálása

A **beállítás** lapon jelölje ki a megfelelő tárolási redundancia lehetőséget. Jelölje ki a redundancia tárolási lehetőség a legjobb időpontot, jobbra, miután létrehozott egy tárolóból elemre, és mielőtt bármilyen gépek regisztrált rá.

>[AZURE.WARNING] Egy elem van regisztrálva a tárolóból elemre kattintva, amikor a redundancia tárolási lehetőség zárolva van, és nem módosíthatók.

![Konfigurálása](./media/backup-azure-manage-windows-server-classic/configure.png)

Lásd: Ez a cikk további információt a [tárhely redundancia](../storage/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure biztonsági másolat ügynök feladatok

### <a name="console"></a>Konzol

Nyissa meg a **Microsoft Azure biztonsági másolat ügynök** (megtalálja a számítógép keres a *Microsoft Azure biztonsági másolat*).

![Biztonsági másolat agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

A jobb oldali biztonsági ügynök konzol elérhető **Műveletek** hajtsa végre az alábbi felügyeleti műveleteket:

- Kiszolgáló regisztrálása
- Ütemezés biztonsági mentése
- Biztonsági mentése
- Tulajdonságainak módosítása

![Ügynök konzol műveletek](./media/backup-azure-manage-windows-server-classic/console-actions.png)

>[AZURE.NOTE] **Adatok helyreállítása**olvassa el [a Windows server és a Windows ügyfélgép fájlok visszaállítása](backup-azure-restore-windows-server.md).

### <a name="modify-an-existing-backup"></a>Módosítsa a meglévő biztonsági másolat

1. Kattintson a Microsoft Azure biztonsági másolat agent **Ütemezés biztonsági másolatot**.

    ![A Windows Server biztonsági másolat ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

2. Az **Ütemterv biztonsági varázsló** hagyja bejelölve a **biztonságimásolat-elemek vagy az időpontok módosítása** lehetőséget, és kattintson a **Tovább**gombra.

    ![Ütemezett biztonsági módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

3. Ha hozzáadásához vagy módosításához az elemek, a **Biztonsági másolat elemek kijelölése** képernyőn kattintson a **Elemek hozzáadása**gombra.

    Ezen a lapon, a varázsló is beállíthatja **Való felelősség kizárását beállításait** . Ha ki szeretné zárni a fájlok vagy fájltípusok, olvassa el az eljárás a [való felelősség kizárását beállítások](#exclusion-settings)hozzáadására.

4. Jelölje ki a fájlokat és mappákat, készítsen biztonsági másolatot szeretne készíteni, kattintson a **rendben**.

    ![Elemek hozzáadása](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)

5. Adja meg a **biztonsági mentés ütemezése** , és kattintson a **Tovább**gombra.

    (A 3 időpontok naponta legfeljebb) napi vagy heti biztonsági ütemezheti.

    ![Adja meg a biztonsági másolat ütemezése](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

    >[AZURE.NOTE] Adja meg a biztonsági mentés ütemezése magyarázata a jelen [cikk](backup-azure-backup-cloud-as-tape.md)részletesen.

6. Jelölje ki a biztonsági másolat **Adatmegőrzési szabály** , és kattintson a **Tovább**gombra.

    ![Jelölje ki az adatmegőrzési szabály](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)

7. A **megerősítési** képernyőn tekintse át az információkat, és kattintson a **Befejezés gombra**.

8. Miután a varázsló végzett, az **Ütemezés**létrehozását, kattintson a **Bezárás**gombra.

    Védelem módosítása után győződhet meg, hogy biztonsági másolatok megfelelően vannak kiváltó megerősíti, hogy tükröződni fognak a biztonsági másolat feladatok és a **feladatok** lap.

### <a name="enable-network-throttling"></a>Hálózati szabályozásának engedélyezése  
Az Azure biztonsági másolat ügynök tartalmaz egy Throttling-lapon, amely lehetővé teszi, hogy használatának szabályozása hálózati sávszélesség adatátvitel során. Ez a beállítás akkor lehet hasznos, ha módosítani szeretné a biztonsági másolatot során adatok munkaórák, de nem szeretné a biztonsági mentés más internetes forgalmat zavarja. Az adatok szabályozásának átadás vonatkozik biztonsági mentése és visszaállítása a tevékenységeket.  

Ahhoz, hogy szabályozása:

1. Kattintson a **biztonsági másolat ügynök** **Tulajdonságainak módosítása**.

2. Jelölje be az **internetes sávszélesség-használat biztonsági műveletekhez szabályozásának engedélyezése** jelölőnégyzetet.

    ![Hálózati szabályozása](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)

3. Szabályozásának engedélyezése után adja meg a biztonsági másolat adatátvitel engedélyezett sávszélesség **Munkaidő** és **a nem munkaidő**alatt.

    A sávszélesség-értékek 512 KB (KB) másodpercenként a kezdődik, és válassza a legfeljebb 1023 megabájt (MB) másodpercenként. Is kijelölni a kezdési és befejezési a **Munkaidő**, és amelyek a hét napjai számítanak munka nap. A kijelölt munkaidő-Ön kívüli idő munkaidő nem számít.

4. Kattintson az **OK gombra**.

## <a name="exclusion-settings"></a>Kizárás beállítások

1. Nyissa meg a **Microsoft Azure biztonsági másolat ügynök** (megtalálja a számítógép keres a *Microsoft Azure biztonsági másolat*).

    ![Nyissa meg a biztonsági másolat agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

2. Kattintson a Microsoft Azure biztonsági másolat agent **Ütemezés biztonsági másolatot**.

    ![A Windows Server biztonsági másolat ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

3. Az ütemterv biztonsági varázslóban hagyja bejelölve a **biztonságimásolat-elemek vagy az időpontok módosítása** lehetőséget, és kattintson a **Tovább**gombra.

    ![Ütemezett módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

4. Kattintson a **Kivételek beállítások**gombra.

    ![Jelölje be a kizárandó elemek](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)

5. Kattintson a **kivétel hozzáadása**gombra.

    ![Kivételek hozzáadása](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)

6. Jelölje ki azt a helyet, és kattintson az **OK gombra**.

    ![Kizárás helyének kiválasztása](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)

7. A fájl-bővítmény hozzáadása a **Fájl típusa** mezőben.

    ![Fájltípus kizárása](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Egy .mp3 bővítmény hozzáadása

    ![Példa a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Egy másik bővítmény hozzáadása, **Kizárás hozzáadása** gombra, és adjon meg egy másik típusú fájlkiterjesztést (a .jpeg-bővítmény hozzáadása).

    ![Egy másik példa a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)

8. Ha minden bővítmény helyezett el, kattintson az **OK gombra**.

9. Az ütemterv biztonsági másolat varázsló folytassa addig, amíg a **Megerősítés lapon**a **Tovább gombra** kattintva, majd kattintson a **Befejezés gombra**.

    ![Kizárás megerősítése](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Következő lépések
- [A Windows Server vagy a Windows-ügyfél visszaállítása az Azure](backup-azure-restore-windows-server.md)
- Azure biztonsági mentéssel kapcsolatos további tudnivalókért lásd: [Azure biztonsági mentés – áttekintés](backup-introduction-to-azure-backup.md)
- Keresse fel a [Azure biztonságimásolat-fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)
