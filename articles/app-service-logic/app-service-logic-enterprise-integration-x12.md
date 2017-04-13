<properties 
    pageTitle="X12 és a nagyvállalati integrációs csomag áttekintése |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja a X12 rendelkezést logika alkalmazások létrehozása" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>X12 vállalati integrációja 

>[AZURE.NOTE]A lap magában foglalja a X12 szolgáltatások logika alkalmazások. További információ a EDIFACT [Itt](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Hozzon létre egy X12 szerződés 
Mielőtt akkor kicserélheti X12 üzeneteket, létre kell hoznia egy X12 szerződés és tárolja a integrációs-fiókjában. Az alábbi lépésekkel végigvezeti egy X12 létrehozásának folyamata a szerződést.

### <a name="heres-what-you-need-before-you-get-started"></a>Mire van szükség megkezdése előtt
- Az Azure-előfizetése definiált [integrációs fiók](./app-service-logic-enterprise-integration-accounts.md)  
- Integráció fiókban már definiált legalább két [partnerek](./app-service-logic-enterprise-integration-partners.md)  

>[AZURE.NOTE]Olyan megállapodást létrehozásakor a szerződés fájl tartalmának egyeznie kell a szerződés típusát.    


Ha már [létrehozott integrációs fiók](./app-service-logic-enterprise-integration-accounts.md) , és [megjelenik a partnerek](./app-service-logic-enterprise-integration-partners.md), vagy létrehozhat egy X12 megállapodás az alábbi lépésekkel:  

### <a name="from-the-azure-portal-home-page"></a>Az Azure portál Kezdőlap lapján

Miután bejelentkezett az [Azure portál](http://portal.azure.com "Azure portál")be:  
1. A bal oldalon a menüből válassza a **Tallózás gombra** .  

>[AZURE.TIP]Ha nem látja a **tallózással keresse meg** hivatkozást, előfordulhat, bontsa ki előbb a menüt. Ehhez a **menü megjelenítése** hivatkozásra kattint, amely a található összecsukott menü bal felső.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integráció* a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** a találatok listájában.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. A **Fiókok integráció** a lap fel a megnyíló jelölje ki a integrációs-fiókot, amelyben a szerződést hoz létre. Ha nem látja a bármilyen integrációt fiókok listák [Hozzon létre egy újat első](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Jelölje ki a **rendelkezést** csempére. Ha nem látja a rendelkezést csempére, először felvennie azt.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. A **Hozzáadás** gombra, amely megnyitja az rendelkezést lap kijelölése  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Írja be egy **nevet** a szerződés, majd válassza ki a **szerződés típusa**, a **Fogadó Partner**, a **Host identitás**, a **Vendég Partner**, a **Vendég identitás**a megnyíló rendelkezést lap.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Miután beállította a fogadási beállítások tulajdonságai, jelölje ki az **OK** gomb  
Vegyük továbbra is:  
8. Kattintson a **Fogadási beállítások** konfigurálása hogyan ezt a szerződést keresztül a Beérkezett üzenetek kezelésének történik.  
9. A fogadási beállítások beállítás meg van osztva az alábbi szakaszok, beleértve a azonosítók, nyugtát, sémák, borítékok, vezérlő számok, ellenőrzések és belső beállítások. Állítsa be ezeket a szerződés partnerrel fog üzenetek cseréje alapján tulajdonságait. Az alábbiakban ezeket a vezérlőket nézetének, állítsa be őket, hogy miként kívánja azonosításához és kezelje a bejövő üzenetek a jelen szerződés alapján:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Az **OK** gombra a beállítások mentéséhez.  

### <a name="identifiers"></a>Azonosítók

|A tulajdonság|Leírás |
|---|---|
|ISA1 (engedélyezés minősítő)|Válassza az engedélyezés minősítő értéket a legördülő listából.|
|ISA2|Nem kötelező. Írjon be engedélyt információk értéket. Ha az érték ISA1 megadott eltérő 00, adja meg a legalább egy alfanumerikus karaktert és legfeljebb 10.|
|ISA3 (biztonsági minősítő)|Válassza a biztonság minősítő értéket a legördülő listából.|
|ISA4|Nem kötelező. Adja meg a biztonsági információk értékét. Ha az érték ISA3 megadott eltérő 00, adja meg a legalább egy alfanumerikus karaktert és legfeljebb 10.|

### <a name="acknowledgments"></a>Visszaigazolások 

|A tulajdonság|Leírás |
|----|----|
|Várható TA1|Jelölje be ezt a jelölőnégyzetet, való visszatéréshez egy technikai (TA1) visszajelzés adatcsere feladójának. A visszaigazolások elküldi a interchange feladó, a szerződés küldése beállításainak alapján.|
|Várható be|Jelölje be ezt a jelölőnégyzetet, való visszatéréshez egy funkcionális (be) visszajelzés adatcsere feladójának. Adja meg, hogy azt szeretné, hogy a 997 vagy 999 nyugták, a séma-verziók használata alapján. A visszaigazolások elküldi a interchange feladó, a szerződés küldése beállításainak alapján.|
|Olyan AK2/IK2 ciklus|Jelölje be ezt az elfogadott tranzakció eredménye a funkcionális visszaigazolások AK2 hurkok létrehozásának engedélyezése jelölőnégyzetet. Megjegyzés: Ez a jelölőnégyzet engedélyezett csak akkor, ha a kijelölt a várt módon be jelölőnégyzet.|

### <a name="schemas"></a>A sémák

Válassza ki a séma minden egyes tranzakció típusát (ST1) és a feladó alkalmazás (GS2). A fogadási folyamat visszafejti a bejövő üzenet, az egyező értékeket ST1 és GS2 a bejövő üzenet Itt adhatja meg az értékeket és a séma használata a bejövő üzenet a séma Itt adhatja meg.

|A tulajdonság|Leírás |
|----|----|
|Verzió|Jelölje ki a X12 verziója|
|Tranzakció típusát (ST01)|Válassza ki a tranzakció|
|Feladó alkalmazás (GS02)|Jelölje ki a feladó alkalmazást|
|Séma|Jelölje ki a kívánt kapcsolatfelvételi sémafájl. Sémafájlok integrációs fiókjában található.|

### <a name="envelopes"></a>Borítékok

|A tulajdonság|Leírás |
|----|----|
|ISA11 használatát|Ez a mező segítségével adja meg az elválasztó tranzakció meg:</br></br>Jelölje be a szabványos azonosító használni a tizedes tört formátumban, a "." a decimális jelölés a szerkesztése a bejövő dokumentum helyett folyamat kap.</br></br>Jelölje ki az ismétlődés elválasztó megadhatja az egyszerű adatelem vagy ismételt adatszerkezet ismételt előfordulásainak elválasztó karaktert. (^) Például általában szolgál az ismétlődés elválasztó karaktert. A sémák HIPAA (^) csak használhatja.|

### <a name="control-numbers"></a>Vezérlőelem számok

|A tulajdonság|Leírás |
|----|----|
|Ellenőrző szám Interchange ismétlődések letiltása|Jelölje be ezt a lehetőséget ismétlődő csomópontokat is blokkolni. Ha ki van jelölve, a BizTalk Services portál ellenőrzi, hogy a kapott interchange adatcsere-ellenőrző szám (ISA13) nem egyezik meg a interchange ellenőrző szám. Egyező lép fel, ha a fogadási folyamat nem dolgozza fel a interchange.<br/>Ha azt választotta, ismétlődő interchange vezérlő számok tiltotta le, majd megadhatja, amelynél az ellenőrzés végzi a ismétlődő ISA13 ellenőrzése a megfelelő érték megadásával x naponta napok számát.|
|Csoport vezérlőelem szám ismétlődések letiltása|Ismétlődő csoport vezérlőelem számokkal csomópontokat is blokkolása ezt a jelölőnégyzetet.|
|Transaction beállítása vezérlő szám ismétlődések letiltása|Jelölje be ezt a lehetőséget ismétlődő tranzakció beállítása vezérlő számokkal csomópontokat is blokkolni.|

### <a name="validations"></a>Adatérvényesítés

|A tulajdonság|Leírás |
|----|----|
|Üzenet típusa|Például 850 megrendelés vagy 999-végrehajtási visszaigazoló üzenet szerkesztése típusa.|
|Adatérvényesítési szerkesztése|Hajt végre szerkesztése érvényesítése a séma, hossz korlátozások, üres adatok elemek és záró elválasztójelek tulajdonságainak szerkesztése által meghatározott adattípusokkal kapcsolatos.|
|Bővített érvényesítése|Ha adattípusát nem szerkesztése, az érvényesítési adatok elem kötelező van, és az ismétlődés felsorolások és elem hossza adatérvényesítés (min és max) engedélyezett.|
|Vezető/sorzáró nullákkal engedélyezése|További területet és nulla karakter, bevezető vagy záró megőrződnek. Nem törlődnek.|
|Záró elválasztót házirendet|Hozza létre a kapott interchange a záró elválasztójelek. A választható lehetőségek NotAllowed, a Tudjanak róla és a kötelező.|

### <a name="internal-settings"></a>Belső beállításai

|A tulajdonság|Leírás |
|----|----|
|Hallgatólagos decimális formátum Nn alapjául szolgáló 10 numerikus érték konvertálása|A formázás Nn a 10-es numerikus értékre a BizTalk Services portál köztes XML-adatok a megadott szerkesztése számot konvertál.|
|Üres XML-címkéket létrehozni, ha a záró elválasztójelek engedélyezettek|Jelölje be ezt a jelölőnégyzetet, hogy a interchange feladó, a záró elválasztójelek üres XML-címkéket tartalmazza.|
|Bejövő eljárásnak feldolgozási|Osztott Interchange mint tranzakció beállítása – felfüggesztheti tranzakció készletek hiba: elemzi a megfelelő boríték alkalmazása tranzakció megadása egy külön XML-dokumentumokba interchange meg tranzakciók. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd BizTalk szolgáltatások felfüggeszti a csak adott tranzakció készleteket. </br></br>Felosztott Interchange mint tranzakció beállítása – felfüggesztheti interchange hiba: elemzi a megfelelő boríték alkalmazásával egy külön XML-dokumentumokba interchange beállítása tranzakciók. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd BizTalk szolgáltatások felfüggeszti a teljes interchange.</br></br>Adatcsere megőrzése - tranzakció készletek felfüggesztése hiba: érintetlenül adatcsere, a teljes kötegelt adatcserét XML-dokumentum létrehozása. Ezt a beállítást választja Ha onAe vagy több tranzakció állítja be a interchange sikertelen érvényesítés, majd BizTalk szolgáltatások felfüggeszti a csak adott tranzakció készleteket, miközben továbbra minden tranzakció készletben feldolgozása.</br></br>Adatcsere megőrzése – felfüggesztheti interchange hiba: érintetlenül adatcsere, a teljes kötegelt adatcserét XML-dokumentum létrehozása. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd BizTalk szolgáltatások felfüggeszti a teljes interchange.</br></br>|

A szerződés áll készen kezelje a bejövő üzeneteket, amelyek megfelelnek a a kijelölt sémában.

A partnerek küldött üzenetek kezelésére beállítások konfigurálása:  
11. Jelölje ki **Küldési beállítások** konfigurálása a hogyan ezt a szerződést keresztül küldött üzenetek kezelésének vannak.  

A küldési beállítások vezérlő meg van osztva a következőkben azonosítóját, a visszaigazoló, a sémák, a borítékok, a vezérlő számok, a karakterkészlet és a elválasztójelek és a érvényességi. 

Az alábbiakban ezeket a vezérlőket nézetének. Adja meg a beállításokat, hogyan szeretné kezelni a jelen szerződés partnerektől üzenetekhez alapján:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Az **OK** gombra a beállítások mentéséhez.  

### <a name="identifiers"></a>Azonosítók
|A tulajdonság|Leírás |
|----|----|
|Engedély minősítő (ISA1)|Válassza az engedélyezés minősítő értéket a legördülő listából.|
|ISA2|Írjon be engedélyt információk értéket. Ha ez az érték nem 00, majd adja meg, legalább egy alfanumerikus karaktert és legfeljebb 10.|
|Biztonsági minősítő (ISA3)|Válassza a biztonság minősítő értéket a legördülő listából.|
|ISA4|Adja meg a biztonsági információk értékét. Ha az érték nem 00 érték (ISA4) mezőbe írja be egy alfanumerikus értéket a minimum és legfeljebb 10.|

### <a name="acknowledgment"></a>Nyugta
|A tulajdonság|Leírás |
|----|----|
|Várható TA1|Jelölje be ezt a jelölőnégyzetet, való visszatéréshez egy technikai (TA1) visszajelzés adatcsere feladójának. Ezzel a beállítással megadhatja, hogy a host partnert, aki az üzenetet küld a a szerződés Vendég partnertől visszaigazolást kéri. A visszaigazolások által a fogadó partner a fogadási beállítások a megállapodás alapján várható.|
|Várható be|Jelölje be ezt a jelölőnégyzetet a funkcionális (be) nyugta visszatérhet a interchange feladó, és válassza a 997 vagy 999 nyugták, a séma-verziók használata alapján kíván-e. A host partner a fogadási beállítások a megállapodás alapján várható ezek visszaigazolások.|
|BE verziója|Jelölje ki a be verziót|

### <a name="schemas"></a>A sémák
|A tulajdonság|Leírás |
|----|----|
|Verzió|Jelölje ki a X12 verziója|
|Tranzakció típusát (ST01)|Válassza ki a tranzakció|
|SÉMA|Jelölje ki a séma használata. A sémák integrációs fiókban találhatók. Elérésére, hogy a sémák, először hivatkozás integrációs fiókját az logika alkalmazást.|

### <a name="envelopes"></a>Borítékok
|A tulajdonság|Leírás |
|----|----|
|ISA11 használatát|Ez a mező segítségével adja meg az elválasztó tranzakció meg:</br></br>Jelölje be a szabványos azonosító használni a tizedes tört formátumban, a "." a decimális jelölés a szerkesztése a bejövő dokumentum helyett folyamat kap.</br></br>Jelölje ki az ismétlődés elválasztó megadhatja az egyszerű adatelem vagy ismételt adatszerkezet ismételt előfordulásainak elválasztó karaktert. (^) Például általában szolgál az ismétlődés elválasztó karaktert. A sémák HIPAA (^) csak használhatja.</br>|
|Az ismétlődés elválasztó|Adja meg az ismétlődés elválasztó|
|Vezérlőelem verziószámmal (ISA12)|Jelölje ki a szokásos X12 egy kimenő interchange létrehozása a BizTalk szolgáltatások portálja által használt verzióját.|
|Használati mutató (ISA15)|Adja meg, hogy egy interchange környezetében információk (I), gyártás-adatok (P), vagy (T) adatok vizsgálata. A szerkesztése fogadási folyamat ajánl ezt a tulajdonságot a környezetben.|
|Séma|Hogyan a a BizTalk Services portál létrehoz egy X12 kódolva, a Küldés folyamat küld adatcserét a levelek és ST szegmensek adhat meg.</br></br>A GS1, GS2, GS3, GS4, GS5, GS7 és GS8 adatok összetevők tranzakció típusúak értékekkel és verzió/kiadási adatok elemek értékének társíthat. A BizTalk Services portál határozza meg, amikor egy XML-üzenet a tranzakció típusát, és a rács a sorokban lévő verziót/megjelenés elemek értékének van, majd azt feltölti a borítékot az értékeket az ugyanabban a sorban a rács adatcsere kimenő GS1, GS2, GS3, GS4, GS5, GS7 és GS8 adatok elemét. Az értékeket a tranzakció típusát, és verzió/megjelenés elemek egyedinek kell lennie.</br></br>Nem kötelező. GS1 jelölje ki a funkcionális kód egy értéket a legördülő listából.</br></br>Szükséges. Írja be egy alfanumerikus érték GS2, az alkalmazás feladó egy legalább két karakter és legfeljebb 15 karaktert.</br></br>Szükséges. Írja be egy alfanumerikus érték GS3, az alkalmazás címzett egy legalább két karakter és legfeljebb 15 karaktert.</br></br>Nem kötelező. GS4 jelölje be a CCYYMMDD vagy ééhhnn.</br></br>Nem kötelező. GS5 jelölje be a HHMM, ÓÓPPMM vagy HHMMSSdd.</br></br>Nem kötelező. GS7 jelölje ki a felelős hivatal egy értéket a legördülő listából.</br></br>Nem kötelező. A GS8 írja be a dokumentum egy karakterrel minimum, maximum 12 karakter derült egy alfanumerikus érték.</br></br>**Megjegyzés**: ezeket az értékeket, amely a BizTalk Services portál beírja a GS mezők adatcsere készíti, ha a tranzakció írja be, illetve az ugyanazon sor verzió/megjelenés elemek egyezés képletkészítőkhöz társítva az interchange.|

### <a name="control-numbers"></a>Vezérlőelem számok
|A tulajdonság|Leírás |
|----|----|
|Interchange ellenőrző szám (ISA13)|Szükséges. Írja be a interchange ellenőrző szám generálásához egy kimenő interchange BizTalk Services portál által használt tartományba. Írjon be egy számértéket legalább 1 és 999999999 legfeljebb.|
|Csoport vezérlőelem száma (GS06)|Szükséges. Írja be a számokat a BizTalk Services portál a csoport vezérlőelem számot kell használni a tartományt. Írja be a legalább egy karakterrel és a maximum kilenc karakter numerikus értéket.|
|Transaction vezérlő szám (ST02) beállítása|Tranzakció beállítása vezérlőelem szám (ST02) írja be a szükséges középső mezők numerikus értékből és alfanumerikus értékek egy cellatartomány a választható előtag és utótag. Mind a négy mezőt maximális hossza kilenc karakter.|
|Előtag|Jelölhet ki tranzakció beállítása vezérlő számok nyugtát használt tartományát, adja meg a ACK-ellenőrzési száma (ST02) mezők értékeit. Írja be a középső név kezdőbetűjét két mezőt egy numerikus értéket, és egy alfanumerikus érték (ha szükséges) a előtag és utótag mezőket. A középső név kezdőbetűjét mezők szükség, és a vezérlő száma; minimális és maximális értékeit tartalmazzák az előtag és utótag nem kötelező. Az összes három mezőt maximális hossza kilenc karakter.|
|Utótag|Jelölhet ki tranzakció beállítása vezérlő számok nyugtát használt tartományát, adja meg a ACK-ellenőrzési száma (ST02) mezők értékeit. Írja be a középső név kezdőbetűjét két mezőt egy numerikus értéket, és egy alfanumerikus érték (ha szükséges) a előtag és utótag mezőket. A középső név kezdőbetűjét mezők szükség, és a vezérlő száma; minimális és maximális értékeit tartalmazzák az előtag és utótag nem kötelező. Az összes három mezőt maximális hossza kilenc karakter.|

### <a name="character-sets-and-separators"></a>Karakter készletek és Elválasztókkal
Eltérő karakterkészlet, más beállítási minden üzenet típushoz használandó határoló adhat meg. Ha egy adott üzenet séma karakterkészlet nincs megadva, az alapértelmezett karakterkészlet használják.

|A tulajdonság|Leírás |
|----|----|
|Használandó karakterkészlet|Jelölje ki a X12 karakterkészletet érvényesítése, amelyet a szerződés tulajdonságait.</br></br>**Megjegyzés**: A BizTalk Services portál csak használja ezt a beállítást a kapcsolódó szerződés tulajdonságok értékek érvényesítéséhez. A fogadási folyamat vagy a Küldés folyamat figyelmen kívül hagyja a karakterkészlet-tulajdonság futási idejű feldolgozás végrehajtásakor.|
|Séma|Jelölje ki a (+) szimbólumot, és válassza a legördülő listából a séma. A kijelölt séma jelölje be a elválasztójelek használandó beállítása:</br></br>Összetevő elem elválasztó – Enter egyetlen karakter összetett adatelemeket elkülönítésére.</br></br>Adatok elem elválasztó – Enter egyetlen karakter külön egyszerű adatelemeket összetett adatelemeket belül.</br></br></br></br>Helyettesítő karakter – jelölje be ezt a jelölőnégyzetet, ha az adatok található tartalom karaktereket, adatok, szakasz vagy összetevő elválasztójelek is használhatók. Ezután adhat meg helyettesítő karaktert. A kimenő létrehozásakor X12 üzenet jelenik meg, az adatok helyére kerülnek a megadott karakter tartalom elválasztó karakter összes előfordulását.</br></br>Befejezés szakaszokhoz – adja meg, hogy egy szerkesztése szakasz végén egyetlen karakter.</br></br>Utótag – jelölje ki a karaktert, amely a szakasz azonosítóval. Utótag megadásakor a szakasz Befejezés adatelem üres lehet. Ha a szakasz Befejezés üres, majd ki kell jelölnie toldalékot.|

### <a name="validation"></a>Adatérvényesítési
|A tulajdonság|Leírás |
|----|----|
|Üzenet típusa|Ezt a lehetőséget választva engedélyezi a érvényesítése parancs az adatcsere címzettje. Az ellenőrzés szerkesztése érvényességi tranzakció által beállított adatok elemen, érvényesítése adattípusok, hossz korlátozások és üres adatok elemeket, és a záró elválasztójelek hajt végre.|
|Adatérvényesítési szerkesztése||
|Bővített érvényesítése|Ezt a lehetőséget választva lehetővé teszi, hogy a bővített ellenőrzése a interchange feladó kapott csomópontokat is. Ide tartoznak a érvényességi mezőhosszát, lehetőség és ismétlési számláló típusú adatérvényesítés XSD mellett. Bővítmény érvényességi szerkesztése érvényesítés engedélyezése nélkül engedélyezheti vagy fordítva.|
|Engedélyezi a kezdő és záró nullákkal|Ezt a lehetőséget választva adja meg, hogy egy kapott a féltől szerkesztése interchange nem sikertelen érvényességi, ha egy szerkesztése interchange adatelem nem felel meg a hosszát megkövetelésének miatt vagy záró szóközöket, de a hosszát megkövetelésének felel meg vannak eltávolításakor.|
|Záró elválasztó|Ezt a lehetőséget választva adja meg a kapott a féltől szerkesztése interchange sikertelen érvényességi, ha egy szerkesztése interchange adatelem nem felel meg a hosszát megkövetelésének kezdő (vagy záró) nullákkal vagy záró szóközöket, de a hosszát megkövetelésének felel meg vannak eltávolításakor.</br></br>Ha nem szeretné, hogy egy interchange feladótól kapott interchange záró határolójel és elválasztójelek engedélyezni, válassza a nem engedélyezett. Ha a interchange záró határolójel és elválasztójelek tartalmaz, akkor deklarálva érvénytelen.</br></br>Jelölje be a nem kötelező, fogadja el vagy záró határolójel és elválasztójelek anélkül csomópontokat is.</br></br>Jelölje be a kötelező, ha a kapott interchange tartalmaznia kell záró határolójel és elválasztójelek.|

Miután rögzítéséhez nyissa meg **az OK** gombra:  
13. Válassza a fiók integrációs lap a **rendelkezést** csempét, és jelennek meg az újonnan hozzáadott megállapodás szerepel a listában.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>tudj meg többet
- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag")  
