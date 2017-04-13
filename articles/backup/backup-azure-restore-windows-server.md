<properties
   pageTitle="Adatok visszaállítása a Windows Server vagy Windows-ügyfél az Azure az erőforrás-kezelő telepítési modell |} Microsoft Azure"
   description="Megtudhatja, hogy miként állíthatja helyre a Windows Server vagy Windows-ügyfél."
   services="backup"
   documentationCenter=""
   authors="saurabhsensharma"
   manager="shivamg"
   editor=""/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
     ms.date="08/02/2016"
     ms.author="trinadhk; jimpark; markgal;"/>

# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>A Windows server és a Windows ügyfélgép erőforrás-kezelő telepítési modell használata a fájlok visszaállítása

> [AZURE.SELECTOR]
- [Azure portál](backup-azure-restore-windows-server.md)
- [Klasszikus portál](backup-azure-restore-windows-server-classic.md)

Ez a cikk bemutatja a kétféle visszaállítási műveletek elvégzéséhez szükséges lépéseket:

- Állítsa vissza az adatokat, amelyhez a biztonsági másolatok készítésének ugyanarra a számítógépre.
- Adatok visszaállítása bármelyik másik számítógépre.

Mindkét esetben az adatok beolvasása az Azure helyreállítási szolgáltatások tárolóból elemre.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

## <a name="recover-data-to-the-same-machine"></a>Adatok ugyanarra a számítógépre visszaállítása
Ha véletlenül törölte egy fájlt, és szeretné visszaállítani a ugyanarra a gépre (amely a biztonsági mentés származik), az alábbi lépésekkel segítséget nyújt az adatok helyreállítása.

1. Nyissa meg a **Microsoft Azure biztonsági másolat** beépülő modul.
2. **Adatok helyreállítása** a munkafolyamat indítása gombra.

    ![Adatok visszaállítása](./media/backup-azure-restore-windows-server/recover.png)

3. Jelölje ki a * *a kiszolgáló (*yourmachinename*) ** beállításra a biztonsági másolatot fájlról ugyanazon a gépen.

    ![Ugyanarra a gépre](./media/backup-azure-restore-windows-server/samemachine.png)

4. Válassza a **fájlok keresse meg** vagy **fájlok keresése**.

    Ha szeretne egy vagy több fájlt, amelynek az elérési útját ismert visszaállítása, hagyja az alapértelmezett beállítás. Ha kapcsolatban a mappaszerkezet nem biztos, de szeretne megkeresni egy fájlt, válassza ki a **fájlok keresése** lehetőséget. Ez a szakasz céljából azt folytatódik, az alapértelmezett beállítást választja.

    ![Fájlok keresése](./media/backup-azure-restore-windows-server/browseandsearch.png)

5. Jelölje ki a hangerőt, amelyből ki szeretne a fájl visszaállítása.

    Szükség esetén visszaállíthatja minden helyétől időben. **Félkövér** a naptárban megjelenő dátumok azt jelzik, hogy a visszaállítási pont elérhetőségét. Dátum kijelölése után a biztonsági mentés ütemezése (és a biztonsági mentés sikeres) alapján kijelölhet egy időben a **idő** legördülő listáról lefelé.

    ![Mennyiség és a dátum](./media/backup-azure-restore-windows-server/volanddate.png)

6. Jelölje ki a helyreállítani az elemeket. Több elem kijelölése/mappafájlok visszaállítani kívánt is.

    ![Válassza a fájlok](./media/backup-azure-restore-windows-server/selectfiles.png)

7. A helyreállítási paramétert.

    ![Helyreállítási lehetőségeket](./media/backup-azure-restore-windows-server/recoveroptions.png)

  - Ha egy visszaállítása az eredeti helyre (amelyben a fájl/mappa volna felül) vagy egy másik helyre a ugyanarra a gépre lehetőséget.
  - Ha létezik a visszaállítani kívánt fájl vagy mappa a célt, példányt (ugyanannak a fájlnak két verziója) hoz létre, felülírja a fájlokat a célhelyen vagy ugorja át a fájlokat, amelyek léteznek a célhely visszanyerése.
  - Kilépés a, a hozzáférés-vezérlési listák, a fájlokat, amelyek helyreállítása folyamatban lévő visszaállítása az alapértelmezett beállítás nagyon ajánlott.

8. Miután érhetők el ezeket a bemeneti adatok alapján, kattintson a **Tovább**gombra. A helyreállítási munkafolyamat ezen a számítógépen lévő fájlok visszaállítása, meg is kezdi.

## <a name="recover-to-an-alternate-machine"></a>Egy másik gépre helyreállítása
A teljes kiszolgáló nem vesznek el, ha továbbra is visszaállíthatja adatok Azure biztonsági másolatból egy másik számítógépre. A következő lépések bemutatják a munkafolyamatot.  

Az alábbi lépéseket a terminológia tartalmazza:

- *Forrás gépi* – a biztonsági mentés történt, és jelenleg nem érhető el ez az eredeti számítógépen.
- *Cél gépi* – a számítógépen, amelyre az adatokat visszaállítani.
- *Minta tárolóra* – a *forrás gépi* és a *cél gépi* regisztrált, amelyhez a helyreállítási szolgáltatások tárolóból elemre. <br/>

> [AZURE.NOTE] A számítógépen, amely az operációs rendszer egy korábbi verziójában nem lehet visszaállítani a számítógépről készített biztonsági másolatok. Például biztonsági másolatok veszik a Windows 7 gépről, ha, ahonnan a Windows 8 vagy a számítógép felett. A fordítva azonban még nem tartsa igaz.

1. Nyissa meg a **Microsoft Azure biztonsági másolat** beépülő modul a *cél gépi*.
2. Győződjön meg arról, hogy a *célhely gépi* és a *forrás gépi* regisztrált az azonos helyreállítási szolgáltatások tárolóból elemre kattintva.
3. **Adatok helyreállítása** a munkafolyamat indítása gombra.

    ![Adatok visszaállítása](./media/backup-azure-restore-windows-server/recover.png)

4. Jelöljön ki **egy másik kiszolgálót**

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server/anotherserver.png)

5. Adja meg a tárolóból elemre hitelesítőadat-fájlt, amely megfelel a *minta tárolóból elemre*kattintva. Ha a tárolóból elemre hitelesítőadat-fájl érvénytelen (vagy lejárt) töltse le a új tárolóból elemre hitelesítőadat-fájl a *minta tárolóból elemre* az Azure-portálon. Miután a tárolóból elemre hitelesítőadat-fájl hiányzik, szemben a tárolóból elemre hitelesítőadat-fájlt a helyreállítási szolgáltatások tárolóra jelenik meg.

6. Jelölje be a *forrás gépi* megjelenített gépek a listából.

    ![Számítógépek listája](./media/backup-azure-restore-windows-server/machinelist.png)

7. Válassza a **fájlok keresése** vagy **fájlok keresése** lehetőséget. Ez a szakasz céljából fogjuk használni a **fájlok keresése** lehetőséget.

    ![Keresés](./media/backup-azure-restore-windows-server/search.png)

8. Jelölje ki a következő képernyőn a mennyiség és a dátum. Keresse meg a visszaállítani kívánt mappa vagy fájl nevét.

    ![Elemek keresése](./media/backup-azure-restore-windows-server/searchitems.png)

9. Jelölje ki azt a helyet, ahol a fájlok szükséges visszaállíthatók.

    ![Visszaállítás helye](./media/backup-azure-restore-windows-server/restorelocation.png)

10. Adja meg a titkosítási jelszó *forrás gépi* regisztráció során kapott *minta tárolóból elemre*.

    ![Titkosítás:](./media/backup-azure-restore-windows-server/encryption.png)

11. Miután a bemeneti megadva, kattintson a **helyreállítani**, amely elindítja a biztonsági másolat fájlokat a cél, feltéve hogy helyreállítása.

## <a name="next-steps"></a>Következő lépések
- Most, hogy Ön már helyreállított a fájlokat és mappákat, akkor [a biztonsági másolatok kezelése](backup-azure-manage-windows-server.md).
