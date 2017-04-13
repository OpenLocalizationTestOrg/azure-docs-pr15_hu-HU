<properties
    pageTitle="Azure AD Connect szinkronizálási: ismertetése deklaráció kiépítési |} Microsoft Azure"
    description="Azure AD Connect deklaráció kiépítési konfigurációs modell ismerteti."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect szinkronizálása: deklaráció kiépítési ismertetése
Ez a témakör ismerteti a Azure AD Connect konfigurációs modellt. A modell neve deklaráció kiépítési, és lehetővé teszi, hogy a konfiguráció könnyen módosíthatja. Ebben a témakörben ismertetett számos dolgot speciális, és a találatokkal ügyfél nem szükséges.

## <a name="overview"></a>– Áttekintés
Deklaráció kiépítési csatlakoztatott adatforrás könyvtárában érkező objektumok feldolgozó, és azt határozza meg, hogyan az objektumok és attribútumok kell átalakítását forrásból származó cél. Egy objektum feldolgozása a szinkronizálás során, és a folyamat megegyezik a bejövő és kimenő szabályokat. Egy bejövő szabályt az összekötő szóközzel az metaverse és kimenő szabály származik, de a metaverse összekötő területre.

![Szinkronizálási folyamat](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

A folyamat számos különböző modulok tartalmaz. Az objektum szinkronizálási koncepciója mindegyik feladata.

![Szinkronizálási folyamat](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Forrás, a Forrásobjektum
- A [hatókör](#scope)megtalálja az összes szinkronizálása szabályok, amelyek a hatókör
- [Csatlakozás](#join), összekötő terület és metaverse meghatározza viszonya
- [Átalakítás](#transform), számítja ki, hogyan lehet a attribútumok transzformált és továbbításához
- [Végrehajtási sorrendje](#precedence), úgy oldja fel az ütköző attribútum adományok
- Cél, a cél objektum

## <a name="scope"></a>Hatókör
A hatókör modul objektum értékel és határozza meg, hogy a hatókör és feldolgozása szerepelnie kell a szabályokat. Attól függően, hogy az az objektum attribútumok értékeket a hatókör más szinkronizálási szabályok értékeli ki. Nincs Exchange-postafiók letiltott felhasználó például különböző szabályok vonatkoznak, mint egy engedélyezett felhasználó postafiók van.  
![Hatókör](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

A hatókör-csoportok és kikötések definíciója. A záradékok van egy csoporton belül. Logikai és között egy csoport összes záradékok használják. Ha például (részleg informatikai és ország = = Dánia). Logikai vagy csoportok között használják.

![Hatókör](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
A képen a hatókör kell értelmezni (részleg informatikai és ország = = Dánia) vagy (ország = svédországi). Ha 1 vagy csoport 2 kiértékelésének értéke igaz, akkor a szabály hatóköre van.

A hatókör modul támogatja a következő műveleteket.

Művelet | Leírás
--- | ---
EGYENLŐ, NOTEQUAL | Karakterlánc összehasonlítása, ha az érték megegyezik az értéket a attribútumban kiértékelő. Többértékű attribútum olvassa el a ISIN és ISNOTIN című témakört.
LESSTHAN, LESSTHAN_OR_EQUAL | Egy karakterlánc-összehasonlítás, amelyek kiértékeléskor, ha az érték záró érték a attribútumból.
TARTALMAZ, NOTCONTAINS | Egy karakterlánc-összehasonlítás, amely eredménye, ha az érték megtalálható valahol érték belül az attribútum.
STARTSWITH, NOTSTARTSWITH | Egy karakterlánc összehasonlítási, amely eredménye, ha az attribútum értéke elejére értéke.
ENDSWITH, NOTENDSWITH | Egy karakterlánc-összehasonlítás, amely kiértékeli a Ha az attribútum értéke végén található érték.
GREATERTHAN, GREATERTHAN_OR_EQUAL | Egy karakterlánc összehasonlítási, amely eredménye, ha az argumentum értéke nagyobb, mint a attribútumból érték.
ISNULL, ISNOTNULL | Kiértékeli a Ha az attribútum távol objektumából. Ha az attribútum nem bemutató és ezért null, a szabály hatóköre egy.
ISIN, ISNOTIN | Kiértékeli a Ha az érték szerepel a megadott attribútum. Ez a művelet nem egyenlő és NOTEQUAL többértékű változata. Az attribútum egy többértékű attribútum kell használni, és ha az érték valamelyikét attribútum találhatók, majd a szabály egy hatókör.
ISBITSET, ISNOTBITSET | Ha egy adott bit van állítva az eredmény. Ha például használható bit userAccountControl tekintheti meg, ha egy felhasználó engedélyezve van-e ki szeretné számítani.
ISMEMBEROF, ISNOTMEMBEROF | Az érték az összekötő terület csoportban DN kell tartalmaznia. Ha az objektumot a megadott csoport tagjának, a szabály hatóköre szerepel.

## <a name="join"></a>Csatlakozás
A csatlakozás modul a szinkronizálás során a felelős a célhely az objektumot a forrás- és objektum közötti kapcsolatra kereséséhez. Egy bejövő szabályt, kattintson a kapcsolat lenne objektumhoz kapcsolat keresése a metaverse összekötő szóközt objektumának.  
![Csatlakozás cs és TÉ elemet között](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
A cél, ha van egy objektum már a metaverse létrehozott egy másik összekötő, meg kell társítva. Például egy fiókot Erőforráserdő a a felhasználó a fiók erdőből kell összekapcsolhatók az erőforráserdőből felhasználóval.

Illesztés szolgálnak főleg a bejövő szabályok összekötő terület objektumok metaverse ugyanazon objektumhoz összekapcsolására.

Az illesztés meghatározásuk szerint egy vagy több csoportot a. Belül egy csoportot Ha záradékok. Logikai és között egy csoport összes záradékok használják. Logikai vagy csoportok között használják. A csoportok feldolgozása sorrend fentről lefelé. Ha egy csoport objektum pontosan egy egyező talált a cél, nincs más illesztési szabályok kiértékelése. Nulla vagy több, mint egy objektum érhető el, ha a következő csoport szabályok feldolgozása továbbra is. Emiatt a szabályok létrehozott leginkább explicit első sorrend és további zavaros végén kell lennie.  
![Csatlakozás meghatározása](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Ezt a képet az illesztések feldolgozása, felülről lefelé. A szinkronizálás során először látja, van-e a Alkalmazottkód egyezést. Ha nem, a második szabály látja, ha a fiók nevét az objektumok fűzhetők össze is használható. Ez nem egyező vagy, a harmadik és záró szabály esetén több zavaros egyező használatával a felhasználó nevét.

Ha nem pontosan van több egyezés összes illesztés szabály van értékeli, a **Leírás** lapon a **Hivatkozás típusa** használják. Ha ezt a beállítást **kiépítése**van beállítva, akkor a célhely egy új objektum jön létre.  
![Rendelkezni, illetve értekezletbe](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objektum csak a hatókör több egy szinkronizálási szabály illesztés szabályokkal kell rendelkeznie. Ha vannak több szinkronizálási szabályok, ahol a Bekapcsolódás definiálva van, hiba történik. Csatlakozás ütközések feloldása nem szolgál elsőbbségi. Objektum illesztés szabály rendelkeznie kell a bejövő/kimenő megegyező irányba-adatfolyam attribútumok hatókörét. Ha a bejövő és kimenő ugyanazon objektumhoz kell folyamat attribútumok, egy bejövő és Bekapcsolódás egy kimenő szinkronizálási szabály kell rendelkeznie.

Kimenő illesztés van egy speciális viselkedése, amikor megpróbálja kiépítése cél összekötő szóközt egy objektumot. A DN attribútum először próbálkozzon a Fordított sorrend illesztés szolgál. Ha a cél összekötő térköz az azonos DN a már van egy objektumot, az objektumok adatbázis.

Az illesztés modul csak értékelni, amikor az új szinkronizálási szabályt hatókör kijelölésen után. Objektum csatlakozott, amikor a rendszer nem disjoining még akkor is, ha az illesztés feltétel nem teljesül. Ha szeretne egy objektum disjoin, a szinkronizálási szabályt, amelyhez csatlakozott az objektumok lépjen ki a hatókör.

### <a name="metaverse-delete"></a>Metaverse törlése
Egy metaverse objektum marad addig van egy szinkronizálási szabály hatóköre, **A kapcsolat típusának** beállítása **rendelkezést** vagy **StickyJoin**. Egy StickyJoin használja, ha az összekötő nem engedélyezett hozhatók létre egy új objektum a metaverse, azonban, amelyekhez csatlakozott, akkor törölnie kell a forrásban a metaverse objektum törlése előtt.

Ha töröl egy metaverz objektumhoz, egy kimenő szinkronizálási szabály-e jelölve **a rendelkezés** társított összes objektumot a törlés vannak megjelölve.

## <a name="transformations"></a>Átalakítások
Átalakítások használatával határozza meg, hogyan attribútumok kell flow a forrásból származó a célba. A folyamatok beállíthatja, hogy az alábbi **folyamat típusú**: közvetlen, állandó vagy kifejezés. Közvetlen flow, átfolyása az attribútumérték, mint-meg, nincs további átalakítások. Egy konstans érték a megadott értékre állítja be. Kifejezés express hogyan az átalakítás kell lennie az deklaráció kiépítési nyelvén használja. A részletek, a kifejezés nyelvhez a [ismertetése deklaráció kiépítési nyelvén](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) témakörben találhatók.

![Rendelkezni, illetve értekezletbe](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

A **egyszer alkalmazása** jelölőnégyzet határozza meg, hogy az attribútum csak meg kell az objektum létrehozásakor. Ebben a konfigurációban például egy új felhasználói objektum kezdeti jelszó beállításához használható.

### <a name="merging-attribute-values"></a>Attribútum értékei egyesítése
Az attribútum flow van határozza meg, ha a többértékű attribútum egyesíti kell-e az számos különböző összekötők egy beállítást. Az alapértelmezett értéke **frissítést**, amely jelzi, hogy a legnagyobb prioritású a szinkronizálási szabály win kell.

![Egyesítés típusai](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Van még **egyesítése** és **MergeCaseInsensitive**. A beállításokkal különböző forrásokból származó értékek egyesítéséhez teszi lehetővé. Ha például használhatná a tag, vagy a proxyAddresses attribútum számos különböző erdőkből egyesítése. Használja ezt a lehetőséget, ha a hatókör-objektum az összes szinkronizálási szabály egyesítés azonos típusú kell használnia. **Frissítés** egy összekötő és **egyesítése** egy másik nem definiálhatók. Ha megpróbál, egy hibaüzenet jelenik meg.

**Egyesítés** és **MergeCaseInsensitive** közötti különbség ismétlődő attribútum értékek feldolgozási módjának. A sync engine ellenőrzi, ismétlődő értékek nem illeszt be a cél attribútum. **MergeCaseInsensitive**, az ismétlődő értékek különbség csak abban az esetben nem fog jelen. Ha például nem kellene látnia mindkét "SMTP:bob@contoso.com" és "smtp:bob@contoso.com" a cél attribútumban. **Egyesítés** csak megjeleníti a pontos értékek és a több érték esetén csak a különbség az esetet megjelenhetnek.

A **Csere** beállítás ugyanaz, mint a **frissítés**, de nem használható.

### <a name="control-the-attribute-flow-process"></a>Az attribútum folyamat folyamatot vezérlő
Ha több szinkronizálási bejövő szabályok szeretné küldeni a azonos metaverse attribútuma van konfigurálva, elsőbbségi használják a győztes határozza meg. A szinkronizálási szabály legnagyobb prioritású (legkisebb numerikus értéket) szeretné küldeni a az érték fog. Kimenő szabályok ugyanúgy történik. A szinkronizálás a legnagyobb prioritású rendelkező szabály, és a böngészőn a csatlakoztatott könyvtár értéket.

Egyes esetekben, hanem a közreműködés érték, a szinkronizálási szabály meg kell határoznia kell működése a többi szabály. Vannak bizonyos speciális literálok ebben az esetben használható.

A szinkronizálás bejövő szabályok a konstans **NULL** használható jelzi, hogy a folyamat nem tartalmaz értéket szeretné küldeni. Egy másik szabály kisebb prioritású érték is valós. Ha nincs szabály járult érték, a metaverse attribútum törlődik. A kimenő szabály Ha **NULL** érték a végleges az összes szinkronizálása szabályok feldolgozása után majd értékét a program eltávolítja a csatlakoztatott könyvtár.

A konstans **AuthoritativeNull** hasonlít, de azzal a különbséggel, hogy nincs alsó elsőbbségi szabályok hozzájárulhatnak a értéket **Null** .

Egy attribútum folyamat **IgnoreThisFlow**is használhatja. Hasonlít NULL azt jelzi, hogy semmi nem közreműködés értelemben. A különbség, hogy nem távolítja el egy már meglévő értéket a célhely. Van, mint a attribútum folyamat még soha nem volt ott.

Lássunk egy példát:

A *kifelé Active Directory - felhasználó az Exchange hibrid a* program a következő folyamat:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Ez a kifejezés kell értelmezni: Ha a felhasználó postafiók helyezkedik el az Azure Active Directory, majd az attribútum az Azure Active Directory-Active Directory flow. Ha nem, nem enged bármi vissza az Active Directory. Ebben az esetben meg szeretné tartani a meglévő értékét az Active Directory.

### <a name="importedvalue"></a>ImportedValue
A ImportedValue függvény eltér a összes függvénye, mivel a attribútumnév idézőjelek közé kell tenni a szögletes zárójelek közé, hanem árajánlatok:  
`ImportedValue("proxyAddresses")`.

Általában a szinkronizálás során tulajdonság a várt érték használ, akkor is, ha azt még nem exportálva vagy hibát érkezett exportáláskor ("felső részén a torony"). Egy bejövő szinkronizálás feltételezi, hogy nem ért csatlakoztatott könyvtár ahányat attribútum eléri azt. Egyes esetekben fontos csak szinkronizálása egy érték, amely a csatlakoztatott címtár ("hologram és torony importálása delta") erősítette.

Példa ezt a funkciót a kimenő kész szinkronizálási szabály megtalálható *az Active Directory – az Exchange közös felhasználói a*. A hibrid Exchange-ben az Exchange online hozzáadott érték csak kell szinkronizálnia lett arról, hogy az érték exportálása sikerült esetén:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Végrehajtási sorrendje
Megkísérlésekor több szinkronizálási szabályok hozzájárulhatnak a célba attribútumérték, az elsőbbségi értékét használja a győztes határozza meg. A legnagyobb prioritású legkisebb numerikus értéket szabályt szeretné küldeni az ütközés attribútuma fog.

![Egyesítés típusai](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

A sorrend meghatározásához pontosabb attribútum flow az objektumok egy kisebb részét használható. Például a kimenő-a-mezőbe-szabályok győződjön meg róla, hogy a (**Felhasználói AccountEnabled**) engedélyezett fiókból attribútumok elsőbbségi egyéb fiókokból.

Végrehajtási sorrendje összekötők közötti definiálható. Amely lehetővé teszi, hogy az összekötők szeretné küldeni a értékek először jobb adatokkal.

### <a name="multiple-objects-from-the-same-connector-space"></a>Több objektum az összekötő azonos térköz
Több objektum összekötő azonos térköz metaverse ugyanazon objektumhoz illesztés, ha az elsőbbségi is be kell állítani. Ha több objektum ugyanezt a szinkronizálási szabályt körét, majd a szinkronizálási motor segédprogram nem tud elsőbbségi. Azt nem egyértelmű mely Forrásobjektum kell közreműködés a metaverse értéket. Ez a beállítás van készként kétértelmű akkor is, ha a forrásban tulajdonságainak azonos értékkel rendelkezik.  
![Több objektum TÉ elemet ugyanazon objektumhoz illesztés](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

Ebben az esetben frissítenie kell a szinkronizálási szabályokat hatókörének módosítása, így az adatforrás-objektumok más szinkronizálási szabályokat kell hatókör. Amely lehetővé teszi, hogy különböző végrehajtási sorrendje határozza meg.  
![Több objektum TÉ elemet ugyanazon objektumhoz illesztés](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Következő lépések

- További információ a kifejezésekben [ismertetése deklaráció kiépítési](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)nyelvén.
- Lásd: hogyan deklaráció használt kimenő kész [az alapértelmezett beállítások ismertetése](active-directory-aadconnectsync-understanding-default-configuration.md)a kiépítési.
- Megtudhatja, hogy miként deklaráció kiépítési [hogyan lehet módosítani szeretné az alapértelmezett beállítás](active-directory-aadconnectsync-change-the-configuration.md)használatával gyakorlati módosítása.
- Olvassa el a felhasználók és a partnerek összhatását [ismertetése-felhasználók](active-directory-aadconnectsync-understanding-users-and-contacts.md)és a névjegyek továbbra is.

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

**Hivatkozás témakörök**

- [Azure AD Connect szinkronizálása: függvények hivatkozás](active-directory-aadconnectsync-functions-reference.md)
