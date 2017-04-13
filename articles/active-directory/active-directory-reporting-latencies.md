<properties
   pageTitle="Azure Active Directory késések jelentéskészítés |} Microsoft Azure"
   description="Az idő, amíg a jelentési események jelenjen meg az Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Azure Active Directory-jelentés késések

*Ez a dokumentáció az [Azure Active Directory jelentéskészítés útmutató](active-directory-reporting-guide.md)része.*

Jelentés                                                  | Minimális  | Átlag    | Maximum
------------------------------------------------------- | -------- | ---------- | ----------
**Biztonsági jelentések**                                    |          |            |
Szabálytalan bejelentkezési tevékenység                              | 2 óra  | 4 óra    | 8 órát
Bejelentkezési bővítmények ismeretlen forrásokból                           | 2 óra  | 4 óra    | 8 órát
Bejelentkezési bővítmények több hiba után                        | 2 óra  | 4 óra    | 8 órát
A több geographies bejelentkezések                      | 2 óra  | 4 óra    | 8 órát
A gyanús tevékenység IP-címek bejelentkezések     | 2 óra  | 4 óra    | 8 órát
Bejelentkezési bővítmények esetleg fertőzött eszközökről                 | 2 óra  | 4 óra    | 8 órát
Anomalous bejelentkezési tevékenységeket használó felhasználók                   | 2 óra  | 4 óra    | 8 órát
Elvesztett hitelesítő adatokkal rendelkező felhasználók                           | 2 óra  | 4 óra    | 8 órát
**Alkalmazás-jelentések**                                 |          |            |
Tevékenység kiépítési fiók                           | 2 óra  | 4 óra    | 8 órát
Áttelepítési hibákkal fiók                             | 2 óra  | 4 óra    | 8 órát
Alkalmazás használatát.                                       | 2 óra  | 4 óra    | 8 órát
Jelszó átváltási állapota                                | 2 óra  | 4 óra    | 8 órát
**Naplózás és tevékenység-jelentések**                            |          |            |
Naplózási jelentések                                            | 1 perc | 15 percet | 30 percben
Az új jelszó kérésének tevékenység (Azure AD)                      | 2 óra  | 4 óra    | 8 órát
Az új jelszó kérésének tevékenység (identitáskezelő)              | 2 óra  | 4 óra    | 8 órát
Jelszó-nyilvántartási tevékenység (Azure AD)         | 2 óra  | 4 óra    | 8 órát
Jelszó-nyilvántartási tevékenység (identitáskezelő) | 2 óra  | 4 óra    | 8 órát
Önkiszolgáló szolgáltatás csoportok tevékenység (Azure AD)                 | 2 óra  | 4 óra    | 8 órát
Önkiszolgáló szolgáltatás csoportok tevékenység (identitáskezelő)         | 2 óra  | 4 óra    | 8 órát
**Tartalomvédelmi jelentések**                                         |          |            |
Leginkább active Directory tartalomvédelmi szolgáltatások felhasználók                                   | 2 óra  | 4 óra    | 8 órát
Tartalomvédelmi használatát                                               | 2 óra  | 4 óra    | 8 órát
Tartalomvédelmi eszköz használata                                        | 2 óra  | 4 óra    | 8 órát
Engedélyezett RMS Alkalmazáshasználat                           | 2 óra  | 4 óra    | 8 órát
**Saját jelentések**                             |          |            |
Minden felhasználónak bejelentkezés tevékenység                               | 2 óra  | 4 óra    | 8 órát
