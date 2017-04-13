<properties 
    pageTitle="Gyakori kérdések Azure RemoteApp |} Microsoft Azure" 
    description="Megtudhatja, hogy a legtöbb adott válaszok gyakori kérdések a Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Azure RemoteApp – gyakori kérdések

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A következő kérdéseket Azure RemoteApp kapcsolatos leggyakoribb. Hogy mások is? Látogasson el a [RemoteApp fórumok](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) és tudassa velünk, mit kell, hogy, illetve Megjegyzés legördülő listája alatt.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Nem találja, amit keres? Azt nem válaszol kérdése van?
Ha nem találja az adatokat van szüksége, vagy van, hogy éppen nem kiterjedő Itt további kérdés, kérjük, nyissa meg az [Azure RemoteApp fórumát](http://aka.ms/araforum) , és ott kérdését felteheti. Itt további válaszok mindig hozzáadunk is.

## <a name="what-is-azure-remoteapp"></a>Mi az Azure RemoteApp? ##


- **Mi az Azure RemoteApp?** Távoli alkalmazás formájában használva egy Azure szolgáltatás segít a biztonságos, távoli hozzáférés biztosítása az alkalmazások számos különböző felhasználói eszközökről. További információ az [Azure RemoteApp](remoteapp-whatis.md).
- **Mik azok a telepítési lehetőségek?** Két fő RemoteApp gyűjtemények típusba sorolhatók: Felhő és a hibrid. Szükséges melyiket számos tényező, például, hogy szüksége tartomány illesztés függ. Megvitatjuk, az összes e határozatok [Itt](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Gyorstippek Azure RemoteApp használatáról ##
- **Mennyi ideig, amíg meg nem lehet vagyok megszakad a kapcsolata? Mennyi ideig lehet üresjárati az indítási előadása én előtt?** 4 óra. Ha Ön vagy egy felhasználót üresjárati 4 órát, automatikusan bejelentkezett lesz Azure RemoteApp ki. Nézze meg az [Azure-előfizetés és a szolgáltatás korlátozások, a kvótákat, és a kényszerek](../azure-subscription-service-limits.md)más alapértelmezett beállításait.
- **Kipróbálhatom a szolgáltatást az ingyenes?** igen. Nem érhető el az ingyenes próbaverzióra 30 nap. A próbaidőszak lejártakor átmenet is fizetett fiókhoz (amelynek használatával gyártási), illetve függeszthetem fel a az szolgáltatással. Az ingyenes próbaverziót először váltson [portal.azure.com](http://portal.azure.com) – hozzon létre egy új példányát RemoteApp. Az ingyenes próbaverziót, a készíthet RemoteApp 2 előfordulását példányonként 10 felhasználókkal. Ne feledje, hogy ez a próbaverzió csak él 30 nap.
## <a name="azure-remoteapp-subscription-details"></a>Azure RemoteApp az előfizetés részletei ##

- **Mik azok a szolgáltatás korlátozások?** Az alapértelmezett beállítások és Azure RemoteApp [Azure előfizetés szolgáltatás korlátozások, kvótákat,](../azure-subscription-service-limits.md)és a kényszerek szolgáltatás határain megismerheti. Tudassa velünk, ha további kérdései vannak.
- **Hány felhasználóm, hogy van?** Nincs legalább 20 felhasználók. Ismételje meg a kiemelt világosság, hogy – a minimális érték 20. Fog, 20 vonatkozó számlát kapni. 
- **Mennyibe kerül az RemoteApp költség?** Nézze meg az [Azure RemoteApp árak részleteket ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Nem a webhelycsoport különböző költség több, mint egy másik?** Igen, akkor is, attól függően, hogy az webhelycsoport igényeknek megfelelően alakíthatja. Hibrid gyűjtemény a helyszíni hálózaton az Azure RemoteApp kapcsolatot igényel. Ha meglévő VNET/Express útvonalat használ, nem külön költség nélkül. De ha egy új Azure VNET, illetve egy átjáró vagy Express útvonal használ, akkor ráterheljük a [virtuális Magánhálózati átjáró](https://azure.microsoft.com/pricing/details/vpn-gateway) vagy [Express útvonal](https://azure.microsoft.com/pricing/details/expressroute/). A költség (részletes hivatkozások) van a havi Azure RemoteApp fölött költségeket.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Webhelycsoportok - mely szolgáltatások támogatottak, amely kell használnia, és mások
- **Egyéni sor üzleti (üzleti) alkalmazások használhatók?** igen. Egyéni alkalmazás az Azure RemoteApp, hozzon létre egy [egyéni sablont képet](remoteapp-create-custom-image.md), és töltse fel a RemoteApp gyűjteményben.
- **Saját egyéni üzleti alkalmazások működni fognak Azure RemoteApp?** Legegyszerűbben úgy, hogy ki, hogy tesztelje ábrán. Nézze meg a [Távoli asztali kompatibilitási központ](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Milyen telepítési mód (felhő vagy hibrid) a legjobb a szervezetemhez?** Hibrid gyűjtemények adja meg a legtöbb teljes felület, ha azt szeretné, hogy egyszeri bejelentkezés (SSO) teljes integráció és biztonságos helyszíni hálózati kapcsolat. Felhőalapú gyűjtemények lehetővé teszik-Agilis és könnyen azonosítani a üzembe több hitelesítési módszer használatával. További információ a [telepítési lehetőségek](remoteapp-whatis.md).
- **Van még SQL, vagy egy másik adatbázis-használatát vagy a helyszíni és Azure-ban. Melyik telepítési típus célszerű használni?** Ez attól függ, hogy hol van a az kódmentes vagy az SQL-adatbázis. Ha az adatbázis magánhálózat, a a hibrid gyűjtemény használható. Ha az adatbázis van kitéve az internethez, és lehetővé teszi, hogy a ügyfél való kapcsolatok, a felhőben gyűjtemény is használhatja.
- **Mit kell tudni a megfeleltetés meghajtót, USB és a soros, vágólap megosztása és nyomtató átirányítás?** Ezek a funkciók támogatottak az Azure RemoteApp. Alapértelmezés szerint engedélyezve van a vágólap megosztása és a nyomtató átirányítása. A átirányítás többet is megtudhat [Itt](remoteapp-redirection.md). 


## <a name="template-images"></a>A webhelysablonok képét
- **Van lehetőség a felhőben vagy a meglévő virtuális gép sablonként a RemoteApp vonatkozóan?** igen! Hozzon létre egy képet az Azure virtuális alapján, használja a képek az előfizetéshez tartozó egyikét vagy egyéni kép készítése. Nézze meg a [Távoli alkalmazás formájában használva Képbeállítások](remoteapp-imageoptions.md).


## <a name="network-options"></a>Hálózati beállítások
- **A hibrid webhelycsoport egy VNET szükséges. A meglévő VNET is használni?** Ha a meglévő VNET az Azure VNET is. Lásd: "1 lépés: a virtuális hálózat beállítása" a további információt a [hibrid webhelycsoport utasításokat](remoteapp-create-hybrid-deployment.md) .
- **Használhatom-e egy VNET felhő gyűjtemény?** Valóban is. Nézze meg [a felhőben gyűjtemény létrehozása](remoteapp-create-cloud-deployment.md), különösen ugorjon az 1 további információt.

## <a name="authentication-options"></a>Hitelesítési beállítások



- **Hogyan kell tudni hitelesítést? Milyen módszerekkel támogatottak?** A felhő gyűjtemény támogatja a Microsoft-fiókok és Azure Active Directory-fiókok, amelyek, valamint az Office 365-fiókok. A hibrid gyűjtemény csak Azure Active Directory-fiókok (például az [Azure Active Directory szinkronizáló](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)eszköz használata) szinkronizált támogatja a Windows Server Active Directory telepítés; pontosabban vagy szinkronizálja a jelszó-szinkronizálás beállítást, vagy beállította az Active Directory összevonási szolgáltatások (AD FS) összevonási szinkronizálja. [Többtényezős hitelesítés (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/)is beállítható.

>[AZURE.NOTE]Az Azure Active Directory-felhasználók kell lennie a bérlőjének, amely az előfizetéshez van társítva. (Megtekintése és módosítása az előfizetés a **Beállítások** lapon a portálon. [Módosítás az Azure Active Directory-bérlői RemoteApp által használt](remoteapp-changetenant.md) további információt talál.)

- **Miért nem lehet az Azure Active Directory fiók hozzáférési engedély?** Az Azure Active Directory-felhasználók, amely az előfizetéshez van társítva a címtárból kell lennie. Meg vagy módosítsa a beállítások lapon a portálon könyvtárra. További információt a [RemoteApp által használt módosítása az Azure Active Directory-bérlői webhelyen](remoteapp-changetenant.md) talál.

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Ügyfél - eszköztől is Azure RemoteApp eléréséhez használni?
Jó ügyfél adatait, beleértve az a különböző ügyfelek [az alkalmazások az Azure RemoteApp elérése](remoteapp-clients.md)a telepítési lépéseinek is megkeresheti.

- **Eszközök és operációs rendszerek az ügyfél-alkalmazásokat támogatják?**
Első, a számítógépen vagy táblagépen: 
    - A Windows 10 (ügyfél előzetes verzió)
    - A Windows 8.1 és Windows 8-ban
    - Windows 7 Service Pack 1
    - Mac OS X
    - A Windows RT rendszerben
    - Android-táblagépekhez
    - iPad eszközök és a telefonok:
    - iPhone-alapú
    - Android-telefonon
    - Windows Phone
 
    [Töltse le](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) egy RemoteApp ügyfélalkalmazás letöltése.
- **Azure RemoteApp támogatja a vékony ügyfeleknek?** Igen, az alábbi Windows beágyazott vékony ügyfeleknek támogatottak:
    - Windows 7 szabványos beágyazott
    - Windows 8 Standard beágyazott
    - Windows 8.1-es iparágban beágyazott Pro
    - A Windows 10 IoT vállalati

- **A Windows Server melyik verziója támogatott esetében a távoli asztali munkamenet Host (RDSH)?** A Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Támogatás és visszajelzés


- **ActiveX-vezérlőket RemoteApp támogatási megtervezése** Számlázási és előfizetési támogatás áttérhet megadva. Technikai támogatás érhető el az [Azure milyen szolgáltatáscsomagok](https://azure.microsoft.com/support/plans/)keresztül. Ingyenes Közösségben támogatási az [Azure vitafórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)keresztül is elérheti. 
- **Hogyan visszajelzés küldése?** Látogassa meg a [Visszajelzési fórum](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Ki is beszélhet Ha többet szeretne tudni az Azure RemoteApp?** Saját [vitafórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), amely kiváló hely az, hogy kérdéseiket, mellett a heti [Ask a szakértők webinárium](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), ahol az összes dolog, amit RemoteApp megvitatjuk bekapcsolódhat.
- **Mi a helyzet RemoteApp dokumentáció?** Sajnos neked, akkor a rendszer. A Súgó tartalma a portál súgója fiókban mellett (egyszerűen kattintson a **?** az egyik lapon a portálon) az alábbi cikkekben érhetők el a lapon, akkor minden RemoteApp tudni:
    - **Első lépések:**
        - [Mi az RemoteApp?](remoteapp-whatis.md)
        - [Mi az a RemoteApp sablon képeket?](remoteapp-images.md)
        - [Licencelési működése](remoteapp-licensing.md)
        - [Hogyan RemoteApp és az Office működnek együtt?](remoteapp-o365.md)
        - Az [átirányítás működése a RemoteApp](remoteapp-redirection.md)?
    - **Telepítése:**
        - [Hozzon létre egy egyéni sablont képe](remoteapp-create-custom-image.md)
        - [A hibrid webhelycsoport létrehozása](remoteapp-create-hybrid-deployment.md)
        - [Hozzon létre egy felhőalapú gyűjteményt](remoteapp-create-cloud-deployment.md)
        - [Azure Active Directory RemoteApp konfigurálása](remoteapp-ad.md)
        - [Az alkalmazás RemoteApp közzététele](remoteapp-publish.md)
    - **Kezelése:**
        - [Felhasználók hozzáadása](remoteapp-user.md)
        - [Ajánlott eljárások beállításának és használatának RemoteApp](remoteapp-bestpractices.md)  

    Videók! Azt is RemoteApp videókat számos. Bevezetés ([Azure RemoteApp – bevezetés](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) néhány adja meg, míg mások végigvezetjük telepítési ([felhő telepítési](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) és [a hibrid telepítés](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Nézze meg őket!

 
### <a name="help-us-help-you"></a>Segítse a szolgáltatás segítségével 
Tudta, hogy mellett ez a cikk minősítés, és a megjegyzések megadására lefelé alatt, módosíthatja a cikkben magát? Valami hiányzó? Valami hiba történt? DID lehet Írjon valamit, amely csak áttekinthetőbb legyen? Görgetés felfelé, és kattintson a **szerkesztése a GitHub** módosításához - e érkezzenek us véleményezésre, és majd, ha azt jelentkezzen, a őket, látni fogja a módosításokat, és itt javítása.
