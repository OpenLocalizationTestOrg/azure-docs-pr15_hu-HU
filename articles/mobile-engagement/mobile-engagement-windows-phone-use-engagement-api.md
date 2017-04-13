<properties 
    pageTitle="A tetszés szerint elmélyedhet API-val Windows Phone Silverlight használatáról" 
    description="A tetszés szerint elmélyedhet API-val Windows Phone Silverlight használatáról"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>A tetszés szerint elmélyedhet API-val Windows Phone Silverlight használatáról

A dokumentum egy bővítmény szükséges a dokumentumba [történő mobil tetszés szerint elmélyedhet a Windows Phone Silverlight alkalmazásban integrálásáról](mobile-engagement-windows-phone-integrate-engagement.md). A z olvashat a tetszés szerint elmélyedhet API segítségével az alkalmazás statisztikai jelentést biztosít.

Ha csak a tetszés szerint elmélyedhet jelentést, az alkalmazás munkamenetek, tevékenységek, összeomlik és műszaki információkat szeretne, majd a legegyszerűbben, hogy az összes a `PhoneApplicationPage` alszint osztályok örökölhet a `EngagementPage` osztály.

Ha azt szeretné, többet, ha módosítani szeretné a jelentést az alkalmazás bizonyos eseményeket, a hibák és a feladatokat, például, vagy ha az alkalmazás tevékenységek jelentés eltérő módon, mint az egyik szerepelni fog a `EngagementPage` osztályok, akkor használja a tetszés szerint elmélyedhet API-t, majd.

A tevékenységek API által biztosított a `EngagementAgent` osztály. Ezeket a módszereket keresztül elérheti `EngagementAgent.Instance`.

Akkor is, ha a ügynök modul hibásan inicializált, minden hívás az API-nak elhalasztott van, és fog újra végre kell hajtania a agent esetén érhető el.

##<a name="engagement-concepts"></a>Tetszés szerint elmélyedhet fogalmak

A következő részekből finomíthatja a Mobile tetszés szerint elmélyedhet fogalmak a Windows Phone-platform.

### <a name="session-and-activity"></a>`Session`és`Activity`

Egy *tevékenység* , akkor általában egy oldallal az alkalmazás, azaz a *tevékenység* elindul, amikor a lap jelenik meg, és leállítja a lap bezárásakor: Ez a helyzet, ha a tevékenységek SDK integrált használatával a `EngagementPage` osztály.

De *tevékenységek* is vezérelhető manuálisan a tetszés szerint elmélyedhet API. Ez egy adott oldal, több sub elemet (például hogy milyen gyakran ismert, és mennyi ideig párbeszédpanelek használt belül erre a lapra) lap használatáról további részleteket a felosztandó lehetővé teszi.

##<a name="reporting-activities"></a>Jelentés a tevékenységek

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység

#### <a name="reference"></a>Hivatkozás

            void StartActivity(string name, Dictionary<object, object> extras = null)

Meg kell fordulnia `StartActivity()` minden alkalommal, amikor a felhasználó tevékenység módosításokat. Ez a függvény első hívás indítása új felhasználói munkamenet.

> [AZURE.IMPORTANT] A SDK csomagjában talál automatikusan hívja fel a EndActivity módszer, ha az alkalmazás be van zárva. Így erősen ajánlott hívja fel a StartActivity módszer, amikor a tevékenységet, a felhasználók változásról, és soha hívja fel a EndActivity módszer, mivel ez a módszer hívásához kezd az aktuális munkamenet be kell fejezni.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Felhasználói lezárja a jelenlegi tevékenységét.

#### <a name="reference"></a>Hivatkozás

            void EndActivity()

Meg kell fordulnia `EndActivity()` legalább egyszer amikor a felhasználó befejezése utolsó tevékenységét. Ez a tetszés szerint elmélyedhet SDK tájékoztatja, hogy a felhasználó éppen dolgozik, és lejár, hogy a felhasználó munkamenet kell zárni, egyszer a munkamenet-időtúllépés (ha hív meg `StartActivity()` a munkamenet-időtúllépés lejárta előtt a munkamenet egyszerűen továbbra is).

#### <a name="example"></a>Példa

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Feladatok jelentése

### <a name="start-a-job"></a>Feladat indítása

#### <a name="reference"></a>Hivatkozás

            void StartJob(string name, Dictionary<object, object> extras = null)

A feladat segítségével: bizonyos feladatok nyomon követése a egy időszakra vetített állapotával.

#### <a name="example"></a>Példa

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>A feladat befejezése

#### <a name="reference"></a>Hivatkozás

            void EndJob(string name)

Amint követi nyomon a projekt tevékenység befejeződött, meg kell hívnia EndJob módszer ehhez a feladathoz megadásával a feladat nevére.

#### <a name="example"></a>Példa

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Események jelentése

Események három típusa van:

-   Különálló események
-   Munkamenet-események
-   Feladat-események

### <a name="standalone-events"></a>Különálló események

#### <a name="reference"></a>Hivatkozás

            void SendEvent(string name, Dictionary<object, object> extras = null)

Különálló események akkor fordulhat elő, a munkamenet környezetében kívül.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Munkamenet-események

#### <a name="reference"></a>Hivatkozás

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Munkamenet események általában az egy adott munkamenetben végrehajtott műveletek jelentés használhatók.

#### <a name="example"></a>Példa

**Adatok: nélkül**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Az adatok:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Feladat-események

#### <a name="reference"></a>Hivatkozás

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Feladat események általában a projekt során a felhasználó által végrehajtott műveletek jelentés használhatók.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Hibák jelentése

Három típusú hibákat nem:

-   Különálló hibák
-   Munkamenet hibák
-   Feladat-hibák

### <a name="standalone-errors"></a>Különálló hibák

#### <a name="reference"></a>Hivatkozás

            void SendError(string name, Dictionary<object, object> extras = null)

Ellentétes munkamenet hibák különálló hibák akkor fordulhat elő, a munkamenet környezetében kívül.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Munkamenet hibák

#### <a name="reference"></a>Hivatkozás

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Munkamenet hibák általában érintő a felhasználó saját munkamenetben hibajelentés használhatók.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Feladat-hibák

#### <a name="reference"></a>Hivatkozás

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Hibák a futó feladatok helyett az aktuális felhasználói munkamenethez összetartozó is kapcsolódhat.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Jelentéskészítési összeomlik

A agent kell foglalkozni az lefagy a két módszert kínál.

### <a name="send-an-exception"></a>Kivétel küldése

#### <a name="reference"></a>Hivatkozás

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Példa

Betárcsázással kivételt bármikor küldheti el:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Választható paraméter a tetszés szerint elmélyedhet munkamenet, mint az összeomlást küldése egyszerre befejezéséhez is használhatja. Ehhez hívás:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Az ehhez szükséges lépéseket, ha a munkamenet és a feladatok bezárul csak az összeomlást elküldés után beállítást.

### <a name="send-an-unhandled-exception"></a>Esetén nem kezelt kivételt küldése

#### <a name="reference"></a>Hivatkozás

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Tetszés szerint elmélyedhet is küldhet esetén nem kezelt kivételek módszert kínál. Az különösen akkor hasznos, ha a silverlight UnhandledException eseménykezelő belül használni.

Ez a módszer fog **mindig** ér véget, a munkamenet tetszés szerint elmélyedhet és a feladatok után a hívott.

#### <a name="example"></a>Példa

Használhatja a saját UnhandledException kezelő végrehajtásához (különösen akkor, ha az automatikus összeomlást jelentési felvételi funkció le van tiltva). Ha például az a `Application_UnhandledException` módszer a `App.xaml.cs` fájl:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Hivatkozás

            void OnActivated(ActivatedEventArgs e)

Amikor a felhasználó navigál előre, távolodjon el egy alkalmazást, akkor következik be, a inaktívvá válnak esemény, miután az operációs rendszer megkísérli az alkalmazás inaktív állapotba. Az alkalmazás akkor törlésre. A folyamat az alkalmazások megszakad, de megmarad az alkalmazás és az egyes lapokat, az alkalmazáson belül állapotával kapcsolatos adatokat.

Helyezze be kell `EngagementAgent.Instance.OnActivated(e)` a a `Application_Activated` módszer alaphelyzetbe állítása az tetszés szerint elmélyedhet ügynök, amikor az alkalmazás már törlésre kijelölt App.xaml.cs fájlból.

### <a name="example"></a>Példa

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Eszköz azonosítója

            String GetDeviceId()

Ez a módszer hívja fel az tetszés szerint elmélyedhet eszközazonosítót elérheti.

##<a name="extras-parameters"></a>Extrák paraméterei

Tetszőleges adatokat kapcsolódhat esemény, hiba történt, egy tevékenység vagy egy feladatot. Ezeket az adatokat is strukturált, egy szótár használatával. Bármilyen típusú kulcsok és értékek is lehet.

Extrák adatok szerializálásának vannak, így be szeretné szúrni a saját a extrák esetén az ilyen típusú adat szerződést hozzáadni.

### <a name="example"></a>Példa

Hozzunk létre egy új osztály "Személy".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Ezután hozzáadunk egy `Person` egy plusz példányt.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Ha más típusú objektumok helyezi, ellenőrizze, azok ToString() módszer alkalmazása egy emberi olvasható karakterlánc jeleníthető meg.

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Minden egyes billentyűt az objektum meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Extrák fognak korlátozódni hívás **1024** karakterszám.

##<a name="reporting-application-information"></a>Jelentéskészítési alkalmazás adatai

### <a name="reference"></a>Hivatkozás

            void SendAppInfo(Dictionary<object, object> appInfos)

Manuális jelentést küldhet függvény nyomon követése a SendAppInfo() használatával információval (vagy bármely más alkalmazás bizonyos adatai).

Megjegyzendő, hogy ezeket az információkat fokozatosan el lehet küldeni: csak a legújabb értéket az egy adott kulccsal fog tartani, az adott eszköz. Esemény extrák, például használni a\<objektum, objektum\> informations csatolhat.

### <a name="example"></a>Példa

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Minden egyes billentyűt az objektum meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Alkalmazás adatai fognak korlátozódni hívás **1024** karakterszám.

Az előző példában az a kiszolgálóra küldött JSON 44 karakter hosszú:

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Naplózás
###<a name="enable-logging"></a>A naplózás engedélyezése

A SDK beállítható úgy, hogy a IDE konzolban próba naplók kiszámítására.
Ezek a naplók nem aktiválódnak alapértelmezés szerint. Testreszabásához ez a tulajdonság frissítéséhez `EngagementAgent.Instance.TestLogEnabled` az egyik elérhető értékét a `EngagementTestLogLevel` számbavétel, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
