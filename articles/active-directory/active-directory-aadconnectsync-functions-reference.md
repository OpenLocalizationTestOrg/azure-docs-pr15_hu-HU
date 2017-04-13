<properties
    pageTitle="Azure AD Connect szinkronizálása: függvények hivatkozás |} Microsoft Azure"
    description="A hivatkozás Azure AD Connect szinkronizálás deklaráció kiépítési kifejezések."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect szinkronizálása: függvények hivatkozás

Az Azure AD Connect függvények használata az attribútumérték kezelhetik a szinkronizálás során.  
A függvények szintaxisa is kifejezhető, a következő formátumban:  
`<output type> FunctionName(<input type> <position name>, ..)`

Ha túl van terhelve függvényt, és fogadja el a több szintaxis, az összes érvényes szintaxis sorolja fel.  
A függvény kifejezetten írta-e be, és azok ellenőrizze, hogy a típus átadott egyezéseket dokumentált típusát.  
Ha a típus értéke nem egyezik meg, hibaüzenetet elő.

A következő szintaxissal fájltípusok kifejezhető:

- **tároló** – binárissá
- **logikai** : logikai érték
- **tartozik** – UTC dátum/idő
- **Felsorolás** – ismert állandók felsorolása
- **exp** – kifejezés, amely várhatóan kiértékelésének eredménye csak logikai érték
- **mvbin** – Multi-Valued bináris
- **mvstr** – Multi-Valued karakterlánc
- **mvref** – Multi-Valued hivatkozás
- **num** – numerikus
- **ref** – referencia
- **str** – karakterlánc
- **var** – variant (szinte) bármilyen típusú
- **Érvénytelen** – nem egy értéket adnak eredményül

A típusok **mvbin**, **mvstr**és **mvref** a függvények csak többértékű attribútum is dolgozhat. Egyetlen értékű mind a többértékű attribútum **bin**, **str**és **hivatkozási** függvények használata.

## <a name="functions-reference"></a>Hivatkozás függvény

Függvények listája | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Átalakítás** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Dátum / idő** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Most](#now)
[NumFromDate](#numfromdate) |  
**A címtár** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Kiértékelés** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Matematikai** |  
[Bit.és](#bitand) | [Bit.vagy](#bitor) | [RandomNum](#randomnum)
**Többértékű mező** |  
[Tartalmaz](#contains) | [Darabszám](#count) | [Elem](#item) | [ItemOrNull](#itemornull)
[Csatlakozás](#join) | [RemoveDuplicates](#removeduplicates) | [A felosztott](#split) |
**Program továbbításához** |  
[Hibaüzenet](#error) | [AZ IIF FÜGGVÉNY](#iif)  | [Váltás](#switch)
**Szöveg** |  
[GLOBÁLISAN EGYEDI AZONOSÍTÓJA](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[Balra](#left) | [Hossz](#len) | [Az LTrim](#ltrim) | [Közép](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Csere](#replace)
[ReplaceChars](#replacechars) | [Jobbra](#right) | [RTrim](#rtrim) | [Vágása](#trim)
[UCase](#ucase) | [A Word](#word)

----------
### <a name="bitand"></a>Bit.és

**Leírás:**  
A bit.és függvény megadott bittel egy értékre állítja be.

**Szintaxis:**  
`num BitAnd(num value1, num value2)`

- Érték1, érték2: számértékeket együtt kell érték

**Megjegyzés:**  
Ez a függvény a bináris ábrázolás mindkét paraméter alakítja, és egy kicsit állítja be,:

- 0 - e végre a megfelelő bit *maszk* és *jelző* 0
- 1 – Ha a megfelelő eltolással mindkettő 1.

Más szóval akkor adja vissza 0 minden esetben, kivéve ha a megfelelő bittel mindkét paraméterek 1.

**Példa:**  
`BitAnd(&HF, &HF7)`  
7 adja eredményül, mivel ez az érték logikai értéket hexadecimálissá "F" és "F7".

----------
### <a name="bitor"></a>Bit.vagy

**Leírás:**  
A bit.vagy függvény megadott bittel egy értékre állítja be.

**Szintaxis:**  
`num BitOr(num value1, num value2)`

- Érték1, érték2: együtt kell köztük numerikus értékek

**Megjegyzés:**  
Ez a függvény a bináris ábrázolás mindkét paraméter alakítja, és a állítja be egy kicsit 1, ha végre a megfelelő bit maszk és jelző 1 és 0, ha a megfelelő eltolással mindkettő 0. Más szóval akkor eredménye 1 kivéve, ha a megfelelő bittel mindkét paraméterek 0, minden esetben.

----------
### <a name="cbool"></a>CBool

**Leírás:**  
A CBool függvény logikai érték alapján a kiértékelt kifejezés eredményét adja vissza.

**Szintaxis:**  
`bool CBool(exp Expression)`

**Megjegyzés:**  
Ha a kifejezés nullától különböző érték, akkor CBool igaz értéket ad vissza, még akkor hamis értéket ad vissza.

**Példa:**  
`CBool([attrib1] = [attrib2])`  

Eredménye IGAZ, ha mindkét attribútumok azonos értékkel rendelkezik.

----------
### <a name="cdate"></a>CDate

**Leírás:**  
A CDate függvény UTC DateTime karakterláncként adja eredményül. DateTime nem szinkronizálja natív attribútum típus, de egyes funkciók használják.

**Szintaxis:**  
`dt CDate(str value)`

- Érték: Olyan karakterlánc, egy dátumot, időt, és tetszés szerint időzóna

**Megjegyzés:**  
A visszaadott karakterlánc értéke mindig UTC szerint megadva.

**Példa:**  
`CDate([employeeStartTime])`  
A kezdő időpont DateTime az alkalmazott alapján adja eredményül.

`CDate("2013-01-10 4:00 PM -8")`  
Egy DateTime, amely eredménye "2013-01-11-es 12:00 de"

----------
### <a name="cguid"></a>CGuid

**Leírás:**  
A CGuid függvény a bináris ábrázolás GUID karakterlánc ábrázolása konvertál.

**Szintaxis:**  
`bin CGuid(str GUID)`

- A minta formázott karakterlánc: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Tartalmaz

**Leírás:**  
A Contains függvény nem talál egy karakterlánc egy többértékű attribútum belül

**Szintaxis:**  
`num Contains (mvstring attribute, str search)`-és nagybetűket  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-és nagybetűket

- attribútum: a többértékű attribútum kereséséhez.
- Keresés: Ha az attribútum karakterlánc.
- Casetype: CaseInsensitive vagy CaseSensitive.

A többértékű attribútum, ahol a karakterlánc található indexet adja eredményül. 0 értéket adja vissza, ha a karakterlánc nem található.

**Megjegyzés:**  
Többértékű karakterlánc attribútumok a keresés összefűzendő karakterláncokat megkeresi az értékeket.  
Hivatkozás attribútumok esetén a keresett karakterláncot pontosan egyeznie kell figyelembe kell venni az egyező értéket.

**Példa:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Ha a proxyAddresses attribútumban elsődleges e-mail címmel rendelkezik (nagybetűs jelölt "SMTP:"), térjen vissza az proxyAddress attribútum, más hibát-e eredményül.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Leírás:**  
A ConvertFromBase64 függvény a megadott base64 kódolt értéket normál karakterlánc értékké alakítja át.

**Szintaxis:**  
`str ConvertFromBase64(str source)`-feltételezi, hogy a Unicode kódolást az  
`str ConvertFromBase64(str source, enum Encoding)`

- Forrás: Base64 címként kódolt karakterláncot  
- Kódolásra: Unicode, ASCII, UTF8

**Példa**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Két példa visszatérési "*Helló, világ!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Leírás:**  
A ConvertFromUTF8Hex függvény a megadott kódolva UTF8 Hex értékre konvertálja a karakterlánc.

**Szintaxis:**  
`str ConvertFromUTF8Hex(str source)`

- Forrás: UTF8 2 bájtos kódolt karakterlánc

**Megjegyzés:**  
Ez a függvény, illetve ConvertFromBase64([],UTF8) az, hogy az eredménye a DN attribútum rövid közötti különbség.  
Ez a formátum Azure Active Directory DN használja.

**Példa:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Eredménye "*Helló, világ!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Leírás:**  
A ConvertToBase64 függvény Unicode base64 karakterlánc alakít át.  
Az alap-64 számjegynél van kódolva egyenértékű karakteres egész számok tömbjével értékének konvertálja.

**Szintaxis:**  
`str ConvertToBase64(str source)`

**Példa:**  
`ConvertToBase64("Hello world!")`  
"SABlAGwAbABvACAAdwBvAHIAbABkACEA" függvény

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Leírás:**  
A ConvertToUTF8Hex függvény értékké karakterlánc kódolva UTF8 Hex.

**Szintaxis:**  
`str ConvertToUTF8Hex(str source)`

**Megjegyzés:**  
Ez a függvény a kimeneti formátum Azure Active Directory DN attribútum formátuma használja.

**Példa:**  
`ConvertToUTF8Hex("Hello world!")`  
48656C6C6F20776F726C6421 adja eredményül.

----------
### <a name="count"></a>Darabszám

**Leírás:**  
A Count függvény elemek számát adja meg a többértékű attribútum

**Szintaxis:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Leírás:**  
A CNum függvény karakterlánc tart, és egy numerikus adattípust ad vissza.

**Szintaxis:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Leírás:**  
Egy karakterlánc egy hivatkozás attribútum alakít át.

**Szintaxis:**  
`ref CRef(str value)`

**Példa:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Leírás:**  
String adattípus alakítja a CStr függvény.

**Szintaxis:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- érték: lehet egy numerikus érték, hivatkozás attribútum vagy logikai érték.

**Példa:**  
`CStr([dn])`  
Visszatérési sikerült "cn = Mintaügyvezető, adatközpont = contoso, adatközpont = hu"

----------
### <a name="dateadd"></a>DateAdd

**Leírás:**  
Egy, amelyhez hozzá lett adva megadott időközönként dátumot tartalmazó dátumot adja eredményül.

**Szintaxis:**  
`dt DateAdd(str interval, num value, dt date)`

- intervallum: a hozzáadni kívánt időtartományt tartalmazó karakterlánc. A karakterlánc kell rendelkeznie az alábbi értékek közül:
 - yyyy év
 - kérdések negyedév
 - m hónap
 - év napjának y
 - d nap
 - Weekday w
 - a hét ww
 - h óra
 - a perc n
 - s második
- érték: a mennyiségek el szeretné helyezni. Lehet, hogy a (az első későbbi dátumokat) pozitív vagy negatív (az első múltbeli dátumok).
- dátum: dátum, amelyre az intervallumra, amely DateTime.

**Példa:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Összeadja a 3 hónap és a "2001-04-01", amely egy DateTime adja eredményül.

----------
### <a name="datefromnum"></a>DateFromNum

**Leírás:**  
Egy DateTime típusú értéket AD meg dátum formátum DateFromNum függvény alakítja át.

**Szintaxis:**  
`dt DateFromNum(num value)`

**Példa:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Ad vissza, amely 2012-01-01 DateTime 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Leírás:**  
A DNComponent függvény egy megadott DN összetevő áttekintés bal oldalról értékét adja eredményül.

**Szintaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN: a hivatkozás attribútum értelmezése
- ComponentNumber: A visszaadandó megkülönböztető összetevőjének

**Példa:**  
`DNComponent([dn],1)`  
Ha a dn "cn = Mintaügyvezető, ou =,...," Mintaügyvezető adja vissza

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Leírás:**  
A DNComponentRev függvény egy megadott DN összetevő-ról jobbra (Befejezés) értékét adja eredményül.

**Szintaxis:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN: a hivatkozás attribútum értelmezése
- ComponentNumber – a megkülönböztető való visszatéréshez a összetevő
- Beállítások: Adatközpont – figyelmen kívül minden összetevő az "adatközpont ="

**Példa:**  
Ha dn "cn = Mintaügyvezető, szervezeti Szegeden, ou = Georgia, ou = = US, adatközpont = contoso, adatközpont = hu" majd  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Azonos adnak eredményül US.

----------
### <a name="error"></a>Hibaüzenet

**Leírás:**  
A hibafüggvény szolgál a egyéni hibaüzenetet adja vissza.

**Szintaxis:**  
`void Error(str ErrorMessage)`

**Példa:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Ha az attribútum fióknév nem található, hibaüzenetet küldjön az objektumon.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Leírás:**  
A EscapeDNComponent függvény DN egy részét elfoglalja, és úgy is képviselteti LDAP lehet kilépni.

**Szintaxis:**  
`str EscapeDNComponent(str value)`

**Példa:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Ellenőrzi, hogy az objektum az LDAP-címtár létrehozható, akkor is, ha a displayName attribútum karakterek, amelyek az LDAP kell megjelölni.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Leírás:**  
A FormatDateTime függvény segítségével karakterlánc DateTime formázása a megadott formátumban

**Szintaxis:**  
`str FormatDateTime(dt value, str format)`

- érték: egy értéket a DateTime formátumban
- formátum: a formátumot átalakítása képviselő karakterlánc.

**Megjegyzés:**  
A formátum a lehetséges értékek megtalálható itt: [Felhasználó által definiált dátum/idő formátumok (formátum függvény)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Példa:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007-12-25" eredményez.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
"20140905081453.0Z" eredményez

----------
### <a name="guid"></a>GLOBÁLISAN EGYEDI AZONOSÍTÓJA

**Leírás:**  
A függvény globálisan egyedi azonosítója létrehoz egy új véletlen globálisan egyedi azonosítója

**Szintaxis:**  
`str GUID()`

----------
### <a name="iif"></a>AZ IIF FÜGGVÉNY

**Leírás:**  
Az IIF függvénnyel a megadott feltétel alapján lehetséges értékek egyikét adja vissza.

**Szintaxis:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- feltétel: bármilyen érték vagy kifejezés, amely igaz vagy HAMIS értékként kiértékelhető.
- valueIfTrue: Ha a feltétel kiértékelésének eredménye IGAZ, a visszaadott érték.
- valueIfFalse: Ha az eredmény HAMIS, a visszaadott érték.

**Példa:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Ha a felhasználó-Műszerészek, adja vissza, a felhasználó "t-" azt egyéb elejére hozzáadott aliasát, akkor a felhasználó alias adja eredményül.

----------
### <a name="instr"></a>InStr

**Leírás:**  
A InStr függvény nem talál egy karakterlánc első előfordulásának egy karakterláncban

**Szintaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- stringcheck: karakterláncot szeretne keresni
- stringmatch: karakterlánc találhatók
- Indítsa el: keresse meg a karakterlánc pozíciótól kezdve
- összehasonlítása: vbTextCompare vagy vbBinaryCompare

**Megjegyzés:**  
Hol található a karakterlánc vagy 0, ha nem található pozícióját adja vissza.

**Példa:**  
`InStr("The quick brown fox","quick")`  
5-ös Evalues

`InStr("repEated","e",3,vbBinaryCompare)`  
Az eredmény 7

----------
### <a name="instrrev"></a>InStrRev

**Leírás:**  
A InStrRev függvény nem talál egy karakterlánc utolsó előfordulásának egy karakterláncban

**Szintaxis:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- stringcheck: karakterláncot szeretne keresni
- stringmatch: karakterlánc találhatók
- Indítsa el: keresse meg a karakterlánc pozíciótól kezdve
- összehasonlítása: vbTextCompare vagy vbBinaryCompare

**Megjegyzés:**  
Hol található a karakterlánc vagy 0, ha nem található pozícióját adja vissza.

**Példa:**  
`InStrRev("abbcdbbbef","bb")`  
7 adja eredményül.

----------
### <a name="isbitset"></a>IsBitSet

**Leírás:**  
A Ha jelző bit IsBitSet vizsgálatok függvény értéke-e

**Szintaxis:**  
`bool IsBitSet(num value, num flag)`

- érték: numerikus érték, akkor evaluated.flag: egy numerikus érték, amelynek a kiértékelendő bit

**Példa:**  
`IsBitSet(&HF,4)`  
Igaz értéket adja eredményül, mivel bit "4" "F" hexadecimális értékét be van állítva

----------
### <a name="isdate"></a>IsDate

**Leírás:**  
Ha a kifejezés lehet értékeli ki egy DateTime típusú, majd a IsDate függvény eredménye igaz.

**Szintaxis:**  
`bool IsDate(var Expression)`

**Megjegyzés:**  
Azt határozza meg, ha CDate() is lesznek.

----------
### <a name="isempty"></a>IsEmpty

**Leírás:**  
Ha az attribútum megtalálható a CS vagy té Elemet, de az eredmény üres karakterláncot, majd a IsEmpty függvény eredménye igaz.

**Szintaxis:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Leírás:**  
Ha a karakterlánc GUID konvertálhatók, majd a IsGuid függvény kiértékelt igaz.

**Szintaxis:**  
`bool IsGuid(str GUID)`

**Megjegyzés:**  
GUID követő ezek a minták közül karakterlánc definíciója: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Azt határozza meg, ha CGuid() is lesznek.

**Példa:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
A StrAttribute formátuma globálisan egyedi azonosítója, ha egy bináris ábrázolás vissza, egyéb esetben lépjen vissza a Null.

----------
### <a name="isnull"></a>IsNull

**Leírás:**  
Ha a kifejezés értéke Null értékre, majd a IsNull függvény értéke igaz.

**Szintaxis:**  
`bool IsNull(var Expression)`

**Megjegyzés:**  
A tulajdonság egy Null az attribútum hiányában van megadva.

**Példa:**  
`IsNull([displayName])`  
Eredménye IGAZ, ha az attribútum nem szerepel a CS vagy té Elemet.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Leírás:**  
Ha a kifejezés üres vagy üres karakterláncot, majd a IsNullOrEmpty függvény értéke igaz.

**Szintaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Megjegyzés:**  
A tulajdonság ez volna igaz értéket, ha az attribútum hiányzik, vagy nem tartalmaz adatokat, de egy üres karakterlánc.  
Ez a függvény inverze IsPresent neve.

**Példa:**  
`IsNullOrEmpty([displayName])`  
Eredménye IGAZ, ha az attribútum nem szerepel a vagy üres karakterláncot a CS vagy té Elemet.

----------
### <a name="isnumeric"></a>IsNumeric

**Leírás:**  
A IsNumeric függvény jelezve, hogy egy kifejezés kiértékelhető-szám típus logikai értéket adja eredményül.

**Szintaxis:**  
`bool IsNumeric(var Expression)`

**Megjegyzés:**  
Azt határozza meg, ha a kifejezés elemzésére sikeres lehet-e a CNum().

----------
### <a name="isstring"></a>IsString

**Leírás:**  
Ha a kifejezés kiértékelhető egy karakterlánc típusú, majd a IsString függvény ad vissza IGAZ értéket.

**Szintaxis:**  
`bool IsString(var expression)`

**Megjegyzés:**  
Azt határozza meg, ha a kifejezés elemzésére sikeres lehet-e a CStr().

----------
### <a name="ispresent"></a>IsPresent

**Leírás:**  
Ha a kifejezés nem üres és nem üres karakterláncot, majd a IsPresent függvény értéke igaz.

**Szintaxis:**  
`bool IsPresent(var expression)`

**Megjegyzés:**  
Ez a függvény inverze IsNullOrEmpty neve.

**Példa:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Elem

**Leírás:**  
Az elem függvény egy elemet egy többértékű karakterlánc attribútum adja eredményül.

**Szintaxis:**  
`var Item(mvstr attribute, num index)`

- attribútum: többértékű attribútum
- index: index a többértékű karakterláncban ponthoz.

**Megjegyzés:**  
Az elem függvény akkor hasznos, együtt a Contains függvény, mivel ez utóbbi függvény a tárgymutató-a többértékű attribútum elemre.

Hibát okoz, ha az index tartományán kívül esik.

**Példa:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Adja vissza az elsődleges e-mail címét.

----------
### <a name="itemornull"></a>ItemOrNull

**Leírás:**  
A ItemOrNull függvény egy elemet egy többértékű karakterlánc attribútum adja eredményül.

**Szintaxis:**  
`var ItemOrNull(mvstr attribute, num index)`

- attribútum: többértékű attribútum
- index: index a többértékű karakterláncban ponthoz.

**Megjegyzés:**  
A ItemOrNull funkció akkor hasznos, együtt a Contains függvény, mivel ez utóbbi függvény a tárgymutató-a többértékű attribútum elemre.

Ha index kívül esik, adja vissza egy Null érték.

----------
### <a name="join"></a>Csatlakozás

**Leírás:**  
A Join függvény többértékű karakterlánc tart, és egyetlen értékű karakterláncot ad eredményül a megadott elválasztó az egyes elemek közötti beszúrt.

**Szintaxis:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- attribútum: többértékű attribútum tartalmazó karakterláncok tartományhoz.
- elválasztó: minden olyan karakterlánc, a visszaadott karakterlánc az összefűzendő karakterláncokat elválasztására szolgáló. Ha nincs megadva, a szóközzel ("") használják. Ha a határoló nulla hosszúságú karakterlánc ("") vagy semmi, a lista összes eleme kezdődik vannak, utána pedig nincs határoló.

**Megjegyzések:**  
A csatlakozás és a felosztott függvény között van eltérés áll fenn. A Join függvény tömb karakterláncot, majd csatlakozik, őket az elválasztó karakterlánc, egy egyetlen karakterlánc jeleníthető meg. A felosztott függvény karakterlánc tart, és elválasztja a az elválasztó való visszatéréshez karakterláncok tömbje a. A fő különbséget azonban, hogy az illesztés is összefűzés bármely elválasztó karakterlánccal, osztott csak elválaszthatja egymástól elválasztó karakter használata karakterláncok.

**Példa:**  
`Join([proxyAddresses],",")`  
Vissza lehet:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Leírás:**  
A LCase függvény minden karaktereinek kisbetű konvertál.

**Szintaxis:**  
`str LCase(str value)`

**Példa:**  
`LCase("TeSt")`  
A függvény eredménye "próba".

----------
### <a name="left"></a>Balra

**Leírás:**  
A Left függvény bal oldalán található karakterlánc megadott számú karaktert adja eredményül.

**Szintaxis:**  
`str Left(str string, num NumChars)`

- karakterlánc: a karakterláncot a karakterek eredményül adása
- NumChars: egy szám megadott karakterszám számítva karakterlánc elején (balra)

**Megjegyzés:**  
Karakterlánc első karaktere numChars tartalmazó karakterlánc:

- Ha numChars = 0, üres karakterlánc.
- Ha numChars < 0, beviteli karakterláncot ad vissza.
- Ha üres karakterláncot, lépjen vissza az üres karakterlánc.

Ha a szám a megadott numChars-nál kevesebb karaktereket tartalmaz, egy karakterlánc (amely, 1 paraméter az összes karakter) azonos karakterláncot ad vissza.

**Példa:**  
`Left("John Doe", 3)`  
Eredménye "Joh".

----------
### <a name="len"></a>Hossz

**Leírás:**  
A hossz függvény egy karakterlánc lévő karakterek számát adja eredményül.

**Szintaxis:**  
`num Len(str value)`

**Példa:**  
`Len("John Doe")`  
8 adja eredményül.

----------
### <a name="ltrim"></a>Az LTrim

**Leírás:**  
Az LTrim függvény eltávolítja a karakterlánc kezdő szóköz.

**Szintaxis:**  
`str LTrim(str value)`

**Példa:**  
`LTrim(" Test ")`  
Eredménye "próba"

----------
### <a name="mid"></a>Közép

**Leírás:**  
A közép függvény a megadott pozícióban, a karakterlánc megadott számú karaktert adja eredményül.

**Szintaxis:**  
`str Mid(str string, num start, num NumChars)`

- karakterlánc: a karakterláncot a karakterek eredményül adása
- Indítsa el: azonosítása, a kezdő szám elhelyezése karakterláncot, a karakterek eredményül adása
- NumChars: egy számot a karakterek száma azonosító karakterláncban pozíciójában ad eredményül.

**Megjegyzés:**  
Karakterláncban indítsa el a feladó numChars karakterek pozíciótól kezdve.  
Pozíció kezdő karakterláncban numChars karaktert tartalmazó karakterlánc:

- Ha numChars = 0, üres karakterlánc.
- Ha numChars < 0, beviteli karakterláncot ad vissza.
- Ha a start > karakterlánc hossza visszatérési bemeneti karakterlánc.
- Ha indítása < = 0, a bemeneti karakterlánc.
- Ha üres karakterláncot, lépjen vissza az üres karakterlánc.

Ha nincs numChar karakter a karakterlánc elejétől pozíció hátralévő, a lehető legtöbb karaktert ad vissza.

**Példa:**  
`Mid("John Doe", 3, 5)`  
Eredménye "hn Do".

`Mid("John Doe", 6, 999)`  
"Jakab" függvény

----------
### <a name="now"></a>Most

**Leírás:**  
A Now függvény egy DateTime megadása az aktuális dátum és idő aszerint, hogy a számítógép dátumot és időt adja vissza.

**Szintaxis:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Leírás:**  
A NumFromDate függvény dátumformátum AD meg egy dátumot adja vissza.

**Szintaxis:**  
`num NumFromDate(dt value)`

**Példa:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
129699324000000000 adja eredményül.

----------
### <a name="padleft"></a>PadLeft

**Leírás:**  
A PadLeft függvény bal – csillapítók a megadott tartalomelrendezésre karaktert használni egy megadott hosszúságú karakterlánc.

**Szintaxis:**  
`str PadLeft(str string, num length, str padCharacter)`

- karakterlánc: a karakterlánc szegélynél.
- hossza: egy egész szám, amely a kívánt karakterlánc hossza.
- padCharacter: a billentyűzet karakterként használni egy karaktert tartalmazó karakterlánc

**Megjegyzés:**

- Ha a karakterlánc hossza kisebb, mint a hossz, majd többször padCharacter van ellátva (balra) karakterlánc mindaddig, amíg egy hossza hossza egyenlő elejéig.
- PadCharacter egy szóközt, de nem lehet üres érték.
- Ha karakterlánc hossza egyenlő vagy nagyobb, mint a hossz, karakterláncot ad vissza nem változik.
- Karakterlánc hossza a nagyobb vagy egyenlő hossza, ha egy karakterlánc azonos karakterláncot ad vissza.
- Ha a karakterlánc hossza kisebb, mint a hossz, majd a kívánt hosszúságú karakterlánc új értéket adja vissza tartalmazó karakterlánc, egy padCharacter konvertálása
- Ha üres karakterláncot, a függvény üres karakterláncot ad vissza.

**Példa:**  
`PadLeft("User", 10, "0")`  
A függvény eredménye "000000User".

----------
### <a name="padright"></a>PadRight

**Leírás:**  
A PadRight függvény jobb-csillapítók a megadott tartalomelrendezésre karaktert használni egy megadott hosszúságú karakterlánc.

**Szintaxis:**  
`str PadRight(str string, num length, str padCharacter)`

- karakterlánc: a karakterlánc szegélynél.
- hossza: egy egész szám, amely a kívánt karakterlánc hossza.
- padCharacter: a billentyűzet karakterként használni egy karaktert tartalmazó karakterlánc

**Megjegyzés:**

- Ha a karakterlánc hossza kisebb, mint a hossz, majd padCharacter többször hozzáfűzi karakterlánc (jobbra) vége mindaddig, amíg egy hosszúságú hossza rendelkezik.
- padCharacter egy szóközt, de nem lehet üres érték.
- Ha karakterlánc hossza egyenlő vagy nagyobb, mint a hossz, karakterláncot ad vissza nem változik.
- Karakterlánc hossza a nagyobb vagy egyenlő hossza, ha egy karakterlánc azonos karakterláncot ad vissza.
- Ha a karakterlánc hossza kisebb, mint a hossz, majd a kívánt hosszúságú karakterlánc új értéket adja vissza tartalmazó karakterlánc, egy padCharacter konvertálása
- Ha üres karakterláncot, a függvény üres karakterláncot ad vissza.

**Példa:**  
`PadRight("User", 10, "0")`  
Eredménye "User000000".

----------
### <a name="pcase"></a>PCase

**Leírás:**  
A PCase függvény minden szóközzel tagolt szó egy karakterlánc első karakterének alakítja a nagybetűs, és minden karaktert kisbetű alakulnak.

**Szintaxis:**  
`String PCase(string)`

**Megjegyzés:**

- Ez a funkció jelenleg nem nyújt megfelelő géphez szeretne alakítani, például egy rövidítést teljesen nagybetűs szó.

**Példa:**  
`PCase("TEsT")`  
A függvény eredménye "Próba".

`PCase(LCase("TEST"))`  
Eredménye "próba"

----------
### <a name="randomnum"></a>RandomNum

**Leírás:**  
A RandomNum függvény eső véletlen számot egy adott intervallum között.

**Szintaxis:**  
`num RandomNum(num start, num end)`

- Indítsa el: készítése az alsó határ véletlen érték azonosítása egy szám
- Záró: egy szám, a felső határ véletlen érték azonosítása készítése

**Példa:**  
`Random(100,999)`  
734 térhet vissza.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Leírás:**  
A RemoveDuplicates függvény többértékű karakterlánc tart, és ellenőrizze, hogy az egyes értékek egyedi.

**Szintaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Példa:**  
`RemoveDuplicates([proxyAddresses])`  
Ha el lett távolítva az összes ismétlődő értékek egy letisztított proxyAddress attribútum adja eredményül.

----------
### <a name="replace"></a>Csere

**Leírás:**  
A Replace függvény egy karakterlánc, egy másik karakterlánc összes előfordulását váltja fel.

**Szintaxis:**  
`str Replace(str string, str OldValue, str NewValue)`

- karakterlánc: az értékek lecserélése karakterláncot.
- OldValue: Szöveg keresése és cseréje.
- Új érték: Helyettesíteni kívánt karakterláncot.

**Megjegyzés:**  
A függvény a következő speciális kézjegyek felismer:

- \n – új sor
- \r – kocsivissza
- \t – lap

**Példa:**  
`Replace([address],"\r\n",", ")`  
CRLF lecseréli egy vesszővel és egy szóközt, és a "Egy Microsoft módon, Redmond, Cs, USA" vezethet

----------
### <a name="replacechars"></a>ReplaceChars

**Leírás:**  
A ReplaceChars függvény minden előfordulását ReplacePattern karakterláncban található karakterek váltja fel.

**Szintaxis:**  
`str ReplaceChars(str string, str ReplacePattern)`

- karakterlánc: kicserélendő karakterek karakterlánc.
- ReplacePattern: karakterek cseréje a szótár tartalmazó karakterlánc.

{Source1} formátuma: {target1}, {source2}: {target2}, {sourceN}, {targetN} forrás esetén keresse meg és jelölje ki a karakterláncot cserélje ki a karaktert.

**Megjegyzés:**

- A függvény minden előfordulást definiált források tart, és cseréli őket a cél.
- A forrás pontosan egy (unicode) karakterrel kell lennie.
- A forrás lehet üres vagy hosszabb, mint egy karakterrel (feldolgozási hiba).
- A target beállíthatja, hogy több karakter, például ö:oe, β:ss.
- Lehet, hogy a célhely üres, jelezve, hogy el kell távolítani a karaktert.
- A forrás kis-és nagybetűk, és a pontos egyezést kell lennie.
- (Vessző) és: (kettőspont) fenntartott karakter, és nem ezzel a függvénnyel kell cserélni.
- Szóközt és más fehér karaktereket a ReplacePattern karakterláncban figyelmen kívül hagyja.

**Példa:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Raksmorgas adja eredményül.

`ReplaceChars("O’Neil",%ReplaceString%)`  
Eredménye "ONeil", az egyetlen osztásjelek eltávolítjuk van megadva.

----------
### <a name="right"></a>Jobbra

**Leírás:**  
Right függvény jobbra (Befejezés) szerepel egy karakterlánc megadott számú karaktert adja eredményül.

**Szintaxis:**  
`str Right(str string, num NumChars)`

- karakterlánc: a karakterláncot a karakterek eredményül adása
- NumChars: egy szám megadott karakterszám számítva karakterlánc (jobbra) vége

**Megjegyzés:**  
NumChars karakterek a visszaadott karakterlánc utolsó pozíciójában.

Utolsó numChars karaktereinek tartalmazó karakterlánc:

- Ha numChars = 0, üres karakterlánc.
- Ha numChars < 0, beviteli karakterláncot ad vissza.
- Ha üres karakterláncot, lépjen vissza az üres karakterlánc.

Ha a szám a megadott NumChars-nál kevesebb karaktereket tartalmaz, egy karakterlánc azonos karakterláncot ad vissza.

**Példa:**  
`Right("John Doe", 3)`  
Eredménye "Jakab".

----------
### <a name="rtrim"></a>RTrim

**Leírás:**  
A RTrim függvény záró szóközök eltávolítása egy karakterlánc.

**Szintaxis:**  
`str RTrim(str value)`

**Példa:**  
`RTrim(" Test ")`  
A függvény eredménye "Vizsgálat".

----------
### <a name="split"></a>A felosztott

**Leírás:**  
A felosztott függvény elválasztó elválasztva karakterlánc tart, így a többértékű karakterlánc.

**Szintaxis:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- érték: a karakterlánc a el egymástól elválasztó karaktert.
- elválasztó: egyetlen helyőrzőként az elválasztó karakter.
- határérték: is eredményező értékek maximális száma.

**Példa:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
A 2 elemek többértékű karakterláncot ad vissza attribútum proxyAddress esetében hasznos lehet.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Leírás:**  
A StringFromGuid függvény megnyitja a bináris globálisan egyedi azonosítója, és a karakterlánc alakít át.

**Szintaxis:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Leírás:**  
A StringFromSid függvény egy karakterlánc biztonsági azonosítót tartalmazó bájt tömb alakítja át.

**Szintaxis:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Váltás

**Leírás:**  
A Váltás függvény segítségével kiértékelt körülmények egyetlen értéket adnak vissza.

**Szintaxis:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- kifejezés: kiértékelni kívánt Variant típusú kifejezés.
- érték: a megfelelő kifejezés igaz értéke esetén eredményül adott érték.

**Megjegyzés:**  
A Váltás függvény argumentum listában, ahol a kifejezések és az érték párokká. A kifejezés kiértékelése balról jobbra, és a társított első kifejezés kiértékelésének eredménye IGAZ értéket ad vissza. Ha a kijelzők nem megfelelően párosított, egy futási idejű hiba történik.

Ha például Kif1 értéke igaz, ha kapcsoló érték1 argumentumot adja vissza. Kifejezés-1 értéke HAMIS, de a kifejezés-2 értéke igaz, ha kapcsoló adja eredményül, érték-2, és így tovább.

Váltás ad vissza egy semmi ha:

- A kifejezések egyike sem, az igaz.
- Az első igaz kifejezést, amely a NULL megfelelő értéket tartalmaz

Váltás az eredmény minden kifejezés, annak ellenére, hogy csak az egyik adja eredményül. Emiatt a nemkívánatos hatásai kell nézni. Ha például bármely kifejezés kiértékelésétől eredményez val való osztási hibát, ha hiba történik.

Egyéni karakterláncként adja vissza hibafüggvény értéke is lehet.

**Példa:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
A nyelv néhány főbb városai beszélt adja eredményül, egyébként a függvény hibát jelez.

----------
### <a name="trim"></a>Vágása

**Leírás:**  
A Kimetsz függvény eltávolítja a kezdő és záró szóközök karaktereit adja vissza.

**Szintaxis:**  
`str Trim(str value)`  

**Példa:**  
`Trim(" Test ")`  
A függvény eredménye "Próba".

`Trim([proxyAddresses])`  
Eltávolítja a kezdő és záró szóközöket a proxyAddress attribútumot minden értékére.

----------
### <a name="ucase"></a>UCase

**Leírás:**  
Az UCase függvény összes karaktereinek alakítja a nagybetűs.

**Szintaxis:**  
`str UCase(str string)`

**Példa:**  
`UCase("TeSt")`  
A függvény eredménye "Próba".

----------
### <a name="word"></a>A Word

**Leírás:**  
A Word függvény egy megadott paramétereket és a word szám csonkolásával adja vissza az határolójel leíró található szó.

**Szintaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

- karakterlánc: a karakterlánc való visszatéréshez a Word programban.
- WordNumber: egy szám, mely szó számot azonosítására térjen vissza.
- határoló: olyan karakterlánc, amely a delimiter(s), amellyel azonosítani a szavakat

**Megjegyzés:**  
Mindegyik szöveges karakterláncot elválasztva a határoló karakter a karakterlánc karaktereinek azonosítja szavak:

- Ha < 1-es számú, a függvény üres karakterláncot.
- Ha üres karakterláncot, üres karakterláncot ad vissza.

Ha a karakterlánc kisebb, mint a szavak száma tartalmaz, vagy a karakterlánc nem tartalmaz bármelyik határolójel jelölt szót, üres karakterláncot ad vissza.

**Példa:**  
`Word("The quick brown fox",3," ")`  
Eredménye "Katalin"

`Word("This,string!has&many separators",3,",!&#")`  
"Van" adja vissza

## <a name="additional-resources"></a>További források

* [Deklaráció kiépítési kifejezések ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure Active Directory csatlakozás szinkronizálása: A szinkronizálás beállításainak testreszabása](active-directory-aadconnectsync-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
