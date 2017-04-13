<properties
    pageTitle="Azure MFA-kiszolgáló használata az AD FS 2.0 |} Microsoft Azure"
    description="Ez a leírja, hogy miként veheti használatba Azure MFA és AD FS 2.0 Azure többtényezős hitelesítést weblapot."
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

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Azure többtényezős hitelesítést kiszolgáló használata esetén az AD FS 2.0 felhő és a helyszíni erőforrások biztosítása

Ez a témakör az azoknak a szervezeteknek szól, így a szövetséges szervezetekbe Azure Active Directory tartozó biztonságossá tétele források, amelyek a helyszíni, vagy a felhőben. Az erőforrások védelme az Azure többtényezős hitelesítést kiszolgálót használ, és konfigurálja úgy, hogy az Active Directory összevonási szolgáltatások kezelése, hogy a magas érték végpontjait induljanak kétlépcsős hitelesítési.

Ez a dokumentáció leírja, hogy az AD FS 2.0 Azure többtényezős hitelesítést kiszolgálót használ.  További információk [biztonságossá tétele felhő és a helyszíni](multi-factor-authentication-get-started-adfs-w2k12.md)erőforrások Azure többtényezős hitelesítést kiszolgáló a Windows Server 2012 R2 Active Directory összevonási szolgáltatások használata esetén.


## <a name="secure-ad-fs-20-with-a-proxy"></a>Active Directory összevonási szolgáltatások 2.0-s biztonságos egy proxyval
Biztonságos AD FS 2.0 egy proxyval, az Azure többtényezős hitelesítést Server telepítése az ADFS proxykiszolgálót, és állítsa be a kiszolgáló.

### <a name="configure-iis-authentication"></a>IIS-hitelesítés konfigurálása

1. Többtényezős hitelesítés Azure kiszolgálón belül kattintson a **Hitelesítést az IIS** ikonra a bal oldali menüben.
2. Kattintson a **Képernyő-alapú** fülre.
3. Kattintson a **Hozzáadás** gombra.
<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Felhasználónév, a jelszó és a tartomány változók automatikusan észleli, írja be a bejelentkezési URL-cím (például https://sso.contoso.com/adfs/ls) a Auto-Configure Form-Based webhely párbeszédpanelen, és kattintson az OK gombra.
5. Jelölje be a **Azure többtényezős hitelesítés szükséges felhasználói megfelelően** jelölőnégyzetet, ha a minden felhasználónak, hogy vagy a program importálja a kiszolgálón, és két részből álló ellenőrzését. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve kétlépcsős hitelesítési alól lesz, a üresen hagyja bejelölve. Ezt a beállítást további tudnivalókért lásd a súgófájlt.
6. Ha a lap változók nem lehet automatikusan észlelni, kattintson a az **Adja meg, kézzel...** a webhely Auto-Configure Form-Based párbeszédpanel gombjára.
7. Add Form-Based webhely párbeszédpanelen írja be az URL-cím elküldése URL-cím mezőjében (például https://sso.contoso.com/adfs/ls) ADFS bejelentkezési lapjára, és írja be az alkalmazás nevét (nem kötelező). Az alkalmazás neve Azure többtényezős hitelesítés jelentések jelenik meg, és jelennek meg az SMS vagy mobilalkalmazás hitelesítési üzenetekben. További információt a súgófájl nézze meg a Küldés URL-CÍMÉT.
8. "TARTALMAKAT, vagy KÉRJEN." kérelem formátumának beállítása
9. Írja be a felhasználónevét változó (ctl00$ ContentPlaceHolder1$ UsernameTextBox) és a jelszó változó (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Ha az űrlap-alapú bejelentkezési lapja jelenik meg a tartomány szövegdobozban, adja meg a tartomány változót is. Előfordulhat, hogy bejelentkezési lapjának egy webböngészőben, kattintson a jobb gombbal a lapon, és jelölje be a **Forrás megtekintése** belül a bejelentkezési lapja a beviteli mezőkben nevének kereséséhez.
10. Jelölje be a **Azure többtényezős hitelesítés szükséges felhasználói megfelelően** jelölőnégyzetet, ha a minden felhasználónak, hogy vagy a program importálja a kiszolgálón, és két részből álló ellenőrzését. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve kétlépcsős hitelesítési alól lesz, a üresen hagyja bejelölve.
<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Kattintson a **Speciális...** Tekintse át a Speciális beállítások gombra. Beállítások, beleértve az azt jelenti, hogy jelöljön ki egy egyéni megtagadását lap fájlt gyorsítótár sikeres hitelesítési cookie-k használata a webhelyre, és adja meg, hogyan elsődleges hitelesítő adatainak beállítása
12. Az ADFS proxykiszolgáló valószínűleg nem lehet a tartományhoz, mivel a tartományvezérlőnek a felhasználói importálásának és a előtti hitelesítéshez csatlakozhat LDAP is használhatja. Advanced Form-Based webhely párbeszédpanelen kattintson az **Elsődleges hitelesítési** fülre, és válassza a **LDAP kötést létrehozni** a előtti hitelesítés típusa esetében.
13. Amikor elkészült, kattintson az **OK** gombra kattintva visszatérhet a webhely Add Form-Based párbeszédpanel. További információt a súgófájl nézze meg a Speciális beállítások.
14. Kattintson az **OK** gombra kattintva zárja be a párbeszédpanelt.
15. Miután az URL-CÍMEK és lap változók észlelt, vagy a megadott, a webhelyek adatainak az űrlapokra épülő panel jeleníti meg.
16. Kattintson a **Natív modul** fülre, és jelölje ki a kiszolgálón, a webhely futtató az ADFS-proxy csoportban (például "alapértelmezett webhely") vagy az ADFS-proxy alkalmazás (például "ls" a "adfs") ahhoz, hogy az IIS beépülő modult a kívánt szintre.
17. Jelölje be a **hitelesítést az IIS engedélyezése** elemre a képernyő tetején.
18. Az IIS-hitelesítés engedélyezve van.

### <a name="configure-directory-integration"></a>Konfigurálja a címtár-integráció

Ha engedélyezte a hitelesítést az IIS, de szeretné az Active Directory (AD) keresztül LDAP a előtti hitelesítéshez meg kell adnia a tartományvezérlőnek a LDAP-kapcsolatot.

1. Kattintson a **Címtár-integrációs** ikonra.
2. A beállítások lapon jelölje be az **adott LDAP konfiguráció használata** választógombot.
<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Kattintson a **Szerkesztés...** gombra.
4. LDAP konfigurációs szerkesztése párbeszédpanelen a mezőket az Active Directory tartományi vezérlő csatlakozni a szükséges információk adataival. Leírások a mezők az alábbi táblázatban szereplő. Ez az információ is szerepel a Azure többtényezős hitelesítést kiszolgáló súgófájlt.
5. Tesztelje a LDAP-kapcsolat **tesztelése** gombra kattintva.
<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Ha az LDAP kapcsolat tesztelésének sikerességéről tájékoztat, kattintson az **OK** gombra.

### <a name="configure-company-settings"></a>Vállalati beállításainak konfigurálása

1. Ezután kattintson a **Vállalat beállítások** gombra, és válassza ki a **Felhasználónév kiküszöbölése** fülre.
2. Jelölje be a **használata LDAP egyedi azonosító attribútum egyező felhasználónevét** választógombot.
3. Ha a felhasználó "tartomány\felhasználónév" formátumú adja meg a felhasználónév, a kiszolgáló engedélyezni szeretné, hogy törli a tartomány, felhasználónév ki, amikor létrehozza az LDAP-lekérdezés igényeknek megfelelően. Amely a beállításjegyzék beállításon keresztül elvégezhető.
4. Nyissa meg a Beállításszerkesztőt, és lépjen a hálózatok/PhoneFactor pozitív HKEY_LOCAL_MACHINE/szoftver/Wow6432Node 64 bites kiszolgálón. Ha a kiszolgálón 32 bites, hogy ki az elérési út "Wow6432Node". Hozzon létre egy "UsernameCxz_stripPrefixDomain" nevű duplaszó (DWORD) beállításkulcsot, és adja meg az értéket 1. Azure többtényezős hitelesítés most biztonságossá tétele a az ADFS-proxy.

Győződjön meg arról, hogy felhasználók hoztak-e az Active Directoryból be a kiszolgáló. Lásd: a [megbízható IP -címei szakaszban](#trusted-ips) , a mezőkódokkal whitelist belső IP-címek, hogy a bejelentkezéskor a webhelyet a helyekre, kétlépcsős hitelesítési nincs szükség.

<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>Active Directory összevonási szolgáltatások 2.0-s közvetlen proxy nélkül

Ha nem használja az AD FS-proxy AD FS biztonságát. Az Azure többtényezős hitelesítést kiszolgáló telepítse az Active Directory összevonási szolgáltatások kiszolgálón, és adja meg a kiszolgáló egy, az alábbi lépéseket:

1. Többtényezős hitelesítés Azure kiszolgálón belül kattintson a **Hitelesítést az IIS** ikonra a bal oldali menüben.
2. Kattintson a **HTTP** fülre.
3. Kattintson a **Hozzáadás** gombra.
4. Az alap URL-cím hozzáadása párbeszéd mezőbe írja be az URL-címet a ADFS-webhely, ahol HTTP hitelesítés történik (például https://sso.domain.com/adfs/ls/auth/integrated) a következő URL-cím mezőjébe. Ezután adja meg az alkalmazás nevét (nem kötelező). Az alkalmazás neve Azure többtényezős hitelesítés jelentések jelenik meg, és előfordulhat, hogy az SMS vagy mobilalkalmazás hitelesítési üzenetekben jeleníthető meg.
5. Ha azt szeretné, módosítsa a üresjárati időtúllépés és a Maximum munkamenet időpontok.
6. Jelölje be a **Azure többtényezős hitelesítés szükséges felhasználói megfelelően** jelölőnégyzetet, ha a minden felhasználónak, hogy vagy a program importálja a kiszolgálón, és két részből álló ellenőrzését. Ha nagyszámú felhasználók még importálása nem történt meg a kiszolgáló be, illetve kétlépcsős hitelesítési alól lesz, a üresen hagyja bejelölve. Ez a funkció a további tudnivalókért lásd a súgófájl.
7. Ha azt szeretné, jelölje be a cookie-k gyorsítótárazás jelölőnégyzetet.
<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Kattintson az **OK** gombra.
9. Kattintson a **Natív modul** fülre, és jelölje ki a kiszolgálón, a webhely (például "alapértelmezett webhely") vagy az ADFS-alkalmazást (például "ls" a "adfs") ahhoz, hogy az IIS beépülő modult a kívánt szintre.
10. Jelölje be a **hitelesítést az IIS engedélyezése** elemre a képernyő tetején. Azure többtényezős hitelesítés most biztonságossá tétele a ADFS.

Győződjön meg arról, hogy felhasználók hoztak-e az Active Directoryból be a kiszolgáló. A megbízható IP-címei című a mezőkódokkal whitelist belső IP-címek, hogy a bejelentkezéskor a webhelyet a helyekre, kétlépcsős hitelesítési nincs szükség.


## <a name="trusted-ips"></a>Megbízható IP-címei
Megbízható IP-címei engedélyezése a felhasználók számára, hagyja ki az Azure többtényezős hitelesítés adott IP-címek vagy alhálózat származó webhely kérelmekhez. Például érdemes adómentes felhasználók kétlépcsős hitelesítési a Ha bejelentkezni az az office-ból. Meg szeretné adni az office-alhálózat megbízható IP-címei bejegyzésének.

### <a name="to-configure-trusted-ips"></a>Állítsa be a megbízható IP-címei


1. A hitelesítést az IIS csoportban kattintson a **Megbízható IP -címei** fülre.
1. Kattintson a **Hozzáadás** gombra.
1. A hozzáadása a megbízható IP-címei párbeszédpanel megjelenésekor válassza ki a **Egyetlen IP**, **IP-tartomány**vagy **alhálózat** választógombok egyikét.
1. Adja meg az IP-cím, az IP-címek vagy alhálózathoz, amelyet whitelisted kell lennie. Ha megad egy alhálózat, jelölje ki a megfelelő maszk, és kattintson az **OK** gombra. A megbízható IP most hozzá lett adva.


<center>![A telepítő](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
