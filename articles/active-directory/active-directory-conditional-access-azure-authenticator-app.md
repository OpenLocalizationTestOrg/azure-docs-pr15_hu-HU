
<properties
    pageTitle="Az Android-alapú Azure hitelesítő |} Microsoft Azure"
    description="Microsoft Azure hitelesítő alkalmazás való bejelentkezéshez a munka típusú erőforrások elérésére használható. Az Azure hitelesítő alkalmazás értesítést küld a függőben lévő varianciaanalízis ellenőrzési kérelmének jelennek meg értesítés a mobileszközre."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="azure-authenticator-for-android"></a>Az Android-alapú Azure hitelesítő

A rendszergazda segítségét előfordulhat, hogy rendelkezik ajánlott szeretne használni, a Microsoft Azure hitelesítő való bejelentkezéshez a munka típusú erőforrások eléréséhez. Ez az alkalmazás a következő két bejelentkezési lehetőséget nyújt:

* Többtényezős hitelesítés lehetővé teszi a munkahelyi vagy iskolai fiók használatához kétlépcsős hitelesítési a biztonságos. Akkor bejelentkezés használata valamit, amit meg, hogy (például a jelszó), és a fiók védelme még tovább számmal van (kulcs a alkalmazásból). Az Azure hitelesítő alkalmazás értesítést küld a függőben lévő varianciaanalízis ellenőrzési kérelmének jelennek meg értesítés a mobileszközre. Egyszerűen megtekintése az alkalmazást, és koppintson a kérelem ellenőrzése, illetve megszüntetése kell. Másik megoldás kérheti a jelenik meg az alkalmazás hozzáférési kódjának megadásához.

* A munkahelyi fiókjával lehetővé teszi az Android-telefonon vagy táblagépen alakítani egy megbízható eszközt, és adja meg az egyszeri bejelentkezés (SSO) vállalati alkalmazások. A rendszergazda segítségét előfordulhat, hogy csak munkahelyi fiók hozzáadása a vállalati erőforrások elérése érdekében. Egyszeri bejelentkezés segítségével egyszeri bejelentkezés, és automatikusan veheti igénybe a bejelentkezés az Önnek a vállalat elérhetővé tett mindegyik számára.

## <a name="installing-the-azure-authenticator-app"></a>Az Azure hitelesítő alkalmazás telepítése

Az Azure hitelesítő alkalmazást telepítheti a Google Play áruházból.
Az utasításokat követve munkahelyi fiók hozzáadása a Samsung Android rendszerű eszköz és egy nem Samsung Android rendszerű készüléken kissé eltérnek. Az utasításokat, mindkét felsorolása olvasható.

<a name="adding-the-work-account-from-samsung-android-device"></a>A munkahelyi fiók hozzáadása Samsung Android-eszközön
----------------------------------------------------------------------------------------------------------------
###<a name="adding-the-work-account-through-the-app-home-screen"></a>A munkahelyi fiók – az alkalmazás kezdőképernyőjén

Az alábbi lépéseket kell alkalmazni Samsung GS3 és telefonokon vagy 2. táblagépeken felett.

1. A az alkalmazás kezdőképernyőjén fogadja el a végfelhasználói licencszerződésben találja (EULA).
2. A fiók aktiválásához képernyőn kattintson a jobb oldalon a helyi menü, és válassza a **munkahelyi fiókjával**.
3. A fiók hozzáadása képernyőn jelölje be** A munkahelyi fiókjával**.
4. Aktiválás eszközt rendszergazdai képernyő kattintson az **Aktiválás**gombra.
5. Adatvédelmi nyilatkozat képernyőn jelölje be a jelölőnégyzetet, és kattintson a **Megerősítés**gombra.
6. A munkahelyi Bekapcsolódás képernyőn adja meg a felhasználóazonosító a szervezete által biztosított, és kattintson a **Csatlakozás**gombra.
7. Jelentkezzen be az Azure hitelesítő alkalmazást, írja be a szervezeti egy x Ügyfélszám és a jelszavát, és kattintson a **Bejelentkezés**gombra.
8. A következő képernyőn többtényezős hitelesítés (MFA) adatait megjelenítő van a fokozott biztonság és a lépés nem kötelező. Ha a munkahelyi vagy iskolai második varianciaanalízis hitelesítést igényel munkahelyi fiók létrehozása az alábbi képernyő jelenik meg. További-fiók ellenőrzése utasításokat biztosít.
9. A munkahelyi csatlakozási képernyője jeleníti meg az üzenetet, a "**Csatlakozás a munkahelyi**". Az Azure hitelesítő alkalmazást az eszköz munkahelyével csatlakozni próbál.
10. A következő képernyőn látnia kell a tartományhoz munkahelye üzenetet.

>[AZURE.NOTE]
> Egy egyetlen munkahelyi fiókjából az eszközön használhatók.

### <a name="adding-the-work-account-from-the-settings-menu"></a>A munkahelyi fiók hozzáadásához kattintson a beállítások menü
Az Azure hitelesítő alkalmazás telepítése után is létrehozhat egy munkahelyi fiókjából az Android-alapú fiókkezelő.

1. Kattintson a beállítások menü **fiókok** keresse meg, és kattintson a **Fiók hozzáadása**gombra.
2. Kövesse a lépéseket a munkahelyi fiók alkalmazás kezdőképernyőjén munkahelyi fiók felvétele – 3 – 10 lépéseit.

<a name="adding-the-work-account-from-a-non-samsung-android-device"></a>A munkahelyi fiók hozzáadása a Samsung Android-eszközről
------------------------------------------------------------------------------------------------------------------
### <a name="adding-the-work-account-through-the-app-home-screen"></a>A munkahelyi fiók – az alkalmazás kezdőképernyőjén

1. A az alkalmazás kezdőképernyőjén fogadja el a végfelhasználói licencszerződésben találja (EULA).
2. A fiók aktiválásához képernyőn kattintson a jobb oldalon a helyi menü, és válassza a **munkahelyi fiókjával**.
3. A fiókok képernyőn kattintson a **Fiók hozzáadása**gombra.
4. Ha a partnerek képernyőn látható, kattintson a **fiók hozzáadása**gombra. Ha egy munkahelyi fiókkal már korábban létrehozott, látni fogja a szinkronizálási képernyőjén látható, a meglévő munkahelyi fiók. A munkahelyi fiók segítségével megőrizheti a egyszerűen koppintva a vissza nyílra a kezdőképernyőre. Másik megoldás választhatja ki a fiók eltávolítása és újbóli létrehozása egy új munkahelyi fiók meg a munkahelye csatlakozási képernyője, adja meg a felhasználóazonosító a szervezete által biztosított és kattintson a Bekapcsolódás elemre.
5. Jelentkezzen be a hitelesítő Azure-alkalmazásba, adja meg a szervezeti fiók jelszavát, és kattintson a **Bejelentkezés**gombra.
7. A következő képernyőn többtényezős hitelesítés (MFA) adatait megjelenítő van a fokozott biztonság és a lépés nem kötelező. Ha a munkahelyi vagy iskolai második varianciaanalízis hitelesítést igényel munkahelyi fiók létrehozása az alábbi képernyő jelenik meg. További-fiók ellenőrzése utasításokat biztosít.
8. A következő képernyőn kattintson az **OK gombra** . A tanúsítvány neve nem változik.
az üzenet, "Csatlakozás a munkahelyi". Az Azure hitelesítő alkalmazást az eszköz munkahelyével csatlakozni próbál.
A következő képernyőn látnia kell a munkahelye csatlakozott üzenetet.

>[AZURE.NOTE]
> Egy egyetlen munkahelyi fiókjából az eszközön használhatók.

Az Azure hitelesítő alkalmazás telepítése után is létrehozhat egy munkahelyi fiókjából az Android-alapú fiókkezelő.

1. Kattintson a **Beállítások** menü fiókok keresse meg, és kattintson a **Fiók hozzáadása**gombra.
2. Kövesse a lépéseket, – az alkalmazás kezdőlap képernyőn **, a munkahelyi fiók hozzáadása munkahelyi fiók felvétele 2 – 7 lépéseit.

### <a name="how-to-find-out-which-version-is-installed"></a>Hogyan megtudhatja, hogy melyik verziója van telepítve

1. Láthatja, hogy a Azure hitelesítő alkalmazás melyik verzióját, és a társított szolgáltatás verziókkal telepítve az eszközön.
2. Kattintson az előugró menü **kapcsolatban**.
3. A névjegy képernyő megjeleníti a szolgáltatásokat az alkalmazás és a verziók telepítve az eszközön.
 
### <a name="sending-log-files-to-report-issues"></a>Naplófájlok küld a kimutatásokkal kapcsolatos problémák

1. Hajtsa végre a lépéseket a Microsoft Support az Azure hitelesítő alkalmazással az esemény jelentést, az esemény számnak beszerzése és küldje el a naplófájlokat szemben a hozzárendelt az esemény számot az alábbi képlettel történik:
2. Az előugró menüben kattintson a **Bejelentkezés**elemre.
3. Ha a Microsoft Support megnyitott esemény, jegyezze fel a az esemény szám (szüksége lesz rá egy újabb lépés). Ha még nem hozott létre a támogatási kérelmeiket és támogatási szeretné, hajtsa végre az útmutató [A Microsoft támogatási](https://support.microsoft.com/en-us/contactus) megnyitása egy új esemény elemre.
4. A naplózás képernyőn kattintson a **Küldés gombra**.
5. Jelölje ki a használni kívánt e-mail szolgáltatónál.
7. Ha már van Microsoft Support megnyitott esemény, lépjen kapcsolatba a támogatási adatbázismodellbe rendelt megtudhatja, hogy miként küldhet a naplóadatokat a problémát, és társította az esemény. A támogatás adatbázismodellbe biztosítja az e-mail címet és tárgyat sorhoz adatokkal. Ha még nem rendelkezik a támogatási kérelmeiket, kövesse az útmutatást a Microsoft Support megnyitása egy új esemény.
9. Szerkessze a **címzett** sorban és **a Microsoft Support kapott adatokat tárgyát** .
10. Az Azure hitelesítő alkalmazást a naplófájl csatolja az e-mailt küldi. Írja le a problémát tapasztal, frissítse a címzettek listájának (nem kötelező), és az e-mailt küldeni.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>A munkahelyi fiók törlése és a munkahelye elhagyása

A munkahelyi fiókjával létrehozott bármikor módon távolíthatja el:

**A beállítások menüben a munkahelyi fiók törlése**

1. Jelölje ki a fiókkezelő **munkahelyi fiókjával**.
2. A munkahelyi fiókjával képernyőn az **Általános beállítások**csoportban válassza a **Fiókbeállítások – hagyja a munkahelyi hálózaton**.
3. Válassza a **Kilépés** a **Munkahelye csatlakozási** képernyője.
4. Amikor a "Is szeretne munkahelye hagyja" üzenet jelenik meg, kattintson az **OK gombra** .
5. Ezzel biztosíthatja, hogy a munkaterület munkahelyi fiókját törölt.

>[AZURE.NOTE]
>Ajánlott nem használható a fiók eltávolítása lehetőséget munkahelyi fiók törlése, mint ez a beállítás nem működik az Android-alapú egyes korábbi verzióiban.

##<a name="uninstalling-the-app"></a>Az alkalmazás eltávolítása

Samsung Android-eszközön eszközt rendszergazdai jogosultságokkal el kell távolítani a következőképpen eltávolítása előtt a 
1. Kattintson a **biztonsági** **Beállítások**, a **rendszer**.
2. D**evice felügyeleti**kattintson az **eszköz rendszergazdák**gombra. Győződjön meg arról, hogy az **Azure hitelesítő** melletti jelölőnégyzetből.

##<a name="troubleshooting"></a>Hibaelhárítás

**Keystore hiba**jelenik meg, ha ez akkor fordulhat elő nincs zárolás beállítása képernyőn be a PIN-kódot. Ez a probléma megkerüléséhez távolítsa el a Azure hitelesítő alkalmazást, konfigurálása a PIN-kódot a zárolási képernyőn, és telepítse újra az alkalmazást.
