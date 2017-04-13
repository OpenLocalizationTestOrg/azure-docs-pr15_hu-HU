<properties
    pageTitle="Azure VMs adott RDP hibaüzenetek |} Microsoft Azure"
    description="Dátumtáblázatok ismertetése, adott hibaüzeneteket jelenhet meg, amikor megpróbál használata távoli asztali kapcsolaton, virtuális géphez Windows Azure-ban"
    keywords="Távoli asztali hiba, a távoli asztali kapcsolat hiba, nem tud csatlakozni a virtuális, távoli asztal – hibaelhárítás"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Hibaelhárítás a Windows Azure m. adott RDP hibaüzenetek
Távoli asztali kapcsolaton, virtuális géphez (virtuális) a Windows Azure-ban használatakor egy adott hibaüzenet jelenhet meg. Ez a cikk részletesen bemutat néhány, a Gyakori hibaüzenetek észlelt a hibaelhárítási lépéseket Megoldásukhoz együtt. Ha a RDP segítségével virtuális problémák merülnek fel, de nem kell egy adott hibaüzenet, olvassa el a [hibaelhárítási útmutatójának távoli asztali](virtual-machines-windows-troubleshoot-rdp-connection.md)című témakört.

Adott hibaüzeneteket tájékoztatást talál a következő:

- [A távoli munkamenetet meg lett szakítva, mert nincsenek távoli asztali licenc kiszolgálók licenc számára érhető el](#rdplicense).
- [Távoli asztal nem találja a számítógép "név"](#rdpname).
- Hitelesítés [Hiba történt. A helyi biztonsági szervezet nem lehet Önnel kapcsolatba lépni](#rdpauth).
- [Windows biztonsági hiba: A hitelesítő adatok nem működött](#wincred).
- [Ezen a számítógépen a távoli számítógép nem tud csatlakozni](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>A távoli munkamenetet meg lett szakítva, mert nincsenek távoli asztali licenc kiszolgálók licenc számára érhető el.

OK: Az távoli asztali kiszolgálói szerepkör licencelési türelmi 120 napos lejárt, és telepítenie kell ezeket a licenceket.

Kerülő az RDP-fájlt egy helyi példányt menteni a portálról, és ez a parancs futtatása egy PowerShell parancssorba való csatlakozáshoz. Ebben a lépésben letiltja licencelése, csak a kapcsolat:

        mstsc <File name>.RDP /admin

Ha már nincs szükség egyidejű távoli asztali kapcsolat kettőnél több a virtuális ténylegesen, távolítsa el a távoli asztali kiszolgálói szerepkör Kiszolgálókezelő segítségével.

További tudnivalókért lásd: a blogbejegyzésből [Azure virtuális kiszolgálókkal"nincs távoli asztali licenc érhető el" nem sikerült](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Távoli asztali számítógép "név" nem található.

OK: Az távoli asztali ügyfél, a számítógépen nem találja meg a beállításai között az RDP-fájlt a számítógép nevét.

Lehetséges megoldásukat:

- Ha a szervezet intraneten, ellenőrizze, hogy a számítógép hozzáfér a proxykiszolgáló és küldhet HTTPS-forgalom neki.

- Ha használja a helyben tárolt RDP-fájlt, próbáljon meg azt, amely a portal által generált. Ebben a lépésben ellenőrzi, hogy rendelkezik-e a megfelelő DNS-neve a virtuális gép vagy a felhőbeli szolgáltatástól és a virtuális végpont portja. Az alábbiakban a portálja által generált RDP mintafájl:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

A cím részét a RDP foglalja magában:
- A teljesen minősített tartománynév a felhőalapú szolgáltatás, amely tartalmazza a virtuális ("Dejójáték Kft-azdatatier.cloudapp.net" Ebben a példában).

- A külső portot a végpont távoli asztali forgalmához (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Hitelesítési hiba történt. A helyi biztonsági szervezet nem lehet Önnel kapcsolatba lépni.

OK: A cél virtuális nem keresse meg a biztonsági szervezet a felhasználó neve részét a hitelesítő adatait.

Amikor felhasználóneve formájában *SecurityAuthority*\\*felhasználónév* (Példa: CORP\User1), az *SecurityAuthority* részt vagy a virtuális számítógép nevét (a helyi biztonsági szervezet), vagy az Active Directory tartománynév.

Lehetséges megoldásukat:

- Ha a fiókja a virtuális helyi, győződjön meg arról, hogy virtuális neve helytelen.

- Ha a fiók a Active Directory-tartománya, jelölje be a helyesírás-ellenőrzés a tartománynév.

- Ha az Active Directory tartományi fiók, és a tartomány neve helytelen, ellenőrizze, hogy a tartományvezérlőnek elérhető az adott tartományban. Ez a jelenség az Azure virtuális hálózatok, amelyeknél a tartomány vezérlők, hogy a tartományvezérlőnek akkor nem érhető el, mert nem kezdődött. Megoldás a helyi rendszergazdafiók helyett a tartományi fiók is használhatja.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>A Windows biztonsági hiba: A hitelesítő adatok nem működött.

OK: A céllista virtuális nem tudja érvényesíteni a fiók nevét és jelszavát.

A Windows-alapú számítógépen ellenőrizheti a helyi fiók vagy a tartományi fiók hitelesítő adatait.

- Helyi fiókok, használja a *számítógépnév*\\*felhasználónév* szintaxis (Példa: SQL1\Admin4798).
- Tartományi fiókok, használja a *tartománynév*\\*felhasználónév* szintaxis (Példa: CONTOSO\peterodman).

Ha egy új Active Directory erdőben tartományvezérlőnek a virtuális van előléptetett, a helyi rendszergazdai fiókkal jelentkezett be, ugyanazt a jelszót az új erdő és tartományban megfelelője fiókja alakul. A helyi fiók majd törlődik.

Ha DC1\DCAdmin helyi fiókkal jelentkezett be, és ezután előléptetett a virtuális gép egy új erdő vallalat.kontraktor.hu tartományhoz tartozó tartományvezérlőnek, például a DC1\DCAdmin helyi fiók törlése, és új tartományi fiók (CORP\DCAdmin) létrehozott ugyanazt a jelszót.

Győződjön meg arról, hogy a fiók nevét a név, amely a virtuális gép ellenőrizheti fiókként érvényes, és a jelszó helyességét.

Ha módosítania kell a helyi rendszergazdafiók jelszavát, megtudhatja, [hogy miként állítsa alaphelyzetbe a jelszó vagy a Windows virtuális gépeken futó távoli asztali szolgáltatást](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Ez a számítógép nem tud kapcsolódni a távoli számítógépen.

OK: A fiókhoz való csatlakozáshoz használt nincs engedélye távoli asztali bejelentkezési.

Minden Windows rendszerű távoli asztali felhasználók helyi csoport, amely tartalmazza a partnerek és a csoportok, jelentkezhet be távolról rendelkezik. A helyi Rendszergazdák csoport tagjainak is, akkor is, ha azokat a fiókokat nem jelennek meg a távoli asztali felhasználók helyi csoport. A tartományhoz gépekhez a helyi Rendszergazdák csoport a tartományhoz tartozó domain rendszergazdái is tartalmaz.

Győződjön meg arról, hogy a fiókot, hogy használja a csatlakozás távoli asztali bejelentkezési jogosultsága van-e. Megoldásként használjon a tartomány vagy helyi rendszergazdafiók csatlakozás távoli asztali kapcsolaton keresztül. Vegye fel a kívánt fiókot a távoli asztali felhasználók helyi csoporthoz, használja a Microsoft Management Console beépülő modul (**Rendszereszközök > helyi felhasználók és csoportok > csoportok > Távoli asztali felhasználók**).


## <a name="next-steps"></a>Következő lépések
Ha a hibák egyike sem történt, és a csatlakozás RDP használatával ismeretlen hiba van, nézze meg a [hibaelhárítási útmutatójának távoli asztal](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure IaaS (Windows) diagnosztika csomag](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- A hibaelhárítási lépéseket egy virtuális futó alkalmazások elérése, [az Azure virtuális futó alkalmazás hibaelhárítása access](virtual-machines-linux-troubleshoot-app-connection.md)témakörben talál.
- Ha nehézségei vannak a Linux virtuális Azure-ban való kapcsolódáshoz biztonságos rendszerhéj (SSH) használ, lásd: [kapcsolatos hibák elhárítása SSH kapcsolatokat az Azure-ban egy Linux virtuális](virtual-machines-linux-troubleshoot-ssh-connection.md).