<properties 
    pageTitle="Media Services frissítése után tároló hívóbetűk közbeni |} Microsoft Azure" 
    description="Ez a cikk ad útmutatást a Media Services frissítése után tároló hívóbetűk közbeni." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Frissítse a Media Services tároló hívóbetűk közbeni után

Amikor létrehoz egy új Azure Media Services-fiókot, rendszer is kéri, hogy jelöljön ki egy Azure tárterület-fiókot, amely a multimédiás tartalmak tárolására szolgál. Figyelje meg, hogy is [egynél több tárterületet fiókok felvétele](meda-services-managing-multiple-storage-accounts.md) a Media Services-fiókjába.

Tároló új fiók létrehozása az Azure két 512 bites tároló hívóbetűk, hitelesítést végezni a tárterület-fiók eléréséhez használt hoz létre. A tároló kapcsolatokat biztonságosabbá megtartásához ajánlott rendszeresen újragenerálása, és a tárhely hívóbetű elforgatása. Két hívóbetűk (az elsődleges és másodlagos) ahhoz, hogy a tárhely fiókra egy access-billentyűt, miközben a többi hívóbetű követező létrehozásakor kapcsolatok kezelése állnak rendelkezésre. Ez az eljárás "közbeni hívóbetűk" néven is ismert.

Media Services nyújtott tároló kulcs függ. Adatfolyam-vagy letöltése eszközeit használt a Locator kifejezetten, a megadott tárterület hívóbetű függ. AMS fiók létrehozásakor függőség felveszi alapértelmezés szerint a elsődleges tároló hívóbetű, de felhasználóként frissítheti a AMS tartalmazó tároló billentyűt. Győződjön meg arról, hogy tudja, melyik kulcs használatához kövesse az ebben a témakörben leírt Media Services. Is, ha közbeni tároló hívóbetűk, kell ügyeljen arra, hogy a Locator így frissítése nem fog a továbbított szolgáltatásban (Ez a lépés van még a következő témakörben olvashat) megszakítás nélkül.

>[AZURE.NOTE]Ha több tárterület-fiókot, ezt az eljárást mindegyik tárterület-fiókkal kell elvégeznie.
>
>Végrehajtása egy gyártási fiók ebben a témakörben ismertetett lépéseket, mielőtt feltétlenül tesztelését előtti gyártási-fiókjában.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Lépés: 1: Újragenerálása másodlagos tároló hívóbetű

Kiindulás bezárhatja a másodlagos tároló billentyűt. Media Services nem használható a alapértelmezés szerint a másodlagos kulcsa.  Tárterület billentyűk vetítődnek tudnivalókért lásd [hogyan: nézet, a másolás és a hívóbetűk újragenerálása tároló](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Lépés: 2: Frissítés Media Services, akkor az új másodlagos tároló billentyűvel

Frissítse a Media Services, a másodlagos tároló access billentyűvel. Ahhoz, hogy a regenerált tároló kulcs szinkronizálni a Media Services használhatja az alábbi két módszer közül.

- Az Azure portál használata: a név és a kulcs értékek megkeresése az Azure portált, és jelölje ki a fiókját. Beállítások ablakban a jobb oldalon megjelenik. A beállítások ablakban jelölje ki a billentyűk. Attól függően, hogy melyik tároló kulcs, amelyet a szinkronizálás a Media Services, jelölje be a szinkronizálás elsődleges kulcs, vagy ha szinkronizálja a másodlagos gombra. Ebben az esetben a másodlagos billentyűvel.

- Media Services kezelése REST API-t használja.

A következő példa bemutatja, hogyan a https://endpoint/***subscriptionId/szolgáltatások/mediaservices/fiókok/fióknév*/StorageAccounts/*storageAccountName*/Key kérelem annak érdekében, hogy a megadott tárterület kulcs szinkronizálása a Media Services Egyenletszerkesztővel. Ebben az esetben a másodlagos tároló kulcs értékét használja. További tudnivalókért lásd: [hogyan: használata Media Services kezelése REST API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Ezt a lépést követően frissítése meglévő Locator (függ, hogy függőség a régi tároló billentyűt), ahogy a következő lépéssel.

>[AZURE.NOTE]Várja meg a Media Services (például létrehozása új Locator) műveletek végrehajtása annak érdekében, hogy bármilyen hatásának megakadályozása a függőben lévő feladatok előtt 30 percig.

##<a name="step-3-update-locators"></a>Lépés 3: Frissítés Locator

>[AZURE.NOTE]Tároló hívóbetűk közbeni, amikor frissíti a meglévő Locator, így a megszakítás nélkül az adatfolyam szolgáltatás nem feltétlenül szeretne.

Várjon legalább 30 perc után az új tároló kulcs szinkronizálása AMS. Ezután létrehozhatja a OnDemand Locator, függőség tanulmányozza a megadott tárterület billentyűt, és karbantartása a meglévő URL-CÍMÉT.

Figyelje meg, hogy frissítésekor (vagy hozza létre ismét) egy Társítások megnevezés, az URL-cím mindig változik.

>[AZURE.NOTE] Győződjön meg arról, a meglévő URL-címét a OnDemand Locator megőrzése, kell a meglévő megnevezés törlése, és hozzon létre egy újat ugyanazzal az azonosítóval.

A .NET az alábbi példa szemlélteti újra a megnevezés ugyanazzal az azonosítóval.

a magánjellegű statikus ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ és a meglévő megnevezés tulajdonságok mentéséhez.
var eszköz megnevezés =. Tárgyi eszköz; var accessPolicy = megnevezés. AccessPolicy; var locatorId = megnevezés. Azonosító; var kezdésidátum = megnevezés. A kezdő időpont; var locatorType = megnevezés. Írja be a; var locatorName = megnevezés. Neve;

Törölje a régi megnevezés.
Megnevezés. Delete();

Ha (megnevezés. ExpirationDateTime < = DateTime.UtcNow) {throw új kivétel (String.Format ("nem hozza létre a megnevezés Id = {0}, mert a megnevezés lejárati ideje van múltbeli", megnevezés. ID-val)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Lépés az 5: Újragenerálása elsődleges tároló hívóbetű

Az elsődleges tároló hívóbetű újragenerálása. Tárterület billentyűk vetítődnek tudnivalókért lásd [hogyan: nézet, a másolás és a hívóbetűk újragenerálása tároló](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Lépés a 6: Frissítés Media Services, akkor az új elsődleges tároló billentyűvel
    
Használja a eljárást csak most [lépés: 2](media-services-roll-storage-access-keys.md#step2) ismertetett módon szinkronizálása az új elsődleges tároló hívóbetű a Media Services fiókkal.

>[AZURE.NOTE]Várja meg a Media Services (például létrehozása új Locator) műveletek végrehajtása annak érdekében, hogy bármilyen hatásának megakadályozása a függőben lévő feladatok előtt 30 percig.

##<a name="step-7-update-locators"></a>7 lépés: Frissítés Locator  

30 perc múlva létrehozhatja a OnDemand Locator úgy, hogy tanulmányozza az új tároló elsődleges kulcs függőség, karbantartása: a meglévő URL-CÍMÉT.

A fenti eljárással a [3](media-services-roll-storage-access-keys.md#step-3-update-locators)leírt módon.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Visszaigazolások 

Azt szeretné, nyugtázza a következő, akik járult felé létrehozni a dokumentumot: Cenk Dingiloglu, Milánó Gada, Seva Titov.
