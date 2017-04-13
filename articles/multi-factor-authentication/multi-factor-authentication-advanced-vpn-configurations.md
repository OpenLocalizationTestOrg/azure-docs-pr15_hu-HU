<properties
    pageTitle="Azure többtényezős hitelesítés és a 3 fél VPN adatai speciális felhasználási területei"
    description="Ez a lap 3 fél tonnában Azure többtényezős lépésenkénti beállítása konfigurációban ismerteti."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Azure többtényezős hitelesítés és a 3 fél VPN speciális felhasználási területei
Azure többtényezős hitelesítés használható különféle 3 fél VPN-megoldások zökkenőmentes csatlakozni.  Ide tartoznak a Cisco® ASA VPN készülék Citrix NetScaler SSL VPN készülék és a boróka hálózatok biztonságos hozzáférés/Pulse biztonságos kapcsolatot biztonságos SSL VPN készülék.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN készülék és Azure többtényezős hitelesítés
Azure többtényezős hitelesítés zökkenőmentesen együttműködik a Cisco® ASA VPN készülék nyújt további biztonsági Cisco AnyConnect® VPN bejelentkezések portál access a.  Ezt megteheti a LDAP vagy a RADIUS protokoll használatával.  Jelölje ki azt az alábbiak szerint töltse le a részletes, lépésenkénti konfigurációs segédvonalak.

Konfigurációs útmutató  | Leírás
------------- | ------------- |
[Cisco ASA LDAP Anyconnect VPN és Azure MFA beállításai](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Zökkenőmentes integráció a a Cisco ASA VPN készülék az Azure MFA LDAP segítségével|
[Cisco ASA RADIUS Anyconnect VPN és Azure MFA beállításai](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Zökkenőmentes integráció a a Cisco ASA VPN készülék az Azure MFA RADIUS használata

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN és Azure többtényezős hitelesítés
Azure többtényezős hitelesítés zökkenőmentesen együttműködik a Citrix NetScaler SSL VPN készülék nyújt további biztonsági Citrix NetScaler SSL VPN bejelentkezések portál access a.  Ezt megteheti a LDAP vagy a RADIUS protokoll használatával.  Jelölje ki azt az alábbiak szerint töltse le a részletes, lépésenkénti konfigurációs segédvonalak.

Konfigurációs útmutató  | Leírás
------------- | ------------- |
[LDAP Citrix NetScaler SSL VPN és Azure MFA konfigurációjának](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Zökkenőmentes integráció a a Citrix NetScaler SSL VPN az Azure MFA készülék LDAP segítségével|
[RADIUS Citrix NetScaler SSL VPN és Azure MFA konfigurálása](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Zökkenőmentes integráció a a Citrix NetScaler SSL VPN készülék az Azure MFA RADIUS használata

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Biztonságos SSL VPN boróka/Pulse készülék és Azure többtényezős hitelesítés
Azure többtényezős hitelesítés zökkenőmentesen együttműködik a boróka/Pulse biztonságos SSL VPN készülék nyújt további biztonsági boróka/Pulse biztonságos SSL VPN bejelentkezések portál access a.  Ezt megteheti a LDAP vagy a RADIUS protokoll használatával.  Jelölje ki azt az alábbiak szerint töltse le a részletes, lépésenkénti konfigurációs segédvonalak.

Konfigurációs útmutató  | Leírás
------------- | ------------- |
[LDAP boróka/Pulse biztonságos SSL VPN és Azure MFA konfigurációjának](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Zökkenőmentes integráció a a boróka/Pulse biztonságos SSL VPN az Azure MFA készülék LDAP segítségével|
[RADIUS boróka/Pulse biztonságos SSL VPN és Azure MFA konfigurálása](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Zökkenőmentes integráció a a boróka/Pulse biztonságos SSL VPN készülék az Azure MFA RADIUS használata
