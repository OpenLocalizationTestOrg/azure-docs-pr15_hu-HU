<properties
    pageTitle="Azure Active Directory fő átváltási bejelentkezés |} Microsoft Azure"
    description="Ez a cikk azt ismerteti, hogy a az aláírási kulcs átváltási gyakorlati tanácsok az Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Bejelentkezés az Azure Active Directory fő átváltási

Ez a témakör azt ismerteti, hogy mit kell tudni a nyilvános kulcsok felhasznált az Azure Active Directory (Azure Active Directory) biztonsági tokenek jelentkezzen be. Fontos tudni, hogy ezek billentyűk átváltási rendszeres időközönként, és a sürgős esetben sikerült vetítve azonnal. Azure Active Directory használó összes alkalmazások látnia kell programozás útján kezelni a fő átváltási folyamat vagy periodikus kézi átváltási folyamatot. Folytatni szeretné az olvasást, ha meg szeretné érteni, hogy a billentyűk működésének, hogy miként mérje fel, hogy milyen következményekkel járnak a átváltási az alkalmazás és hogyan frissítheti az alkalmazást, vagy periodikus kézi átváltási folyamatot kulcs átváltási kezelésére, ha szükséges.

## <a name="overview-of-signing-keys-in-azure-ad"></a>A bejelentkezés a billentyűparancsok az Azure Active Directory – áttekintés

Azure Active Directory használja a nyilvános kulcs titkosítási iparági szabványok épül megbízhatóvá magát, és azt használó alkalmazások között. Gyakorlatilag, ez hogyan működik az alábbi módon: Azure AD egy aláírási kulcs, amely a nyilvános és titkos kulcs két áll használja. Amikor a felhasználó bejelentkezik a Azure AD-hitelesítés használó alkalmazást, az Azure AD a biztonsági jogkivonat, amely tartalmazza a felhasználó adatai hoz létre. Ez a token Azure Active Directory, a titkos kulccsal vissza az alkalmazást az elküldés előtt van aláírva. Ellenőrizze, hogy a token érvényes és Azure Active Directory a ténylegesen kezdeményezésű, az alkalmazás ellenőriznie kell a nyilvános kulccsal Azure Active Directory, a bérlő [OpenID csatlakozás feltárás](http://openid.net/specs/openid-connect-discovery-1_0.html) vagy SAML/WS-Fed [összevonási metaadat-dokumentum](active-directory-federation-metadata.md)lévő által elérhetővé tett a token aláírás.

Biztonsági okokból Azure Active Directory aláírási rendszeres időközönként és vészhelyzet esetén, ha a fő tekercsben sikerült állítható azonnal. Bármely alkalmazásban, amely integrálódik az Azure Active Directory kezelése a fő átváltási eseményeket, függetlenül attól, hogy milyen gyakran akkor fordulhat elő kell készíteni. Ha nem, és az alkalmazás megkísérli lejárt billentyűvel ellenőrizze az aláírás jogkivonat, a bejelentkezési kérelem sikertelen lesz.

Nincs mindig egynél több érvényes kulcs OpenID csatlakozás feltárás és az összevonási metaadatok dokumentum érhető el. Az alkalmazás használata a dokumentumban szereplő kulcsok hajlandó kell lennie, egy kulcsot hamarosan vetítve is, mivel egy másik előfordulhat, hogy a helyettesítő, és így tovább.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Hogyan kell értékelni, ha az alkalmazás hatással lesz, és mi a teendő

Változó például az alkalmazás vagy használt milyen identitás protokollt, mind a tár típusától függ, hogy hogyan kezeli az alkalmazás a fő átváltási. Az alábbi szakaszokban mérje fel, hogy a leggyakoribb alkalmazások hatással van a fő átváltási és útmutatásokat találhat frissítése az alkalmazás automatikus átváltási támogatása, vagy manuálisan frissíteni az a billentyűt.

* [Natív ügyfélalkalmazásokban erőforrások elérése](#nativeclient)
* [Webalkalmazások és az API-khoz erőforrások elérése](#webclient)
* [Webalkalmazások API-khoz erőforrások védelme és készült Azure alkalmazás szolgáltatások használata](#appservices)
* [Webalkalmazások és az API-k használata a .NET OWIN OpenID csatlakozni, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme](#owin)
* [Webalkalmazások és az API-khoz erőforrásoknak .NET Core OpenID csatlakozás vagy JwtBearerAuthentication köztes védelme](#owincore)
* [Webalkalmazások és az API-khoz erőforrásoknak Node.js passport-azure-Active Directory modul védelme](#passport)
* [Webalkalmazások API-khoz jelszóval védenie az erőforrások és a Visual Studio 2015 készült](#vs2015)
* [Webalkalmazások jelszóval védenie az erőforrások és a Visual Studio 2013-ban létrehozott](#vs2013)
* [Erőforrások védelme és a Visual Studio 2013-ban készült webes API-khoz](#vs2013_webapi)
* [Webalkalmazások jelszóval védenie az erőforrások és a Visual Studio 2012 készült](#vs2012)
* [Webalkalmazások jelszóval védenie az erőforrások és a Visual Studio 2010, használja a Windows identitás Foundation 2008 o készült](#vs2010)
* [Webalkalmazások és az API-khoz erőforrásokat bármely más könyvtárak segítségével vagy manuálisan végrehajtása a támogatott protokollok védelme](#other)

Ez az útmutató van **nem** alkalmazható:

* Alkalmazások hozzáadása az Azure Active Directory alkalmazás gyűjteményből (beleértve az egyéni) van kapcsolatos aláírási kulcsok külön útmutatást. [További információt.](active-directory-sso-certs.md)
* A helyszíni alkalmazás proxyn keresztül közzétett alkalmazások nem kell aggódnia aláírási kulcsok.

### <a name="nativeclient"></a>Natív ügyfélalkalmazásokban erőforrások elérése

Erőforrások (azaz csak csatlakozó alkalmazások Microsoft Graph, KeyVault, Outlook API-val és a többi Microsoft APIs) általában csak szerezze be a token és adja át mentén, az erőforrás tulajdonosa. Tekintve, hogy azok nem védelmére erőforrásokat, nem vizsgálja meg a jogkivonat, és így nem kell biztosítani, hogy helyesen van aláírva.

Natív ügyfele alkalmazások, hogy asztali vagy hordozható, ebbe a kategóriába sorolhatók, és nem így hatással a átváltási.

### <a name="webclient"></a>Webalkalmazások és az API-khoz erőforrások elérése

Erőforrások (azaz csak csatlakozó alkalmazások Microsoft Graph, KeyVault, Outlook API-val és a többi Microsoft APIs) általában csak szerezze be a token és adja át mentén, az erőforrás tulajdonosa. Tekintve, hogy azok nem védelmére erőforrásokat, nem vizsgálja meg a jogkivonat, és így nem kell biztosítani, hogy helyesen van aláírva.

Webalkalmazások, valamint a webes API-hoz, használja az alkalmazás csak a folyamat (ügyfél hitelesítő adatait / ügyfél-tanúsítvány), ebbe a kategóriába esik, és így nem érinti a átváltási.

### <a name="appservices"></a>Webalkalmazások API-khoz erőforrások védelme és készült Azure alkalmazás szolgáltatások használata

Azure alkalmazás szolgáltatások hitelesítési / engedélyezési (EasyAuth) funkciók már a szükséges logika kulcs átváltási automatikusan kezelheti.

### <a name="owin"></a>Webalkalmazások és az API-khoz .NET OWIN OpenID csatlakozni, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrásoknak védelme

Ha az alkalmazás a .NET OWIN OpenID csatlakozni, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes, az már rendelkezik a szükséges logika kulcs átváltási automatikusan kezelheti.

Győződhet meg, hogy az alkalmazás bármely, az alábbiak szerint keres az alábbi kódrészletek, az alkalmazás Startup.cs vagy Startup.Auth.cs bármelyikét használ

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webalkalmazások és az API-khoz erőforrásoknak .NET Core OpenID csatlakozás vagy JwtBearerAuthentication köztes védelme

Az alkalmazás a .NET Core OWIN OpenID csatlakozás vagy JwtBearerAuthentication köztes használja, ha már rendelkezik a szükséges logika kulcs átváltási automatikusan kezelheti.

Győződhet meg, hogy az alkalmazás bármely, az alábbiak szerint keres az alábbi kódrészletek, az alkalmazás Startup.cs vagy Startup.Auth.cs bármelyikét használ

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Webalkalmazások és az API-khoz erőforrásoknak Node.js passport-azure-Active Directory modul védelme

Az alkalmazás a Node.js passport-Active Directory modult használja, ha már rendelkezik a szükséges logika kulcs átváltási automatikusan kezelheti.

Akkor is győződjön meg arról, hogy az alkalmazás passport ADFS-a következő kódtöredékének az alkalmazás app.js a kereséssel

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webalkalmazások API-khoz jelszóval védenie az erőforrások és a Visual Studio 2015 készült

Az alkalmazáshoz készült webes alkalmazás sablon használatával a Visual Studio Skype 2015 vállalati, és kattintson a **Módosítás hitelesítési** menü kijelölt **Munkahelyi és iskolai fiókjából** , ha már rendelkezik a szükséges logika kulcs átváltási automatikusan kezelheti. A logika OWIN OpenID csatlakozás köztes beágyazva olvassa be a billentyűket a OpenID csatlakozás feltárás dokumentumból gyorsítótárát és rendszeres időközönként frissítse őket.

Felvett hitelesítési a megoldás manuálisan, ha az alkalmazás nem lehet a szükséges kulcs átváltási logika. Meg kell írni saját magát, vagy kövesse a [webalkalmazások / bármely más tárak használata, illetve manuálisan alkalmazása a támogatott protokollok API-khoz.](#other).

### <a name="vs2013"></a>Webalkalmazások jelszóval védenie az erőforrások és a Visual Studio 2013-ban létrehozott

Ha az alkalmazáshoz készült webes alkalmazás sablon használatával a Visual Studio 2013-ban, és kattintson a **Módosítás hitelesítési** menü kijelölt **Szervezeti fiókok** , már rendelkezik a szükséges logika kulcs átváltási automatikusan kezelheti. Ez a módszer a projekttel kapcsolatos két adatbázistáblák a szervezet egyedi azonosító, és az aláírási főbb információk tárol. A kapcsolati karakterláncot az adatbázishoz a fájlt a projekt található.

Felvett hitelesítési a megoldás manuálisan, ha az alkalmazás nem lehet a szükséges kulcs átváltási logika. Meg kell írni saját magát, vagy kövesse a [webalkalmazások / bármely más tárak használata, illetve manuálisan alkalmazása a támogatott protokollok API-khoz.](#other).

Az alábbi lépésekkel ellenőrizze, hogy az érték az alkalmazás megfelelően működnek nyújt segítséget.

1. A Visual Studio 2013-ban nyissa meg azt a megoldást, és kattintson a jobb oldali **Server Explorer** lapján.
2. Bontsa ki az **Adatkapcsolatok**, **DefaultConnection**, majd **táblákat**. Keresse meg a **IssuingAuthorityKeys** táblázat, kattintson a jobb gombbal, és válassza a **Táblázat-adatok megjelenítése**.
3. A **IssuingAuthorityKeys** táblázatban legalább egy sort, amely megfelel a kulcs ujjlenyomat érték lesz. A táblázat bármely sorok törlése.
4. Kattintson a jobb gombbal a **bérlők** táblázatot, és kattintson a **Táblázat-adatok megjelenítése**gombra.
5. A **bérlők** táblázatban legalább egy sort, amely megfelel egy egyedi címtár bérlői azonosító lesznek. A táblázat bármely sorok törlése. Ha nem törli a **bérlők** táblázat és **IssuingAuthorityKeys** tábla is a sorokat, futásidőben hiba lép fel.
6. Építse fel, és futtassa az alkalmazást. Miután be van jelentkezve a fiókjához, akkor leállíthatja a az alkalmazás.
7. Térjen vissza a **Kiszolgáló Explorerben** , és tekintse meg az értékeket a **IssuingAuthorityKeys** és a **bérlők** táblázatban. Észre fogja venni, hogy azok van már automatikusan adni a megfelelő információkkal, az összevonási metaadat-alapú dokumentumok.

### <a name="vs2013"></a>Erőforrások védelme és a Visual Studio 2013-ban készült webes API-khoz

Ha egy webes API-alkalmazás létrehozása a Visual Studio 2013-ban a webes API-sablon segítségével, és ezután kijelölt **Szervezeti fiókok** a **Változás hitelesítési** menüből, már rendelkezik a szükséges logika a alkalmazásban.

Ha manuálisan konfigurálja a hitelesítés, kövesse az utasításokat az alábbi szeretné beállítani a webes API automatikusan frissíti a főbb információk.

A következő kódrészletet bemutatja, hogyan lehet a legújabb billentyűk lekérése az összevonási metaadat-dokumentum, majd a [JWT jogkivonat kezelő](https://msdn.microsoft.com/library/dn205065.aspx) a token ellenőrzése. A kódrészletet feltételezi, hogy használni a saját gyorsítótárazási pályától tartós a billentyűt az Azure Active Directory, a jövőbeli tokenek érvényességét egy adatbázist, konfigurációs fájl vagy máshol legyen.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Webalkalmazások jelszóval védenie az erőforrások és a Visual Studio 2012 készült

Ha az alkalmazás a Visual Studio 2012 készült, valószínűleg az identitás- és az Access eszköz használt konfigurálhatja az alkalmazást. Is valószínű, hogy a [Érvényesítése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx)használja. A VINR a felelős tájékozódhat arról, hogy megbízható Identitásszolgáltatók (Azure Active Directory) és a billentyűk őket által kibocsátott tokenek érvényesítéséhez használt karbantartásának. A VINR is egyszerűen automatikusan frissíti a termékkulccsal kapcsolatos adatokat egy fájlt a legújabb összevonási címtárában hajtsa társított metaadatok dokumentum letöltésével tárolt annak ellenőrzése, ha a konfigurációs elavult a dokumentum mellett a legújabb, és az új termékkulccsal szükség szerint az alkalmazás frissítését.

Ha az alkalmazás valamelyik mintakódok vagy a Microsoft által nyújtott forgatókönyv dokumentáció használatával létrehozott, a fő átváltási logika már szerepel a projektben. Megfigyelheti, hogy az alábbi kód már létezik a projektben. Ha az alkalmazás nem rendelkezik a logika csoportban hajtsa végre az alábbi lépésekkel fel szeretne venni, és ellenőrizze, hogy helyesen működik-e.

1. A **Megoldás Explorer**a megfelelő projekthez a **System.IdentityModel** összeállítás mutató hivatkozás hozzáadása.
2. Nyissa meg a **Global.asax.cs** fájlt, és adja hozzá a következő irányelvek használata:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Adja hozzá a következő módszerrel a **Global.asax.cs** fájlt:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. A **RefreshValidationSettings()** módszer a **Global.asax.cs** **Application_Start()** módszer meghívni, ahogy azt:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Hogy elvégezte ezeket a lépéseket, amikor az alkalmazás Web.config frissülnek a legújabb adatokkal, a dokumentumból összevonási metaadatok, beleértve a legújabb billentyűk. A frissítés akkor fordul elő, minden alkalommal, amikor a alkalmazáskészlet rendszer akkor hasznosítja újra az IIS; Alapértelmezés szerint az IIS értéke Lomtár alkalmazások 29 óránként.

Ellenőrizze, hogy a kulcsfontosságú átváltási összefüggés működik-e az alábbi lépésekkel.

1. Hogy az alkalmazás a fenti kódot használ ellenőrzése után nyissa meg **a fájlt** , és nyissa meg azt a **<issuerNameRegistry>** letiltása, az alábbi kifejezetten keres néhány sor:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. Az a **<add thumbprint=””>** az ujjlenyomat értékének beállítása, módosítása tetszőleges karakter egy másik képre cserélésével. Mentse **a fájlt** .

3. Az alkalmazás összeállítása, majd futtatásával. Ha a bejelentkezési folyamat befejezéseként az alkalmazás sikeresen frissíti a kulcsot a szükséges információkat a directory összevonási metaadat-alapú dokumentumok letöltésével. Ha a bejelentkezéskor hibát tapasztal, győződjön meg róla, a módosításokat, az alkalmazás [Hozzáadása bejelentkezés a webes alkalmazás használata Azure AD a](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) kapcsolódó témakörök vagy le, és a következő kódot minta vizsgálata helyesek: [Több bérlői felhő alkalmazás az Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Webalkalmazások jelszóval védenie az erőforrások és a Visual Studio 2008 vagy 2010-ben létrehozott és a Windows identitás Foundation (WIF) 1.0 for .NET 3.5

Ha egy alkalmazás WIF 1.0 készített, nem nincs megadott mechanizmusa automatikusan frissíteni az új termékkulccsal az alkalmazás-konfiguráció.

- *Legegyszerűbb módja* Használja a FedUtil szerszámok a WIF SDK csomagjában talál, amelyek is beolvasása a legújabb metaadat-dokumentum és frissítése a konfigurációban szerepel.
- A .NET 4.5, amelyek tartalmazzák még a legújabb verzióját, a rendszer névtér található WIF alkalmazás frissítése A [Érvényesítése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) segítségével majd végezze el az alkalmazás-konfiguráció az automatikus frissítések.
- Végezze el a manuális átváltási szerinti végén található ez az útmutató dokumentum a képernyőn megjelenő utasításokat.

Képernyőn megjelenő utasításokat követve frissítse a konfigurációban a FedUtil használatával:

1. Ellenőrizze, hogy rendelkezik-e a WIF 1.0 SDK for Visual Studio 2008 vagy 2010-ben a fejlesztés számítógépre telepítve. Azt is megteheti [Töltse le a további lehetőségek](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , még nem telepítette az, ha.
2. A Visual Studióban nyissa meg azt a megoldást, kattintson a jobb gombbal a megfelelő projekt és válassza a **metaadat-alapú összevonás frissítése**. Ez a beállítás nem érhető el, ha FedUtil és/vagy a WIF 1.0 SDK nincs telepítve.
3. A megjelenő párbeszédpanelen kattintson a **frissítés** az összevonási metaadatainak frissítése a kezdéshez. Ha van hozzáférése a kiszolgáló környezettel hol helyezkedik el az alkalmazást, FedUtil's [metaadatainak automatikus frissítése a Feladatütemező](https://msdn.microsoft.com/library/ee517272.aspx)másik lehetőségként használhatja.
4. Kattintson a **Befejezés gombra** a frissítési folyamat befejezéséhez.

### <a name="other"></a>Webalkalmazások és az API-khoz erőforrásokat bármely más könyvtárak segítségével vagy manuálisan végrehajtása a támogatott protokollok védelme

Ha bizonyos egyéb tár használja, vagy manuálisan végrehajtani a támogatott protokollok, kell tekintse át a tárban vagy annak érdekében, hogy a csatlakozás OpenID felderítési dokumentum vagy a az összevonási metaadat-dokumentum beolvasni a kulcs Önnél. Ez az ellenőrzés egyik módja való kereséshez a kód vagy a tár kódot vagy a OpenID feltárás vagy az összevonási metaadatok dokumentumhoz ki bármelyik hívásokhoz.

Ha ezek kulcs tárolt valahol vagy szoftveresen kötött az alkalmazásban, manuálisan is beolvashatja a kulcsot, és a frissítést, ennek megfelelően szerint végezze el a manuális átváltási szerinti végén található ez az útmutató dokumentum a képernyőn megjelenő utasításokat. Ez a cikk későbbi megszakadása elkerülése érdekében a Vázlat, **erősen javasolt, hogy az alkalmazás a támogatási automatikus átváltási növelheti célszerű** az alábbi módszerek bármelyikével és felsőbb, ha az Azure Active Directory növeli átváltási cadence vagy egy vészhelyzeti sávon átváltási rendelkezik.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Az alkalmazás határozza meg, ha azt hatással tesztelése

Ellenőrizheti, hogy az alkalmazás automatikus kulcs átváltási támogatja a parancsfájlok le, és utasításait követve [e GitHub tárházba.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Hogyan végezhetők el a kézi átváltási, ha Ön alkalmazás nem támogatja az automatikus átváltási

Ha az alkalmazás **nem** támogatja az automatikus átváltási jelent, szüksége lesz egy folyamat, amely rendszeresen figyeli Azure AD az aláírási kulcsot, és ennek megfelelően végrehajtja a kézi átváltási létrehozására. [A GitHub tárházba](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) parancsfájlok és az erre vonatkozó útmutatást tartalmaz.
