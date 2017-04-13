<properties
   pageTitle="Jelszó alaphelyzetbe állítása helyi Windows Ha nincs telepítve a Azure Vendég ügynök |} Microsoft Azure"
   description="Hogyan kell a helyi Windows felhasználói fiók jelszavának alaphelyzetbe állítása, ha az Azure Vendég agent nincs telepített vagy nem működik a virtuális a"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Helyi Windows-jelszó visszaállítása Azure virtuális
Visszaállíthatja az Azure segítségével az [Azure portálja vagy az Azure PowerShell](virtual-machines-windows-reset-rdp.md) az Azure Vendég ügynök telepítve van egy virtuális helyi Windows jelszavát. Ez a módszer elsődleges módja a jelszó alaphelyzetbe állítása az Azure virtuális. Ha nem válaszol Azure Vendég ügynök tapasztal problémákat, vagy az adatkapcsolat telepítése után egy egyéni képet feltölteni, manuálisan is Windows a jelszó alaphelyzetbe állítása. Ez a cikk a helyi fiók jelszavának alaphelyzetbe állítása a által a forrás OS virtuális lemezre csatol egy másik virtuális részletezi. 

> [AZURE.WARNING] Csak használja ezt a folyamatot utolsó lehetőségként. Mindig próbálja meg a jelszó alaphelyzetbe állítása az [Azure portálja vagy az Azure PowerShell](virtual-machines-windows-reset-rdp.md) először használja.


## <a name="overview-of-the-process"></a>A folyamat áttekintése
Helyi jelszó alaphelyzetbe egy Windows virtuális Azure-ban, ha nem lehet hozzáférni az Azure Vendég ügynökének elvégzéséhez alapvető lépései a következőképpen történik:

- A forrás virtuális törölhető. A virtuális lemez megőrződnek.
- A forrás virtuális OS lemez csatolhat egy másik virtuális belül az Azure előfizetés. A virtuális a hibaelhárítási virtuális nevezik.
- A forrás virtuális-OS lemezen néhány config fájlokat a hibaelhárítási virtuális használatával hozhat létre.
- A virtuális OS lemez át a hibaelhárítási virtuális leválasztása.
- Hozzon létre egy virtuális az eredeti virtuális lemezzel egy erőforrás-kezelő sablonnal.
- Az új virtuális indításakor a config fájlokat hoz létre frissítse a szükséges a felhasználó jelszavát.


## <a name="detailed-steps"></a>A lépések részletes leírását
Mindig próbálja meg a jelszó alaphelyzetbe állítása az [Azure portálja vagy az Azure PowerShell](virtual-machines-windows-reset-rdp.md) használata előtt az alábbi lépéseket. Győződjön meg arról, hogy a biztonsági másolatot a virtuális megkezdése előtt. 

1. Az Azure-portálon a szóban forgó virtuális törölhető. Csak a virtuális törlése törli a metaadatokat, a belül Azure virtuális hivatkozását. A virtuális törlésekor a program megőrzi a virtuális lemez:

    - Jelölje ki a virtuális az Azure-portálon, kattintson a *Törlés*gombra:

    ![Meglévő virtuális törlése](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. A forrás virtuális OS lemezre csatolása a hibaelhárítási virtuális. A hibaelhárítási virtuális kell lennie, mint a forrás virtuális OS lemez ugyanabban a régióban (például: `West US`):

    - Jelölje ki a hibaelhárítási virtuális az Azure-portálon. Kattintson a *lemez* | *csatolása meglévő*:

    ![Meglévő lemez csatolása](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Jelölje ki a *Virtuális fájlt* , és válassza a a tárterület-fiókot, amely tartalmazza a forrást virtuális:

    ![Jelölje ki a tárterület-fiók](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Jelölje be a forrás tároló. A forrás tároló általában a következő *VHD*:

    ![Jelölje ki a tárhely tároló](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Jelölje ki az operációs rendszer virtuális csatolhat. Kattintson a *Jelölje ki* a befejezése:

    ![Jelölje be a forrás pillanatkép](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Csatlakozás a hibaelhárítási virtuális távoli asztali változatában, és biztosítani, hogy a forrás virtuális OS lemez látható:

    - Jelölje ki a hibaelhárítási virtuális az Azure-portálra, és kattintson a *Csatlakozás*gombra.
    - Nyissa meg a letöltődő RDP-fájlt. Írja be a felhasználónevét és jelszavát, a hibaelhárítási virtuális.
    - A Fájlkezelőben keresse meg a csatlakoztatott adatok lemez. Ha a forrás virtuális a virtuális a csak olyan adatokat lemez a hibaelhárítási virtuális csatolva, f meghajtó kell:

    ![Csatolt adatok lemez megtekintése](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Hozzon létre `gpt.ini` a `\Windows\System32\GroupPolicy` a forrás virtuális meghajtón (ha létezik hozzá a(z), nevezze át gpt.ini.bak):

    > [AZURE.WARNING] Győződjön meg arról, hogy Ön nem véletlenül hoznak létre a következő fájlok C:\Windows, a hibaelhárítási virtuális OS meghajtóra. A következő fájlok létrehozása a forrást, adatok lemezen csatolt virtuális OS meghajtóra.

    - Írja be a következő sorokat be a `gpt.ini` létrehozott fájl:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Hozzá a(z) létrehozása](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Hozzon létre `scripts.ini` a `\Windows\System32\GroupPolicy\Machine\Scripts`. Ellenőrizze, hogy a rejtett mappák jelennek meg. Ha szükséges, hozza létre a `Machine` vagy `Scripts` mappák.

    - Adja hozzá a következő sort a `scripts.ini` létrehozott fájl:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Scripts.ini létrehozása](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Hozzon létre `FixAzureVM.cmd` a `\Windows\System32` cseréjével kapcsolatban, a következő tartalommal `<username>` és `<newpassword>` saját értékű:

    ```
    NET USER <username> <newpassword>
    ```

    ![FixAzureVM.cmd létrehozása](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Meg kell felelniük beállított jelszó összetettsége a virtuális az új jelszó megadásakor.

7. Az Azure-portálon leválasztása át a hibaelhárítási virtuális a lemez:

    - Jelölje ki a hibaelhárítási virtuális az Azure-portálon *lemezre*parancsára.
    - Jelölje ki a lépés: 2 a csatlakoztatott adatok lemezt, kattintson a *Leválasztás*gombra:

    ![Lemezen leválasztása](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Mielőtt egy virtuális hoz létre, szerezze be a forrás-OS lemezre az URI:

    - Jelölje ki a tárterület-fiókot az Azure-portálon *BLOB*parancsára.
    - Jelölje ki a tárolóhoz. A forrás tároló rendszerint a következő *VHD*:

    ![Jelölje ki a tárhely blob-fiók](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Jelölje ki a virtuális OS virtuális forrást, és az *URL-címe* melletti *Másolás* gombjára:

    ![URI lemezre másolása](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Hozzon létre egy virtuális a forrás virtuális-OS lemezről:

    - Hozzon létre egy virtuális speciális merevlemezről [a Azure erőforrás-kezelő](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) sablonnal. Kattintson a `Deploy to Azure` gombra kattintva nyissa meg az Azure portált az sablon meg töltve adatokkal.
    - Ha szeretné megőrizni a korábbi beállításokhoz a virtuális, jelölje be a *sablon szerkesztése* a meglévő VNet, alhálózat, hálózati kártya vagy nyilvános IP.
    - Az a `OSDISKVHDURI` paraméter szövegdobozra, az előző lépésben beszerzése a URI a forrás virtuális beillesztése:

    ![A virtuális létrehozása sablonból](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. A távoli asztali változatában a megadott új jelszavával virtuális csatlakozás után az új virtuális fut, a `FixAzureVM.cmd` parancsfájl.

11. A távoli munkamenetet az új virtuális, a távolítsa el a következő fájlok érdekében konszolidálhatjuk a környezet:

    - A %windir%\System32
        - FixAzureVM.cmd eltávolítása
    - A %windir%\System32\GroupPolicy\Machine\
        - scripts.ini eltávolítása
    - A %windir%\System32\GroupPolicy
        - Távolítsa el a hozzá a(z) (Ha hozzá a(z) előtt volt között, és gpt.ini.bak, nevezze át a .bak fájl visszatérhet hozzá a(z) átnevezett)

## <a name="next-steps"></a>Következő lépések
Ha még mindig nem tud csatlakozni távoli asztali változatában, olvassa el a [RDP hibaelhárítási útmutatója](virtual-machines-windows-troubleshoot-rdp-connection.md)című témakört. A [részletes RDP hibaelhárítási útmutatót](virtual-machines-windows-detailed-troubleshoot-rdp.md) vizsgálja meg hibaelhárítási lépéseivel, hanem módszereket. Is megtekintheti a rutinszerzést segítségért [Nyissa meg az Azure támogatási kérelmet](https://azure.microsoft.com/support/options/) .