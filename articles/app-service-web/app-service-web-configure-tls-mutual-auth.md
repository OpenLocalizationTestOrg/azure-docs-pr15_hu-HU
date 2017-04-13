<properties 
    pageTitle="TLS kölcsönös hitelesítés webalkalmazás konfigurálása" 
    description="További információ a web app használata TLS ügyfél tanúsítvány-hitelesítés konfigurálása." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>TLS kölcsönös hitelesítés webalkalmazás konfigurálása

## <a name="overview"></a>– Áttekintés ##
Az access, mivel a különböző típusú hitelesítés, korlátozhatja az Azure webalkalmazást. Ehhez egy úgy, hogy hitelesítést végezni, amikor a kérelem véget ért az SSL/TLS ügyfél tanúsítvánnyal. Ez az eljárás TLS kölcsönös hitelesítés vagy hitelesítési és ez a cikk részletes információkat a webalkalmazás ügyfél tanúsítványokkal történő hitelesítés beállítása ügyfél-tanúsítvány neve.

> **Megjegyzés:** Ha a HTTP-és HTTPS-nem rendelkezik hozzáféréssel a webhelyhez, nem kap bármilyen ügyfél-tanúsítványt. Ha az alkalmazás ügyfél tanúsítványok nem engedélyezze kéréseket az alkalmazás HTTP protokollon keresztül.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>Webalkalmazás konfigurálása az ügyfél tanúsítvány-hitelesítés ##
Az ügyfél tanúsítványokat kell hozzáadni a clientCertEnabled beállítását a webalkalmazásban, és állítsa be az IGAZ megkövetelésére webalkalmazást beállítása. Ez a beállítás jelenleg nem áll rendelkezésre az adatkezelési folyamatok az portálon keresztül, és a REST API kell ehhez használható.

A [ARMClient eszköz](https://github.com/projectkudu/ARMClient) segítségével könnyen állítson össze a REST API-hívást. Jelentkezzen be az eszközt szüksége lesz az alábbi parancsot:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
Mindent {} helyettesítve a webalkalmazás információk és létrehozott egy fájlt nevű enableclientcert.json a következő JSON a tartalom:

> {"hely": "A Web App tartózkodási",   
>   "Tulajdonságok": {  
>     "clientCertEnabled": igaz}}  

Ügyeljen arra, hogy az érték az "hely" átállítása bárhol a web App alkalmazásban található pl. Észak központi US vagy nyugati USA-beli stb.

> **Megjegyzés:** Ha a Powershell ARMClient, meg kell escape-a @ a JSON-fájlt egy vissza osztásjelek szimbólum ".

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Az ügyfél-tanúsítvány elérése a Web App alkalmazásból ##
ASP.NET használ, és állítsa be az ügyfél-tanúsítvány hitelesítés használni az alkalmazást, ha a tanúsítvány érhetők el a **HttpRequest.ClientCertificate** tulajdonság keresztül. Az egyéb alkalmazásban készleteket az ügyfél tanúsítvány érhetők el az alkalmazás – kódolt base64 érték az "X-ARR-ClientCert" kérelem fejlécében. Az alkalmazás ezt az értéket a tanúsítvány létrehozása és a használható az alkalmazás hitelesítési és engedélyezési céljából.

## <a name="special-considerations-for-certificate-validation"></a>Tanúsítvány érvényességi megfontolandó különleges szempontok ##
Az ügyfél tanúsítvány, amely a rendszer elküldi az alkalmazás nem hajtsa végre bármely érvényességi a webalkalmazások Azure platform. A tanúsítvány érvényesítése feladata a web App alkalmazásban. Az alábbiakban a minta ASP.NET kódot, amely ellenőrzi a hitelesítési tanúsítvány tulajdonságait.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
