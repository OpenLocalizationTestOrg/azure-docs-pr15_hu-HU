<properties 
    pageTitle="A tevékenységek API-val Windows univerzális használatáról" 
    description="A tevékenységek API-val Windows univerzális használatáról"            
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-universal"></a>A tevékenységek API-val Windows univerzális használatáról

Ezt a dokumentumot a dokumentum [tetszés szerint elmélyedhet integráció a Windows univerzális hogyan](mobile-engagement-windows-store-integrate-engagement.md)bővítménye: a mélység olvashat a tetszés szerint elmélyedhet API segítségével az alkalmazás statisztikai jelentést biztosít.

Felhívjuk a figyelmét arra, hogy ha csak a tetszés szerint elmélyedhet jelentést, az alkalmazás munkamenetek, tevékenységek, összeomlik és műszaki információkat szeretne, majd a legegyszerűbben, hogy az összes a `Page` alszint osztályok örökölhet a `EngagementPage` osztály.

Ha azt szeretné, többet, ha módosítani szeretné a jelentést az alkalmazás bizonyos eseményeket, a hibák és a feladatokat, például, vagy ha az alkalmazás tevékenységek jelentés eltérő módon, mint az egyik szerepelni fog a `EngagementPage` osztályok, akkor használja a tetszés szerint elmélyedhet API-t, majd.

A tevékenységek API által biztosított a `EngagementAgent` osztály. Ezeket a módszereket keresztül elérheti `EngagementAgent.Instance`.

Akkor is, ha a ügynök modul hibásan inicializált, minden hívás az API-nak elhalasztott van, és fog újra végre kell hajtania a agent esetén érhető el.

##<a name="engagement-concepts"></a>Tetszés szerint elmélyedhet fogalmak

A következő részekből finomíthatja a Windows univerzális Platform [Mobile tetszés szerint elmélyedhet fogalmak](mobile-engagement-concepts.md) közös.

### <a name="session-and-activity"></a>`Session`és`Activity`

Egy *tevékenység* , akkor általában egy oldallal az alkalmazás, azaz a *tevékenység* elindul, amikor a lap jelenik meg, és leállítja a lap bezárásakor: Ez a helyzet, ha a tevékenységek SDK integrált használatával a `EngagementPage` osztály.

De *tevékenységek* is vezérelhető manuálisan a tetszés szerint elmélyedhet API. Ez lehetővé teszi, hogy egy adott lapon, a további részleteket szeretne megtudni az e weblapra (például, hogy milyen gyakran és meddig párbeszédpanelek használják-e a lapon belül) használatát több sub részből felosztása.

##<a name="reporting-activities"></a>Jelentés a tevékenységek

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység

#### <a name="reference"></a>Hivatkozás

            void StartActivity(string name, Dictionary<object, object> extras = null)

Meg kell fordulnia `StartActivity()` minden alkalommal, amikor a felhasználó tevékenység módosításokat. Ez a függvény első hívás indítása új felhasználói munkamenet.

> [AZURE.IMPORTANT] A SDK csomagjában talál automatikusan hívások EndActivity módszer, ha az alkalmazás be van zárva. Így erősen ajánlott hívja fel a StartActivity módszer, amikor a tevékenységet, a felhasználói módosításokat, és soha hívja fel a EndActivity módszer, mivel ez a módszer hívásához kezd az aktuális munkamenet be kell fejezni.

#### <a name="example"></a>Példa

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Felhasználói lezárja a jelenlegi tevékenységét.

#### <a name="reference"></a>Hivatkozás

            void EndActivity()

Ez a művelet lezárja a tevékenységet, és a munkamenetet. Ez a módszer nem kell hívnia, hacsak nem igazán tudja művelettől.

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

Háromféle hibák van:

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

            void SendCrash(Exception e)

Tetszés szerint elmélyedhet esetén nem kezelt kivételek küldése a **LETILTOTT** tetszés szerint elmélyedhet automatikus **összeomlik** jelentéskészítés esetén módszert is biztosít. Az különösen akkor hasznos, ha az alkalmazás UnhandledException eseménykezelő belül használni.

Ez a módszer fog **mindig** ér véget, a munkamenet tetszés szerint elmélyedhet és a feladatok után a hívott.

#### <a name="example"></a>Példa

A saját UnhandledExceptionEventArgs kezelő végrehajtásához felhasználhatja azt. Például vegye fel a `Current_UnhandledException` módszer a `App.xaml.cs` fájl:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

A "Nyilvános App() {}" App.xaml.cs hozzáadása:

            Application.Current.UnhandledException += Current_UnhandledException;

##<a name="device-id"></a>Eszköz azonosítója

            String EngagementAgent.Instance.GetDeviceId()

Ez a módszer hívja fel az tetszés szerint elmélyedhet eszközazonosítót elérheti.

##<a name="extras-parameters"></a>Extrák paraméterei

Tetszőleges adatokat kapcsolódhat esemény, hiba történt, egy tevékenység vagy egy feladatot. Ezeket az adatokat is strukturált, egy szótár használatával. Bármilyen típusú kulcsok és értékek is lehet.

Extrák adatok szerializálásának vannak, így be szeretné szúrni a saját a extrák esetén az ilyen típusú adat szerződést hozzáadni.

### <a name="example"></a>Példa

Hozzunk létre egy új osztály "Személy".

            using System.Runtime.Serialization;
            
            namespace Microsoft.Azure.Engagement
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

Megjegyzés: az adatok küldhető fokozatosan: csak a legújabb értéket az egy adott kulccsal fog tartani, az adott eszköz. Esemény extrák, például használni a\<objektum, objektum\> adatok csatolása.

### <a name="example"></a>Példa

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Minden egyes billentyűt az objektum meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Alkalmazás adatai hívás **1024** karakterszám korlátozódik.

Az előző példában az a kiszolgálóra küldött JSON 44 karakter hosszú:

            {"birthdate":"1983-12-07","gender":"female"}

##<a name="logging"></a>Naplózás
###<a name="enable-logging"></a>A naplózás engedélyezése

A SDK beállítható úgy, hogy a IDE konzolban próba naplók kiszámítására.
Ezek a naplók nem aktiválódnak alapértelmezés szerint. Testreszabásához ez a tulajdonság frissítéséhez `EngagementAgent.Instance.TestLogEnabled` az egyik elérhető értékét a `EngagementTestLogLevel` számbavétel, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
 
