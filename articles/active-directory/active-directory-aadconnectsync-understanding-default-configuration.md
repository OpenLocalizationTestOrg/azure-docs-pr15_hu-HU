<properties
    pageTitle="Azure AD Connect szinkronizálása: az alapértelmezett beállítások ismertetése |} Microsoft Azure"
    description="Ez a cikk ismerteti az alapértelmezett beállítások Azure AD Connect szinkronizálás."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect szinkronizálása: az alapértelmezett beállítások ismertetése
Ez a cikk ismerteti a kimenő kész konfigurációs szabályokat. Azt a dokumentumok szabályokat és milyen hatással van a konfigurációs ezeket a szabályokat. Azt is bemutatja az Azure AD Connect szinkronizálása az alapértelmezett beállítása. A cél, hogy az olvasó megérti, hogyan működik a konfigurációs modell nevű deklaráció kiépítési a valós életből vett példa a. Ez a cikk tartalma feltételezi, hogy már telepítette, és állítsa be az Azure AD Connect szinkronizálás a telepítővarázslóval.

Ha meg szeretné érteni a konfigurációs modell, olvassa el a [Ismertetése deklaráció kiépítési](active-directory-aadconnectsync-understanding-declarative-provisioning.md)részleteit.

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Azure AD a helyszíni out kész szabályok
Az alábbi kifejezések a kimenő kész konfigurációban találhatók.

### <a name="user-out-of-box-rules"></a>Felhasználói out kész szabályok
Szabályok vonatkoznak a iNetOrgPerson objektumtípus.

A user objektumban, a rendszer szinkronizálja a következőképpen kell megfelelnie:

- Egy sourceAnchor kell rendelkeznie.
- Az objektum létrehozása az Azure Active Directory, után sourceAnchor nem módosítható. Ha az érték megváltozott a helyszíni, az objektum megszakítja a szinkronizálása visszatérhet az előző értékét a sourceAnchor módosításáig.
- Kell rendelkeznie a accountEnabled (userAccountControl) attribútum kitölti. A helyszíni Active Directoryval Ez az attribútum értéke mindig bemutató és feltöltött.

Objektumok a következők felhasználói **nem** szinkronizálja és Azure Active Directory:

- `IsPresent([isCriticalSystemObject])`. Sok out kész objektumok az Active Directory, például a beépített rendszergazdai fiók biztosítása, nem szinkronizálódnak.
- `IsPresent([sAMAccountName]) = False`. Gondoskodjon arról, hogy nincs sAMAccountName attribútummal felhasználói objektumok nem szinkronizálódnak. Ebben az esetben csak gyakorlatilag volna történhet NT4 frissítettük tartományban.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. A rendszer nem szinkronizálja a szolgáltatásfiók Azure AD Connect szinkronizálási és annak régebbi verzióit használják.
- Nem szinkronizálja az Exchange-fiókok nem működő az Exchange Online.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Olyan objektumok, amelyek nem működő az Exchange Online nem szinkronizálja.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Bitmaszk (és H21C07000) szeretné kiszűrése a következő objektumoknak:
    - Nyilvános mappa levelezési
    - Az Attendant postaláda rendszer
    - Postaláda adatbázis postaláda (rendszer postaláda)
    - Univerzális biztonsági csoport (a felhasználó nem vonatkoznak, de nem tartalmaz adatokat régebbi okokból)
    - Nem-univerzális csoport (a felhasználó nem vonatkoznak, de nem tartalmaz adatokat régebbi okokból)
    - Postaláda terv
    - Adatfeltárási postaláda
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. A rendszer nem szinkronizálja replikációs áldozat objektumokat.

A következő attribútum szabályok érvényesek:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. A sourceAnchor attribútum nem járult, csatolt postaládákból. Feltételezzük, hogy ha talált egy csatolt postaláda, a tényleges fiók tagja később.
- A kapcsolódó Exchange attribútumok vannak szinkronizálva csak ha az attribútum **mailnickname-beli** érték szerepel.
- Ha több erdők, majd attribútumok felhasznált a következő sorrendben:
    1. Az engedélyezett fiók akinek közzé kapcsolatos bejelentkezési attribútumokat (például a userPrincipalName).
    2. Az Exchange-postaládához akinek közzé egy Exchange (globális címjegyzék) globális címlistában található, amelynek tulajdonságait.
    3. Ha nincs postaláda található, majd következő attribútumok származnak bármely erdőből.
    4. Exchange kapcsolódó attribútumok (nem látható a globális címlistában a technikai attribútumok) közzé a erdőből hol `mailNickname ISNOTNULL`.
    5. Ha több erdők, amelyek megfelelnek volna végre ezeket a szabályokat, létrehozási sorrendjének (dátum/idő) az összekötők (forests) használatos határozza meg, mely erdő beleszámít a tulajdonságait.

### <a name="contact-out-of-box-rules"></a>Lépjen kapcsolatba a kimenő kész szabályok
A kapcsolattartó objektumot a rendszer szinkronizálja a következőképpen kell megfelelnie:

- Kapcsolat a levelezési kell lennie. A következő szabályok ellenőrzött lesz:
    - `IsPresent([proxyAddresses]) = True)`. Kell értéket a proxyAddresses attribútumból.
    - Egy elsődleges e-mail címét vagy a proxyAddresses attribútumból, vagy a levelezési attribútum találhatók. A jelenlétét egy @ használatos annak ellenőrzéséhez, hogy a tartalom e-mail címre. A két szabályokat kell értékelni, igaz.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Van-e egy bejegyzést a "SMTP:" és az is, ha egy @ a karakterláncban található?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. A levelezési attribútum töltve, és ha igen, akkor is egy @ a karakterláncban található?

Az alábbi kapcsolattartói objektumok **nem** szinkronizálja és Azure Active Directory:

- `IsPresent([isCriticalSystemObject])`. Gondoskodjon arról, hogy nincs megjelölve kritikusként kapcsolattartói objektumok vannak szinkronizálva. Ne lehet bármely, az alapértelmezett beállításokkal.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Az objektumok nem működnek az Exchange Online.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Nem szinkronizálja a replikáció áldozat objektumokat.

### <a name="group-out-of-box-rules"></a>Kimenő kész szabályok csoport
A csoport objektumának a rendszer szinkronizálja a következőképpen kell megfelelnie:

- Kisebb, mint 50 000 tagok kell rendelkeznie. A számláló értéke a helyszíni csoport tagjainak száma.
    - Ha további tagokat előtt szinkronizálás az első alkalommal elindul, a csoporthoz nem szinkronizálja a rendszer.
    - Ha tagjainak száma a nagyobb, először létrehozásának, majd amikor 50 000 tagok eléri a leállítja mindaddig, amíg a tagság száma nem kisebb, mint 50 000 újra szinkronizálását.
    - Megjegyzés: Az 50 000 tagsági száma akkor is vannak érvényben az Azure Active Directory. Ön nem tudja csoportok szinkronizálása további tagokat, ha ez a szabály eltávolítása vagy módosítása.
- Ha a csoport egy **Terjesztési csoportot**, majd is kell mail engedélyezve van. Lásd: a [partner out kész szabályok](#contact-out-of-box-rules) a szabály érvényesül.

A következő csoport objektumok **nem** szinkronizálja és Azure Active Directory:

- `IsPresent([isCriticalSystemObject])`. Sok out kész objektumok az Active Directory, például a beépített rendszergazdacsoportjának biztosítása, nem szinkronizálódnak.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Régi csoport DirSync által használt.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Szerepkör-csoportot.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. A rendszer nem szinkronizálja replikációs áldozat objektumokat.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out kész szabályok
FSP adatbázis "bármelyik" (\*) a metaverse objektumra. A valóságban ez az illesztés csak történik a felhasználók és a biztonsági csoportokat. Ebben a konfigurációban biztosítja, hogy a erdőközi tagságot a rendszer és Azure Active Directory megfelelően jeleníti.

### <a name="computer-out-of-box-rules"></a>Számítógép out kész szabályok
Egy számítógép-objektumot a rendszer szinkronizálja a következőképpen kell megfelelnie:

- `userCertificate ISNOTNULL`. Csak a Windows 10-es számítógépek attribútum adataival. Az adott jellemző értéket tartalmazó összes számítógép-objektumok szinkronizálása.

## <a name="understanding-the-out-of-box-rules-scenario"></a>A kimenő kész szabályok eset bemutatása
Ebben a példában a környezet, amelyben egy fiókerdők (A), egy Erőforráserdő (R), és egy Azure Active directory esetén azt.

![Kép a forgatókönyv leírása](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

Ebben a konfigurációban feltételezett van engedélyezve van a fiókerdők fiók és egy csatolt postaláda erőforrás akinek letiltott fiók.

Az alapértelmezett beállításokat tartalmazó célunk:

- Bejelentkezési kapcsolódó attribútumok vannak szinkronizálva az engedélyezett fiókkal akinek át.
- A globális címlistában (globális címjegyzék) található, amelynek tulajdonságait a postaláda akinek vannak szinkronizálva. Ha nincs postaláda található, bármely más erdőjének használják.
- Ha egy csatolt postaláda található, a csatolt engedélyezett fiók az Azure Active Directory exportálandó objektum kell található.

### <a name="synchronization-rule-editor"></a>Szinkronizálás szabály szerkesztő
A konfiguráció lehet megtekinteni és módosítani az eszközzel szinkronizálási szabályok szerkesztő (SRE), és egy parancsikont, akkor a start menüben található.

![Szinkronizálás szabályok szerkesztő ikon](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

A SRE resource kit eszköz, és telepítve van az Azure AD Connect szinkronizálása. Lehessen elindítani, hogy a ADSyncAdmins csoport tagjának kell lennie. Elindul, talál valamit, amit jelennek meg:

![Szinkronizálási szabályok bejövő](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Ez a munkaablakban a konfigurációban által létrehozott összes szinkronizálási szabályok látni. A tábla minden sorához egy szinkronizálási szabály. A szabálytípus bal oldalán, a két különböző felsorolt: a bejövő és kimenő. Bejövő és kimenő a metaverse nézetben. Főként fogja a fókusz a bejövő szabályok az Áttekintés a. A tényleges szinkronizálási szabályok listáját az Active Directory az észlelt séma függ. A fenti képen a fiókerdők (fabrikamonline.com) nem tartalmaz olyan szolgáltatásokat, például az Exchange és a Lync, és nem a szinkronizálási szabályok az alábbi szolgáltatások hozták létre. Az erőforráserdőben (res.fabrikamonline.com) szinkronizálási szabályok keresse meg az alábbi szolgáltatások. A tartalom a szabályok eltér attól függően, hogy az észlelt verzió. Ha például az Exchange 2013-mal környezetben vannak több, mint az Exchange 2010 és 2007-ben beállított attribútum flow.

### <a name="synchronization-rule"></a>Szinkronizálás szabály
Szinkronizálás szabály attribútumok lépés, ha egy feltétel teljesül a konfigurációs objektum. Ismertetik, hogyan viszonyul az összekötő szóközt objektum a metaverse, más néven **illesztés** vagy **felel meg**az objektum is használt. A szinkronizálási szabályok van, azok közötti összefüggéseket egymással jelző elsőbbségi értéket. Alsó numerikus értéket tartalmazó szinkronizálási szabály magasabb elsőbbséget, és egy attribútum folyamat ütközést magasabb elsőbbségi wins az ütközések feloldása.

Példaként, tekintse meg a szinkronizálást szabály **az Active Directory – felhasználói AccountEnabled a**. Ebben a sorban a SRE a megjelölése, és válassza a **Szerkesztés**.

Mivel ez a szabály egy kimenő kész szabályt, figyelmeztetést kap a szabály megnyitásakor. Nem gondoskodnia kell [out kész szabályok módosításait](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), így a program megkérdezi, hogy Mik azok a tervezett. Ebben az esetben csak szeretne megtekinteni a szabályt. Válassza a **nem**.

![Szinkronizálás figyelmeztetés szabályok](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

A szinkronizálás szabályok négy konfigurációs szakaszból: leírás, szűrése, illesztés szabályok és átalakítások hatókör meghatározása.

#### <a name="description"></a>Leírás
Az első rész alapvető információk, például a nevét és leírását.

![Leírás szinkronizálja szabály szerkesztő lapon ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Is információkat talál mely csatlakoztatott rendszerről Ez a szabály kapcsolódik, amely objektum típusa IT a csatlakoztatott rendszerben és a metaverse objektumtípus. A metaverse objektumtípus esetén mindig személy függetlenül attól, hogy az objektum típusa egy felhasználó, iNetOrgPerson vagy kapcsolattartó. A metaverse objektumtípus soha nem kell módosíthatja, így az általános típus létrehozása. A hivatkozás típusa illesztés, StickyJoin vagy rendelkezés beállíthatók. Ez a beállítás alkalmazás és a szabályok Bekapcsolódás szakaszt, és később vonatkozik.

Is megtekintheti, hogy ez a szabály szinkronizálás jelszó-szinkronizálás szolgál. Ha egy felhasználó a szinkronizálási szabály hatóköre, a jelszó szinkronizálja a helyszíni felhőbe (feltételezve, hogy engedélyezte a jelszó-szinkronizálási szolgáltatás).

#### <a name="scoping-filter"></a>Hatókör meghatározása szűrő
A szűrés hatókör meghatározása csoportban állítsa be a mikor legyen alkalmazva a szinkronizálás szabály szolgál. A szinkronizálás szabály készíteni neve azt jelzi, hogy csak engedélyezett felhasználók kell alkalmazni, mivel a hatókör, az Active Directory-attribútum **userAccountControl** nem lehet a 2 átviteli helyesek-e beállítva. Ha a szinkronizálási motor megtalálja a felhasználó Active Directory, azt a szabályt szinkronizálási **userAccountControl** beállítása a decimális érték 512 (normál engedélyezett felhasználói) esetén. A szabály azt nem vonatkozik, amikor a felhasználó rendelkezik-e **userAccountControl** 514 (letiltott normál felhasználói) értékűre.

![Hatókör meghatározása szinkronizálási szabály szerkesztő lapja ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

A tartalmazó szűrőre csoportok és záradékot, amely ágyazhatók be. Egy szinkronizálási szabály az összes záradékok egy csoporton belül kell teljesülnie. Ha több meg vannak adva, majd egy csoport nem teljesül a szabály alkalmazása. Ez azt jelenti, hogy egy logikai vagy operátorral csoportok és a logikai közötti kiértékelt, és egy csoporton belüli kiértékelt. Ebben a konfigurációban példa megtalálható a kimenő szinkronizálási szabály **kifelé a AAD – csoport csatlakozás**. Több szinkronizálási szűrő csoportok, például egy biztonsági csoportok (`securityEnabled EQUAL True`), a másik terjesztési csoportok (`securityEnabled EQUAL False`).

![Hatókör meghatározása a szinkronizálási szabály szerkesztő lapja ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Ez a szabály megadásához, hogy mely csoportok kell kell építenie Azure Active Directory használják. Terjesztési csoportok lehetővé tenni, hogy szinkronizálja az Azure Active Directory mail kell lennie, de biztonsági csoportok e-mailben nem szükséges.

#### <a name="join-rules"></a>Bekapcsolódás a szabályok
A harmadik szakasz konfigurálása a összekötő szóköz objektumainak közötti összefüggéseket a metaverse objektumainak használják. A szabály van nézett korábbi nincs bármely konfiguráció a Bekapcsolódás szabályokat, így inkább fogja meg a **az Active Directory – felhasználói Bekapcsolódás a**.

![Bekapcsolódás a szabályok lapon a szinkronizálási szabály szerkesztő ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

A tartalom a csatlakozás szabály attól függ, hogy a megfelelő beállítást, a telepítővarázslóban. A kiértékelés a forrás összekötő szóköz objektumának kezdődik egy bejövő szabályt, és minden csoport csatlakozás szabályok kiértékelt sorrendben. Ha egy forrásobjektum kiértékelt megfelelően a közül az illesztés szabályok metaverse pontosan egy objektumot, az objektumok adatbázis. Ha az összes szabály van értékeli, és nem szerepel, a Leírás lapon a hivatkozás típusa használják. Ez a beállítás értéke **kiépítése**, majd egy új objektum céllistában, a metaverse jön létre. A metaverse egy új objektum kiépítése is nevezik a **project** a metaverse kívánt objektumot.

Az illesztés szabályok csak egyszer kell kiértékelni. Ha egy összekötő terület objektum és egy metaverse objektum tartományhoz, maradnak illesztett mindaddig, amíg a szinkronizálás a szabály hatóköre továbbra is eleget.

Szinkronizálási szabályok kiértékelésekor megadott illesztési szabályok csak egy szinkronizálási szabály hatóköre kell lennie. Ha több szinkronizálási szabályok illesztés szabályokkal találnak egy adott objektum, hiba történt elő. Emiatt a legjobb módszer is, hogy csak egy szinkronizálási szabály definiált, ha több szinkronizálási szabályok alapján objektumhoz való bekapcsolódás az. A kimenő kész a konfigurációban a Azure AD Connect szinkronizálásához szabályok megjeleníti a név található, és keresse meg a szó végén található a nevét a **Bekapcsolódás** rendelkezők. Szinkronizálás szabály minden megadott illesztési szabályok nélkül az attribútum flow vonatkozik, egy másik szinkronizálási szabály az objektumok összefűzött vagy egy új objektum a célhely kiépítve.

Tekintse meg a fenti képen, láthatja, hogy a szabály partnere kapcsolatba próbál Bekapcsolódás **objectSID** **msExchMasterAccountSid** (Exchange) és az **msRTCSIP-OriginatorSid** (Lync), amely olyan, hogy elképzeléseinek-fiók-erőforrás erdő topológiában. Ugyanezt a szabályt minden erdőket megtalálhatja. A feltételezi, hogy minden erdőben lehet egy fiókot vagy az erőforrás erdő. Ez a beállítás akkor is használható, ha a fiókot, amely élő erdőn, és nem rendelkezik tartományhoz.

#### <a name="transformations"></a>Átalakítások
Az átalakítás szakasz összes attribútum flow alkalmazása a cél objektumra, amikor az objektumok adatbázis és a hatókör szűrő meggyőződött határozza meg. Visszalépés a **a az Active Directory – felhasználói AccountEnabled** szinkronizálási szabályt, a következő átalakításokat megkeresése:

![Átalakítások szinkronizálja szabály szerkesztő lapon ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Ebben a konfigurációban elhelyezni környezet, a fiók-erőforrás erdő példányban várhatóan megkereséséhez engedélyezett fióknak a fiókerdők és letiltott fiók erőforráserdőben Exchange és a Lync-beállításokkal. A szinkronizálás szabály készíteni a bejelentkezés szükséges attribútumokat tartalmazza, és a következő attribútumok az erdőből kell folyamatvezérlés amennyiben van engedélyezett fiók. Ezek attribútumot minden flow közös elhelyezése egy szinkronizálási szabályt.

Az átalakítás beállíthatja, hogy különböző: állandó, közvetlen és kifejezés.

- Egy konstans folyamat mindig folyik át a szoftveresen kötött értéket. Abban az esetben a fenti mindig beállítja az **Igaz** értéket a metaverse attribútum nevű **accountEnabled**.
- Közvetlen adatfolyam mindig átfolyása az az érték az attribútum a cél attribútumhoz, mint a forrás-van.
- A harmadik folyamat típusú kifejezés, és lehetővé teszi speciális beállítások.

Az nyelvén VBA (Visual Basic for Applications), így mások élményt a Microsoft Office vagy a VBScript ismerik fel a formátumot. Attribútumok vannak szögletes zárójelek, [attribútumnév]. Attribútum és függvény nevét és nagybetűk, de a szinkronizálás szabályok szerkesztő kiértékeli a kifejezések, és adja meg egy figyelmeztető üzenetet, ha a kifejezés nem érvényes. Minden kifejezés egymásba ágyazott függvényeket tartalmazó egy sorban van kifejezve. Szeretné megjeleníteni a power konfigurációs nyelv, Íme a folyamat pwdLastSet, de a beszúrt megjegyzésekkel:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

[Ismertetése deklaráció kiépítési kifejezések](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) talál további információt a attribútum folyamatok nyelvén.

### <a name="precedence"></a>Végrehajtási sorrendje
Most már nézett néhány egyes szinkronizálási szabályok, de a konfigurációban közös munka a szabályokat. Egyes esetekben az attribútumérték van járult több szinkronizálási szabályok a az azonos cél attribútumhoz. Ebben az esetben attribútum sorrendje határozza meg, mely attribútum wins használják. Példaként tekintse meg az attribútum sourceAnchor. Az attribútum engedélyezni szeretné, hogy jelentkezzen be az Azure Active Directory fontos attribútum. Egy attribútum folyamat megtalálhatja a két különböző szinkronizálási szabályok az attribútum **a az Active Directory – felhasználói AccountEnabled** és **a az Active Directory – felhasználói közös**. Szinkronizálási szabályok elsőbbségi sorrendjének, miatt sourceAnchor attribútum van járult az engedélyezett fiók akinek először metaverse objektumhoz illesztés több objektumok esetén. Ha nincsenek engedélyezve fiókok, majd a szinkronizálás motor használja a tényleges összes szinkronizálási szabály **az Active Directory – felhasználói közös a**. Ebben a konfigurációban biztosítja, hogy akkor is, hogy le vannak tiltva fiókok számára van még egy sourceAnchor.

![Szinkronizálási szabályok bejövő](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

A szinkronizálás szabályok elsőbbségi állítja be a csoportokban az telepítővarázslóban. Egy csoport összes szabály lehet ugyanaz a neve, de a másik csatlakoztatott könyvtárak kapcsolódnak. A telepítés varázslóban többféle a szabály **a az Active Directory – a felhasználók csatlakozásra** legnagyobb prioritású, és azt át az összes kapcsolódó AD könyvtárak kiszámítására. Ezután a következő csoportokkal szabályok előre meghatározott sorrendben folyamatosan. Belül egy csoportjában a szabályok abban a sorrendben, a varázsló az összekötők hozzáadott kerülnek. Ha másik összekötő hozzá van rendelve, a varázsló lépéseit, a szinkronizálási szabályok átrendezve vannak, és az új összekötő szabályok vannak beszúrva utolsó minden egyes csoporthoz.

### <a name="putting-it-all-together"></a>A teljes kép
Most már tudjuk elég kapcsolatos szinkronizálási szabályok engedélyezni szeretné a konfigurációt a különböző szinkronizálás szabályokkal működésének megértése. Ha egy felhasználó és a tulajdonságait, hogy vannak-e járult hozzá az metaverse megnézi a szabályok alkalmazása a következő sorrendben:

név | Megjegyzés
:------------- | :-------------
Az Active Directory – felhasználói illesztés a | Szabály metaverse összekötő terület objektumok illesztése.
Az Active Directory – a felhasználóifiók engedélyezve | Bejelentkezés az Azure Active Directory-höz szükséges attribútumokat és az Office 365-ben. Azt szeretné, hogy a következő attribútumok engedélyezett-fiókból.
Az Active Directory – az Exchange felhasználói közös a | A globális címlistában található attribútumok. Feltételezzük, hogy hol van a felhasználó postafiók talált erdőben adatok minőségét a legjobb.
Az Active Directory – felhasználói közös a | A globális címlistában található attribútumok. Abban az esetben, ha egy Webhelyfiók nem található, illesztett más objektumok hozzájárulhat a attribútumérték.
Az Active Directory – felhasználói Exchange a | Csak akkor létezik, ha Exchange fordult elő. Az összes infrastruktúra Exchange-attribútumokat folyik át.
Az Active Directory – a Lync felhasználói a | Csak létezik, ha a Lync fordult elő. Az összes infrastruktúra Lync attribútumok folyik át.

## <a name="next-steps"></a>Következő lépések

- További információ a konfigurációs modellt [Deklaráció kiépítési ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- További információ a kifejezésekben [ismertetése deklaráció kiépítési](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)nyelvén.
- Folytassa a kimenő kész konfiguráció való működésének [megértése felhasználók és a partnerek](active-directory-aadconnectsync-understanding-users-and-contacts.md) olvasása
- Megtudhatja, hogy miként deklaráció kiépítési [hogyan lehet módosítani szeretné az alapértelmezett beállítás](active-directory-aadconnectsync-change-the-configuration.md)használatával gyakorlati módosítása.

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
