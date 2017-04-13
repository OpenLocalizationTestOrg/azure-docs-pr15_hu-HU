<properties
    pageTitle="A DirSync és Azure AD-szinkronizálás frissítési |} Microsoft Azure"
    description="Megtudhatja, hogy miként frissíthet DirSync és Azure AD-szinkronizálás Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Frissítse a Windows Azure Active Directory-szinkronizálást ("DirSync") és Azure Active Directory-szinkronizálást ("Azure AD-szinkronizálás")
Azure AD Connect legegyszerűbben az való csatlakozáskor a helyszíni címtárában Azure Active Directory és az Office 365-ben. Ez egy kiváló frissítésének Azure AD Connect a Windows Azure Active Directory szinkronizáló (DirSync) vagy az Azure AD-szinkronizálás ezeket az eszközöket most már nem támogatja, és elérje a támogatás az 2017 április 13 végére.

A két identitás szinkronizálási eszközök, amelyek már nem támogatja a volt felajánlott erdőn ügyfeleknek (DirSync), és több erdőre és más speciális ügyfelek (Azure AD-szinkronizálás). A régebbi eszközökben került a helyére, amelyek minden esetben elérhető egyetlen megoldást: Azure AD Connect. Új funkciók, finomítások és új forgatókönyveket támogatása nyújt. Azure AD a helyszíni identitás adatok szinkronizálása továbbra is láthatja, és az Office 365-ben kifejezetten ajánljuk, hogy frissítsen Azure AD Connect.

A DirSync utolsó verzióját július 2014-es jelent meg, és Azure AD-szinkronizálás utolsó kiadását május 2015 jelent meg.

## <a name="what-is-azure-ad-connect"></a>Mi az Azure AD Connect
Azure AD Connect a DirSync és Azure AD-szinkronizálás követő. Minden esetben e két támogatott egyesít. Erről további [integrálása az Azure Active Directory címtárral a helyszíni identitások](active-directory-aadconnect.md)a vele.

## <a name="deprecation-schedule"></a>Kapcsolójáról ütemezése

Dátum | Megjegyzés
 --- | ---
2016 áprilisi 13 | A Windows Azure Active Directory Sync ("DirSync") és a Microsoft Azure Active Directory Sync ("Azure AD-szinkronizálás") jelenti be, mint kiadásában megszűnt.
2017 április 13 | Támogatása véget ér. Ügyfelek már nem tudja megnyitni a támogatási eset történő frissítés Azure AD Connect először nélkül.

## <a name="how-to-transition-to-azure-ad-connect"></a>Hogyan való áttérés Azure AD Connect
Ha futtatja a DirSync kétféleképpen frissíthető: helyi frissítés és a párhuzamos telepítési. Helyi frissítés ajánlott a legtöbb ügyfelek és esetén a legutóbbi operációs rendszer és kisebb, mint 50 000 objektumok. Egyéb esetben ajánlott végezze el a párhuzamos telepítés hol átkerül a DirSync konfigurációs Azure AD Connect új kiszolgáló.

Ha a helyi frissítés ajánlott Azure AD-szinkronizálás. Ha szeretne, akkor lehet egy új Azure AD Connect server telepítése párhuzamosan, és végezze el a mozgó áttelepítésének az Azure AD-szinkronizálás server Azure AD Connect.

Megoldás | Eset
----- | -----
[A DirSync frissítés](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Ha egy meglévő, már fut a DirSync kiszolgálót.</li>
[Frissítés az Azure AD-szinkronizálás](active-directory-aadconnect-upgrade-previous-version.md)| <li>Ha az Azure AD-szinkronizálás áthelyezni.</li>

Megtudhatja, hogy miként, ha a helyi frissítés DirSync az Azure AD Connect szeretné, majd adjon című témakörben olvashat a csatorna 9 videó:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>GYAKORI KÉRDÉSEK
**Probléma: kapott e-mailben értesítést az Azure csoport, illetve az Office 365 üzenetközpont származó üzenetet, de csatlakozás használom.**  
Az értesítés is küldte Azure AD Connect használó build szám 1.0 ügyfeleknek. \*.0 (használata egy előtti-1.1-es kiadás). A Microsoft Azure AD Connect verziókban naprakész ügyfelek javasolja. Az [Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) funkció teszi 1.1-es, nagyon egyszerűen mindig az Azure AD Connect friss verziójával rendelkezik telepítve.

**Kérdés: fog DirSync/Azure AD-szinkronizálás leállítása 2017 április 13 dolgozik?**  
nem. Ha ezek többé nem fogja tudni kommunikálni az Azure Active Directory dátuma későbbi időpontban jelenti be a rendszer. Ez a témakör, ha lehetséges, hogy információ keresése tudja.

**Kérdés: tölti verziójától is frissíteni az?**  
Bármelyik DirSync verzióját használja-e éppen rendszerről támogatja.

**K: Mit kell tudni az Azure Active Directory-összekötő az FIM/MIM?**  
Az Azure Active Directory-összekötő a FIM/MIM **nem** lett bejelentve a kiadásában megszűnt. A **szolgáltatás rögzítése**; van Nincs új funkciók kerül, és nincs hibajavítás kap. A Microsoft tervezi, hogy az Azure AD Connect áthelyezése használatával ügyfelek javasolja. Nem minden új telepítések használat indításához erősen ajánlott. Ez az összekötő jelenti be a rendszer a jövőben elavult.

## <a name="additional-resources"></a>További források

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
