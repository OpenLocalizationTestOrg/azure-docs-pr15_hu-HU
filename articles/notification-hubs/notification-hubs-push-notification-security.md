<properties
    pageTitle="Biztonsági értesítés hubok"
    description="Ez a témakör ismerteti az Azure értesítés hubok biztonsági."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Biztonsági

##<a name="overview"></a>– Áttekintés

Ez a témakör ismerteti a biztonsági modell az Azure értesítés hubok. Mivel az értesítési hubok egy szolgáltatás Bus személyhez, azok a szolgáltatás Bus azonos biztonsági modell hajtja végre. További információ a [Szolgáltatás Bus hitelesítési](https://msdn.microsoft.com/library/azure/dn155925.aspx) témakörökben olvasható.

##<a name="shared-access-signature-security-sas"></a>Megosztott Access aláírás biztonsági (Társítások) 

Értesítés hubok egy egyed-szintű biztonsági rendszer megvalósítása (megosztott Access-aláírás) Társítások neve. Ez a program lehetővé teszi, hogy 12 engedélyezési szabályokat azok leírását az engedélyeket, hogy a szervezet a deklarálása üzenetben szervezetek.

Szabályok tartalmazza a nevét, fő érték (megosztott titkos), és olyan jogokkal, a "Biztonsági követelések." szakaszban leírtak alapján. Értesítés hubon létrehozásakor automatikusan létrejön egy két szabályok: egyik meghallgatását jogosultságokkal (használó az ügyfél alkalmazást), és egy, az összes jogosultságokkal (használó az alkalmazás kódmentes).

Végrehajtásakor regisztrációs management ügyfél-alkalmazásokból, ha az adatok keresztül küldött értesítések nem bizalmas (például időjárás-frissítések), egy értesítés hubhoz eléréséhez gyakori módja a kulcs értékét a szabály csak meghallgatását hozzáférést adjon az ügyfél alkalmazásba, és az alkalmazás kódmentes adhat a szabály teljes hozzáférés a kulcs értékét.

A kulcs értékét beágyazása a Windows áruházból ügyfélalkalmazások nem ajánlott. Egy úgy, a kulcs értékét beágyazás elkerülése érdekében, hogy az ügyfél-alkalmazás lekérése meg az app kódmentes indításkor.

Fontos, ha meg szeretné érteni, hogy a kulcs meghallgatását hozzáféréssel rendelkező lehetővé teszi, hogy egy ügyfél alkalmazás regisztrálni bármelyik címkére. Ha az alkalmazás regisztrációk kell korlátozása adott címkék meghatározott ügyfelek számára (például amikor címkék jelenítik meg felhasználói azonosítóját), az alkalmazás kódmentes a regisztrációk kell végrehajtania. További tudnivalókért olvassa el a regisztrációs kezelés. Ne feledje, hogy ilyen módon az ügyfél alkalmazás fog nem közvetlen hozzáférés az értesítési hubok.

##<a name="security-claims"></a>Biztonsági követelések

Más szervezetek hasonló értesítés központi műveletek engedélyezettek három biztonsági követelések: meghallgatása, elküldése és kezelése.

| Igénylése | Leírás | Engedélyezett műveletek |
|-------|-------------|--------------------|
| Meghallgatása | Létrehozásának és módosításának, olvassa el és egyetlen regisztrációk törlése | Regisztráció létrehozása és frissítése<br><br>Olvassa el a regisztráció<br><br>Olvassa el az összes regisztrációja az egyik fogópont<br><br>Regisztráció törlése |
| Küldés | Az értesítés elosztóhoz küldésére | Üzenet küldése |
| Kezelése | Értesítés hubok (beleértve a PNS hitelesítő adatait, és a biztonsági kulcsok frissítése) és a címkék alapján olvasási regisztrációk cRUDs | Értesítés létrehozása és frissítése vagy olvasási/törlése hubok<br><br>Olvassa el a regisztrációk címke szerint |


Értesítés hubok fogadja el a Microsoft Azure hozzáférés-vezérlés tokenek és segítségével közvetlenül az értesítési-központban konfigurált megosztott billentyűkkel létrehozott aláírást tokenek nyújtott jogcímeken.