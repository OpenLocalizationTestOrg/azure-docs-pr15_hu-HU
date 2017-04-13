<properties
    pageTitle="MFA-kiszolgáló a Windows Server 2012 R2 Active Directory Összevonási szolgáltatásokkal |} Microsoft Azure"
    description="Ez a cikk leírja, hogy miként veheti használatba Azure többtényezős hitelesítés és a Windows Server 2012 R2 rendszer AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>A felhőben, és a helyszíni erőforrások Azure többtényezős hitelesítés kiszolgáló használata esetén az AD FS a Windows Server 2012 R2 biztosítása

Ha használja az Active Directory összevonási szolgáltatások (AD FS), és a biztonságos, felhőalapú vagy a helyszíni erőforrások, beállíthatja az Azure többtényezős hitelesítést kiszolgáló az Active Directory összevonási szolgáltatások való használatra. Ebben a konfigurációban kétlépcsős hitelesítési értékes végpontok elindítja.

Ebben a cikkben témákat ölelik Azure többtényezős hitelesítést kiszolgáló használata esetén a Windows Server 2012 R2 rendszer AD FS. További információért tájékozódjon [-kiszolgálóval való Azure többtényezős hitelesítést az AD FS 2.0 biztonságos felhő és a helyszíni erőforrásokat](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>A Windows Server 2012 R2 AD FS Azure többtényezős hitelesítés kiszolgáló biztonságos

Azure többtényezős hitelesítést kiszolgáló telepítésekor, a következő lehetőségei vannak:

- Telepítse az Azure többtényezős hitelesítést kiszolgáló helyileg AD FS ugyanazon a kiszolgálón
- Telepítse az Azure többtényezős hitelesítés adaptert helyileg a AD FS-kiszolgáló, és telepítse a többtényezős hitelesítést Server egy másik számítógépen

Mielőtt elkezdené, a felhasználónevek az alábbi adatokat:

- Ön nem szükséges Azure többtényezős hitelesítést Server telepítése az ADFS-kiszolgálóra. Jó helyen jár telepítenie kell a többtényezős hitelesítés kártya Active Directory összevonási szolgáltatások, a Windows Server 2012 R2 rendszer AD FS-kiszolgáló a. Telepítheti a kiszolgáló egy másik számítógépen, ha verzióját, és külön-külön telepítette az Active Directory összevonási szolgáltatások adapteren az Active Directory összevonási szolgáltatások összevonási kiszolgálón. Lásd: az alábbi eljárások című témakörben olvashat a kártya külön-külön telepítése.

- A MFA kiszolgáló AD FS-adaptert tervezésekor azt várható, hogy az AD FS sikerült átadni a megbízó fél nevét a kártyát. Ezután a megbízó fél neve is használható az alkalmazás nevét. Azonban ki van kapcsolva a esetben nem tud. Szöveges üzenet vagy mobilalkalmazásban hitelesítési módszerek a szervezete használ, ha a vállalat beállításaiban megadott karakterláncok tartalmaz egy helyőrző, < $ a $*application_name*>. A helyőrző nem módosul automatikusan, ha az AD FS-adaptert. Azt javasoljuk, hogy távolítsa el a helyőrzőt a megfelelő karakterláncok az Active Directory összevonási szolgáltatások védelméhez.

- A bejelentkezéshez használt fiók biztonsági csoportok létrehozása az Active Directory-szolgáltatás a felhasználói engedélyekkel kell rendelkeznie.

- Az többtényezős hitelesítés AD FS-adaptert telepítővarázslóban PhoneFactor rendszergazdák az Active Directory-példányban nevű biztonsági csoport hoz létre. Kattintson az Active Directory összevonási szolgáltatások szolgáltatásfiók az összevonási szolgáltatás hozzáadása ennek a csoportnak. Azt javasoljuk, hogy, ellenőrizze a tartományvezérlőnek a, hogy valóban létrejön a PhoneFactor rendszergazdák csoport tagja az Active Directory összevonási szolgáltatások szolgáltatásfiók ennek a csoportnak. Ha szükséges, manuális felvétele az Active Directory összevonási szolgáltatások szolgáltatásfiók a tartományvezérlőnek a PhoneFactor rendszergazdák csoport.

- A felhasználó Portal segítségével a Web Service SDK telepítéséről további információért olvassa el a [üzembe helyezése a felhasználó portál Azure többtényezős hitelesítést Server.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Telepítse az Azure többtényezős hitelesítést kiszolgáló helyileg az Active Directory összevonási szolgáltatások kiszolgálón

1. Töltse le és Azure többtényezős hitelesítést Server telepítése az ADFS-kiszolgálóra. A telepítési információ további információ: [Azure többtényezős hitelesítést Server – első lépések](multi-factor-authentication-get-started-server.md).
2. Az Azure többtényezős hitelesítést kiszolgáló kezelése konzolban kattintson az **Active Directory összevonási szolgáltatások** ikonra, és válassza a beállítások **engedélyezése felhasználói regisztrációs** , és **Válassza a mód engedélyezése a felhasználóknak**.
3. Jelölje ki a kívánt adja meg a szervezet további beállításokat.
4. Kattintson a **telepítse az Active Directory összevonási szolgáltatások kártya**.
<center>![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Az Active Directory-ablak jelenik meg, ha ez azt jelenti, két dolgot. A számítógép csatlakozik egy tartományt, és az AD FS-adaptert, és a többtényezős hitelesítés szolgáltatás közötti kommunikáció biztonságossá tétele az Active Directory adatokat nem fejeződött be. Kattintson a **Tovább** gombra automatikusan ez a beállítás befejezése, vagy jelölje ki a **automatikus az Active Directory-konfiguráció átugrása, és beállításainak manuális** jelölőnégyzetet, és kattintson a **Tovább gombra**.
6. Ha a helyi csoportházirend windows jelenik meg, ez azt jelenti, két dolgot. A számítógép nem csatlakozik egy tartományt, és a helyi csoport konfiguráció biztonságossá tehető az Active Directory összevonási szolgáltatások kártya és a többtényezős hitelesítés szolgáltatás közötti kommunikáció nem fejeződött be. Kattintson a **Tovább** gombra automatikusan ez a beállítás befejezése, vagy jelölje ki a **automatikus helyi csoportházirend-konfiguráció átugrása, és beállításainak manuális** jelölőnégyzetet, és kattintson a **Tovább gombra**.
7. A telepítés varázslót kattintson a **Tovább gombra**. Azure többtényezős hitelesítést kiszolgáló hoz létre a PhoneFactor rendszergazdák csoport, és felveszi az Active Directory összevonási szolgáltatások szolgáltatásfiók a PhoneFactor rendszergazdák csoportjának.
<center>![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. A **Telepítő indítása** lapon kattintson a **Tovább**gombra.
9. Többtényezős hitelesítés Active Directory összevonási szolgáltatások kártya installer kattintson a **Tovább gombra**.
10. A telepítés befejezésekor kattintson a **Bezárás** gombra.
11. Ha telepítve van a kártyát, akkor az AD FS regisztrálnia. Nyissa meg a Windows Powershellt, és futtassa az alábbi parancsot:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Az imént bejegyzett kártya használatához a globális hitelesítési házirendet, az Active Directory összevonási szolgáltatások szerkesztése. Az Active Directory összevonási szolgáltatások kezelőkonzol nyissa meg a **Hitelesítési házirendek** csomópontot. **Többtényezős hitelesítés** csoportban kattintson a hivatkozás **szerkesztése** mellett a **Globális beállításai** csoportban. A **Globális hitelesítési a házirend módosítása** ablakban jelölje be a **Többtényezős hitelesítés** további hitelesítési módszer, és kattintson **az OK**gombra. A kártya WindowsAzureMultiFactorAuthentication azonosítóként regisztrált. Indítsa újra az Active Directory összevonási szolgáltatások szolgáltatás csak akkor lépnek érvénybe a regisztráció.

<center>![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Ezen a ponton többtényezős hitelesítést kiszolgáló van beállítva egy további hitelesítési szolgáltató kell az Active Directory összevonási szolgáltatások használatára.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Az Active Directory összevonási szolgáltatások kártya önálló példány telepítése a Web Service SDK használatával
1. A Web Service SDK telepíthető a többtényezős hitelesítést Servert futtató kiszolgáló.
2. Másolja a következő fájlokat a \Program Files\Multi-tényező hitelesítési kiszolgáló Directoryval, hogy a kiszolgáló, amelyen telepítse az Active Directory összevonási szolgáltatások adaptert megtervezése:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. A MultiFactorAuthenticationAdfsAdapterSetup64.msi telepítőfájl futtatása.
4. Többtényezős hitelesítés AD FS kártya installer kattintson a **Tovább** gombot a telepítés megkezdéséhez.
5. A telepítés befejezésekor kattintson a **Bezárás** gombra.

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>A MultiFactorAuthenticationAdfsAdapter.config fájl szerkesztése

Kövesse az alábbi lépéseket a MultiFactorAuthenticationAdfsAdapter.config fájl szerkesztése:

1. Állítsa a **UseWebServiceSdk** csomópont **Igaz**.  
2. Az érték megadása **WebServiceSdkUrl** többtényezős hitelesítést Web Service SDK URL-címében. Példa: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, ahol a certificatename a tanúsítvány nevét.  
3. A külső.FÜGGV-MultiFactorAuthenticationAdfsAdapter.ps1 parancsfájl szerkesztése hozzáadásával *- ConfigurationFilePath &lt;elérési út&gt; * végére a `Register-AdfsAuthenticationProvider` parancs, hol * &lt;elérési út&gt; * van a fájl teljes elérési útját a MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>A Web Service SDK állítson be egy felhasználónevet és jelszót

Két lehetőség a webes szolgáltatás SDK konfigurációs. Az első a felhasználónevet és jelszót, a második pedig az ügyfél-tanúsítványt. Kövesse ezeket a lépéseket az első lehetőséget, és ugorja át a következő, a második.  

1. Az érték megadása **WebServiceSdkUsername** egy olyan fiókba, a PhoneFactor rendszergazdák biztonsági csoport tagja. Használja a &lt;tartomány&gt;& #92; &lt;felhasználónév&gt; formátum.  
2. Adja meg az értéket a **WebServiceSdkPassword** a megfelelő fiók jelszava.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Állítsa be a webes szolgáltatás SDK ügyfél-tanúsítvány

Ha nem szeretné használni a felhasználónevet és jelszót, kövesse az alábbi lépéseket a Web Service SDK állítható be az ügyfél-tanúsítványt.

1. A Web Service SDK operációs rendszert futtató kiszolgáló tanúsítványszolgáltatótól szerezze be az ügyfél-tanúsítványt. Ismerje meg, [ügyfél tanúsítványok](https://technet.microsoft.com/library/cc770328.aspx)beszerzése.  
2. Az ügyfél-tanúsítvány importálása a helyi számítógép személyes tanúsítvány tároló a a Web Service SDK operációs rendszert futtató kiszolgáló. Győződjön meg róla, hogy a nyilvános tanúsítványát megbízható legfelső szintű tanúsítványok tanúsítvány áruházban.  
3. A nyilvános és titkos kulcsok, az ügyfél tanúsítvány exportálása .pfx fájl.  
4. A nyilvános kulcs Base64 formátumban .cer fájl exportálása  
5. A Kiszolgálókezelő győződjön meg arról, hogy telepítve van-e a webkiszolgáló (IIS) \Web Server\Security\IIS ügyfél-leképezés hitelesítés szolgáltatást. Ha nincs telepítve, jelölje be a **Szerepkörök hozzáadása és szolgáltatások** hozzáadása a szolgáltatás.  
6. Az IIS-kezelőben területen kattintson duplán a **Konfigurációs szerkesztő** a webhelyet, amely tartalmazza a Web Service SDK virtuális könyvtár. Fontos, hogy kövesse az alábbi lépéseket a webhely szintjén, és nem a virtuális könyvtár szintjén.  
7. Ugorjon a **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. **True**engedélyezett beállítása.  
9. OneToOneCertificateMappingsEnabled **true**értékűre.  
10. OneToOneMappings mellett lévő **…** gombra, és kattintson a **Hozzáadás** hivatkozást.  
11. Nyissa meg a korábban exportált Base64 .cer fájl. Távolítsa el a *---kezdje el a tanúsítvány---* *---BEFEJEZHETI a tanúsítvány---*és bármely sortörések. Másolja az eredményül kapott karakterlánc.  
12. Az előző lépésben a vágólapra másolt karakterlánccal tanúsítvány beállítása.  
13. **True**engedélyezett beállítása.  
14. UserName beállítása egy olyan fiókba, a PhoneFactor rendszergazdák biztonsági csoport tagja. Használja a &lt;tartomány&gt;& #92; &lt;felhasználónév&gt; formátum.  
15. A megfelelő fiók jelszavát a jelszó beállítása, és zárja be a konfigurációs szerkesztő.  
16. Kattintson az **Alkalmaz** hivatkozásra.  
17. Kattintson duplán a Web Service SDK virtuális könyvtár **hitelesítési**.  
18. Győződjön meg arról, hogy **engedélyezve**ASP.NET megszemélyesítési és alapszintű hitelesítés van beállítva, és, hogy az összes elem **Letiltva**vannak-e beállítva.  
19. A Web Service SDK virtuális könyvtár kattintson duplán a **SSL-beállításokat**.  
20. Ügyfél-tanúsítványok beállítása **Elfogadás**, és kattintson az **Alkalmaz**gombra.  
21. A korábban exportált az AD FS-adaptert futtató kiszolgálóra .pfx fájl másolása  
22. A .pfx fájl importálása a helyi számítógép személyes tanúsítvány áruházból.  
23. Kattintson a jobb gombbal, és válassza ki a **Titkos kulcsokat kezelése**, és az AD FS-be való bejelentkezéshez használt fiókjában majd olvasási hozzáférést.  
24. Nyissa meg az ügyfél-tanúsítvány, és másolja a ujjlenyomat a **Részletek** fülre.  
25. A MultiFactorAuthenticationAdfsAdapter.config fájlban beállítása **WebServiceSdkCertificateThumbprint** az előző lépésben a vágólapra másolt karakterlánccal.  


Végül a regisztrálhatja a kártyát, futtassa a \Program Files\Multi-PowerShell tényező hitelesítési Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 parancsfájl. A kártya WindowsAzureMultiFactorAuthentication azonosítóként regisztrált. Indítsa újra az Active Directory összevonási szolgáltatások szolgáltatás csak akkor lépnek érvénybe a regisztráció.

## <a name="related-topics"></a>Kapcsolódó témakörök

Súgó című témakörben a [Azure többtényezős hitelesítést gyakori kérdések](multi-factor-authentication-faq.md)
