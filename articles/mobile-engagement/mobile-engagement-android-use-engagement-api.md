<properties
    pageTitle="A tevékenységek API használata Android-eszközön"
    description="Legújabb Android SDK - a tevékenységek API használata Android-eszközön"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>A tevékenységek API használata Android-eszközön

Ezt a dokumentumot a dokumentum [Android Mobile tetszés szerint elmélyedhet SDK jelentéskészítés speciális beállításainak](mobile-engagement-android-advanced-reporting.md)bővítménye. A z olvashat a tetszés szerint elmélyedhet API segítségével az alkalmazás statisztikai jelentést biztosít.

Felhívjuk a figyelmét arra, hogy ha csak a tetszés szerint elmélyedhet jelentést, az alkalmazás munkamenetek, tevékenységek, összeomlik és műszaki információkat szeretne, majd a legegyszerűbben, hogy az összes a `Activity` alszint osztályok örökölhet a megfelelő `EngagementActivity` osztály.

Ha azt szeretné, többet, ha módosítani szeretné a jelentést az alkalmazás bizonyos eseményeket, a hibák és a feladatokat, például, vagy ha az alkalmazás tevékenységek jelentés eltérő módon, mint az egyik szerepelni fog a `EngagementActivity` osztályok, akkor használja a tetszés szerint elmélyedhet API-t, majd.

A tevékenységek API által biztosított a `EngagementAgent` osztály. Az osztály egy példányának tudja visszaszerezni hívja fel a `EngagementAgent.getInstance(Context)` statikus módszer (vegye figyelembe, hogy a `EngagementAgent` objektumban egyszeres).

##<a name="engagement-concepts"></a>Tetszés szerint elmélyedhet fogalmak

A következő részekből finomíthatja a közös [Mobile tetszés szerint elmélyedhet fogalmak](mobile-engagement-concepts.md), az Android platform.

### <a name="session-and-activity"></a>`Session`és`Activity`

Ha a felhasználó több, mint néhány másodpercet üresjárati a két *tevékenység*közötti marad, akkor a *tevékenységek* sorozatának két különböző *munkamenetek*bontja a függvény. Ezek néhány másodpercet "munkamenet-időtúllépés" nevezik.

Egy *tevékenység* , akkor általában egy képernyő-az alkalmazás, azaz a *tevékenység* elindul, amikor a képernyő jelenik meg, és leállítja a képernyő bezárásakor: Ez a helyzet, ha a tevékenységek SDK integrált használatával a `EngagementActivity` osztályok.

De *tevékenységek* is vezérelhető manuálisan a tetszés szerint elmélyedhet API. Ez lehetővé teszi, hogy a sub több részből további részleteket szeretne megtudni az alábbi képernyő (például hogy milyen gyakran ismert, és mennyi ideig párbeszédpanelek használt belül a képernyő) használatát egy adott képernyőjére felosztása.

##<a name="reporting-activities"></a>Jelentés a tevékenységek

> [AZURE.IMPORTANT] Ha használja a jelen szakaszban ismertetett tevékenységek például jelentés nem kell a `EngagementActivity` osztály és a változatok tetszés szerint elmélyedhet integrálni szeretné a hogyan Android a dokumentumhoz című cikkben ismertetett módon.

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Meg kell fordulnia `startActivity()` minden alkalommal, amikor a felhasználó tevékenység módosításokat. Ez a függvény első hívás indítása új felhasználói munkamenet.

Ez a függvény a lehető legjobb hely be van kapcsolva az egyes tevékenységek `onResume` visszahívási.

### <a name="user-ends-his-current-activity"></a>Felhasználói lezárja a jelenlegi tevékenységét.

            EngagementAgent.getInstance(this).endActivity();

Meg kell fordulnia `endActivity()` legalább egyszer amikor a felhasználó befejezése utolsó tevékenységét. Ez a tetszés szerint elmélyedhet SDK tájékoztatja, hogy a felhasználó éppen dolgozik, és lejár, hogy a felhasználó munkamenet kell zárni, egyszer a munkamenet-időtúllépés (ha hív meg `startActivity()` a munkamenet-időtúllépés lejárta előtt a munkamenet egyszerűen folytatódik).

Ez a függvény a lehető legjobb hely be van kapcsolva az egyes tevékenységek `onPause` visszahívási.

##<a name="reporting-events"></a>Események jelentése

### <a name="session-events"></a>Munkamenet-események

Munkamenet események általában az egy adott munkamenetben végrehajtott műveletek jelentés használhatók.

**Példa a felesleges adatok nélkül:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Példa a felesleges adatok:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Különálló események

Ellentétes munkamenet eseményeket különálló események akkor fordulhat elő, a munkamenet környezetében kívül.

**Példa:**

Tegyük fel, jelentés események előforduló közvetített fogadó elindításakor szeretné:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Hibák jelentése

### <a name="session-errors"></a>Munkamenet hibák

Munkamenet hibák általában érintő a felhasználó saját munkamenetben hibajelentés használhatók.

**Példa:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Különálló hibák

Ellentétes munkamenet hibák különálló hibák akkor fordulhat elő, a munkamenet környezetében kívül.

**Példa:**

A következő példa bemutatja, hogyan jelentés hibaüzenetet, amikor alacsony a telefonon a memória válik az alkalmazás folyamat futtatása közben.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Feladatok jelentése

### <a name="example"></a>Példa

Tegyük fel, hogy az időtartam, a bejelentkezési folyamat jelenteni kívánt:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>A projekt közben hibák jelentése

Hibák a futó feladatok helyett az aktuális felhasználói munkamenethez összetartozó is kapcsolódhat.

**Példa:**

Tegyük fel, hogy a jelentés alatt, hiba történt a bejelentkezési folyamatot:

[...] nyilvános érvénytelenítése bejelentkezik (helyi környezetben,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>A projekt közben jelentéskészítési események

Is kapcsolatos eseményekről helyett az aktuális felhasználói munkamenethez összetartozó futó feladatot.

**Példa:**

Tegyük fel van még egy közösségi hálózaton, és a teljes időt, amely alatt a felhasználó csatlakozik a kiszolgáló a feladat jelentés használjuk. A felhasználó is tarthatja a háttérben még azt egy másik alkalmazás használja, vagy a telefon alvó szándékos, nincs munkamenetet.

A felhasználó saját barátok fogadhat üzenetet, és a feladat az esemény.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>További paramétereket

Események, a hibák, a tevékenységek és a feladatok tetszőleges adatokat is csatolva.

Ezeket az adatokat is lehet strukturált, akkor használja az Android-készülék az első lépésekhez class (ténylegesen, akkor működik, mint az Android módok további paramétereket). Figyelje meg, hogy egy köteg tartalmazhatnak, tömbök, vagy egy másik az első lépésekhez példányok.

> [AZURE.IMPORTANT] Ha parcelable vagy szerializálható paraméter helyezi, ellenőrizze, hogy azok `toString()` módszer alkalmazása egy olvasható karakterlánc jeleníthető meg. Nem tranziens mezőket, amelyek nem szerializálható tartalmazó szerializálható osztályok láthatóvá válik Android összeomlik, hívásakor megadhat lesz`bundle.putSerializable("key",value);`

> [AZURE.WARNING] További paramétereket a ritka tömbök nem támogatottak, ez azt jelenti, hogy nem használhatók, amelyekről tömbként. Érdemes táblává őket szabványos tömbök további paramétereket a használat előtt.

### <a name="example"></a>Példa

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Az egyes billentyűt a `Bundle` meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Extrák fognak korlátozódni **1024** karakterszám hívás (egyszer kódolt JSON tetszés szerint elmélyedhet szolgáltatás).

Az előző példában az a kiszolgálóra küldött JSON 58 karakter hosszú:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Jelentéskészítési alkalmazás adatai

Manuális jelentheti a nyomkövetési információk (vagy bármely más alkalmazás adott információ) használata a `sendAppInfo()` függvény.

Megjegyzendő, hogy ezeket az információkat fokozatosan el lehet küldeni: csak a legújabb értéket az egy adott kulccsal fog tartani, az adott eszköz.

Az első lépésekhez osztály esemény extrák, például szolgál absztrakt alkalmazás adatai, megfigyelheti, hogy alszint kötegeket vagy tömböknek kezelik strukturálatlan karakterláncként (JSON szerializálási használata).

### <a name="example"></a>Példa

A következő kódot minta felhasználói gender és SzületésiDátum mező küldése:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Korlátai

#### <a name="keys"></a>Billentyűk

Az egyes billentyűt a `Bundle` meg kell egyeznie a következő kifejezéssel:

`^[a-zA-Z][a-zA-Z_0-9]*`

Az azt jelenti, hogy a billentyűk és legalább egy beírt betűk, számok vagy aláhúzás követ kell indítani (\_).

#### <a name="size"></a>Mérete

Alkalmazás adatai fognak korlátozódni **1024** karakterszám hívás (egyszer kódolt JSON tetszés szerint elmélyedhet szolgáltatás).

Az előző példában az a kiszolgálóra küldött JSON 44 karakter hosszú:

            {"expiration":"2016-12-07","status":"premium"}
