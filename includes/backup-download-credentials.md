## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Tárolóra hitelesítő adatokkal hitelesítést végezni az Azure biztonsági másolat szolgáltatással

A helyszíni kiszolgálót (Windows-ügyfél vagy a Windows Server vagy az adatok védelme kiszolgáló) kell sikerült hitelesíteni a biztonsági másolat tárolóra azt is biztonsági másolatokat Azure előtt. A hitelesítés használatával "tárolóra hitelesítő adatok" érhető el. Amely a tárolóból elemre hitelesítő adatok hasonlít az Azure PowerShell használt "közzétételi beállításokat" fájl fogalmának.

### <a name="what-is-the-vault-credential-file"></a>Mi az a tárolóból elemre hitelesítőadat-fájl?

A tárolóból elemre hitelesítő adatok fájl minden biztonsági tárolóból elemre a portál által generált tanúsítvány. A portál majd feltölti a nyilvános kulcshoz az Access vezérlő szolgáltatást (ACS). A titkos kulcs a tanúsítvány elérhetővé válik a felhasználónak a munkafolyamatot, amely meg van adva a gép regisztrációs munkafolyamat-bemeneteként részeként. Ez hitelesíti a gép adatok biztonsági másolatának küldése a biztonsági másolat Azure szolgáltatás egy azonosított tárolóból elemre.

A tárolóból elemre hitelesítő csak a regisztrációs munkafolyamat során használják. Feladata a felhasználó annak érdekében, hogy a tárolóból elemre hitelesítő adatok fájlt nem sérül. A pálcikák bármely engedélyezetlen felhasználó tartozik, ha a tárolóból elemre hitelesítő adatok fájl használható regisztrálhatja más gépek szemben az azonos tárolóból elemre. Jó helyen jár az adatok biztonsági másolatának egy jelszót, amelyhez tartozik az ügyfélnek titkosított, mint a meglévő adatok biztonsági másolatának nem sérül. Csökkentésében ezt a problémát, tárolóra hitelesítő adatok beállítása 48hrs múlva lejár. Töltse le a biztonsági másolat tárolóból elemre a tárolóból elemre hitelesítő adatai tetszőleges számú alkalommal – azonban csak a legújabb tárolóra hitelesítő fájlt alkalmazható a regisztrációs munkafolyamat során.

### <a name="download-the-vault-credential-file"></a>Töltse le a tárolóból elemre hitelesítőadat-fájlt

A tárolóból elemre hitelesítőadat-fájl letöltése az Azure portálról biztonságos csatornát keresztül. Az Azure biztonsági másolat szolgáltatás nem észleli a titkos kulcs a tanúsítvány és a titkos kulcs nem állandó a portálon vagy a szolgáltatás. Kövesse az alábbi lépéseket a tárolóból elemre hitelesítőadat-fájl letöltése a helyi számítógépre.

1.  Jelentkezzen be az [adatkezelési portálon](https://manage.windowsazure.com/)
2.  **Helyreállítási szolgáltatások** kattintson a bal oldali navigációs ablakban, és jelölje be a biztonsági másolat tárolóból elemre, amelyek nem hozott létre. Kattintson a felhőikonjára az első lépések nézet: a biztonsági másolat tárolóból elemre.

    ![Gyorsnézete](./media/backup-download-credentials/quickview.png)

3.  Első lépések lapon kattintson a **Letöltés tárolóra hitelesítő adatok**. A portál hoz létre a hitelesítő adatok letöltésre elérhetővé válik tárolóra fájlt.

    ![Letöltés](./media/backup-download-credentials/downloadvc.png)

4.  A portál hoz létre egy tárolóból elemre a hitelesítő adatok használata a tárolóból elemre nevét és az aktuális dátumot. Kattintson a **Mentés** töltse le a helyi fiók letöltések mappában a tárolóból elemre hitelesítő adatokat, vagy válassza a Mentés másként parancsot a Mentés adjon meg egy helyet a tárolóból elemre hitelesítő adatokat.

### <a name="note"></a>Megjegyzés:
- Győződjön meg arról, hogy olyan helyre, ahol a számítógépről is elérhető mentette a tárolóból elemre hitelesítő adatokat. Ha egy fájl megosztás/kis-és Középvállalatok van tárolva, jelölje be a a hozzáférési engedélyek.
- Csak a regisztrációs munkafolyamat során a tárolóból elemre hitelesítő adatok fájlt használ.
- A tárolóból elemre hitelesítő adatok fájl 48hrs után jár le, és a portálról letölthető.
- Keresse meg a Azure biztonsági másolat [– Gyakori kérdések](../articles/backup/backup-azure-backup-faq.md) a munkafolyamat a kérdéseit.
