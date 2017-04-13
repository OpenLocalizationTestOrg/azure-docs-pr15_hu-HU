<properties
    pageTitle="Az Azure Active Directory Proxy összekötő csendes telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy hogyan hajthatja végre Azure Active Directory alkalmazás Proxy összekötőt a helyszíni alkalmazások távoli biztonságos hozzáférés nyújtása."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Az Azure Active Directory Proxy összekötő csendes telepítése

Kívánja küldeni az olyan telepítési parancsfájlt, több Windows-kiszolgálók vagy nincs engedélyezve a felhasználói felület Windows-kiszolgálók. Ebből a témakörből megtudhatja, hogy miként hozhat létre egy Windows PowerShell-parancsprogramot, amely lehetővé teszi, hogy szeretné telepíteni, és regisztráljon az Azure Active Directory Proxy összekötő felügyelet telepítés.

## <a name="enabling-access"></a>Hozzáférés engedélyezése
Szolgáltatásalkalmazás-Proxy működik, az összekötő belül a hálózat nevű slim Windows Server szolgáltatás telepítése. Az alkalmazás Proxy összekötő a munkát, fel kell regisztrált a globális rendszergazdai és jelszavával Azure AD-címtárban. Általában ennek szerepel egy előugró párbeszédpanel az összekötő telepítése során. Másik lehetőségként a regisztrációs adatok megadását hitelesítőadat-objektum létrehozása a Windows PowerShell is használhatja, vagy létrehozhat saját jogkivonat, és a regisztrációs adatok megadását használatával.

## <a name="step-1--install-the-connector-without-registration"></a>Lépés: 1: Regisztráció nélkül az összekötő telepítése


Az összekötő MSIs telepítse a következőképpen regisztrálása az összekötő nélkül:


1. Nyisson meg egy parancssort.
2. A következő parancsot, amelyben a/q a csendes telepítés – azt jelenti, hogy a telepítés nem kérni fogja fogadja el a végfelhasználói licencszerződésben találja.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Lépés: 2: Az összekötő regisztrálása az Azure Active Directory
A lehet elvégezni a következő módszerek valamelyikével:


- Az összekötő a Windows PowerShell hitelesítőadat-objektum segítségével regisztrálhatja
- Az összekötő használatával létrehozott kapcsolat nélküli jogkivonat regisztrálása

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Az összekötő a Windows PowerShell hitelesítőadat-objektum segítségével regisztrálhatja


1. A Windows PowerShell-hitelesítőadatok-objektum létrehozása a következő futtatásával hol "<username>"és"<password>" a felhasználónevét és jelszavát a könyvtár kell cserélni:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Nyissa meg a **C:\Program Files\Microsoft AAD alkalmazás Proxy összekötő** , és futtassa a létrehozott PowerShell hitelesítő adatok objektumot használva, hol $cred-e a PowerShell neve hitelesítő adatok létrehozott objektum:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Az összekötő használatával létrehozott kapcsolat nélküli jogkivonat regisztrálása

1. Hozzon létre egy offline jogkivonat, a AuthenticationContext osztály az értékek használata a kódrészletet használatával:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Ha befejezte a jogkivonat, hozzon létre egy SecureString a token használatával: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Futtassa az alábbi Windows PowerShell-parancsot, ahol SecureToken a nevét a fentebb létrehozott jogkivonat, tenantID pedig a bérlő globálisan egyedi azonosítója: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Lásd még:

- [Azure Active Directory szolgáltatásalkalmazás-Proxy engedélyezése](active-directory-application-proxy-enable.md)
- [A saját tartománynév alkalmazások közzététele](active-directory-application-proxy-custom-domains.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Szolgáltatásalkalmazás-Proxy tapasztal kapcsolatos problémák megoldása](active-directory-application-proxy-troubleshoot.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
