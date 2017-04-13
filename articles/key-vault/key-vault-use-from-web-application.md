<properties
    pageTitle="Azure kulcs tárolóból elemre egy webalkalmazás használata |} Microsoft Azure"
    description="Ebben az oktatóanyagban segítségével miként használhatja az Azure kulcs tárolóból elemre egy webalkalmazás."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Azure kulcs tárolóból elemre egy webalkalmazás használata #

## <a name="introduction"></a>– Bevezetés  
Ebben az oktatóanyagban segítségével miként használhatja az Azure kulcs tárolóból elemre egy webalkalmazás Azure-ban. Végigvezeti a folyamaton, a titkos elérése a az Azure kulcs tárolóból elemre, hogy a webalkalmazásban is használható.

**Becsült időt vesz igénybe:** 15 percet


Azure kulcs tárolóra kapcsolatos információ áttekintése [Mi az Azure kulcs tárolóra?](key-vault-whatis.md)

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez az alábbiakat kell rendelkeznie:

- Egy titkos a az Azure kulcs tárolóból elemre kattintva URI
- Ügyfél-azonosító, és egy ügyfél titkos kulcs webalkalmazást regisztrált Azure Active Directory fér hozzá a kulcs tárolóból elemre.
- Webalkalmazás. Fogja azt megjelenítő telepítését az Azure a Web App alkalmazás ASP.NET MVC vonatkozó lépéseket.

> [AZURE.NOTE]  Fontos, hogy elvégezte a lépéseket az [Első lépések az Azure kulcs tárolóból elemre](key-vault-get-started.md) az ebben az oktatóanyagban, hogy a URI egy titkos és az ügyfél-azonosító és az ügyfél titkos webes alkalmazáshoz.

A webes alkalmazás, a kulcs tárolóra csatlakozó regisztrálva van az Azure Active Directory és a kapott hozzáférést a kulcs tárolóból elemre kattintva. Ha ez nem térjen vissza külső.FÜGGV egy alkalmazást, az első lépések oktatóprogram és ismételje meg, a lépések láthatók.

Ebben az oktatóanyagban, amelyek a webalkalmazások létrehozása a Azure alapjait megérteni a webes fejlesztők számára készült. Azure Web Apps alkalmazások kapcsolatos további tudnivalókért olvassa el a [Web Apps alkalmazások – áttekintés](../app-service-web/app-service-web-overview.md)című témakört.



## <a id="packages"></a>Nuget csomagok hozzáadása ##
Vannak olyan a webalkalmazás kell telepítve van, hogy a két csomagot.

- Active Directory Authentication Library - tartalmaz módszerek az Azure Active Directory személlyel és felhasználói identitás kezelése
- Azure kulcs tárolóra - médiatárban módszerek Azure kulcs tárolóra személlyel


Ezekről a csomagokról mindkét is telepítve van a konzolon Manager csomag a telepítés-csomag paranccsal.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Web.Config módosítása ##
Vannak, amelyek hozzá kell adni a fájlt a következőképpen három beállításai.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Ha nem fog tárolni a meg az Azure Web App alkalmazást, majd meg kell adja hozzá a tényleges ClientId, az ügyfél titkos és a titkos URI értéket a web.config. Az üres értékek ellenkező hagyja, mert azt hozzáadja a tényleges értékek biztonsági további szintű az Azure-portálon.


## <a id="gettoken"></a>A Get-hozzáférési jogkivonat mód hozzáadása ##
A kulcs tárolóra API használata egy hozzáférési jogkivonat van szüksége. A kulcs tárolóra ügyfél kezeli a hívások a kulcs tárolóra API-hoz, de adjon, amely a hozzáférési jogkivonat kapja függvényt kell.  

Az alábbiakban a kód beolvasása a hozzáférési jogkivonat Azure Active Directory. Az alkalmazás tetszőleges válassza ezt a kódot. E hozzá szeretné adni egy Utils vagy EncryptionHelper osztály.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Ügyfél-azonosító és ügyfél titkos legkönnyebben az Azure Active Directory-alkalmazások hitelesítést végezni. És használja a webalkalmazásban lehetővé teszi a feladatok és további beállítási lehetőségekre a kulcskezelő elkülönítése. De azt az ügyfél titkos illusztráló a megadott beállításokat, amely egyes lehet, hogy a kockázatos, mint a titkos a konfigurációs beállítások védeni kívánt elhelyez támaszkodhat. Olvassa el a vitafórum használatát egy ügyfél-azonosító és a tanúsítvány ügyfél-azonosító, és az ügyfél titkos helyett az Azure Active Directory-alkalmazás hitelesítést végezni.



## <a id="appstart"></a>Az alkalmazás indítása a titkos beolvasása ##
Most már szükség hívja fel a kulcs tárolóra API, valamint a titkos kódot. A következő kódot bárhova helyezhető, feltéve, hogy ennek neve előtt kell használni. E illesztette kód be az alkalmazás indítása a Global.asax esemény, hogy a kezdőképernyőn egyszer fut, és elérhetővé teszi a titkos az alkalmazás.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Alkalmazás beállítások hozzáadása az Azure-portálon (nem kötelező) ##
Ha rendelkezik az Azure Web App most felvehet a tényleges értékek esetében a AppSettings az Azure-portálon. Ezzel az eljárással a tényleges értékek a Web.config nem lesz, de védett meg külön elérést vezérlő azt a portálon keresztül. Ezeket az értékeket a Web.config megadott értékek program helyettesíti. Győződjön meg arról, hogy a nevek megegyeznek.

![Az alkalmazásbeállítások jelenik meg az Azure-portálon][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Egy tanúsítványt egy ügyfél titkos helyett a hitelesítéshez
Az Azure Active Directory-alkalmazások hitelesítést végezni úgy, egy ügyfél-azonosító, és az ügyfél titkos helyett egy ügyfél-azonosító és a tanúsítvány használatával. Az alábbiakban a lépéseket követve egy tanúsítványt az Azure Web App használata:

1. Első vagy tanúsítvány létrehozása
2. A tanúsítvány társítása az Azure Active Directory-alkalmazások
3. Kód hozzáadása a tanúsítványt használja a Web App alkalmazásban
4. Tanúsítvány felvétele a Web App alkalmazásba


**Első vagy tanúsítvány létrehozása** Mostani célok szempontjából a teszttanúsítvány fog VÁLLALUNK. Következik néhány parancsokat tartalmazó tanúsítvány létrehozása a fejlesztői parancssor is használhatja. Módosítsa a címtár oda, ahová a tanúsítvány fájlokat hozható létre.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Jegyezze fel a befejezési dátumot és a jelszavát a .pfx (ebben a példában: 07 és 31/2016 és test123). Szüksége lesz rájuk az alábbi.

Teszttanúsítvány létrehozásával kapcsolatos további tudnivalókért lásd: [Útmutató: A saját tesztelése tanúsítvány létrehozása](https://msdn.microsoft.com/library/ff699202.aspx)


**A tanúsítvány az Azure Active Directory-alkalmazások hozzárendelése** Most, hogy van egy tanúsítványt, kell társítani azt az Azure Active Directory-alkalmazások. De az Azure adatkezelési portálon nem támogatja a pillanatban. Ehelyett van Powershell-lel. Az alábbiakban futtatásához szükséges parancsokat:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Ezek a parancsok futtatása után megjelenik az Azure Active Directory-alkalmazás. Ha nem látja az alkalmazás első, keressen "Alkalmazások vállalaton tulajdonosa" helyett "Alkalmazást a vállalatom használ".

Azure Active Directory alkalmazás és a ServicePrincipal objektumok kapcsolatos további tudnivalókért lásd: az [alkalmazás és a szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md)



**Kód hozzáadása a tanúsítványt használja a Web App alkalmazásban** Most már azt hozzáadja kódot a Web App és a tanúsítvány hitelesítési használhassa.

Először van a tanúsítvány eléréséhez kódot.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Ne feledje, hogy a StoreLocation értéke CurrentUser LocalMachine helyett. És, hogy azt is ellátása "false", a keresés módszerrel, mert azt használja a próba-tanúsítvány.


Ezután van kódot, amely a CertificateHelper használ, és létrehoz egy ClientAssertionCertificate, amely hitelesítés szükséges.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Az alábbiakban az jogkivonat veheti az új kódot. A fenti GetToken módszert Ez helyettesíti. E adott azt egy másik nevet kényelmesebbé.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

E van az összes kód helyezze a lemezt a Web App a project Utils osztály az egyszerű használat érdekében.

Az utolsó kód módosítása a Application_Start módszer van. Először azt kell fordulnia a GetCert() módszer a ClientAssertionCertificate betöltéséhez. És azt módosítsa a visszahívás módszer, amely azt adja meg egy új KeyVaultClient létrehozásakor. Figyelje meg, hogy ez az eljárás lecseréli a kódot, amely fölött volt.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**A Web App alkalmazásban – az Azure portál tanúsítvány hozzáadása** A Web App alkalmazásban a tanúsítvány felvétele az alábbi egyszerű lépéssel. Első lépésként nyissa meg az Azure-portálra, és nyissa meg azt a Web App. A Web App beállításai lap kattintson a bejegyzés "egyéni tartományok és az SSL". Kattintson a lap, amely megnyitja az Ön fogja tudni feltölteni a tanúsítvány, Ön által létrehozott KVWebApp.pfx fölött, és győződjön meg arról, hogy ne felejtse el a jelszót a pfx az.

![Tanúsítvány felvétele az Azure-portálon webalkalmazást][2]


Egy Alkalmazásbeállítással hozzáadása a Web App, amelynek a név webhely kell tennie, utolsó lépésként\_BETÖLTÉS\_tanúsítványok és egy értéket a *. Ezzel biztosíthatja, hogy az összes tanúsítványok töltődnek. Ha csak a feltöltött saját tanúsítványok betöltése, azt majd adhatja meg a thumbprints vesszővel tagolt listáját.

Tanúsítvány webalkalmazást való felvételéről további információért lásd: a [Tanúsítványok használatával az Azure webhelyek alkalmazásaiban](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Hozzáadás egy tanúsítványt, a titkos kulcs tárolóra** A tanúsítvány közvetlenül a Web App szolgáltatás feltöltésekor, helyett tárolhatja, mint egy titkos kulcs tárolóra, és beállítaná őket onnan. Ez egy két lépésből álló folyamat, amely a következő blogbejegyzésben [Üzembe helyezése Azure Web App tanúsítvány – kulcs tárolóból elemre](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) a keret



## <a id="next"></a>Következő lépések ##


[Hivatkozások programozási, Azure kulcs tárolóból elemre C# ügyfélprogram API – segédlet](https://msdn.microsoft.com/library/azure/dn903628.aspx)című cikkben.


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
