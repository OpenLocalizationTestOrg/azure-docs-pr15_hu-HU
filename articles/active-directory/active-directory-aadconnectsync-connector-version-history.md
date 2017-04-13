<properties
   pageTitle="Összekötő megjelenés korábbi verziók |} Microsoft Azure"
   description="Ez a témakör felsorolja minden operációs rendszere, az összekötőket, a Forefront identitás kezelő FIM () és a Microsoft identitás kezelő (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Összekötő megjelenés korábbi verziók
A Forefront identitás Manager (FIM) és a Microsoft identitás Manager (MIM) az összekötők gyakran frissül.

>[AZURE.NOTE]
Ez a témakör a csak akkor FIM és MIM. Ezek az összekötők Azure AD Connect nem támogatottak.

Ez a témakör az összekötőket, amelyek jelentek összes verziójának sorolja fel.

Kapcsolódó hivatkozások:

- [Töltse le a legújabb összekötők](http://go.microsoft.com/fwlink/?LinkId=717495)
- [Általános LDAP összekötő](active-directory-aadconnectsync-connector-genericldap.md) hivatkozási dokumentáció
- [Általános SQL-összekötő](active-directory-aadconnectsync-connector-genericsql.md) hivatkozási dokumentáció
- [Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) hivatkozási dokumentáció
- [A PowerShell összekötő](active-directory-aadconnectsync-connector-powershell.md) hivatkozási dokumentáció
- [Lotus Domino-összekötő](active-directory-aadconnectsync-connector-domino.md) hivatkozási dokumentáció

## <a name="111170"></a>1.1.117.0
Ki: március 2016

**Új összekötő**  
Az [Általános SQL-összekötő](active-directory-aadconnectsync-connector-genericsql.md)kezdeti kiadását.

**Új funkciók:**

- Általános LDAP összekötő:
    - Delta importálása a Isode hozzáadott támogatása.
- Web Services Connector:
    - Frissítette a csEntryChangeResult tevékenység és setImportErrorCode tevékenység objektum szintű hibák vissza kell vissza a szinkronizálási motor engedélyezni.
    - Az új objektum szintű hiba funkciónak a használatához a SAP6 és SAP6User sablonok frissül.
- Lotus Domino-összekötő:
    - Az Exportálás kell egy certifier egy címjegyzékben. Az összes certifiers ugyanazt a jelszót, így könnyebben kezelése most már használhatja.

**Javított problémákat:**

- Általános LDAP összekötő:
    - Az IBM Tivoli Directory tartományi szolgáltatások néhány hivatkozás attribútum nem észlelt megfelelően.
    - Nyissa meg a LDAP a delta az importálás során szóközzel az elején és végén található karakterláncok voltak lesz csonkítva.
    - Novell és NetIQ szervezeti/tárolók és a egyszerre egy objektum áthelyezett exportálása átnevezni az objektum sikertelen volt.
- Web Services Connector:
    - Ha a webszolgáltatás tartalmazott több előfizetéskor azonos kötés, majd az összekötő nem megfelelően találta ezek előfizetéskor.
- Lotus Domino-összekötő:
    - A név attribútum a levelezési adatbázisok exportálása nem működnek.
    - Egy exportálás, amely egyaránt hozzáadott és tag eltávolítása egy csoportból csak exportálja a hozzáadott tagokat.
    - Ha a jegyzetek dokumentum érvénytelen (az attribútum isValid hamis értékre van állítva), majd a összekötő sikertelen.

## <a name="older-releases"></a>A régebbi verziókban
Március 2016, mielőtt az összekötők megjelentek, mint – támogatási témakörök.

**Általános LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 szeptember
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, március 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 január
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014-es szeptember
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014-es március

**Problémák megoldásához segítséget**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014-es szeptember

**A PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014-es szeptember

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 szeptember
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, március 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014-es augusztus
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014-es február  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 október
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 augusztus

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
