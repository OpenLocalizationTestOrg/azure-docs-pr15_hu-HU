<properties
   pageTitle="Azure AD Connect: Tervezése fogalmak |} Microsoft Azure"
   description="Ez a témakör részletezi bizonyos végrehajtása tervezés területek"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Fogalmak tervezése
Ez a témakör célja területek tervezése Azure AD Connect végrehajtása során kell gondolatot ismertetik. Ez a témakör bizonyos területeken a mély merülési és e fogalmak röviden ismertetik, valamint az egyéb témaköröket.

## <a name="sourceanchor"></a>sourceAnchor
A sourceAnchor attribútum *megváltoztatható objektum élettartama során attribútum*definíciója. Egy objektum, hogy az azonos objektum helyszíni és az Azure Active Directory egyedileg kell azonosítania. Az attribútum rövidítése **immutableId** , és a két nevet cserélhető szolgálnak.

A word megváltoztatható, "nem módosítható", fontos, hogy ez a témakör. Mivel ez attribútumérték nem lehet módosítani, akkor beállítása után, célszerű válasszon tervet, amely támogatja az Ön esetében.

Az attribútum alapul a következő esetekben:

- Az új szinkronizálási motor kiszolgáló beépített, vagy az újonnan létrehozott után egy visszaállításához összeomlást követő helyreállítás során, ha az attribútum meglévő objektumok Azure AD az objektumok a helyszíni hivatkozásait.
- A felhőben identitás áthelyezése a szinkronizált identitás modell, majd az attribútum lehetővé "kemény egyezés" meglévő objektumok az Azure Active Directory helyszíni objektumokkal.
- Összevonási használata esetén kattintson a függvény a együtt a **userPrincipalName** attribútum azonosítja a felhasználó lévő igényt használják.

Ez a témakör csak szól sourceAnchor felhasználók vonatkozik. Minden objektumtípus ugyanazokat a szabályokat alkalmazni, de csak azoknak a felhasználóknak a probléma rendszerint a problémát.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Jelölje ki a helyes sourceAnchor attribútum
Az attribútumérték kell hajtsa végre a következő szabályokat:

- Kisebb, mint 60 karakter hosszúságú lehet.
    - Nem, a – z, A – Z karaktereket, vagy a 0 – 9-es kódolt és 3 karakteres számít
- Speciális karakter nem tartalmaz: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ < > () '; : , [ ] " @ _
- Globálisan egyedinek kell lennie
- Egy karakterlánc, egész vagy bináris kell lennie
- Felhasználónév, a módosítások nem lehet alapján
- Nem kell a kis-és nagybetűket és értékek, amelyek szerint eset változhat elkerülése
- Kell-e kiosztani az objektum létrehozásakor

Ha a kijelölt sourceAnchor nem karakterlánc típusú, majd Azure Active Directory csatlakozás Base64Encode az attribútumérték ahhoz, hogy nincsenek olyan speciális karakterek jelennek meg. Ha egy másik összevonási kiszolgáló ADFS-nél, feltétlenül kiszolgálója az attribútum Base64Encode is használható.

A sourceAnchor attribútum kis-és nagybetűk. A "Kovacsjanos" értéke nem ugyanaz, mint "kovacsjanos". De nem kell esetben csak a különbség a két különböző objektumokat.

Ha van erdőn a helyszíni, majd az attribútum kell használnia a **objectGUID**. Az is használja, ha kifejezetten beállítások az Azure AD Connect attribútum, és a DirSync által használt attribútum.

Ha több erdő, és nem váltás felhasználók erdők és tartományok, **objectGUID** egy jó attribútum ebben az esetben is használni.

Ha a felhasználók között erdők és tartományok, majd meg kell attribútumot, amely nem változik, vagy áthelyezheti a felhasználók az áthelyezés során. A javasolt megközelítés, a bemutató egy szintetikus attribútum. Attribútum élvező sikerült valamit, a következőhöz hasonló lenne, hogy csoporttagokkal egy globálisan egyedi azonosítója. Objektum létrehozása során egy új globálisan egyedi azonosítója létrejön, és a felhasználó ellátott. Egyéni szinkronizálási szabály létrehozása a **objectGUID** alapján ezt az értéket, és ÖSSZEADJA a kijelölt attribútum frissítése szinkronizálási motor Server hozhat létre. Ha át az objektumot, feltétlenül is másolhat ezt az értéket a tartalmát.

Másik megoldás, ha tudja, hogy nem változtatja meg egy meglévő attribútum kiválasztása. A gyakran használt attribútumok közé tartozik **Az Alkalmazottkód oszlop**. Fontolja meg a betűk tartalmazó attribútum, ha mindenképpen legyen a módosíthatja az attribútumérték nem okoz az esetet (kisbetű és nagybetű). Nem lehet használni hibás attribútumok közé tartozik az adott attribútumok annak a felhasználónak a nevét a. Házasság vagy válás nevét szeretné módosítani, várható az attribútum nem engedélyezett. Ez a miért például **userPrincipalName**, a **levelezés**és a **targetAddress** attribútumok nem is lehetséges, jelölje be az Azure AD Connect telepítővarázslóban több oka is. Adott attribútumokat tartalmazó a @-character, a sourceAnchor nem engedélyezett.

### <a name="changing-the-sourceanchor-attribute"></a>A sourceAnchor attribútum módosítása
A sourceAnchor attribútumérték nem módosíthatók, az objektum lett létrehozva Azure Active Directory és a rendszer szinkronizálja az identitás után.

Emiatt a következő megkötések vonatkoznak Azure AD Connect:

- A sourceAnchor attribútum csak a kezdeti telepítésekor állítható be. Futtassa újra a a telepítés varázslót, ha ez a beállítás csak olvasható. Ha módosítania kell ezt a beállítást, majd kell távolítania, és telepítse újra.
- Ha egy másik Azure AD Connect kiszolgálót, majd ki kell választania a korábban használt azonos sourceAnchor attribútum. Ha korábban használták DirSync és Azure AD Connect áthelyezése, majd kell használnia **objectGUID** mivel ez a DirSync által használt attribútum.
- SourceAnchor érték megváltozásakor után a program az az objektum exportált Azure Active Directory, majd Azure AD Connect, szinkronizálási hibát okoz, és nem engedélyezi a további módosításokat meg, hogy objektum, mielőtt a probléma megoldását és a sourceAnchor vissza az adatforrások könyvtárban változik.

## <a name="azure-ad-sign-in"></a>Azure Active Directory-bejelentkezés
A helyszíni címtárában integrálása az Azure Active Directory, miközben fontos megértéséhez, hogy hogyan a szinkronizálási beállítások hatással lehetnek a módszer felhasználó ellenőrzi. Azure Active Directory csatlakozó felhasználók hitelesítését userPrincipalName (UPN) használja. Szinkronizálja a felhasználók, ha az attribútum használandó értéket a userPrincipalName körültekintően kell választania.

### <a name="choosing-the-attribute-for-userprincipalname"></a>A userPrincipalName attribútum kiválasztása
Amikor elemre kattintva biztosítsa a attribútum biztosítania kell-e a használandó Azure egy egyszerű Felhasználónévi értékének

- Az attribútum értékei megfelelnek a van, a formátumot kell legyen egyszerű Felhasználónévi szintaxis (RFC 822),username@domain
- Az értékek utótag megegyezik egy igazolt egyéni tartományt az Azure Active Directory

Express-beállításai az feltételezett választás az attribútum userPrincipalName. Ha a userPrincipalName attribútum nem tartalmaz az érték azt szeretné, hogy a felhasználóknak, hogy jelentkezzen be az Azure, majd választania kell az **Egyéni telepítés**.

### <a name="custom-domain-state-and-upn"></a>Egyéni tartomány állapotát és a (UPN)
Fontos, hogy van-e a UPN-utótag igazolt tartományt.

Adott felhasználó contoso.com. Azt szeretné, hogy a helyszíni egyszerű felhasználónév használandó János john@contoso.com való bejelentkezéshez Azure követően a felhasználók már szinkronizálta a az Azure Active directory contoso.onmicrosoft.com. Ehhez szeretne hozzáadni, és ellenőrizze a contoso.com egyéni tartományt, az Azure Active Directory, a felhasználók szinkronizálásának indítása előtt. Ha az UPN-utótag János, például: contoso.com, az Azure Active Directory nem egyezik meg egy igazolt tartományt, majd Azure Active Directory cseréli az UPN-utótag contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Helyszíni nem átirányítható tartományokat és az Azure Active Directory (UPN)
Néhány a szervezet nem átirányítható tartományokat, például contoso.local vagy egyszerű egyetlen címke tartománynevek, mint például a contoso rendelkezik. Ön nem tudja az Azure Active Directory-átirányítható tartomány hitelesítése. Azure AD Connect az Azure Active Directory szinkronizálhatók csak igazolt tartományt. Amikor létrehoz egy Azure Active directory, a Azure AD például contoso.onmicrosoft.com az alapértelmezett tartomány váló átirányítható tartomány hoz létre. Így szükségessé válik a tartomány tulajdonjogának igazolása bármely más átirányítható az ilyen példa abban az esetben, ha nem szeretné az alapértelmezett onmicrosoft.com végződésű tartományt való szinkronizálásához.

Olvassa el az [Azure Active Directory az egyéni tartománynév hozzáadása](active-directory-add-domain.md) további információt a felvételének és ellenőrzése a tartományok.

Azure AD Connect észleli, ha nem átirányítható tartomány környezetben rendszert futtat, és szeretné megfelelően figyelmezteti az előre express beállítások. Ha egy nem átirányítható tartomány működik, akkor valószínű, hogy a felhasználók (UPN) túl van-e nem átirányítható utótag. Például ha futtatja a contoso.local csoportban, a Azure AD Connect javasol használatát az egyéni beállításokat, hanem kifejezetten beállításokkal. Egyéni beállításokat használ, Ön megadhatja a használandó egyszerű Felhasználónévi, jelentkezzen be az Azure követően a felhasználók a rendszer szinkronizálja az Azure Active Directory attribútum.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
