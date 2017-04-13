## <a name="download-install-and-register-the-azure-backup-agent"></a>Letöltése, telepítése, és regisztráljon az Azure biztonsági másolat ügynök

Miután létrehozta a Azure biztonsági másolat tárolóból elemre, ügynökszoftvert egyes a Windows gépek (Windows Server, Windows-ügyfél, System Center adatok védelme Manager kiszolgáló vagy Azure biztonsági másolat Server számítógépen), amely lehetővé teszi, hogy készítsen biztonsági másolatot az adatokat és Azure alkalmazásokat kell telepíteni.

1. Jelentkezzen be az [adatkezelési portálon](https://manage.windowsazure.com/)

2. Kattintson a **Helyreállítás szolgáltatások**, majd jelölje be a kiszolgáló regisztrálása kívánt biztonsági tárolóból elemre. A biztonsági másolat tárolóból elemre az első lépések lap jelenik meg.

    ![Rövid útmutató](./media/backup-install-agent/quickstart.png)

3. Első lépések lapján kattintson a **Windows Server vagy System Center adatok védelme Manager vagy Windows-ügyfél** beállítás **Ügynök töltse le**a. Kattintson a **Mentés** a helyi számítógépre másolja a vágólapra.

    ![Ügynök mentése](./media/backup-install-agent/agent.png)

4. Miután telepített a agent, kattintson duplán az Azure biztonsági másolat ügynök a telepítés elindításához MARSAgentInstaller.exe. Válassza a telepítés és a agent szükséges üres mappát. A megadott gyorsítótár helyét szabad területet, amely az adatok biztonsági másolatának legalább 5 %-át kell rendelkeznie.

5.  Ha a proxykiszolgáló használatával csatlakozik az internethez, a **Proxybeállítások** képernyőn adja meg a proxy kiszolgáló adatait. Ha hitelesítéssel működő proxy, írja be a felhasználó nevét és jelszavát adatai a képernyőn.

6.  Az Azure biztonsági másolat ügynök telepíti a .NET-keretrendszer 4.5 és a Windows PowerShell (Ha még nem szerepel érhető el) a telepítés befejezéséhez.

7.  A agent telepítése után a **regisztrációhoz Folytatás** gombra a munkafolyamat folytatásához.

    ![Regisztráció](./media/backup-install-agent/register.png)

8. Tallózással keresse meg a hitelesítő adatok képernyőn tárolóból elemre, és jelölje ki a tárolóból elemre hitelesítő adatok fájlt, amely a korábban letöltött.

    ![Tárolóra hitelesítő adatok](./media/backup-install-agent/vc.png)

    A tárolóból elemre hitelesítő adatok fájlt csak esetén érvényes 48 óra (a portálról letöltés) után. Ha felmerülő hibákat a képernyőre (például "tárolóra hitelesítő adatok fájl biztosítja a lejárt."), jelentkezzen be az Azure portáljára, és ismét le a tárolóból elemre hitelesítő adatok fájlt.

    Arról, hogy a tárolóból elemre hitelesítő adatok fájl érhető el a telepítő alkalmazás által elérhető helyen. Ha elérése a kapcsolódó hibák, ezen a számítógépen egy ideiglenes helyre másolja a tárolóból elemre hitelesítő adatok fájlt, és ismételje meg a műveletet.

    Ha a program hibát jelez érvénytelen tárolóra hitelesítő adatait (például "érvénytelen tárolóra hitelesítő adatok") a fájl sérült, vagy nem nem rendelkezik a legújabb hitelesítő adatokat a helyreállítás szolgáltatással társított. Új tárolóból elemre hitelesítőadat-fájl letöltésével a portálon múlva próbálkozzon ismét. Ezt a hibát jellemzően látható, ha a felhasználó a **Letöltés tárolóra hitelesítő** funkciót, az Azure-portálon rövid egymás után. Ebben az esetben csak a második tárolóra hitelesítő adatok fájl nem érvényes.

9. A **titkosítási beállítás** képernyőn készítése egy jelszót, vagy adja meg egy jelszót (legalább 16 karaktert). Ne feledje, hogy a jelszó mentése biztonságos helyen.

    ![Titkosítás:](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Ha a jelszó elveszett vagy elfelejtett; Microsoft súgó nem az adatok biztonsági másolatának visszaállítása. A végfelhasználó tulajdonosa a titkosítási jelszó és a Microsoft nem rendelkezik betekintést kap abba, hogy a felhasználó által használt jelszó. Mentse a fájlt egy biztonságos helyen, mert szükséges a helyreállítási művelet során.

10. A **Befejezés** gombra kattint, a gép sikeresen jogosult a tárolóból elemre, és most már készen áll a kezdésre biztonsági mentést Microsoft Azure.

11. Microsoft Azure biztonsági másolat önálló használata esetén kattintson a a Microsoft Azure biztonsági másolat mmc beépülő modul a **Tulajdonságainak módosítása** parancs a regisztrációs munkafolyamat során megadott beállítások módosíthatók.

    ![Tulajdonságainak módosítása](./media/backup-install-agent/change.png)

    Azt is megteheti adatok védelme Manager használata esetén a **beállítás** parancsra kattintva **Online** csoportban a **kezelés** fülre kattintva során a regisztrációs munkafolyamat megadott beállításokat módosíthatja.

    ![Azure biztonsági másolat konfigurálása](./media/backup-install-agent/configure.png)
