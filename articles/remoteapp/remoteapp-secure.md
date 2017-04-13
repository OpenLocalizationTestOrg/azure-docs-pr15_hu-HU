
<properties
    pageTitle="Alkalmazások és a források Azure RemoteApp biztonságos |} Microsoft Azure"
    description="Megtudhatja, hogy miként alkalmazások és az Azure RemoteApp források zárolása"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Biztonságos alkalmazások és a források Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp felhasználók hozzáférést biztosít az központilag felügyelt Windows alkalmazásokat, amely lehetővé teszi, hogy azt szabályozzák, hogy mely a felhasználókat, és nem végezhető műveletek.  Az különösen akkor hasznos, amikor a felhasználó csatlakozik egy nem felügyelt eszközről (például a személyes Macbook), és a felhasználói hozzáférés szabályozása vagy merülhetnek fel szeretné.

Például ha használja az Active Directory-felhasználói hitelesítés és szeretné megakadályozni, hogy a felhasználóknak ki az alkalmazás adatok másolása, beállíthatja a távoli asztali csoportházirend letiltása felhasználóknak az adatok másolása.

Egy másik példa, ha szeretné-e letiltani a gyűjteményben egy app internet-hozzáférés. Létrehozhat egy a Windows tűzfal akadályozza az access, a kép a gyűjtemény létrehozásakor szabályt.

## <a name="implementation-options"></a>Végrehajtási beállítások

  Az alábbiakban a fő végrehajtása beállításokat, amelyek a egyenként vagy kapcsolatszolgáltatásokkal használható, szükség szerint:

1.  Ha a RemoteApp webhelycsoport tartományhoz csatlakoztatott bármely [Csoportházirend](https://technet.microsoft.com/library/cc725828.aspx) (kivételével az inaktív és a kapcsolat bontása időtúllépés házirendek leírt [Itt](../azure-subscription-service-limits.md)) alkalmazhat.
2.  (Ha a webhelycsoport nem tartományhoz csatlakoztatott vagy az Active Directory nem rendelkezik a megfelelő jogosultságokkal) csoportházirend alternatívájaként beállíthatja [Helyi házirendeket](https://technet.microsoft.com/library/cc775702.aspx) be a sablon képe.  Megjegyzés: a csoport adu helyi házirendek házirendeket, ha nincs ütközés.
3.  Bizonyos operációs rendszer/alkalmazás beállítások nem konfigurálható házirenden keresztül, de a [RegEdit eszköz](./remoteapp-hybridtrouble.md) használata közben a sablon kép beállítása beállításkulcs keresztül is lehet.
4.  Használhatja [A Windows tűzfal](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) szeretne, és a számítógépről a hálózati hozzáférés vezérléséhez az alkalmazást futtató. Adja meg, hogy az URL-EK és a definiált az alábbi portokat nem letiltja.
5.  Szabályozható, hogy mely alkalmazások és a fájlok felhasználók futtatását is lehetővé teszi [az AppLocker](https://technet.microsoft.com/library/hh831440.aspx) is használhatja. Ha például biztonságban felhasználók is azt kell megállapítania hogyan tehető függővé alkalmazásokat, hogy senki nem közzé, de érhetők el a kép hozta létre a gyűjteményben, amely - AppLocker letilthatja a.

## <a name="detailed-information"></a>Részletes információk

- Az alábbi távoli asztali házirendek valószínűleg leghasznosabb:
    - [Eszközök és erőforrások átirányítását](https://technet.microsoft.com/library/ee791794.aspx)
    - [A nyomtató átirányítása](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profilok](https://technet.microsoft.com/library/ee791865.aspx).
- Figyelje meg, hogy a RemoteApp PowerShell átirányításának beállítása a modul (mint látható [Itt](./remoteapp-redirection.md)) támaszkodik az ügyfélgép a házirendet, megkövetelésére, így ha biztonsági az elsődleges cél szeretné-e be a házirendet a sablon képe helyi házirenden keresztül vagy csoportházirenden keresztül.
- [A Windows Server 2012 R2 házirendeket](https://technet.microsoft.com/library/hh831791.aspx).
- [Az Office 2013-házirendek](https://technet.microsoft.com/library/cc178969.aspx) (beleértve [az Office eszköztár testreszabása](https://technet.microsoft.com/library/cc179143.aspx)).
