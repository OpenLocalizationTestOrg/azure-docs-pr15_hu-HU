<properties 
    pageTitle="Azure RemoteApp hálózati sávszélesség - tippet olvashat a |} Microsoft Azure"
    description="Ismerje meg a Azure RemoteApp webhelycsoportok és-alkalmazások egyszerű hálózati sávszélesség útmutatást olvashat."
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
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp hálózati sávszélesség - tippet olvashat a (Ha nem lehet tesztelni saját)

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ha nem rendelkezik az idő vagy a [hálózati sávszélesség-vizsgálatok](remoteapp-bandwidthtests.md) futtatása az Azure RemoteApp lehetőség, az alábbiakban útmutatást, amely segíthet a hálózati sávszélesség felhasználónként becslése meglehetősen általános olvashat.

Ha van ilyen helyzetekkel ismerkedhet kombinálja, nem javasoljuk bármi kisebb (vagy azzal egyenlő) 10 MB-os/s, a MINIMUM hálózati sávszélesség-modern internetkapcsolattal rendelkező alkalmazások távoli környezetben. (Bár tárgyalt, ez nem garantálja jobban mint átlagos felhasználói felület.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Összetett PowerPoint, egyszerű PowerPoint

Azure RemoteApp legjobb 100 MB a helyi hálózaton hajtja végre. Meg a 10 MB-os/s hálózati profilját feletti 120 ms vibrálás esetén 5 %-nál, a felhasználó jelenik meg egy átlagos élmény. 1 MB/s a különböző van glaring – például diavetítés közben a felhasználó előfordulhat, hogy egyáltalán nem látni animált áttűnések mivel keretek kimaradnak.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer, a PDF-fájlt, a PDF-fájlt, a szöveg vegyes

10 MB-os/s hálózati profil közelébe LAN van, a legtöbb jellemzőjét. 1 MB/s olyan OK élményt biztosítja, bár nem lehet az egyes vibrálás felhasználó görgetés közben ekkor megjelennek a képernyőn megjelenő képek.

## <a name="flash-video-youtube"></a>Flash-videó (YouTube-on)

100 MB/s LAN az optimális használat érdekében biztosít, míg a 10 MB-os/s elfogadható (azaz a keret ráta nyilvántartása, de azt nő szolgáltatás). 1 MB s vibrálás pedig a nagyon magas észrevehető.

## <a name="word-typing-word-remote-input"></a>Word-gépelési (Word távoli beviteli)
Ez a példa az kis sávszélességű használatát. 256 KB/s kínálunk, amely a helyes LAN-élmény.

## <a name="learn-more"></a>tudj meg többet
- [Azure RemoteApp hálózati sávszélesség-használat becslése](remoteapp-bandwidth.md)

- [Azure RemoteApp – hogyan hálózat sávszélessége és minőségét tapasztalja munka együtt?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting a hálózati sávszélesség-használat a néhány gyakori alkalmazási területek](remoteapp-bandwidthtests.md)