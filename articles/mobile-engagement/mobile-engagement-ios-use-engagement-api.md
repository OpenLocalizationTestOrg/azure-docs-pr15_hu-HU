<properties
    pageTitle="A tevékenységek API iOS használatáról"
    description="Legújabb iOS SDK - történő használatáról a tetszés szerint elmélyedhet API iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>A tevékenységek API iOS használatáról

A dokumentum a dokumentum bővítménye tetszés szerint elmélyedhet integráció a iOS hogyan: biztosít a mélység olvashat a tetszés szerint elmélyedhet API segítségével az alkalmazás statisztikai jelentést.

Felhívjuk a figyelmét arra, hogy ha csak a tetszés szerint elmélyedhet jelentést, az alkalmazás munkamenetek, tevékenységek, összeomlik és műszaki információkat szeretne, majd a legegyszerűbben, hogy az összes egyéni `UIViewController` objektumok örökölhet a megfelelő `EngagementViewController` osztály.

Ha azt szeretné, többet, ha módosítani szeretné a jelentést az alkalmazás bizonyos eseményeket, a hibák és a feladatokat, például, vagy ha az alkalmazás tevékenységek jelentés eltérő módon, mint az egyik szerepelni fog a `EngagementViewController` osztályok, akkor használja a tetszés szerint elmélyedhet API-t, majd.

A tevékenységek API által biztosított a `EngagementAgent` osztály. Az osztály egy példányának tudja visszaszerezni hívja fel a `[EngagementAgent shared]` statikus módszer (vegye figyelembe, hogy a `EngagementAgent` objektumban egyszeres).

Mielőtt bármilyen API-hívások a `EngagementAgent` objektumot inicializálni kell hívja fel a módszer`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Tetszés szerint elmélyedhet fogalmak

A következő részekből finomíthatja a közös [Mobile tetszés szerint elmélyedhet fogalmak](mobile-engagement-concepts.md) az iOS-platform.

### <a name="session-and-activity"></a>`Session`és`Activity`

Egy *tevékenység* , akkor általában egy képernyő-az alkalmazás, azaz a *tevékenység* elindul, amikor a képernyő jelenik meg, és leállítja a képernyő bezárásakor: Ez a helyzet, ha a tevékenységek SDK integrált használatával a `EngagementViewController` osztályok.

De *tevékenységek* is vezérelhető manuálisan a tetszés szerint elmélyedhet API. Ez lehetővé teszi, hogy a sub több részből további részleteket szeretne megtudni az alábbi képernyő (például hogy milyen gyakran ismert, és mennyi ideig párbeszédpanelek használt belül a képernyő) használatát egy adott képernyőjére felosztása.

##<a name="reporting-activities"></a>Jelentés a tevékenységek

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Meg kell fordulnia `startActivity()` minden alkalommal, amikor a felhasználó tevékenység módosításokat. Ez a függvény első hívás indítása új felhasználói munkamenet.

### <a name="user-ends-his-current-activity"></a>Felhasználói lezárja a jelenlegi tevékenységét.

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Akkor **Soha** kell keresnie őket felhívja ezt a funkciót, kivéve, ha a felosztandó több munkamenet az alkalmazás használatát: ezt a funkciót a hívásokat lenne a befejezése aktuális azonnal, Igen, a további hívásokat `startActivity()` szeretne új indítása. Ez a funkció automatikusan neve a SDK szerint az alkalmazás bezárásakor.

##<a name="reporting-events"></a>Események jelentése

### <a name="session-events"></a>Munkamenet-események

Munkamenet események általában az egy adott munkamenetben végrehajtott műveletek jelentés használhatók.

**Példa a felesleges adatok nélkül:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Példa a felesleges adatok:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Különálló események

Ellentétes munkamenet eseményeket különálló események kívüli munkamenet környezetében használható.

**Példa:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Hibák jelentése

### <a name="session-errors"></a>Munkamenet hibák

Munkamenet hibák általában érintő a felhasználó saját munkamenetben hibajelentés használhatók.

**Példa:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Különálló hibák

Ellentétes munkamenet hibák különálló hibák kívüli munkamenet környezetében használható.

**Példa:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Feladatok jelentése

**Példa:**

Tegyük fel, hogy az időtartam, a bejelentkezési folyamat jelenteni kívánt:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>A projekt közben hibák jelentése

Hibák a futó feladatok helyett az aktuális felhasználói munkamenethez összetartozó is kapcsolódhat.

**Példa:**

Tegyük fel, hogy a bejelentkezési folyamat során hibajelentések:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>A projekt közben események

Is kapcsolatos eseményekről helyett az aktuális felhasználói munkamenethez összetartozó futó feladatot.

**Példa:**

Tegyük fel van még egy közösségi hálózaton, és a teljes időt, amely alatt a felhasználó csatlakozik a kiszolgáló a feladat jelentés használjuk. A felhasználó saját barátok fogadhat üzenetet, és a feladat az esemény.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>További paramétereket

Események, a hibák, a tevékenységek és a feladatok tetszőleges adatokat is csatolva.

Ezeket az adatokat is lehet strukturált, akkor adja meg az iOS NSDictionary osztály.

Figyelje meg, hogy extrák tartalmazhatnak `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` vagy más `NSDictionary` példányok.

> [AZURE.NOTE] A felesleges paraméter szerializált a JSON-ban. Fent leírtaktól eltérő objektumok át szeretné, ha az osztályához be kell állítani az alábbi módon:
>
             -(NSString*)JSONRepresentation;
>
> A módszer térjen vissza az objektum JSON ábrázolása.

### <a name="example"></a>Példa

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Az egyes billentyűt a `NSDictionary` meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Extrák fognak korlátozódni **1024** karakterszám hívás (egyszer kódolt JSON a tetszés szerint elmélyedhet ügynök).

Az előző példában az a kiszolgálóra küldött JSON 58 karakter hosszú:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Jelentéskészítési alkalmazás adatai

Manuális jelentheti a nyomkövetési információk (vagy bármely más alkalmazás adott információ) használata a `sendAppInfo:` függvény.

Megjegyzendő, hogy ezeket az információkat fokozatosan el lehet küldeni: csak a legújabb értéket az egy adott kulccsal fog tartani, az adott eszköz.

Esemény extrák, például a `NSDictionary` osztály használható absztrakt alkalmazás adatai, megfigyelheti, hogy alszint szótárak vagy tömböknek kezelik strukturálatlan karakterláncként (JSON szerializálási használata).

**Példa:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Az egyes billentyűt a `NSDictionary` meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Alkalmazás adatai fognak korlátozódni **1024** karakterszám hívás (egyszer kódolt JSON a tetszés szerint elmélyedhet ügynök).

Az előző példában az a kiszolgálóra küldött JSON 44 karakter hosszú:

    {"birthdate":"1983-12-07","gender":"female"}
