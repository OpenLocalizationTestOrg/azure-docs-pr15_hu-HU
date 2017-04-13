<properties 
    pageTitle="A helyszíni identitások integrálása az Azure Active Directory."
    description="Ez a ismerteti, hogy mi az Azure AD Connect és hogy miért szeretne használni."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Építőelem többtényezős hitelesítés az egyéni alkalmazásokba (SDK)

> [AZURE.IMPORTANT]  Ha szeretne a SDK letöltése, szüksége lesz létrehozása az Azure többtényezős hitelesítés szolgáltatóhoz, még akkor is, ha Azure MFA, AAD prémium, vagy EMS licenceket.  Hozzon létre egy Azure többtényezős hitelesítés szolgáltatóval erre a célra, és már rendelkeznek licenccel, érdemes szükséges, hozza létre a szolgáltatót a **Engedélyezett felhasználónkénti** modellel, és a szolgáltató csatolni a címtárban, amely tartalmazza az Azure MFA, Azure Active Directory Premium vagy EMS licenceket.  Ezzel biztosíthatja, hogy, amelyek nem számlát kapni kivéve, ha több, mint a saját licencek száma a SDK használatával egyedi felhasználó.

Az Azure többtényezős hitelesítést szoftver Development csomag (SDK) segítségével telefonhívás és szöveges üzenet ellenőrzése közvetlenül a bejelentkezési vagy tranzakció folyamatok a Azure AD-bérlő alkalmazások össze.

A többtényezős hitelesítést SDK csomagjában talál a és C#, Visual Basic (.NET), Java, Perl, PHP fonetikus érhető el. A SDK kínál egy vékony többtényezős hitelesítés körül. Írja be a kódot, beleértve a megjegyzéssel ellátott forrásfájlok kódot, például fájlok és a részletes fontos fájl kell mindent tartalmazza. Minden egyes SDK is tartalmaz egy tanúsítványt és titkos kulcs tranzakciók titkosítására többtényezős hitelesítést szolgáltatójától egyedi. Mindaddig, amíg megvan a szolgáltató, letöltheti a SDK annyi nyelvek és formátumban, szükség szerint.

Az API-hoz a többtényezős hitelesítést SDK felépítésének használata rendkívül egyszerű. Létrehozhat egy API többtényezős beállítás paraméterekkel, például a hitelesítési módot és a felhasználói adatok, például a telefonszám vagy a PIN-kód szám érvényesítéséhez hívása egy funkcióval. Az API-hoz a felhőalapú Azure többtényezős hitelesítést szolgáltatás be webes szolgáltatási kérelmek függvényhívásban fordítása. Minden hívások tartalmaznia kell a személyes tanúsítvány minden SDK a hivatkozást.

Mivel az API-khoz nem fér hozzá az Azure Active Directory regisztrált felhasználók, meg kell adnia a felhasználói adatok, például a telefonszámok és a PIN-kódok egy fájlt vagy egy adatbázis. Is az API-k hírcsatornájának nincsenek olyan regisztrációs vagy a felhasználó management szolgáltatást, így Önnek kell ezek a folyamatok összeállítása az alkalmazásba.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Az Azure többtényezős hitelesítés SDK letöltése

Az [Azure többtényezős hitelesítés szolgáltató](multi-factor-authentication-get-started-auth-provider.md)letöltése az Azure többtényezős SDK szükséges.  Egy teljes Azure előfizetését, még akkor is, ha az Azure MFA, Azure Active Directory Premium vagy vállalati mobilitás csomagja licencek tulajdonosa ehhez.  A SDK letöltéséhez kell lépjen a többtényezős Kezelőportálja segítségével közvetlenül kezelése a többtényezős hitelesítés szolgáltató által vagy a szolgáltatás beállításai lap MFA **"A portálon lépjen"** hivatkozására kattintva.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Az Azure többtényezős hitelesítést SDK letöltése az Azure portálról


1. Jelentkezzen be rendszergazdaként az Azure portálra.
2. A bal oldalon válassza az Active Directory.
3. Az Active Directory lapon a képernyő tetején kattintson a **Többtényezős hitelesítés szolgáltatók**
4. A képernyő alján kattintson a **kezelés** elemre.
5. Ekkor megnyílik egy új lapon.  A képernyő alján bal oldalon kattintson a SDK csomagjában talál.
<center>![Letöltés](./media/multi-factor-authentication-sdk/download.png)</center>
6. Jelölje ki a kívánt nyelvre egy kattintson a letöltés társított hivatkozásokat.
7. Mentse a letöltést.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Az Azure többtényezős hitelesítést SDK keresztül a szolgáltatás beállításai letöltése


1. Jelentkezzen be rendszergazdaként az Azure portálra.
2. A bal oldalon válassza az Active Directory.
3. Kattintson duplán az Azure Active Directory-példányt.
4. A képernyő tetején kattintson a **Konfigurálás** gombra.
5. Többtényezős hitelesítés csoportban válassza a **Manage szolgáltatás beállításai**
![letöltése](./media/multi-factor-authentication-sdk/download2.png)
6. A szolgáltatások beállításai lapon a képernyő alján kattintson a **Nyissa meg a portálon**.
![Letöltés](./media/multi-factor-authentication-sdk/download3a.png)
7. Ekkor megnyílik egy új lapon.  A képernyő alján bal oldalon kattintson a SDK csomagjában talál.
8. Jelölje ki a kívánt nyelvre egy kattintson a letöltés társított hivatkozásokat.
9. Mentse a letöltést.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Az Azure többtényezős hitelesítés SDK tartalma
A SDK belül találja az alábbiakat:

- **Fontos**. Megtudhatja, hogyan a többtényezős hitelesítést API-k használata egy új vagy meglévő alkalmazásra.
- Többtényezős hitelesítés **adatforrás-fájl**
- Kommunikáció a többtényezős hitelesítés szolgáltatást használó **ügyfél-tanúsítvány**
- A tanúsítvány **titkos kulcs**
- **Hívja fel az eredményeket.** Hívás eredmény kódok listáját. Nyissa meg a fájlt, használja az alkalmazás formázást, például WordPad szöveggel. A hívás eredmény kódok használatával és az alkalmazás többtényezős hitelesítés végrehajtásának hibaelhárítása. Még nem hitelesítési állapot kódok.
- **Példák.** Többtényezős hitelesítés egy egyszerű végezhető műveletek végrehajtása minta kódját.


>[AZURE.WARNING]Az ügyfél-tanúsítvány, amely egy egyedi, különösen a létrehozott személyes tanúsítvány. Ne megosztása és elvesznek a ezt a fájlt. A termékkulcsot, amelyek a többtényezős hitelesítés szolgáltatás folytatott kommunikáció biztonsága.

## <a name="code-sample-standard-mode-phone-verification"></a>Kód minta: Szabványos üzemmódban telefon ellenőrzése

A kód példa bemutatja, hogyan az API-k az Azure többtényezős hitelesítést SDK felvétele segítségével szabványos üzemmódban hangposta hívása ellenőrzése az alkalmazás. Szabványos üzemmódban, a telefonhívás, hogy a felhasználó válaszol a # billentyű lenyomásával.

Ebben a példában a C# .NET 2.0-s többtényezős hitelesítést SDK és C# kiszolgálóoldali logika egy egyszerű ASP.NET alkalmazásban, de a folyamat nagyon hasonlít a más nyelveken egyszerű megvalósítás. Mert a SDK tartalmaz, nem programfájlok forrásfájlok összeállítása a fájlokat és a hivatkozás őket, vagy beleveheti közvetlenül az alkalmazást.

>[AZURE.NOTE]Többtényezős hitelesítés végrehajtásakor használni a további tényezők másodlagos vagy harmadlagos ellenőrzési kiegészítése az elsődleges hitelesítési módot. Ezeket a módszereket nem készült használható elsődleges hitelesítési módszereket.

### <a name="code-sample-overview"></a>Kód minta áttekintése
Minta kód rendkívül egyszerű bemutató webalkalmazáshoz telefonhívás # kulcs választ fejezze be a felhasználóhitelesítés használja. Ez a telefonhívás tényező többtényezős hitelesítés szabványos üzemmódban neve.

Többtényezős hitelesítés-specifikus elemeket nem része a az ügyféloldali kódot. Mivel a további hitelesítési tényezők az elsődleges hitelesítés független, akkor hozzáadhatja őket a meglévő bejelentkezési kapcsolat módosítása nélkül. Az API-hoz a többtényezős SDK lehetővé teszik a felhasználói felület testreszabását, de lehet, hogy nem kell minden módosítható.

A kiszolgálóoldali kód hozzáadása a normál módú hitelesítés a 2. Létrehoz egy PfAuthParams objektum szabványos üzemmódban az ellenőrzéshez szükséges paraméterekkel: felhasználónevét, a telefon számát, és mód, és az ügyfél tanúsítványát (CertFilePath), amely az egyes hívás szükséges elérési útja. Az összes paramétert a PfAuthParams bemutatása a SDK csomagjában talál a példa-fájl megtekintéséhez.

Ezután a kódot a PfAuthParams objektum átadja a pf_authenticate() függvénynek. A visszaadott érték azt jelzi, a megoldás sikeres vagy sikertelen a hitelesítést. A kimenő paraméterek callStatus és errorID, további hívás eredmény adatokat tartalmazzák. A hívás eredmény kódokat a hívás eredmények fájlt a SDK ismertetett.

A minimális végrehajtása egy csak néhány sorra írhatók. Azonban gyártási kódban, tartalmaz bonyolultabb hibakezelés, az adatbázis további kódot és fokozható a felhasználói élmény.

### <a name="web-client-code"></a>Webes ügyfelek kódot.

Az alábbiakban megtalálja a bemutató oldal webes ügyfél kódját.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Kiszolgálóoldali kódot.

A következő kódrészlet kiszolgálóoldali többtényezős hitelesítés van beállítva és futtatása a 2. Szabványos üzemmódban (MODE_STANDARD), a telefonhívás, amelyhez a felhasználó válaszol a # billentyű lenyomásával.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
