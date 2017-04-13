<properties
    pageTitle="Azure többtényezős hitelesítés – a következő lépések"
    description="Ez az a Azure többtényezős hitelesítést lapra, hogy mi a teendő ezután MFA ismerteti.  Ide tartoznak a jelentések, a csalások értesítés, a egyszeri figyelmen kívül hagyása, a egyéni hangüzenetekről gyorsítótárazás megbízható IP-címei és az alkalmazás jelszót."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Azure többtényezős hitelesítés beállítása

Ez a cikk segít kezelése Azure többtényezős hitelesítés, most, hogy Ön a lépéseket.  Témakörök, amelyek segítségével hatékony kihasználása Azure többtényezős hitelesítés számos lefedi.  Ezek a funkciók közül nem mindegyik érhetők el az Azure többtényezős hitelesítés minden verziójában.

Az alábbi funkciók közül egyesek az adatokat az Azure többtényezős hitelesítést adatkezelési portálon található. Kétféleképpen különböző a MFA kezelőportálja érheti el, amelyek mindkét végzett az Azure portálon keresztül. Az első többtényezős hitelesítés szolgáltató kezelésével, felhasználási-alapú MFA használata van. A második MFA-szolgáltatás beállításai található. A második lehetőség többtényezős hitelesítés szolgáltató vagy egy Azure MFA, az Azure Active Directory Premium vagy a vállalati mobilitás Suite licenc szükséges.

Az Azure többtényezős hitelesítés szolgáltatóját a MFA adatkezelési portálra eléréséhez jelentkezzen be rendszergazdaként az Azure-portálra, és válassza az Active Directory lehetőséget. Kattintson a **Többtényezős hitelesítés szolgáltatók** fülre, majd válassza ki a címtárában, és kattintson a **kezelés** gombra a képernyő alján.

Hozzá szeretne férni a MFA adatkezelési portálra a MFA-szolgáltatás beállításai lapon, jelentkezzen be rendszergazdaként az Azure-portálra, és válassza az Active Directory lehetőséget. Kattintson a címtárában, és kattintson az a **beállítás** fülre. Többtényezős hitelesítés csoportjában válassza a **Manage szolgáltatás beállításai**. A képernyő alján az MFA-szolgáltatás beállításai lapjáról hivatkozásra **a portált** .


A szolgáltatás| Leírás| Mire vonatkozik
:------------- | :------------- | :------------- |
[Csalás értesítés](#fraud-alert)|Értesítés csalás konfigurálható és beállítani, hogy a felhasználók saját forrásokat csalárd próbál jelentést küldhet.|Állítsa be, beállítása és csalás jelentése
[Egyszeri figyelmen kívül hagyása](#one-time-bypass) |Egy egyszeri figyelmen kívül hagyása lehetővé teszi, hogy a felhasználó egyetlen "kihagyásával" többtényezős hitelesítés.|Hogyan állíthatja be, és állítsa be a egyszeri figyelmen kívül hagyása
[Egyéni Hangüzenetekről](#custom-voice-messages) |Egyéni hangüzenetekről használja, a saját felvételek vagy a Üdvözlések többtényezős hitelesítés teszi lehetővé. |Hogyan állíthatja be, és állítsa be az egyéni Üdvözlések és üzenetek
[Gyorsítótár](#caching-in-azure-multi-factor-authentication)|Gyorsítótár csoportban adhatja meg egy adott időpontban időszak, hogy automatikusan későbbi hitelesítési kísérlet sikertelen volt. |Hogyan lehet telepíteni és beállítani a gyorsítótár-hitelesítést.
[Megbízható IP-címei](#trusted-ips)|Megbízható IP-címei jellemzi a többtényezős hitelesítés, amely egy felügyelt vagy szövetséges bérlői rendszergazdák kikerülheti többtényezős hitelesítés a felhasználóknak, hogy a vállalat helyi intranet elemet a bejelentkezéshez.|Állítsa be és IP-címek, amelyek adómentes többtényezős hitelesítés beállítása
[Alkalmazás jelszavak](#app-passwords)|Az alkalmazás jelszó lehetővé teszi, hogy egy alkalmazást, amely nem MFA-et többtényezős hitelesítés átlépése, és folytathatja a munkát.|Alkalmazás jelszavakkal kapcsolatos információk.
[Többtényezős hitelesítés emlékeztető megjegyzett eszközök és böngészők](#remember-multi-factor-authentication-for-devices-users-trust)|Ne feledje, hogy az eszközök beállítása számú nap után a felhasználó sikeresen aláírtak MFA használatával teszi lehetővé.|Információ arról, hogy engedélyezi ezt a szolgáltatást, és állítsa be a napok számát.
[Ügyféloldalon választható hitelesítési módszerek](#selectable-verification-methods)|Válassza a felhasználók számára elérhető hitelesítési módszereket teszi lehetővé.|Információ arról, hogy engedélyezés vagy letiltás, például a hívás vagy szöveges üzenetet adott hitelesítési módszereket.



## <a name="fraud-alert"></a>Csalás értesítés
Értesítés csalás konfigurálható és beállítani, hogy a felhasználók saját forrásokat csalárd próbál jelentést küldhet.  Felhasználók jelentheti csalás a mobilalkalmazással vagy a telefonon keresztül.

### <a name="to-set-up-and-configure-fraud-alert"></a>Állíthatja be és csalás értesítés beállítása

1.  Jelentkezzen be http://azure.microsoft.com
2.  Nyissa meg a MFA Kezelőportálja per elemre a lap tetején a képernyőn megjelenő utasításokat.
3.  Az Azure többtényezős hitelesítés felügyeleti portálon kattintson a beállítások a beállítás szakaszban.
4.  A beállítások lap csalás riasztási csoportjában jelölje be a csalás riasztások jelölőnégyzet nyújt a felhasználóknak.
5.  Ha azt szeretné, hogy a felhasználók számára blokkolhatja, ha csalás jelenteni, jelölje be blokk felhasználói, ha csalás jelenteni.
6.  Az a **Kódot a jelentés csalás során kezdeti megszólítás** mezőbe írja be a számkód hívás ellenőrzés során használható. Ha egy felhasználó a kódot, és csak a # jel megadásához helyett # ír, a csalások értesítés kell jelenteni.
7.  A képernyő alján kattintson a Mentés gombra.

>[AZURE.NOTE]
>A Microsoft alapértelmezett hangposta Üdvözlések kérje a felhasználókat, hogy nyomja le a egy csalás riasztás küldhetik 0#. Ha eltérő 0 kódot használni kívánt, rögzítése, és töltse fel a saját egyéni hangposta Üdvözlések a megfelelő utasításokat.


![Felhőalapú](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Jelentés csalás jelzésére
Értesítés csalás kétféleképpen lehet jelenteni.  Akár a mobilalkalmazás, vagy telefonon keresztül.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Jelentés csalás jelzésére a mobilalkalmazással



1. A tartomány ellenőrzéséhez a mobiltelefonjára küldött, jelölje ki azt a Microsoft Authenticator alkalmazás.
2. Jelentés csalás, kattintson a Mégse gombra, és a jelentés csalás. Ekkor megjelenik egy mezőt, amely szerint a rendszer értesíti a felhasználót a szervezet informatikai az ügyfélszolgálati munkatársak.
3. Kattintson a jelentés csalás ellen.
4. Kattintson az alkalmazást kattintson a Bezárás gombra.

![Felhőalapú](./media/multi-factor-authentication-whats-next/report1.png)


![Felhőalapú](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Jelentés csalás jelzésére a telefon

1. A telefon ellenőrzési hívás érkezik, választ adni azt.  
2. Jelentés csalás, írja be a kódot, amely van konfigurálva, hogy megfeleljen a jelentési csalás keresztül, a telefon és a # jel megadásához. Ön értesítést kap, hogy egy csalás riasztás elküldése.
3. Befejezheti a hívást.

### <a name="to-view-the-fraud-report"></a>Az internetes csalásról jelentés megtekintéséhez

1. Jelentkezzen be a [http://azure.microsoft.com](https://azure.microsoft.com/)
2. A bal oldalon válassza az Active Directory.
3. A képernyő tetején válassza a többtényezős hitelesítés szolgáltatók. Ekkor megjelenik a többtényezős hitelesítés szolgáltatók listáját.
4. Ha egynél több többtényezős hitelesítés szolgáltató, jelölje ki a kívánt a jelentés megtekintése a csalások riasztási, és kattintson a kezelés elemre a lap alján. Ha csak egy, a kezelés gombra. Ekkor megnyílik az Azure többtényezős hitelesítést adatkezelési portálra.
5. Az Azure többtényezős hitelesítés adatkezelési portálon, a bal oldali, A jelentés megtekintése a kattintson csalás riasztási.
6. Adja meg a dátumtartományt, amelyet szeretne megjeleníteni a jelentésben. Is megadhatja bármely adott felhasználónevét, telefonszámok és a felhasználó állapota.
7. Kattintson a Futtatás gombra. Ekkor megjelenik a jelentés, amely hasonlít az alábbi egy. Ha a jelentés exportálása szeretne exportálása CSV-fájlok is kattinthat.

## <a name="one-time-bypass"></a>Egyszeri figyelmen kívül hagyása

Egy egyszeri figyelmen kívül hagyása lehetővé teszi, hogy a felhasználó egyetlen "kihagyásával" többtényezős hitelesítés. A figyelmen kívül hagyása ideiglenes, és a megadott számú másodpercig után lejár.  Így olyan esetekben, ahol a mobilalkalmazásban vagy a telefon nem kap egy értesítést vagy a telefonhívás-, lehetősége van engedélyezni a egyszeri figyelmen kívül hagyása, így a felhasználó hozzáférhet a kívánt erőforrás.

### <a name="to-create-a-one-time-bypass"></a>Egy egyszeri figyelmen kívül hagyása létrehozása

1.  Jelentkezzen be http://azure.microsoft.com
2.  Nyissa meg a MFA Kezelőportálja per elemre a lap tetején a képernyőn megjelenő utasításokat.
3.  Az Azure többtényezős hitelesítés Kezelőportálja, ha a bérlő vagy a Azure MFA szolgáltató neve megjelenik a bal oldalon lévő egy mellett látható, kattintson a + lásd: más MFA kiszolgáló replikációs csoportokat és a Azure alapértelmezett csoport. Kattintson a kívánt csoportra.
4.  Felhasználók felügyelete csoportjában kattintson a **One-Time figyelmen kívül hagyása**.
![Felhőalapú](./media/multi-factor-authentication-whats-next/create1.png)
5.  A One-Time figyelmen kívül hagyása lapon kattintson az **Új One-Time figyelmen kívül hagyása**.
6.  Írja be a felhasználó felhasználónevét, hogy hány másodperc a figyelmen kívül hagyása megtalálható lesz, a figyelmen kívül hagyása az az oka, majd kattintson a **figyelmen kívül hagyása**.
![Felhőalapú](./media/multi-factor-authentication-whats-next/create2.png)
7.  Ezen a ponton a felhasználónak kell bejelentkezni az egyszeri figyelmen kívül hagyása lejárta előtt.



### <a name="to-view-the-one-time-bypass-report"></a>Az egyszeri figyelmen kívül hagyása jelentés megtekintéséhez

1. Jelentkezzen be a [http://azure.microsoft.com](https://azure.microsoft.com/)
2. A bal oldalon válassza az Active Directory.
3. A képernyő tetején válassza a többtényezős hitelesítés szolgáltatók. Ekkor megjelenik a többtényezős hitelesítés szolgáltatók listáját.
4. Ha egynél több többtényezős hitelesítés szolgáltató, jelölje ki a kívánt a jelentés megtekintése a csalások riasztási, és kattintson a kezelés elemre a lap alján. Ha csak egy, a kezelés gombra. Ekkor megnyílik az Azure többtényezős hitelesítést adatkezelési portálra.
5. Kattintson a Azure többtényezős hitelesítést Kezelőportálja segítségével, a bal oldali, A jelentés megtekintése a One-Time figyelmen kívül hagyása.
6. Adja meg a dátumtartományt, amelyet szeretne megjeleníteni a jelentésben. Is megadhatja bármely adott felhasználónevét, telefonszámok és a felhasználó állapota.
7. Kattintson a Futtatás gombra. Ekkor megjelenik a jelentés, amely hasonlít az alábbi egy. Ha a jelentés exportálása szeretne exportálása CSV-fájlok is kattinthat.

<center>![Felhőalapú](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Egyéni hangüzenetekről

Egyéni hangüzenetekről használja, a saját felvételek vagy a Üdvözlések többtényezős hitelesítés teszi lehetővé.  Ezek a kívül használhatók, vagy le szeretné cserélni a Microsoft rekordokat.

Első lépések tartsa szem előtt a következőket:

- Támogatott fájlformátumok a következők: .wav és .mp3.
- A fájl mérete korlát 5 MB.
- Javasoljuk, hogy a hitelesítési üzeneteket, amely azt nem lehet 20 másodpercnél. Okozhat, bármi nagyobb, mint ez az ellenőrzés sikertelen lesz, mert a felhasználónak nem válaszol, mielőtt befejeződött üzenet, és az igazolás időtúllépés történik.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Egyéni hangüzenetekről az Azure többtényezős hitelesítés beállítása
1.  Hozzon létre egy egyéni hangüzenet a támogatott fájlformátumok egyikét felhasználva.
2.  Jelentkezzen be http://azure.microsoft.com
3.  Nyissa meg a MFA Kezelőportálja per elemre a lap tetején a képernyőn megjelenő utasításokat.
4.  Az Azure többtényezős hitelesítés adatkezelési portálon kattintson a Hangposta-üzenetek konfigurálása szakaszban.
5.  A Hangposta-üzenetek csoportban kattintson az **Új hangüzenet**.
![Felhőalapú](./media/multi-factor-authentication-whats-next/custom1.png)
6.  A beállítás: új Hangposta-üzenetek lapján kattintson a **Hangfájlok kezelése**.
![Felhőalapú](./media/multi-factor-authentication-whats-next/custom2.png)
7.  A beállítás: fájlok lap Hang, **Hang-fájl feltöltése**parancsra.
![Felhőalapú](./media/multi-factor-authentication-whats-next/custom3.png)
8.  A beállítás: hang fájl feltöltése, kattintson a **Tallózás gombra** , és nyissa meg azt a hangüzenet, kattintson a **Megnyitás**gombra.
![Felhőalapú](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Adjon meg egy leírást, és kattintson a feltöltés elemre.
10. Ha befejeződött, üzenet jelzi, hogy sikerült feltölteni a fájlt.
11. Kattintson a bal oldali Hangüzenetekről.
12. A Hangposta-üzenetek csoportban kattintson az új hangüzenet.
13. Kattintson egy nyelvet a nyelv legördülő listában.
14. Ha ez az üzenet egy adott alkalmazáshoz, adja meg az alkalmazás mezőbe.
15. Az üzenet típusból válassza ki a saját új egyéni üzenettel felülbírálását üzenet.
16. Az a hangfájl legördülő listában jelölje ki a hangfájlt.
17. Kattintson a **létrehozása**gombra. Üzenet jelzi, hogy sikeresen létre hangpostaüzenetet.
![Felhőalapú](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Azure többtényezős hitelesítés gyorsítótárazás

Gyorsítótár csoportban adhatja meg egy adott időpontban időszak, hogy automatikusan későbbi hitelesítési kísérlet sikertelen volt. Ez elsősorban a helyszíni rendszerek, például a virtuális Magánhálózati több hitelesítési kérések küldött, miközben még folyamatban van az első kérelmet. Ezzel automatikusan sikeres, miután a felhasználó előrehaladásának az igazolás sikerrel ezután valaki. Figyelje meg, hogy gyorsítótárba nem célja bejelentkezési bővítmények Azure Active Directory használható.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Gyorsítótár-az Azure többtényezős hitelesítés beállítása

1.  Jelentkezzen be http://azure.microsoft.com
2.  Nyissa meg a MFA Kezelőportálja per elemre a lap tetején a képernyőn megjelenő utasításokat.
3.  Az Azure többtényezős hitelesítés felügyeleti portálon kattintson a gyorsítótár beállítása csoportjában.
4.  Kattintson a konfigurálását, oldal gyorsítótárazás új gyorsítótár
5.  Jelölje ki a gyorsítótár és a gyorsítótár másodperc. Kattintson a Létrehozás gombra.

<center>![Felhőalapú](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Megbízható IP-címei

Megbízható IP-címei jellemzi a többtényezős hitelesítés, amely egy felügyelt vagy szövetséges bérlői rendszergazdák kikerülheti többtényezős hitelesítés a felhasználóknak, hogy a vállalat helyi intranet elemet a bejelentkezéshez. A szolgáltatások az Azure Active Directory prémium, vállalati mobilitás csomagja Azure többtényezős hitelesítés licencekkel rendelkező Azure AD-bérlők érhetők el.


Azure AD-bérlő típusa| Rendelkezésre álló megbízható IP-beállítások
:------------- | :------------- |
Felügyelt|Adott IP-címtartományok – a rendszergazdák hagyhatja többtényezős hitelesítés a felhasználóknak, hogy a céges intraneten való bejelentkezéshez az IP-címtartományokat is megadhat.
Összevont|<li>Minden külső felhasználó - összes szövetséges felhasználók aláírási – a feladó a szervezeten belüli többtényezős hitelesítés használatával az Active Directory összevonási szolgáltatások által kibocsátott igényt hagyja ki.</li><li>Adott IP-címtartományok – a rendszergazdák hagyhatja többtényezős hitelesítés a felhasználóknak, hogy a céges intraneten való bejelentkezéshez az IP-címtartományokat is megadhat.

Ki hagyja ki csak a céges intraneten belül a működik. Így például, ha csak a szövetséges felhasználók az összes kijelölt, valamint a felhasználó bejelentkezik az kívül a céges intraneten, adott felhasználónak el kell többtényezős hitelesítést használ, akkor is, ha a felhasználó által az Active Directory összevonási szolgáltatások igénylése hitelesítő. Az alábbi táblázat ismerteti, hogy mikor többtényezős hitelesítés és az alkalmazás jelszóra szükség a corpnet belüli és kívüli a corpnet ha megbízható IP-címei engedélyezve van.


|A megbízható engedélyezett IP-címei| A megbízható letiltva IP-címei
:------------- | :------------- | :------------- |
Sárga ceruzák corpnet|A böngésző flow többtényezős hitelesítés nem szükséges.|Többtényezős hitelesítés szükséges böngésző folyamatok,
|-Ügyfélprogramok alkalmazások a rendszeres jelszavak akkor működik, ha a felhasználó nem hozott létre a minden alkalmazás jelszavakat. Az alkalmazás jelszó létrehozása után az alkalmazás jelszavak szükség.|-Ügyfélprogramok alkalmazást, az alkalmazás jelszavakat
Külső corpnet|A böngésző flow többtényezős hitelesítés szükséges.|A böngésző flow többtényezős hitelesítés szükséges.
|-Ügyfélprogramok alkalmazást, az alkalmazás jelszavakat.|-Ügyfélprogramok alkalmazást, az alkalmazás jelszavakat.

### <a name="to-enable-trusted-ips"></a>Ahhoz, hogy megbízható IP-címei

1. Bejelentkezés az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Az a címtár kattintson a kívánt megbízható IPsing beállítása a címtár.
4. A könyvtár választotta kattintson a Konfigurálás gombra.
5. Többtényezős hitelesítés csoportban kattintson a szolgáltatás-beállítások kezelése lehetőséget.
6. A szolgáltatás beállításai lapon, a megbízható IP-címei a következők közül választhat:

    - Az érkező kérések szövetséges felhasználók a vállalati intranethez származó – az összes szövetséges felhasználók, akik a vállalati hálózathoz való bejelentkezéshez többtényezős hitelesítés használatával az Active Directory összevonási szolgáltatások által kibocsátott igényt hagyja ki.
    - A megadott tartományba esik, a nyilvános IP-címei – kérelmeket megadása az IP-címek a megfelelő mezőkbe CIDR jelölés használatával. Példa: a tartomány xxx.xxx.xxx.1 – az IP-címek xxx.xxx.xxx.0/24 xxx.xxx.xxx.254 vagy xxx.xxx.xxx.xxx/32 egy egyetlen IP-cím. Legfeljebb 50 IP-címtartományok adhat meg.

7. Kattintson a Mentés gombra.
8. Miután a frissítések alkalmazott, akkor kattintson a Bezárás gombra.



![Megbízható IP-címei](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Alkalmazás jelszavak

Egyes alkalmazások, például az Office 2010-ben vagy a régebbi és az Apple Mail többtényezős hitelesítés nem használható.  Az alkalmazások használatához kell "alkalmazás jelszavak" használata helyett a hagyományos jelszavát.  Az alkalmazás jelszót lehetővé teszi, hogy az alkalmazás többtényezős hitelesítés átlépése, és folytathatja a munkát.

>[AZURE.NOTE] Az Office 2013 ügyfélprogramhoz a modern hitelesítési
>
> Az Office 2013-ügyfélprogramok (beleértve az Outlookot) most támogatja az új hitelesítési protokollok, és engedélyezhető többtényezős hitelesítés támogatására.  Ez azt jelenti, hogy miután engedélyezte a, alkalmazás jelszavak nem szükségesek az Office 2013-ügyfeleknek való használatra.  További tudnivalókért lásd: [az Office 2013 modern hitelesítési nyilvános preview bejelentve a](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Fontos tudni az alkalmazás jelszavak

Egy fontos dolgot, amely a tudnivalók az alkalmazás jelszavak listáját a következő:

- Felhasználók beállíthatja, hogy több alkalmazás jelszavak, amely növeli az adatok eltulajdonításának területet. Óta alkalmazás jelszavak nehezen ne feledje, akkor előfordulhat, hogy ösztönzése jegyezze meg, hogy mások. Ez nem ajánlott és kell javasolt, mert csak egy tényező alkalmazás jelszóval bejelentkezéshez szükséges.
- Alkalmazások, amelyek gyorsítótár-jelszavak, és a helyszíni helyzetekben használható előfordulhat, hogy indítsa el a nem működnek, mivel az alkalmazás jelszó állapota ismeretlen kívül a szervezeti azonosítójával. Példa Exchange e-maileket, amelyek a helyszíni azonban az archivált levelezési a felhőben. Nem működik, ugyanazt a jelszót.
- A tényleges jelszó automatikusan létrejön, és nem adja meg a felhasználót. Mivel az automatikusan generált jelszó nehezebb támadó találat, és biztonságosabb.
- Jelenleg csak 40 jelszavak felhasználónként legfeljebb. Annak érdekében, hogy hozzon létre egy újat az alkalmazást a meglévő jelszavak közül törlendő kéri.
- Többtényezős hitelesítés engedélyezve van egy felhasználói fiókot, amikor a legtöbb nem böngésző ügyfelek, mint például az Outlook és a Lync alkalmazás jelszavak is használható, de felügyeleti műveleteket nem lehet elvégezni alkalmazás jelszavak keresztül nem böngésző-alkalmazásokhoz, például a Windows PowerShell használatával, még akkor is, ha az adott felhasználó rendelkezik rendszergazdai fiókkal.  Fiók hozzárendelése szolgáltatáshoz PowerShell parancsfájlok futtatásának erős jelszóval, és ne engedélyezze az adott fiókból többtényezős hitelesítéshez biztosítása.

>[AZURE.WARNING]  Hibrid környezetekben, ahol ügyfelek mindkét helyszíni kommunikál, és automatikus észlelési végpontok cloud App jelszavak nem működnek. Ennek az oka hitelesítést végezni a helyszíni tartomány jelszavak szükséges, és alkalmazás jelszavak szükséges, az a felhő hitelesítést végezni.


### <a name="naming-guidance-for-app-passwords"></a>Elnevezési alkalmazás jelszavakkal kapcsolatos útmutató
Javasoljuk, hogy alkalmazás jelszó nevek tükrözze az eszközön, amelyen használt. Például ha, amelynek nem böngésző alkalmazások, például Outlook, Word és az Excel hordozható, csak szeretne laptopon nevű egy alkalmazás jelszó létrehozása és használata, hogy a jelszó alkalmazás az összes ezeket az alkalmazásokat. Az összes ezeket az alkalmazásokat külön jelszavak hozhat létre, bár nem ajánlott. Az ajánlási módon, hogy egy app jelszóval egy eszközt.


<center>![Felhőalapú](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Szövetséges (SSO) alkalmazás jelszavak
Azure Active Directory összevonási a helyszíni Windows Server Active Directory tartományi szolgáltatások (AD DS) támogat. Ha a szervezet federated(SSO) az Azure Active Directory és fogja kell használnia az Azure többtényezős hitelesítés, akkor az alábbi fontos adatokat, hogy kell szem előtt, ha az alkalmazás a jelszavak használatáról. Ez csak federated(SSO) ügyfelek vonatkozik.

- Az alkalmazás jelszó Azure Active Directory ellenőrzött lesz, és így az összevonási megkerüli. Összevonás csak aktívan használatos alkalmazás jelszó beállítását.
- A felhasználók federated(SSO) soha nem megyünk eltérően a passzív folyamat identitásszolgáltató (IdP). A jelszavakat tárolja a szervezeti azonosítójával. A felhasználó elhagyja a vállalatot, ha rendelkezik ezekkel az információkkal flow szervezeti azonosító használatával DirSync valós időben. Fiók letiltása és törlése órát is igénybe vehet legfeljebb három szinkronizálni, hogy késleltetné alkalmazás jelszó letiltása és törlése az Azure Active Directory.
- A helyszíni hozzáférés-vezérlés ügyfél beállításai vannak nem fogadja el a jelszó alkalmazás
- A helyszíni hitelesítés naplózás / naplózás lehetőség nem érhető el a jelszó alkalmazás
- További végfelhasználói oktatási szükség a Microsoft Lync 2013 ügyfél számára. A szükséges lépéseket olvassa el a hogyan alakíthat át az e-mailben a jelszót az alkalmazás jelszót című témakört.
- Bizonyos speciális építészeti tervek megkövetelheti kombinációjával szervezeti felhasználónév és a jelszavakat és az alkalmazás jelszavak többtényezős hitelesítés az ügyfelekkel, attól függően, hogy hol hitelesítő használatakor. Az ügyfelek számára, hogy egy helyszíni infrastruktúra hitelesítést használja a egy szervezeti felhasználónevével és jelszavával. Az Azure Active Directory hitelesítő ügyfelek az alkalmazás jelszót használja.

Tegyük fel például, architektúrája, amelyek a következőkből áll:

- A helyszíni Active Directory és az Azure Active Directory példány, amelyek összevonása
- Az Exchange online esetén
- A Lync, amely kifejezetten helyszíni használata
- Azure többtényezős hitelesítés használatával


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 Ezekben az esetekben az alábbiakat kell tennie:

- Bejelentkezés a Lyncbe, használja a szervezetek felhasználónevével és jelszavával.
- Amikor megpróbál a címjegyzék keresztül kapcsolódik az Exchange online, az Outlook-ügyfél eléréséhez, használja az alkalmazás jelszót.

### <a name="allowing-app-password-creation"></a>Alkalmazás jelszó létrehozásának engedélyezése
Alapértelmezés szerint a felhasználók nem hozható létre alkalmazás jelszavakat.  Ez a funkció engedélyezve kell lennie.  Felhasználói jelszavak alkalmazás létre engedélyezéséhez a következő eljárással.

#### <a name="to-enable-users-to-create-app-passwords"></a>Ha engedélyezni szeretné a felhasználóknak alkalmazás jelszavak létrehozása



1. Jelentkezzen be az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Az a címtár kattintson a az engedélyezni kívánt felhasználó a címtárban.
4. A képernyő tetején kattintson a felhasználók elemre.
5. A lap alján kattintson a többtényezős hitelesítési kezelése  
6. Többtényezős hitelesítés lap tetején kattintson a szolgáltatás beállításai parancsra.
7. Győződjön meg arról, hogy be van jelölve a választógomb mellett engedélyezése a felhasználóknak létrehozásához jelentkezzen be az nem böngésző alkalmazások alkalmazás a jelszavakat.


![Alkalmazás jelszavak létrehozása](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Alkalmazás jelszavak létrehozása
A kezdeti regisztráció során a felhasználók hozhatnak létre alkalmazás jelszavakat.  Felsorolásukat végén található a regisztrációs folyamat, amely lehetővé teszi, hogy hozhat létre őket egy lehetőséget.

Ezenkívül felhasználók jelszót is készíthet alkalmazást később azok az Azure-portálon az Office 365-portál beállításainak módosításával vagy szerint

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Alkalmazás jelszavak létrehozása az Office 365 portálon
--------------------------------------------------------------------------------


1. Jelentkezzen be az Office 365 portálon
2. A jobb felső sarokban válassza a beállítások listáját megjelenítő vezérlővel
3. A bal válassza a további biztonsági ellenőrzése
4. A jobb oldalon válassza a **fiók biztonsági használt Telefonszámaim frissítése**
5. A proofup lapon, a képernyő tetején jelölje be az alkalmazás jelszavak
6. Kattintson a **Létrehozás** gombra
7. Írjon be egy nevet az alkalmazás jelszót, és kattintson a **Tovább** gombra
8. Az alkalmazás jelszót másolja a vágólapra, és illessze be az alkalmazás.

<center>![Felhőalapú](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Alkalmazás jelszavak létrehozása az Azure-portálon
--------------------------------------------------------------------------------
1. Jelentkezzen be az Azure klasszikus portálra.
3. A képernyő tetején kattintson a jobb gombbal a felhasználó nevét, és válassza a további biztonsági ellenőrzési.
5. A proofup lapon, a képernyő tetején jelölje be az alkalmazás jelszavak
6. Kattintson a **Létrehozás** gombra
7. Írjon be egy nevet az alkalmazás jelszót, és kattintson a **Tovább** gombra
8. Az alkalmazás jelszót másolja a vágólapra, és illessze be az alkalmazás.


![Alkalmazás jelszavak](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Alkalmazás jelszavak létrehozásához, ha nem rendelkezik az Office 365-ben vagy az Azure-előfizetéssel
--------------------------------------------------------------------------------
1. Jelentkezzen be a [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. A képernyő tetején jelölje ki a profilt.
3. Kattintson a felhasználónevét, majd válassza a további biztonsági ellenőrzési.
5. A proofup lapon, a képernyő tetején jelölje be az alkalmazás jelszavak
6. Kattintson a **Létrehozás** gombra
7. Írjon be egy nevet az alkalmazás jelszót, és kattintson a **Tovább** gombra
8. Az alkalmazás jelszót másolja a vágólapra, és illessze be az alkalmazás.

![Alkalmazás jelszavak](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Többtényezős hitelesítés eszközök felhasználók adatvédelmi emlékeztető

Többtényezős hitelesítés tárolása az eszközök és böngészők, hogy a felhasználók adatvédelmi egy ingyenes szolgáltatás MFA minden felhasználónál.  Lehetővé teszi a felhasználóknak megadni a vezérlőt, amellyel azt MFA beállítása a egy sikeres elvégzése után a napok száma a bejelentkezés MFA használatával. Ez bővítheti a használhatóság a felhasználók számára.

Azonban a felhasználók ne feledje, hogy MFA megbízható eszközök szabad, mivel ez a funkció csökkentheti a fiók biztonsági. Ahhoz, hogy a fiók biztonsági, meg kell visszaállítása többtényezős hitelesítés az eszközök vagy a következő esetekben:

- Ha van a vállalati fiók sérül válik
- Ha egy megjegyzett eszközt elveszett vagy ellopott

> [AZURE.NOTE] Ez a funkció, a böngésző cookie-k gyorsítótárának kiürítése történik. Még nem működik, ha a cookie-k nincsenek engedélyezve.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Hogyan tárolása többtényezős hitelesítés engedélyezése és letiltása

1. Jelentkezzen be az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Az Active Directory kattintson a kívánt eszközök ne feledje, hogy többtényezős hitelesítés beállítása a címtár.
4. A könyvtár választotta kattintson a Konfigurálás gombra.
5. Többtényezős hitelesítés csoportban kattintson a szolgáltatás-beállítások kezelése lehetőséget.
6. A szolgáltatás beállításai lapon a felhasználói eszköz beállításainak kezelése csoportban az **Ne feledje, hogy azok megbízható eszközön többtényezős hitelesítés engedélyezése a felhasználóknak**kiválasztása/kijelölés megszüntetése.
![Ne feledje, hogy a eszközök](./media/multi-factor-authentication-whats-next/remember.png)
8. Állítsa az, hogy hány napig felfüggesztése engedélyezni. Az alapértelmezett 14 napig tart.
9. Kattintson a Mentés gombra.
10. Kattintson a Bezárás gombra.


## <a name="selectable-verification-methods"></a>Ügyféloldalon választható hitelesítési módszerek
A felhő és a helyszíni verziójában megadhatja, melyik hitelesítési módszerek állnak rendelkezésre a felhasználók számára. Az alábbi táblázat az egyes módszerek rövid áttekintést nyújt.

Amikor a felhasználók fiókjukat többtényezős igényléséhez választanak az előnyben részesített ellenőrzési módszer ki a beállításokat, hogy engedélyezve van. Az útmutatást a regisztrációs folyamat foglalt [kétlépcsős hitelesítési a saját fiók beállítása](multi-factor-authentication-end-user-first-time.md)

A módszer|Leírás
:------------- | :------------- |
Hívás telefonra |  Az automatikus hanghívás a hitelesítési telefon helyezi. A felhasználó fogadja a hívást, majd a # megnyomja a telefon billentyűzeten hitelesítést végezni. A helyszíni Active Directory nem szinkronizálja a telefonszámot.
Szöveges üzenet-telefon | A felhasználó számára egy Ellenőrzőkód tartalmazó szöveges üzenetet küld. A rendszer kéri a felhasználó vagy válasz szöveges üzenet az ellenőrzőkódot, vagy írja be a bejelentkezési felületén a az ellenőrzőkódot.
Értesítés keresztül mobilalkalmazásban | Ebben a módban a Microsoft Authenticator alkalmazás megakadályozza, hogy a ezzel az illetéktelen hozzáférést fiókokhoz, és leállítja a csalárd tranzakciók. Ehhez használja egy telefonról vagy eszközről regisztrált a leküldéses értesítést. Csak a megtekintését az értesítés és jogos esetén ellenőrizze koppint. Egyéb esetben előfordulhat, hogy válassza a Megtagadás, vagy megtagadja, és jelentse a csalárd értesítés beállíthatja. A Megtagadás és a jelentés csalás funkció használatával többtényezős hitelesítéshez témakörben olvashat a jelentési csalárd értesítések.</br></br>A Microsoft Authenticator alkalmazás [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)érhető el.|
Ellenőrzőkódot mobilappból | Ebben a módban a Microsoft Authenticator alkalmazás használható a szoftver jogkivonat, az elfogadható ellenőrzőkódot létrehozásához. A ellenőrzőkódot majd meg lehet adni a felhasználónév és jelszó megadására, a második hitelesítési mód együtt</li><br><p> A Microsoft Authenticator alkalmazás [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)érhető el.

### <a name="how-to-enabledisable-authentication-methods"></a>Hogyan hitelesítési módszereket engedélyezése vagy letiltása

1. Jelentkezzen be az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Az Active Directory kattintson a kívánt engedélyezése vagy letiltása a hitelesítési módszereket könyvtár a.
4. A könyvtár választotta kattintson a Konfigurálás gombra.
5. Többtényezős hitelesítés csoportban kattintson a szolgáltatás-beállítások kezelése lehetőséget.
6. A szolgáltatás beállításai lapon a tartomány ellenőrzéséhez beállításai csoportban jelölje be/kijelölés megszüntetése a használni kívánt beállításokat.</br></br>
![Hitelesítési beállítások](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Kattintson a Mentés gombra.
10. Kattintson a Bezárás gombra.
