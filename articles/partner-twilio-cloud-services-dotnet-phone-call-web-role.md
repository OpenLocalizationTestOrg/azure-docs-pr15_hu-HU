<properties 
    pageTitle="Telefonhívás kezdeményezése a Twilio (.NET) hogyan |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Telefonhívás kezdeményezése és egy SMS küldése a Twilio API szolgáltatással a Azure. A .NET írt mintakódok." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Hogyan Twilio használata a webes szerepkörbe Azure a Telefonhívás kezdeményezése

Ez az útmutató bemutatja, hogyan Azure-ban tárolt weblapról hívás Twilio használatával. Az eredményül kapott alkalmazás rákérdez az értékek telefonhívás, az alábbi képernyőképen látható módon.

![Azure hívás űrlap Twilio és ASP.NET használatával][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Előfeltételek

Meg kell az alábbiak szerint ez a témakör a kód használata:

1. Szerezheti be egy Twilio fiókkal és a hitelesítési jogkivonat. Az első lépések Twilio, jelentkezzen a [https://www.twilio.com/try-twilio][try_twilio]. Értékelheti a [http://www.twilio.com/pricing]árak[twilio_pricing]. Twilio által biztosított API-val kapcsolatos további tudnivalókért lásd: [http://www.twilio.com/voice/api][twilio_api].
2. A Twilio .NET-tár hozzáadása a webes szerepkörhöz. "A Twilio tárak hozzáadni a webes szerepkör projekt" című részben Ez a témakör.

Hoz létre egy egyszerű webes szerepkör Azure ismernie kell lennie.

## <a name="howtocreateform"></a>Útmutató: a Telefonhívás kezdeményezése webes űrlap létrehozása

<a id="use_nuget"></a>A Twilio tárak felvétele a webes szerepkör projekthez:

1.  A Visual Studio alkalmazásban nyissa meg azt a megoldást.
2.  Kattintson jobb gombbal a **hivatkozást**.
3.  Kattintson a **NuGet csomagok kezelése**elemre.
4.  Kattintson az **Online**gombra.
5.  A keresés online mezőbe írja be a *twilio*.
6.  A Twilio csomagolásán látható termékszámmal kattintson a **telepítés** gombra.

A következő kódot megtudhatja, hogy miként kezdeményezhet hívásokat a felhasználói adatok beolvasásához webes űrlap létrehozása. Ebben a példában a ASP.NET webes szerepet **TwilioCloud** nevű jön létre.

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Útmutató: a hívás kód létrehozása
A következő kódot, vagyis amikor a felhasználó az űrlap befejezi, a hívás üzenet létrehozása, és hozza létre a hívást. Ebben a példában a kód fut a kattintásra eseménykezelő a gomb az űrlapon. (Használata helyett a helyőrző értékeket **accountSID** és **authToken** az alábbi kód rendelt token Twilio fiók és a hitelesítéshez.)

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

A hívást, és megjelennek az Twilio végpontot, API verzió és a hívás állapotát. Az alábbi képernyőképen látható egy minta Futtatás kimenetét.

![Azure hívás válasz Twilio és ASP.NET használatával][twilio_dotnet_basic_form_output]

További információt a TwiML találhatók [http://www.twilio.com/docs/api/twiml][twiml]. További információ &lt;Ismerkedés&gt; és más Twilio műveletek a következő helyen található [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Következő lépések
Ez a kód kapott mutatja, hogy Twilio használata a Azure webes ASP.NET szerepet alapvető funkcióit. Mielőtt Azure gyártási, érdemes több hibakezelés és más funkciók. Példa:

* Webes űrlap helyett segítségével Azure Blob-tárolóhoz vagy egy SQL Azure-adatbázis példány tárolhatja telefonszámok, és hívja fel a szöveget. Azure BLOB használatával kapcsolatos további tudnivalókért lásd: [a a .NET Azure Blob-tároló szolgáltatás használata][howto_blob_storage_dotnet]. SQL-adatbázis használatával kapcsolatos további tudnivalókért lásd: [Azure SQL-adatbázis .NET alkalmazások használatáról][howto_sql_azure_dotnet].
* A telepítési beállítások kemény coding az értékeket, a képernyő helyett a lekérés a Twilio Fiókazonosítót, és a hitelesítési jogkivonat sikerült használata RoleEnvironment.getConfigurationSettings. A RoleEnvironment osztály információkért lásd a [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Olvassa el a Twilio biztonsági irányelvek a [https://www.twilio.com/docs/security][twilio_docs_security].
* Tudjon meg többet a Twilio a [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Lásd még:
* [Twilio használatáról a hang-és az Azure SMS-funkciók](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
