<properties
    pageTitle="Azure mobil tetszés szerint elmélyedhet webes API-khoz SDK |} Microsoft Azure"
    description="A legújabb frissítések és Azure Mobile tetszés szerint elmélyedhet a webes SDK eljárások"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Az Azure Mobile tetszés szerint elmélyedhet API használata a webes alkalmazáshoz

A dokumentum, amely közli, hogy a dokumentum kiegészítése történő [integrálásáról Mobile tetszés szerint elmélyedhet a webes alkalmazásokban](mobile-engagement-web-integrate-engagement.md). Azt ismerteti, hogyan az Azure Mobile tetszés szerint elmélyedhet API segítségével az alkalmazás statisztikai jelentést meg szeretné vizsgálni részleteit.

A mobil tetszés szerint elmélyedhet API által biztosított a `engagement.agent` objektumot. Az alapértelmezett aliast az Azure Mobile tetszés szerint elmélyedhet Web SDK `engagement`. Az alias a SDK beállítások is definiálhatja.

## <a name="mobile-engagement-concepts"></a>Mobil tetszés szerint elmélyedhet fogalmak

A következő részekből finomíthatja a webes platform közös [Mobile tetszés szerint elmélyedhet fogalmak](mobile-engagement-concepts.md) .

### <a name="session-and-activity"></a>`Session`és`Activity`

Ha a felhasználó több, mint egy pár másodpercre a két tevékenység közötti üresjárati marad, a felhasználót, tetszőleges sorrendben követő tevékenységek oszlik két különböző munkamenetek. Ezek pár másodpercre a munkamenet-időtúllépés nevezik.

Ha a webalkalmazás felhasználói tevékenységek végén nem deklarálhatnak önmagában (hívja fel a `engagement.agent.endActivity` függvény), a mobil tetszés szerint elmélyedhet kiszolgáló automatikusan jár le az alkalmazás lap bezárása után három percen belül a felhasználó munkamenetet. A kiszolgáló időtúllépés Link.

### `Crash`

Automatikus jelentések nem JavaScript kivételek nem alapértelmezés szerint jönnek létre. Jó helyen jár, jelentést küldhet lefagy a manuális használatával a `sendCrash` (lásd a jelentési összeomlik a) működik.

## <a name="reporting-activities"></a>Jelentés a tevékenységek

Új tevékenység indításakor és az aktuális tevékenység befejeződésekor felhasználói tevékenység jelentés tartalmazza.

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység

    engagement.agent.startActivity("MyUserActivity");

Meg kell fordulnia `startActivity()` minden alkalommal felhasználói tevékenység változik. Ez a függvény első hívás indítása új felhasználói munkamenet.

### <a name="user-ends-the-current-activity"></a>Felhasználói befejeződik, az aktuális tevékenység

    engagement.agent.endActivity();

Meg kell fordulnia `endActivity()` legalább egyszer amikor a felhasználó befejezése utolsó tevékenységük. Ez a Mobile tetszés szerint elmélyedhet Web SDK tájékoztatja, hogy a felhasználó éppen dolgozik, és, hogy a felhasználó munkamenet kell zárni, a munkamenet-időtúllépés lejárta után. Ha hív meg `startActivity()` a munkamenet-időtúllépés lejárta előtt a munkamenet egyszerűen folytatódik.

Mivel a nem megbízható hívást, amikor a kezelő ablakban meg van nyitva a, érdemes gyakran megnehezítik vagy felhasználói tevékenységek belül webes környezetben is használhatók végén tájékozódást segíti, hogy nem lehetséges. Ezért a Mobile tetszés szerint elmélyedhet kiszolgáló automatikusan jár le az alkalmazás lap bezárása után három percen belül a felhasználó munkamenetet.

## <a name="reporting-events"></a>Események jelentése

Események jelentés bemutatja a munkamenet és önálló események.

### <a name="session-events"></a>Munkamenet-események

Munkamenet események általában használhatók a felhasználói munkamenet közben a felhasználó által végrehajtott műveletek jelentést.

**Példa a felesleges adatok nélkül:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Példa a felesleges adatok:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Különálló események

Munkamenet események eltérően önálló események akkor fordulhat elő, a munkamenet kívül.

Ezzel ``engagement.agent.sendEvent`` helyett ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Hibák jelentése

A hibák jelentése bemutatja a munkamenet-hibák és önálló hibák.

### <a name="session-errors"></a>Munkamenet hibák

Munkamenet hibák általában szolgálnak, hogy a felhasználó hatással vannak a felhasználó munkamenetben hibajelentés.

**Példa a felesleges adatok nélkül:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Példa a felesleges adatok:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Különálló hibák

Eltérően munkamenet hibák különálló hiba akkor fordulhat elő, munkamenet kívül.

Ezzel `engagement.agent.sendError` helyett `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Feladatok jelentése

Jelentés a jelentéskészítés hibák és a projekt során fellépő események és lefagy a jelentési feladatok vonatkozik.

**Példa:**

AJAX-felkérés figyelni kívánt, ha használja az alábbiakat:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>A projekt közben hibajelentés

Ha nem az aktuális felhasználói munkamenethez futó feladat hibák kapcsolható ki.

**Példa:**

Ha szeretne jelzett hibát, ha nem sikerül egy AJAX kérelmet:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>A projekt közben jelentéskészítési események

Események is kapcsolódhat helyett az aktuális felhasználói munkamenethez futó feladat köszönet a következőknek a `engagement.agent.sendJobEvent` függvény.

Ez a funkció működik például pontosan `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Jelentéskészítés összeomlik

Használja a `sendCrash` manuálisan lefagy a jelentés függvényt.

A `crashid` argumentum olyan karakterlánc, amely azonosítja az összeomlást típusát.
A `crash` argumentum rendszerint a Papírhalom követés karakterláncként az összeomlást a.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>További paramétereket

Egy eseményt, hiba, tevékenységet vagy feladat csatolása tetszőleges adatokat.

Az adatok lehet JSON objektumokat (de ne egy tömb vagy egyszerű típus).

**Példa:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Korlátai

További paramétereket vonatkozó korlátozások vannak a reguláris kifejezésekkel kulcsok, értéktípusok és a méret területen.

#### <a name="keys"></a>Billentyűk

Minden egyes billentyűt az objektum meg kell egyeznie a következő kifejezéssel:

    ^[a-zA-Z][a-zA-Z_0-9]*

Ez azt jelenti, hogy legalább egy betűvel, billentyűk kell kezdődnie követ betűket, számjegyek és aláhúzás karakterek találhatók (\_).

#### <a name="values"></a>Értékek

Értékek korlátozva karaktersorozat, szám vagy logikai típusok.

#### <a name="size"></a>Mérete

Extrák arra korlátozódik hívás 1024 karakterszám (miután a Mobile tetszés szerint elmélyedhet Web SDK kódolja a JSON-ban).

## <a name="reporting-application-information"></a>Jelentéskészítési alkalmazás adatai

Manuális jelentheti nyomkövetési információk (vagy bármely más alkalmazás-specifikus információ) használata a `sendAppInfo()` függvény.

Megjegyzés: Ez az információ fokozatosan küldött. Egy adott eszközhöz folyamatosan csak a legújabb értéket az egy adott billentyűt.

Esemény extrák, például a JSON-objektumokat segítségével absztrakt alkalmazás adatai. Figyelje meg, hogy a részleges objektumok vagy tömböknek kezelni strukturálatlan karakterláncként (JSON szerializálási használata).

**Példa:**

Az alábbiakban a felhasználó gender és a születési dátum küldésének kód minta:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Korlátai

Alkalmazás adatai vonatkozó korlátozások vannak a reguláris kifejezésekkel kulcsok és a méret területen.

#### <a name="keys"></a>Billentyűk

Minden egyes billentyűt az objektum meg kell egyeznie a következő kifejezéssel:

    ^[a-zA-Z][a-zA-Z_0-9]*

Ez azt jelenti, hogy legalább egy betűvel, billentyűk kell kezdődnie követ betűket, számjegyek és aláhúzás karakterek találhatók (\_).

#### <a name="size"></a>Mérete

Alkalmazás adatai hívás 1024 karakterszám korlátozva (miután a Mobile tetszés szerint elmélyedhet Web SDK kódolja a JSON-ban).

Az előző példában az a kiszolgálóra küldött JSON 44 karakter hosszú:

    {"birthdate":"1983-12-07","gender":"female"}
