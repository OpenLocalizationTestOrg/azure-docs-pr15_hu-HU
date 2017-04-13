<properties
    pageTitle="A helyszíni feltételes elérésének Azure Active Directory eszköz regisztrációs beállítása |} Microsoft Azure"
    description="Feltételes hozzáférést biztosít a helyszíni alkalmazások használata az Active Directory összevonási szolgáltatás (AD FS) a Windows Server 2012 R2 részletesen ismerteti."
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


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>A helyszíni feltételes elérésének Azure Active Directory eszköz regisztrációs beállítása

Személyes azonosításra a felhasználók saját tulajdonú eszközök jelölhető ismert a szervezet által a felhasználók munkahelyi illesztés megkövetelése a eszközök: az Azure Active Directory eszköz regisztrációs szolgáltatás. Az alábbi képen a feltételes hozzáférést biztosít a helyszíni alkalmazások használata az Active Directory összevonási szolgáltatás (AD FS) a Windows Server 2012 R2 cikk részletesen ismerteti.

> [AZURE.NOTE]
> Az Office 365-licenc és Azure Active Directory prémium licenc szükség, az Azure Active Directory eszköz regisztrációs szolgáltatás feltételes access házirendek eszközökkel regisztrálásakor. Ide tartoznak a házirendek vannak érvényben az Active Directory összevonási szolgáltatások (AD FS), hogy a helyszíni erőforrásokhoz.

Olvashat a feltételes hozzáférési környezetek kialakításához, helyszíni telepítésű olvassa el a [csatlakozzon, bármilyen eszközről, az egyszeri bejelentkezés, és folytonos második tényező hitelesítési különböző vállalati alkalmazások munkahelye](https://technet.microsoft.com/library/dn280945.aspx)című témakört.

Ezek a funkciók vásárolhat az Azure Active Directory prémium licenc felhasználóknak érhetők el.

<a name="supported-devices"></a>Támogatott eszközök
-------------------------------------------------------------------------
* Windows 7 tartományhoz csatlakoztatott eszközökön.
* Windows 8.1 személyes és a tartomány csatlakozott az eszközöket.
* iOS 6-os vagy újabb verzió, a Safari böngészőt
* Android 4.0-s vagy újabb verzió, Samsung GS3 vagy feletti telefonok, Samsung 2., illetve táblagépeken felett.


<a name="scenario-prerequisites"></a>Forgatókönyv vonatkozó követelmények
------------------------------------------------------------------------
* Az Office 365-előfizetés vagy az Azure Active Directory-támogatás
* Az Azure Active Directory-bérlői webhelyen
* Windows Server Active Directory (Windows Server 2008 vagy újabb)
* A Windows Server 2012 R2 frissített séma
* Licenc az Azure Active Directory prémium verzió
* A Windows Server 2012 R2 összevonási szolgáltatásokat, állítva az Azure Active Directory való egyszeri bejelentkezés
* A Windows Server 2012 R2 webes alkalmazás Proxy a Microsoft Azure Active Directory csatlakoztatása (Azure AD Connect). [Itt letöltése Azure AD Connect](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* A tartomány ellenőrzött.

<a name="known-issues-in-this-release"></a>Ebben a kiadásban ismert problémák
-------------------------------------------------------------------------------
* Alapuló feltételes access házirendeket Active Directoryhoz eszköz objektum írási-vissza kell az Azure Active Directory. Az eszköz objektumok írt vissza az Active Directory legfeljebb 3 óráig is eltarthat
* az iOS 7-eszközök mindig kéri a felhasználót, hogy jelöljön ki egy tanúsítványt során az ügyfél tanúsítvány-hitelesítést.
* IOS8, mielőtt iOS 8.3 nem működnek az egyes verzióiban.

## <a name="scenario-assumptions"></a>Forgatókönyv feltételezések
Ebben az esetben feltételezi, hogy egy hibrid környezet az Azure AD-bérlő és a helyszíni Active Directory álló. Azure AD Connect használja ezeket a bérlők kell csatlakoztatni egy igazolt tartományt, és AD FS-féle egyszeri Bejelentkezést. Az alábbi ellenőrzőlista konfigurálása a fentebb ismertetett szakaszba a környezetet nyújt segítséget.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Feladatlista: Feltételes Access forgatókönyv előfeltételei
--------------------------------------------------------------
Csatlakozás a Azure AD-bérlő a helyszíni Active Directoryval.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory eszköz regisztrációs szolgáltatás konfigurálása
Ez az útmutató segítségével üzembe helyezéséhez és Azure Active Directory eszköz regisztrációs szolgáltatás konfigurálása a szervezet számára.

Ez az útmutató feltételezi, hogy meg van konfigurálva a Windows Server Active Directory és a Microsoft Azure Active Directory előfizetett. Lásd: a fenti előfeltételek.

Azure Active Directory eszköz regisztrációs szolgáltatás telepítése az Azure Active Directory bérlővel rendelkező, az alábbi ellenőrzőlistát sorrendben feladatok végrehajtása. Ha egy hivatkozást megnyitja a elvi tematikus, ezzel az ellenőrzőlistával, hogy a hátralévő feladatokat ezzel az ellenőrzőlistával folytatja a elvi témakör áttekintése után vissza. Bizonyos feladatok fogja tartalmazni, amelyek segítséget nyújtanak a lépés sikeresen befejeződött megerősítése forgatókönyv érvényességi lépés.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>1 rész: Engedélyezése Azure Active Directory eszköz regisztráció

Hajtsa végre az alábbi engedélyezése és beállítása az Azure Active Directory eszköz regisztrációs szolgáltatás ellenőrzőlista.

| Tevékenység                                                                                                                                                                                                                                                                                                                                                                                             | Hivatkozás                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Ha engedélyezni szeretné a munkahely csatlakozni eszközök a Azure Active Directory-bérlő eszköz regisztrációs engedélyezése Alapértelmezés szerint a többtényezős hitelesítés a szolgáltatás nincs engedélyezve. Többtényezős hitelesítés ajánlott azonban egy eszközt regisztrálásakor. A ADRS többtényezős hitelesítés engedélyezése, előtt bizonyosodjon meg arról, hogy az AD FS többtényezős hitelesítés szolgáltató. | [Azure Active Directory eszköz regisztrációs engedélyezése](active-directory-conditional-access-device-registration-overview.md)               |
| Eszközök jól ismert DNS-rekordok megjeleníti az Azure Active Directory eszköz regisztrációs szolgáltatás Fedezze fel. A vállalat DNS meg kell adnia, így eszközök, észleljék az Azure Active Directory eszköz regisztrációs szolgáltatás.                                                                                                                                                   | [Állítsa be az Azure Active Directory eszköz regisztrációs feltárás.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>2 rész: Üzembe helyezéséhez és a Windows Server 2012 R2 Active Directory összevonási szolgáltatások konfigurálása és az Azure Active Directory összevonási kapcsolat beállítása

| Tevékenység                                                                                                                                                                                                                                                                                                                                                                                             | Hivatkozás                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Telepítse az Active Directory tartományi szolgáltatások tartomány a Windows Server 2012 R2 séma kiterjesztésű. Nem kell frissíteni a tartomány vezérlők bármelyikét Windows Server 2012 R2. A séma frissítés egyetlen követelménye. | [Az Active Directory tartományi szolgáltatások séma frissítése](#upgrade-your-active-directory-domain-services-schema)               |
| Eszközök jól ismert DNS-rekordok megjeleníti az Azure Active Directory eszköz regisztrációs szolgáltatás Fedezze fel. A vállalat DNS meg kell adnia, így eszközök, észleljék az Azure Active Directory eszköz regisztrációs szolgáltatás.                                                                                                                                                   | [Felkészülés az Active Directory-támogatás eszközökről](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>3 rész: Engedélyezése eszköz visszaírást az Azure Active Directory

| Tevékenység                                                                                                                                                                                                                                                                                                                                                                                             | Hivatkozás                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| A feladat befejezése engedélyezése eszköz visszaírást az Azure AD Connect 2 részét. Befejeztével vissza Ez az útmutató. | [Az Azure AD Connect eszköz visszaírást engedélyezése](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Választható] Rész 4: Többtényezős hitelesítés engedélyezése

Ajánlott a több lehetőségekkel többtényezős hitelesítés beállítása. MFA megkövetelésére, című témakörben olvashat [Válassza ki a többtényezős biztonsági megoldás](../multi-factor-authentication/multi-factor-authentication-get-started.md). Minden megoldást, és a konfigurálásához lehetőség a megoldást nyújtanak segítséget leírását tartalmazza.

## <a name="part-5-verification"></a>5 rész: ellenőrzése

A telepítés befejeződött. Bizonyos esetekben most próbálja. Hajtsa végre az alábbi hivatkozásokra kattintva a szolgáltatás kipróbálhat, és megismerkedhet a szolgáltatásokkal


| Tevékenység                                                                                                                                                                                                                         | Hivatkozás                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Egyes eszközök esetén csatlakozzon a munkahelyen használja az Azure Active Directory eszköz regisztrációs. Bekapcsolódhat az iOS, a Windows és az Android-eszközön                                                                                         | [Csatlakozás eszközök munkahelyével Azure Active Directory eszköz regisztrációs használatával](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Megtekintheti és engedélyezhetik/letilthatják a rendszergazda portálon bejegyzett eszközök. Ebben a feladatban nézni fog az egyes bejegyzett eszközök esetén a rendszergazda portálon.                                                        | [Azure Active Directory eszköz regisztráció áttekintése](active-directory-conditional-access-device-registration-overview.md)                             |
| Győződjön meg arról, hogy eszköz objektumok kerülnek vissza az Azure Active Directory Windows Server Active Directory.                                                                                                                  | [Győződjön meg róla bejegyzett eszközök írt vissza Active Directoryhoz](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Most, hogy a felhasználók saját eszközök regisztrálhat, alkalmazás hozhat létre access házirendeket, az Active Directory összevonási szolgáltatások, amelyek lehetővé teszik csak regisztrált eszközöket. Ebben a feladatban létrehoz egy alkalmazás hozzáférési szabályokat és egy egyéni-hozzáférési megtagadási üzenet | [Egy alkalmazás hozzáférési házirendet, és az egyéni hozzáférés-megtagadási üzenet létrehozása](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integrálása helyszíni Active Directory
Ez segít a Azure AD-bérlő integrálása a helyszíni active directory Azure AD Connect használata. Bár a lépéseket az Azure klasszikus portálon érhetők el, jegyezze fel az ebben a részben felsorolt speciális utasításokat.

1.  Jelentkezzen be az Azure klasszikus portálon, amely az Azure Active Directory egy globális rendszergazdai fiókkal.
2.  A bal oldali ablaktáblában jelölje ki az **Active Directory**.
3.  A **címtár** lapon jelölje ki a címtárban.
4.  Jelölje ki a **Címtár-integrációs** fülre.
5.  **Üzembe helyezéséhez és kezeléséhez** csoportban kövesse az 1-3-Azure Active Directory integrálása helyszíni címtárában.
  1.    Adja hozzá a tartományok.
  2.    Telepítése és futtatása Azure AD Connect: [egyéni telepítési Azure AD Connect az](./connect/active-directory-aadconnect-get-started-custom.md)alábbi útmutatást követve telepítse az Azure AD Connect.
  3. Ellenőrizze, és a címtár-szinkronizálás kezelése. Ebben a lépésben belül egyszeri bejelentkezéses utasításokat érhetők el.

  > [AZURE.NOTE]
  > Szövetséges Active Directory összevonási szolgáltatások konfigurálása a fenti kapcsolódó dokumentumot című témakörben ismertetett módon. Nem kell konfigurálni az előnézeti funkciók bármelyikét.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Az Active Directory tartományi szolgáltatások séma frissítése

> [AZURE.NOTE]
> Frissítése az Active Directory-séma nem vonható vissza. Javasoljuk, hogy először elkészíti a tesztkörnyezetben.

1. Jelentkezzen be a tartományvezérlőnek a vállalati rendszergazda és a séma rendszergazdai jogokkal rendelkező fiók.
2. Másolja a **[media] \support\adprep** könyvtár és a részleges könyvtárak az Active Directory tartományi vezérlőket.
3. [Media] hol található a elérési útját a Windows Server 2012 R2 telepítési adathordozóját.
4. A parancssorból, nyissa meg azt a adprep könyvtár, majd hajtsa végre: **adprep.exe forestprep**. Kövesse a képernyőn megjelenő utasításokat a séma frissítési folyamat befejezéséhez.

## <a name="prepare-your-active-directory-to-support-devices"></a>Felkészülés az Active Directory eszközök támogatásához

>[AZURE.NOTE] Ez a futtatnia kell készítse elő az Active Directory erdőt eszközök támogatásához egyszeri műveletet. Vállalati rendszergazdai engedélyekkel kell bejelentkeznie, és az Active Directory-erdő rendelkeznie kell a Windows Server 2012 R2 séma, ez az eljárás befejezéséhez.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Készítse elő az Active Directory erdőt eszközök támogatásához

> [AZURE.NOTE]
>Ez a futtatnia kell készítse elő az Active Directory erdőt eszközök támogatásához egyszeri műveletet. Vállalati rendszergazdai engedélyekkel kell bejelentkeznie, és az Active Directory-erdő rendelkeznie kell a Windows Server 2012 R2 séma, ez az eljárás befejezéséhez.

### <a name="prepare-your-active-directory-forest"></a>Készítse elő az Active Directory erdőt

1.  Az összevonási kiszolgáló nyissa meg a Windows PowerShell-parancsablakban és típusa: Initialize-ADDeviceRegistration
2.  **ServiceAccountName**megjelenésekor adja meg a szolgáltatásfiók, az Active Directory összevonási szolgáltatások választotta a fiók nevét. Ha egy gMSA fiókot, adja meg a fiók **domain\accountname$** formátumban. Tartományi fiók használja a formátum **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>Az Active Directory összevonási szolgáltatások eszköz-hitelesítés engedélyezése

1. Az összevonási kiszolgáló nyissa meg az Active Directory összevonási szolgáltatások kezelése konzolról, és nyissa meg azt **az AD FS** > **Hitelesítési házirendek**.
2. Válassza a **Szerkesztés globális elsődleges hitelesítési...** a **Műveletek** ablak.
3. Ellenőrizze, hogy **eszköz-hitelesítés engedélyezése** , és kattintson**az OK**gombra.
4. Alapértelmezés szerint az AD FS rendszeres eltávolítja még nem használt eszközök Active Directoryból. Azure Active Directory eszköz regisztrációs használatakor, így eszközök kezelhető Azure-ban, le kell tiltania ezt a feladatot.


### <a name="disable-unused-device-cleanup"></a>Még nem használt eszköz karbantartása letiltása
1. Az összevonási kiszolgáló nyissa meg a Windows PowerShell-parancsablakban és típusa: Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Azure AD Connect előkészítése eszköz visszaírást

1.  Hajtsa végre a kijelző 1: Felkészülés az Azure Active Directory csatlakozni.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Csatlakozás eszközök munkahelyével Azure Active Directory eszköz regisztrációs használatával

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Bekapcsolódás az Azure Active Directory eszköz regisztrációs használatával iOS-eszközökön

Azure Active Directory eszköz regisztrációs az iOS-eszközök a Over-the-Air profil regisztrációs folyamat használja. Ez az eljárás a felhasználói profil regisztrációs URL-CÍMÉT a Safari böngészővel való csatlakozás kezdődik. Az URL-cím esetén az alábbi képlettel történik:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Ha `yourdomainname` a tartománynév Azure Active Directory beállított. Például: contoso.com a tartománynév esetén az URL-címet a következő lesz:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Nincsenek számos különböző módon, ez az URL a felhasználók kapcsolatba lépni. Egyik javasolt módja az URL-cím közzététele egy egyéni alkalmazás hozzáférést az üzenetet, az Active Directory összevonási szolgáltatások megtagadva. Ez a közelgő szakasz foglalt: [az alkalmazás hozzáférési házirend létrehozása és egyéni hozzáférés-megtagadási üzenetet](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Csatlakozás Windows 8.1 eszköz használatával az Azure Active Directory eszköz regisztráció

1. A Windows 8.1-készüléken nyissa meg azt a **Gépház** > **hálózati** > **munkahelye**.
2. Írja be a felhasználónevét (UPN) formátumban. Ha például dan@contoso.com..
3. Válassza a **Csatlakozás**.
4. Amikor a rendszer kéri, jelentkezzen be a hitelesítő adatait. Az eszköz gombra csatlakozott.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Bekapcsolódás egy Windows 7 eszköz használatával az Azure Active Directory eszköz regisztráció
Windows 7 tartomány regisztrálását csatlakozott a kell telepíteni az eszköz regisztrációs szoftvercsomag eszközök. A szoftvercsomag munkahelye Bekapcsolódás a Windows 7 neve, és letölthető a [Microsoft Connect webhelyet](https://connect.microsoft.com/site1164). A csomag használata érhetők el a [Windows 7-tartomány beállítása automatikus eszköz regisztrációs csatlakozott eszközök](active-directory-conditional-access-automatic-device-registration-windows7.md)utasításokat.

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Bekapcsolódás egy Azure Active Directory eszköz regisztrációs használata Android-eszközzel

Az [Android témakör Azure-hitelesítő](active-directory-conditional-access-azure-authenticator-app.md) Azure hitelesítő alkalmazás telepítése Android-eszközön, és a munkahelyi fiók felvétele van utasításokat. A munkahelyi fiók létrehozását sikeresen Android-eszközön, hogy az eszköz esetén csatlakozik a szervezeti munkahelye.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Győződjön meg róla bejegyzett eszközök írt vissza Active Directoryhoz
Megjelenítése, és ellenőrizze, hogy az eszköz objektumok készült vissza az Active Directory LDP.exe vagy ADSIEdit használatával. Mindkét találhatók az Active Directory-rendszergazdai eszközöket.

Alapértelmezés szerint eszköz objektumok, amelyek írt vissza Azure Active Directory kerül az AD FS-farm ugyanabban a tartományban.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Egy alkalmazás hozzáférési házirendet, és az egyéni hozzáférés-megtagadási üzenet létrehozása
A következő eset: az alkalmazás Relying felek adatvédelmi az AD FS létrehozása és konfigurálása a kiállítási engedélyezési szabályt, amely lehetővé teszi, hogy csak a bejegyzett eszközök. Most már csak regisztrált eszközök engedélyezettek az alkalmazás eléréséhez. Megkönnyítheti a felhasználóknak, hogy az alkalmazás eléréséhez szükséges, állítsa be a egyéni hozzáférés megtagadva üzenetet, amelyet be szeretne kapcsolódni az eszköze útmutatást tartalmaz. A felhasználók ettől a eszközök regisztrálhatja az alkalmazások elérése érdekében zavartalanul.

Az alábbi lépéseket kell követnie ebben az esetben végrehajtásához.

>[AZURE.NOTE]
Ez a szakasz tartalma feltételezi, hogy már beállított használna fél meghatalmazotti az alkalmazás az Active Directory összevonási szolgáltatások.

1. Nyissa meg az Active Directory összevonási szolgáltatások MMC eszközt, és kattintson az Active Directory összevonási szolgáltatások > megbízhatósági kapcsolatok > fél megbízik használna.
2. Keresse meg az alkalmazást, amelyekre az access új szabály vonatkozni fog. Kattintson a jobb gombbal az alkalmazást, és válassza a Szerkesztés állítást szabályok...
3. Válassza a **Kiállítási engedélyezési szabályokat** lapot, és válassza a **Szabály hozzáadása**
4. A **szabályhoz állítás** sablon legördülő listában válassza az **engedély vagy megtagadja felhasználók alapján egy bejövő állítást**. Kattintson a **Tovább gombra**.
5. A szabályhoz állítás nevében: mező típusa: **elérését regisztrált eszközökről**
6. Bejövő formál típusa: legördülő lista, válassza ki a **Regisztrált felhasználói van**.
7. A bejövő formál érték: mezőjébe írja be: **Igaz**
8. Jelölje be a **bejövő állítását rendelkező felhasználók számára elérését** választógombot.
9. Válassza a **Befejezés gombra** , és válassza az **Alkalmaz**.
10. Távolítsa el az imént létrehozott szabály-nél több alkalmazásszolgáltatásokra szabályokat. Például távolítsa el az alapértelmezett **Minden felhasználó számára engedélyezi hozzáférési** szabályokat.

Az alkalmazás letöltése hozzáférés engedélyezése, csak akkor, amikor a felhasználó elérhető lesz a eszközről regisztrálva, és a tartományhoz a munkahely van beállítva. Az access speciális házirendeket, [Hozzáférés-vezérlés többtényezős a kockázatok kezelése](https://technet.microsoft.com/library/dn280949.aspx)talál.

Ezután egyéni hibaüzenetet fog konfigurálni az alkalmazás. A hibaüzenet közli, hogy azok kell az eszköze a munkaterületre előtt csatlakozik az alkalmazás elérhetőségét tárolhatják felhasználóit. Létrehozhat egy egyéni alkalmazás hozzáférés megtagadva üzenet egyéni HTML és a Windows PowerShell használatával.

Az összevonási kiszolgáló nyissa meg a Windows PowerShell-parancsablakban, és írja be a következő parancsot. A parancs részeire cserélje ki a rendszer jellemző elemek:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Ez az alkalmazás eléréséhez regisztrálnia kell az eszközön.

**Ha egy iOS-eszközön használja, jelölje be ezt a hivatkozást, ha be szeretne kapcsolódni az eszközön**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Bekapcsolódás az iOS-eszköz munkahelyével.


**Ha a Windows 8.1 készüléket használ**, akkor csatlakozhat, az eszköz nyissa meg a **Gépház ** >  **hálózati** > **munkahelye**.


"**Használna fél adatvédelmi neve**" hol található az alkalmazások az AD FS használna felek adatvédelmi-objektum nevét.
A tartomány nevét, és az Azure Active Directory konfigurált amelyben a **sajattartomany.com** az. Például: contoso.com.
Győződjön meg arról, ha el szeretné távolítani a html-tartalmat a **Set-AdfsRelyingPartyWebContent** parancsmag átadandó bármely sortörések (ha vannak ilyenek).


Most, amikor a felhasználók érik el az alkalmazást, amely az Azure Active Directory eszköz regisztrációs szolgáltatás nem regisztrált a eszközről, kapnak, amely hasonlít az alábbi képernyőkép lap.

![A hibaüzenet, amikor a felhasználó az eszköze még nem regisztrált Azure Active Directory Screeshot](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
