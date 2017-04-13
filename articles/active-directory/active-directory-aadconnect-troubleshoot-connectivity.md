<properties
    pageTitle="Azure AD Connect: Kapcsolódási problémák elhárítása |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure AD Connect csatlakozási problémáinak."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Azure AD Connect csatlakozási problémák elhárítása
Ez a cikk ismerteti, hogy hogyan működik a Azure AD Connect és Azure Active Directory közötti kapcsolatot, és a kapcsolódási problémák elhárítása. Ezeket a problémákat valószínűséggel láthatja a proxykiszolgáló tartalmazó környezetben.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Az telepítővarázslóban kapcsolódási problémák elhárítása
Azure AD Connect (használatával a ADAL tár) Modern hitelesítési hitelesítéshez használ. A telepítés varázslót, és a TNÉV szinkronizálási motor szükség machine.config helyesen kell beállítani, mivel ezek a .NET-alkalmazások.

Ebben a cikkben bemutatjuk, hogyan Fabrikam Azure AD a proxyn keresztül csatlakozik. A proxykiszolgáló fabrikamproxy neve, és 8080 portot használja.

Először szükség győződjön meg arról, hogy a [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) helyesen van beállítva.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
Az egyes-Microsoft blogok dokumentálni, hogy a módosítások kell tenni miiserver.exe.config helyette. Ez a fájl azonban nem minden még akkor is működik kezdeti telepítés során, ha a rendszer leáll első frissítéskor frissítése felülírja. Emiatt a ajánlási frissítése inkább a machine.config.

A proxykiszolgáló rendelkeznie kell a megnyitott szükséges URL-címeit is. A hivatalos listában az [Office 365 URL-címei és IP-címtartományok ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)ismertetését.

Ezek az alábbi táblázat az abszolút bA minimális engedélyezni szeretné, hogy egyáltalán csatlakoztatása Azure AD. A lista nem tartalmazza bármelyik választható funkció, például a jelszó visszaírást és Azure Active Directory csatlakozás állapota. Azt ismertetését Itt a kezdeti konfigurálása kapcsolatos problémák megoldása érdekében.

URL-CÍME | Port | Leírás
---- | ---- | ----
mscrl.microsoft.com | HTTP-/ 80 | Töltse le a CRL-listák használatával.
\*. verisign.com | HTTP-/ 80 | Töltse le a CRL-listák használatával.
\*. entrust.com | HTTP-/ 80 | Töltse le a Visszavonási listák többtényezős szolgál.
\*. windows.net | HTTPS ÉS 443 | Jelentkezzen be az Azure Active Directory szolgál.
Secure.aadcdn.microsoftonline-p.com | HTTPS ÉS 443 | MFA használható.
\*. microsoftonline.com | HTTPS ÉS 443 | Állítsa be az Azure Active directory és adatokat az importálás/exportálás használható.

## <a name="errors-in-the-wizard"></a>A varázsló hibák
A telepítés varázslót két különböző biztonsági környezetben használja. A **Csatlakozás az Azure Active Directory** lapon a jelenleg bejelentkezett felhasználó használ. A **beállítás** lapon azt megváltoztatása a [fiókot a szinkronizálási motor szolgáltatást futtató](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). A proxy konfigurációk VÁLLALUNK globális a számítógépre is, ha a probléma, a probléma fog a legtöbb valószínűleg már megjelenik a **Csatlakozás az Azure Active Directory** lapon, a varázsló.

Ezek a leggyakoribb hibákat, látni fogja az telepítővarázslóban.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Az telepítővarázslóban nincs megfelelően beállítva
Ez a hiba maga a varázsló nem tudja elérni a proxy fog megjelenni.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Ha akkor jelenik meg, ellenőrizze a [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) helyesen van beállítva.
- Ha, amely megjelenése megfelelő, kövesse az [ellenőrzés proxy kapcsolat](#verify-proxy-connectivity) megtekintéséhez, ha a probléma a varázslót.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>A MFA végpont nem érhető el.
Ez a hiba jelenik meg a végpont **https://secure.aadcdn.microsoftonline-p.com** nem érhető el és a globális rendszergazda MFA engedélyezve van.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Ha akkor jelenik meg, ellenőrizze, hogy a végpont secure.aadcdn.microsoftonline p.com a proxy lett hozzáadva.

### <a name="the-password-cannot-be-verified"></a>A jelszó nem lehet ellenőrizni.
Ha a telepítés varázsló nem sikerül csatlakoztatása az Azure Active Directory, de magát a jelszó nem lehet ellenőrizni, látni fogja a: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- A jelszó ideiglenes jelszót, és meg kell változtatni? A helyes jelszót ténylegesen található ez? Próbálja meg, jelentkezzen be a https://login.microsoftonline.com (egy másik számítógépen mint az Azure AD Connect kiszolgáló), és ellenőrizze a fiók használható.

### <a name="verify-proxy-connectivity"></a>Ellenőrizze a proxy-kapcsolatot
Ellenőrizze, hogy ha az Azure AD Connect kiszolgáló rendelkezik-e a Proxy és az Internet tényleges kapcsolatokkal fogjuk használni néhány PowerShell tekintheti meg, ha a proxy engedélyezi webes kérelmek-e. Egy PowerShell-parancssorában futtassa `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Értelemben az első hívás, ha https://login.microsoftonline.com és, valamint a fog működni, de az egyéb URI gyorsabb, ha a válasz.)

A PowerShell fogja használni a konfigurációt a machine.config lépjen kapcsolatba a proxy. Az winhttp/netsh beállításai nem kell hatással lehet a parancsmagok.

A proxy helyesen van beállítva, ha a sikeres állapotban kapja: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Ha **nem lehet kapcsolódni a távoli kiszolgáló** kap majd PowerShell partnere kapcsolatba próbál közvetlen hívás indítása a proxykiszolgáló használata nélkül, vagy a DNS nincs megfelelően beállítva. Győződjön meg arról, hogy helyesen van beállítva a **machine.config** fájlt.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Ha a proxy nincs megfelelően beállítva, hogy hibaüzenetet kap: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Hibaüzenet | Hibaüzenet | Megjegyzés
---- | ---- | ---- |
403 | Tiltott | A proxy még nincs megnyitva a kért URL. A proxy konfigurációjának látogatnia, és ellenőrizze, hogy az [URL-címek](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) megnyitott.
407 | Proxyhitelesítés szükséges | A proxykiszolgáló kötelező a bejelentkezés, és nincs megadva. Ha a proxykiszolgáló hitelesítést igényel, ellenőrizze, hogy ez a machine.config konfigurált. Gondoskodjon arról, hogy a felhasználó a varázslót, valamint a szolgáltatásfiók mint tartományi fiókok esetén.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>A kapcsolati minta Azure AD Connect és Azure Active Directory között
Ha már elvégezte ezeket a lépéseket, a fenti és még mindig nem tud csatlakozni az összes lehetséges ezen a ponton megkezdése hálózati naplók megjeleníti. Ez a szakasz van dokumentálás, normál és a sikeres csatlakozás mintát. Azt a közös piros hering, amely nyugodtan figyelmen kívül, ha a hálózati naplók olvasása bejegyzésére is van.

- Hívások átirányítása https://dc.services.visualstudio.com lesz. Ez az előnézeti a telepítéshez sikeres megnyitott nem szükséges, és ezek figyelmen kívül hagyható.
- Láthatja, hogy a DNS-feloldási felsorolásban a tényleges állomások el szeretné helyezni a DNS-neve terület nsatc.net és más névtér nem microsoftonline.com csoportban. Azonban nem lesz bármely webes szolgáltatási kérelmek a tényleges kiszolgáló nevét, és nem kell adja hozzá ezeket a proxy.
- A végpontok adminwebservice és provisioningapi (lásd alább a naplókban) feltáráshoz használt végpontokat, és keresse meg a tényleges végpontot használt használja, és a program kell eltér, attól függően, hogy az adott régióban.

### <a name="reference-proxy-logs"></a>Hivatkozás a proxykiszolgáló naplók
Az alábbiakban a kiírása egy tényleges proxy naplófájlból és a telepítés varázsló lapján az, hogy hol történt (ismétlődő elemeket az azonos végpontra eltávolították). Ez a saját proxy és a hálózati naplók egy hivatkozást is használt. A tényleges végpontok eltérhetnek a környezetben (különösen a *dőlt*).

**Azure Active Directory csatlakoztatása**

Idő | URL-CÍME
--- | ---
1/11/2016 8:31. | Connect://Login.microsoftonline.com:443
1/11/2016 8:31. | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:32 | Csatlakozás: / /*bba800-horgony*. microsoftonline.com:443
1/11/2016 8:32 | Connect://Login.microsoftonline.com:443
1/11/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:33 | Csatlakozás: / /*bwsc02-továbbító*. microsoftonline.com:443

**Konfigurálása**

Idő | URL-CÍME
--- | ---
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:43 | Csatlakozás: / /*bba800-horgony*. microsoftonline.com:443
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | Csatlakozás: / /*bba900-horgony*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | Csatlakozás: / /*bba800-horgony*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:46 | Csatlakozás: / /*bwsc02-továbbító*. microsoftonline.com:443

**Kezdeti szinkronizálása**

Idő | URL-CÍME
--- | ---
1/11/2016 8:48 | Connect://Login.Windows.NET:443
1/11/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:49 | Csatlakozás: / /*bba900-horgony*. microsoftonline.com:443
1/11/2016 8:49 | Csatlakozás: / /*bba800-horgony*. microsoftonline.com:443

## <a name="authentication-errors"></a>Hitelesítési hibák
Ez a szakasz ismerteti azokat a hibákat, amely az ADAL (az Azure AD Connect által használt authentication library) és a PowerShell adhatók vissza. A hiba magyarázata kell megismerheti az a következő lépésekkel.

### <a name="invalid-grant"></a>Érvénytelen támogatás
Érvénytelen felhasználónevet és jelszót. [A jelszó nem lehet ellenőrizni](#the-password-cannot-be-verified) talál további információt.

### <a name="unknown-user-type"></a>Ismeretlen felhasználó típusa
Az Azure Active directory nem található, vagy a rendszer. Esetleg megpróbál bejelentkezni a felhasználónév nem ellenőrzött tartományban?

### <a name="user-realm-discovery-failed"></a>Nem sikerült felhasználói tartomány feltárás
Hálózati vagy a proxykiszolgáló konfigurációs problémák. A hálózat nem lehet elérni, lásd: a [Hibaelhárítás csatlakozási problémákat tapasztal a telepítés varázsló](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Felhasználó jelszavának lejárt
A hitelesítő adatok lejárt. A jelszó módosítása.

### <a name="authorizationfailure"></a>AuthorizationFailure
Ismeretlen hiba.

### <a name="authentication-cancelled"></a>Lemondott hitelesítés
A többtényezős hitelesítés (MFA) beavatkozás igazolására szolgáló eljárás meg lett szakítva.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Hitelesítés sikeres volt, de az Azure Active Directory PowerShell-hitelesítés problémája van.

### <a name="azurerolemissing"></a>AzureRoleMissing
Hitelesítés sikeres volt. Ön nem egy globális rendszergazdai.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Hitelesítés sikeres volt. Kiemelt Identitáskezelés engedélyezve van, és Ön éppen nem a globális rendszergazda. További információt talál [Az Identitáskezelés Yammerhez](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Hitelesítés sikeres volt. Vállalati adatok nem lekérése Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Hitelesítés sikeres volt. Nem beolvasni a az Azure Active Directory tartományi információk.

## <a name="troubleshooting-steps-for-previous-releases"></a>A korábbi verziókban hibaelhárítási lépéseket.
Az összeállítás szám (kiadott február 2016) 1.1.105.0 kezdődő kiadásairól az – bejelentkezési segéd ugyan már inaktív. Ez a szakasz- és a konfigurációs már nem szükséges, de hivatkozásként legyen.

Az egyszeri bejelentkezés Segéd a munkát winhttp kell beállítania. Ez történik a [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>A bejelentkezési segéd nincs megfelelően beállítva
Ez a hiba akkor fordulhat elő, a bejelentkezési segéd nem tudja elérni a proxy és a proxy nem engedélyezi a kérést.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Ha akkor jelenik meg, tekintse meg a proxy konfiguráció [netsh](active-directory-aadconnect-prerequisites.md#connectivity) és helyes.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Ha, amely megjelenése megfelelő, kövesse az [ellenőrzés proxy kapcsolat](#verify-proxy-connectivity) megtekintéséhez, ha a probléma a varázslót.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).
