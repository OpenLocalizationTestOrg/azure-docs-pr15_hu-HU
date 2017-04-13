<properties 
    pageTitle="Enterprise-integráció a EDIFACT |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használata EDIFACT rendelkezést logika alkalmazások létrehozása" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>EDIFACT vállalati integrációja 

> [AZURE.NOTE] Ezen az oldalon logika alkalmazások EDIFACT funkciók foglalkozik. További információért X12 kattintson [ide](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Hozzon létre egy EDIFACT szerződés 
EDIFACT üzenetváltásra képes előtt kell hozzon létre egy EDIFACT szerződést, és tárolja azt a integrációs-fiókjában. Az alábbi lépésekkel végigvezeti EDIFACT megállapodást létrehozásának folyamata.

### <a name="heres-what-you-need-before-you-get-started"></a>Mire van szükség megkezdése előtt
- Az Azure-előfizetése definiált [integrációs fiók](./app-service-logic-enterprise-integration-accounts.md)  
- Integráció fiókban már definiált legalább két [partnerek](./app-service-logic-enterprise-integration-partners.md)  

>[AZURE.NOTE]Olyan megállapodást létrehozásakor a partner érkező és az üzenetek meg fog kapni küldése/tartalmának egyeznie kell a szerződés típusát.    


Miután, ha már [létrehozott integrációs fiók](./app-service-logic-enterprise-integration-accounts.md) , és [megjelenik a partnerek](./app-service-logic-enterprise-integration-partners.md), ezeket a lépéseket követve EDIFACT megállapodást hozhat létre:  

### <a name="from-the-azure-portal-home-page"></a>Az Azure portál Kezdőlap lapján

Miután bejelentkezett az [Azure portál](http://portal.azure.com "Azure portál")be:  
1. A bal oldalon a menüből válassza a **Tallózás gombra** .  

>[AZURE.TIP]Ha nem látja a **tallózással keresse meg** hivatkozást, előfordulhat, bontsa ki előbb a menüt. Ehhez a **menü megjelenítése** hivatkozásra kattint, amely a található összecsukott menü bal felső.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. *Integráció* a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** a találatok listájában.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. A **Fiókok integráció** a lap fel a megnyíló jelölje ki a integrációs-fiókot, amelyben a szerződést hoz létre. Ha nem látja a bármilyen integrációt fiókok listák [Hozzon létre egy újat első](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Jelölje ki a **rendelkezést** csempére. Ha nem látja a rendelkezést csempére, először felvennie azt.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. A **Hozzáadás** gombra, amely megnyitja az rendelkezést lap kijelölése  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Írja be egy **nevet** a szerződés, majd válassza ki a **Szerződés típus** EDIFACT **Fogadó Partner**, **Host identitás**, **Vendégként való bekapcsolódáshoz Partner**, **Vendégként való bekapcsolódáshoz identitás**a megnyíló rendelkezést lap.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Miután beállította a szerződés tulajdonságait, jelölje be a **Fogadási beállítások** konfigurálása a hogyan ezt a szerződést keresztül érkezett levelei kezelendő.  
8. A fogadási beállítások beállítás meg van osztva az alábbi szakaszok, beleértve a azonosítók, nyugtát, sémák, vezérlő számok, adatérvényesítési, belső beállítások és parancsfájl. Állítsa be ezeket a szerződés partnerrel fog üzenetek cseréje alapján tulajdonságait. Az alábbiakban ezeket a vezérlőket nézetének, állítsa be őket, hogy miként kívánja azonosításához és kezelje a bejövő üzenetek a jelen szerződés alapján:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Az **OK** gombra a beállítások mentéséhez.  

### <a name="identifiers"></a>Azonosítók

|A tulajdonság|Leírás |
|---|---|
|UNB6.1 (címzett hivatkozás jelszó)|Írjon be egy 1 és 14 karakterek közötti alfanumerikus értéket.|
|UNB6.2 (címzett hivatkozás minősítő)|Adjon meg egy legalább egy karakterrel és legfeljebb két karakterből álló alfanumerikus érték.|

### <a name="acknowledgments"></a>Visszaigazolások 

|A tulajdonság|Leírás |
|----|----|
|Üzenet (CONTRL) fogadása|Jelölje be ezt a jelölőnégyzetet, való visszatéréshez egy technikai (CONTRL) visszajelzés adatcsere feladójának. A visszaigazoló a rendszer elküldi a interchange feladó, a szerződés küldése beállításainak alapján.|
|A visszaigazoló (CONTRL)|Jelölje ki a ezt a jelölőnégyzetet a funkcionális (CONTRL) nyugta szeretne térni a interchange feladó a visszaigazoló a szolgáltatás elküldi a interchange feladó, a szerződés küldése beállításainak alapján.|

### <a name="schemas"></a>A sémák

|A tulajdonság|Leírás |
|----|----|
|UNH2.1 (TÍPUS)|Válasszon ki egy tranzakció beállítása.|
|UNH2.2 (VERZIÓ)|Írja be az üzenet verziószáma. (Minimum, egy karakterrel; legfeljebb, három karakterből).|
|UNH2.3 (VÉGLEGES VERZIÓ)|Írja be az üzenet kiadási számát. (Minimum, egy karakterrel; legfeljebb, három karakterből).|
|UNH2.5 (TÁRSÍTOTT HOZZÁRENDELT KÓD)|Adja meg a tevékenységhez rendelt kódot. (Legfeljebb, hat karaktert. Kell alfanumerikus).|
|UNG2.1 (ALKALMAZÁS FELADÓ AZONOSÍTÓ)|Adjon meg egy legalább egy karakterrel és legfeljebb 35 karakterből álló alfanumerikus érték.|
|UNG2.2 (ALKALMAZÁS FELADÓ KÓD MINŐSÍTŐ)|Adja meg a legfeljebb négy karakterből álló alfanumerikus számnak.|
|SÉMA|Jelölje ki a használni kívánt társított integrációs fiókjából korábban feltöltött sémában.|

### <a name="control-numbers"></a>Vezérlőelem számok

|A tulajdonság|Leírás |
|----|----|
|Ellenőrző szám Interchange ismétlődések letiltása|Jelölje be a ismétlődő csomópontokat is blokkolása jelölőnégyzetet. Kijelölt, EDIFACT dekódolását művelet ellenőrzi, a kapott interchange adatcsere-ellenőrző szám (UNB5) nem egyezik meg a korábban feldolgozott interchange ellenőrzési számot. Ha egyező lép fel, majd a cseréje a feldolgozása nem.
|Jelölje be az ismétlődő UNB5 (naponta)|Ha azt választotta, ismétlődő interchange vezérlő számok tiltotta le, megadhatja, amelynél az ellenőrzés történik a megfelelő érték **keresése az ismétlődő UNB5 (naponta)** beállításhoz értesítéssel napok számát.|
|Csoport vezérlőelem szám ismétlődések letiltása|Jelölje be a ismétlődő csoport vezérlőelem számokkal (UNG5) csomópontokat is blokkolása jelölőnégyzetet.|
|Transaction beállítása vezérlő szám ismétlődések letiltása|Jelölje be a ismétlődő tranzakció beállítása vezérlő számokkal (UNH1) csomópontokat is blokkolása jelölőnégyzetet.|
|EDIFACT nyugtázó ellenőrző szám|A tranzakciók beállítása hivatkozási számok nyugtát használandó kijelölésére, írja be egy értéket az előtag, hivatkozás számokból álló tartomány és utótag beállítva.|

### <a name="validations"></a>Adatérvényesítés

|A tulajdonság|Leírás |
|----|----|
|Üzenet típusa|Adja meg az üzenetet. Minden egyes érvényességi sor elvégzettként egy másik a rendszer automatikusan hozzáadja. Ha szabály nincs megadva, majd a sor megjelölt alapértelmezett adatérvényesítéshez használják.|
|Adatérvényesítési szerkesztése|Jelölje be ezt a jelölőnégyzetet, végre szeretne hajtani szerkesztése érvényesítése a séma, hossz korlátozások, üres adatok elemek és záró elválasztójelek tulajdonságainak szerkesztése által meghatározott típusú adatokat.|
|Bővített érvényesítése|Jelölje be ezt a jelölőnégyzetet a bővített (XSD) érvényesítéséhez interchange feladótól kapott csomópontokat is. Ide tartoznak a érvényességi mezőhosszát, lehetőség és ismétlési számláló típusú adatérvényesítés XSD mellett.|
|Vezető/sorzáró nullákkal engedélyezése|Jelölje be az **Engedélyezés** vezető/követő nullák; engedélyezése **NotAllowed** nem engedélyezi a vezető/sorzáró nulla vagy **vágása** levághatja a sortávolságot és záró nullákat.|
|Vezető/sorzáró nullákkal vágása|Jelölje ki a jelölőnégyzet bejelölésével minden kezdő vagy záró nullákkal vágása|
|Záró elválasztót házirendet|Ha nem szeretné, hogy egy interchange feladótól kapott interchange záró határolójel és elválasztójelek engedélyezni, válassza a **Nem engedélyezett** . Ha a interchange záró határolójel és elválasztójelek tartalmaz, akkor deklarálva érvénytelen. Jelölje be a **nem kötelező** , fogadja el vagy záró határolójel és elválasztójelek anélkül csomópontokat is. Válassza a **kötelező** , ha a kapott interchange tartalmaznia kell záró határolójel és elválasztójelek.|

### <a name="internal-settings"></a>Belső beállításai

|A tulajdonság|Leírás |
|----|----|
|Üres XML-címkéket létrehozni, ha a záró elválasztójelek engedélyezettek|Jelölje be ezt a jelölőnégyzetet, hogy a interchange feladó, a záró elválasztójelek üres XML-címkéket tartalmazza.|
|Bejövő eljárásnak feldolgozása|A választható lehetőségek:</br></br>**Felosztott Interchange mint tranzakció beállítása – felfüggesztheti tranzakció készletek hiba**: elemzi a megfelelő boríték alkalmazása tranzakció megadása egy külön XML-dokumentumokba interchange meg tranzakciók. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd a csak adott tranzakció készleteket felfüggesztett vannak. Osztott Interchange mint tranzakció készletek – felfüggesztheti Interchange hiba: elemzi a megfelelő boríték alkalmazásával egy külön XML-dokumentumokba interchange beállítása tranzakciók. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd a teljes interchange fel kell függeszteni.</br></br>**Megőrzése Interchange – felfüggesztheti tranzakció beállítja a hiba**: érintetlenül adatcsere, a teljes kötegelt adatcserét XML-dokumentum létrehozása. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd a csak adott tranzakció készleteket vannak függeszteni, míg más tranzakció minden egyes készletben feldolgozása.</br></br>**Megőrzése Interchange – felfüggesztheti Interchange hiba**: érintetlenül adatcsere, a teljes kötegelt adatcserét XML-dokumentum létrehozása. Ezt a beállítást választja Ha egy vagy több tranzakció állít be a interchange sikertelen érvényesítés, majd a teljes interchange fel van függesztve.|

A szerződés áll készen kezelje a bejövő üzeneteket, amelyek megfelelnek a a kijelölt beállításokat.

A partnerek küldött üzenetek kezelésére beállítások konfigurálása:  
10. Jelölje ki **Küldési beállítások** konfigurálása a hogyan ezt a szerződést keresztül küldött üzenetek kezelésének vannak.  

A küldési beállítások vezérlő meg van osztva az alábbi szakaszok, többek között például azonosítók nyugtát, sémák, borítékok, karakterkészlet és elválasztójelek, vezérlő számok és érvényességi. 

Az alábbiakban ezeket a vezérlőket nézetének. Adja meg a beállításokat, hogyan szeretné kezelni a jelen szerződés partnerektől üzenetekhez alapján:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Az **OK** gombra a beállítások mentéséhez.  

### <a name="identifiers"></a>Azonosítók
|A tulajdonság|Leírás |
|----|----|
|UNB1.2 (szintaxis verzió)|Jelölje ki az **1** és **4**közötti értékkel.|
|UNB2.3 (feladó fordított útválasztási cím)|Adjon meg egy legalább egy karakterrel és legfeljebb 14 karakterből álló alfanumerikus érték.|
|UNB3.3 (fordított útválasztás címzett címe)|Adjon meg egy legalább egy karakterrel és legfeljebb 14 karakterből álló alfanumerikus érték.|
|UNB6.1 (címzett hivatkozás jelszó)|Adjon meg egy legalább egy és legfeljebb 14 karakterből álló alfanumerikus érték.|
|UNB6.2 (címzett hivatkozás minősítő)|Adjon meg egy legalább egy karakterrel és legfeljebb két karakterből álló alfanumerikus érték.|
|UNB7 (hivatkozás azonosítója)|Adjon meg egy legalább egy karakterrel és legfeljebb 14 karakterből álló alfanumerikus érték|

### <a name="acknowledgment"></a>Nyugta
|A tulajdonság|Leírás |
|----|----|
|Üzenet (CONTRL) fogadása|Jelölje be ezt a jelölőnégyzetet, ha a szolgáltatott partner várhatóan fogadási kap egy technikai (CONTRL). Ezzel a beállítással megadhatja, hogy a szolgáltatott partnertől, akiknek az üzenetet küld, a vendégként való bekapcsolódáshoz partnertől visszaigazolást kéri.|
|A visszaigazoló (CONTRL)|Jelölje be ezt a jelölőnégyzetet, ha a szolgáltatott partner várhatóan kap egy funkcionális (CONTRL). Ezzel a beállítással megadhatja, hogy a szolgáltatott partnertől, akiknek az üzenetet küld, a vendégként való bekapcsolódáshoz partnertől visszaigazolást kéri.|
|HK1/HK4 hurok elfogadott tranzakció készletek létrehozása|Ha azt választotta, a funkcionális visszaigazolás kérése, a HK1/HK4 hurkok létrehozása hatályos funkcionális CONTRL visszaigazolások elfogadott tranzakció eredménye a jelölőnégyzet bejelölése a.|

### <a name="schemas"></a>A sémák
|A tulajdonság|Leírás |
|----|----|
|UNH2.1 (TÍPUS)|Válasszon ki egy tranzakció beállítása.|
|UNH2.2 (VERZIÓ)|Írja be az üzenet verziószáma.|
|UNH2.3 (VÉGLEGES VERZIÓ)|Írja be az üzenet kiadási számát.|
|SÉMA|Jelölje ki a séma használata. A sémák integrációs fiókban találhatók. Elérésére, hogy a sémák, először hivatkozás integrációs fiókját az logika alkalmazást.|

### <a name="envelopes"></a>Borítékok
|A tulajdonság|Leírás |
|----|----|
|UNB8 (feldolgozó prioritás kódot)|Adjon meg egy betűrendbe érték, amely nem egynél több karakter hosszú.|
|UNB10 (kommunikáció szerződés)|Adjon meg egy legalább egy karakterrel és legfeljebb 40 karakterből álló alfanumerikus érték.|
|UNB11 (tesztelése jelző)|A jelölőnégyzet bejelölésével jelezheti, hogy a létrehozott interchange tesztadatokat|
|UNA szakasz (Service karakterlánc tanácsokat) alkalmazása|Jelölje be ezt a jelölőnégyzetet, és elküldését adatcserét UNA szegmens létrehozásához.|
|UNG szegmensek (függvény csoportfej) alkalmazása|Jelölje be ezt a jelölőnégyzetet a funkcionális csoportfejlécen a vendégként való bekapcsolódáshoz partnernek küldött üzenetek csoportosítása szegmensek létrehozására. Az alábbi értékeket az UNG szegmensek létrehozásához használt:</br></br>**UNG1**írja be egy alfanumerikus érték legalább egy karakterrel és legfeljebb hat karaktert.</br></br>**UNG2.1**írja be egy legalább egy karakterrel és legfeljebb 35 karakterből álló alfanumerikus érték.</br></br>A **UNG2.2**adja meg a legfeljebb négy karakterből álló alfanumerikus számnak.</br></br>**UNG3.1**írja be egy legalább egy karakterrel és legfeljebb 35 karakterből álló alfanumerikus érték.</br></br>A **UNG3.2**adja meg a legfeljebb négy karakterből álló alfanumerikus számnak.</br></br>**UNG6**írja be egy legalább egy és legfeljebb három karakterből álló alfanumerikus érték.</br></br>**UNG7.1**írja be egy legalább egy karakterrel és legfeljebb három karakterből álló alfanumerikus érték.</br></br>**UNG7.2**írja be egy legalább egy karakterrel és legfeljebb három karakterből álló alfanumerikus érték.</br></br>**UNG7.3**írja be egy legalább 1 karakter és legfeljebb 6 karakterből álló alfanumerikus érték.</br></br>**UNG8**írja be egy legalább egy karakterrel és legfeljebb 14 karakterből álló alfanumerikus érték.|

### <a name="character-sets-and-separators"></a>Karakter készletek és Elválasztókkal
Eltérő karakterkészlet, más beállítási minden üzenet típushoz használandó határoló adhat meg. Ha egy adott üzenet séma karakterkészlet nincs megadva, az alapértelmezett karakterkészlet használják.

|A tulajdonság|Leírás |
|----|----|
|UNB1.1 (rendszer azonosító)|Jelölje be a kimenő interchange alkalmazandó EDIFACT karakterkészletet.|
|Séma|Jelölje ki a séma a legördülő listából. Minden egyes sorára elvégzettként új sor kerül. A kijelölt séma jelölje be a elválasztójelek használandó beállítása:</br></br>**Összetevő elemnek ezreselválasztó** – Enter egyetlen karakter összetett adatelemeket elkülönítésére.</br></br>**Adatok elemnek ezreselválasztó** – Enter egyetlen karakter külön egyszerű adatelemeket összetett adatelemeket belül.</br></br></br></br>**Helyettesítő karakter** – jelölje be ezt a jelölőnégyzetet, ha az adatok található tartalom karaktereket, adatok, szakasz vagy összetevő elválasztójelek is használhatók. Ezután adhat meg helyettesítő karaktert. A kimenő EDIFACT üzenet létrehozásakor összes előfordulását elválasztó karaktereket a tartalom adatok helyére kerülnek a megadott karaktert.</br></br>**A szakasz Befejezés** – Enter egyetlen karakter, hogy egy szerkesztése szakasz végén.</br></br>**Utótag** – jelölje ki a karaktert, amely a szakasz azonosítóval. Utótag megadásakor a szakasz Befejezés adatelem üres lehet. Ha a szakasz Befejezés üres, majd ki kell jelölnie toldalékot.|

### <a name="control-numbers"></a>Vezérlőelem számok
|A tulajdonság|Leírás |
|----|----|
|UNB5 (Interchange ellenőrző szám)|Adjon meg egy előtagot, interchange ellenőrző szám és utótag tartományba. Ezek az értékek egy kimenő interchange létrehozásához használt. Az előtag és utótag nem kötelező; a vezérlő szám szükség. A vezérlőelem-számozása nő minden új üzenet; az előtag és utótag változatlan marad.|
|UNG5 (csoport ellenőrző szám)|Adjon meg egy előtagot, interchange ellenőrző szám és utótag tartományba. Ezeket az értékeket a csoport vezérlőelem szám generálásához használják. Az előtag és utótag nem kötelező; a vezérlő szám szükség. A vezérlő számozása nő minden új üzenet a legnagyobb érték eléréséig; az előtag és utótag változatlan marad.|
|UNH1 (üzenet fejlécében hivatkozási szám)|Adjon meg egy előtagot, interchange ellenőrző szám és utótag tartományba. Az üzenet fejlécében hivatkozási számának létrehozásához használt ezeket az értékeket. Az előtag és utótag nem kötelező; a hivatkozási számra szükség. A hivatkozási számra értéke növekszik, minden új üzenet; az előtag és utótag változatlan marad.|

### <a name="validations"></a>Adatérvényesítés
|A tulajdonság|Leírás |
|----|----|
|Üzenet típusa|Ezt a lehetőséget választva engedélyezi a érvényesítése parancs az adatcsere címzettje. Az ellenőrzés szerkesztése érvényességi tranzakció által beállított adatok elemen, adattípusok, hossz korlátozások és üres adatok elemek ellenőrzése és képzési elválasztójelek hajt végre.|
|Adatérvényesítési szerkesztése|Jelölje be ezt a jelölőnégyzetet, végre szeretne hajtani szerkesztése érvényesítése a séma, hossz korlátozások, üres adatok elemek és záró elválasztójelek tulajdonságainak szerkesztése által meghatározott típusú adatokat.|
|Bővített érvényesítése|Ezt a lehetőséget választva lehetővé teszi, hogy a bővített ellenőrzése a interchange feladó kapott csomópontokat is. Ide tartoznak a érvényességi mezőhosszát, lehetőség és ismétlési számláló típusú adatérvényesítés XSD mellett. Bővítmény érvényességi szerkesztése érvényesítés engedélyezése nélkül engedélyezheti vagy fordítva.|
|Vezető/kezdő nullákat engedélyezése|Ezt a lehetőséget választva adja meg, hogy egy kapott a féltől szerkesztése interchange nem sikertelen érvényességi, ha egy szerkesztése interchange adatelem nem felel meg a hosszát megkövetelésének miatt vagy záró szóközöket, de a hosszát megkövetelésének felel meg vannak eltávolításakor.|
|Vezető/sorzáró nullákkal vágása|Ezt a lehetőséget választva fog vágása a kezdő és záró nullákat.|
|Záró elválasztó|Ezt a lehetőséget választva adja meg a kapott a féltől szerkesztése interchange sikertelen érvényességi, ha egy szerkesztése interchange adatelem nem felel meg a hosszát megkövetelésének kezdő (vagy záró) nullákkal vagy záró szóközöket, de a hosszát megkövetelésének felel meg vannak eltávolításakor.</br></br>Ha nem szeretné, hogy egy interchange feladótól kapott interchange záró határolójel és elválasztójelek engedélyezni, válassza a **Nem engedélyezett** . Ha a interchange záró határolójel és elválasztójelek tartalmaz, akkor deklarálva érvénytelen.</br></br>Jelölje be a **nem kötelező** , fogadja el vagy záró határolójel és elválasztójelek anélkül csomópontokat is.</br></br>Válassza a **kötelező** , ha a kapott interchange tartalmaznia kell záró határolójel és elválasztójelek.|

Miután kattintson **az OK gombra** a Megnyitás lap:  
12. Válassza a fiók integrációs lap a **rendelkezést** csempét, és jelennek meg az újonnan hozzáadott megállapodás szerepel a listában.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>tudj meg többet
- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag")  
