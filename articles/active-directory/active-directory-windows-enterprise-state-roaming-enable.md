<properties
    pageTitle="Az Azure Active Directory barangoló engedélyezése vállalati állam |} Microsoft Azure"
    description="Gyakori kérdések a Windows-eszközök vállalati állam barangoló beállításairól. Vállalati állam barangoló Itt a felhasználók egy egyesített élményt minden a Windows-eszközön, és csökkenti a konfigurációs új eszköz szükséges időt."
    services="active-directory"
    keywords="barangoló, windows felhőben, hogyan engedélyezheti a vállalati állam barangoló vállalati állam"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Az Azure Active Directory barangoló engedélyezése vállalati állam

Vállalati állam barangoló a szervezet bármely prémium Azure Active Directory (Azure Active Directory) előfizetéssel érhető el. További részletekért elő az Azure Active Directory-ra, akkor olvassa el az [Azure Active Directory-termék lap](https://azure.microsoft.com/services/active-directory)ismerteti.

Ha engedélyezi a vállalati állam barangoló, a szervezet automatikusan megkapja egy ingyenes, korlátozott használható előfizetéshez tartozó licencek Azure Rights Management. Ez az ingyenes előfizetés korlátozódik titkosítása és enterprise és az alkalmazás adatfájl a vállalati állam barangoló szolgáltatása; szinkronizált visszafejtése a fizetős verzióra való az Azure Rights Management teljes szolgáltatását kell rendelkeznie.

Azure Active Directory prémium előfizetés megszerzése, után kövesse az alábbi lépéseket vállalati állam barangoló engedélyezése:

1. Jelentkezzen be az Azure klasszikus portáljára.
2. A bal oldali jelölje ki **Az ACTIVE DIRECTORY**, és válassza a vállalati állam barangoló engedélyezése kívánt könyvtár.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Nyissa meg a **beállítás** lapon, a képernyő tetején.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Görgessen lefelé, és válassza a **felhasználók előfordulhat, hogy SZINKRONIZÁLNI, BEÁLLÍTÁSAIT és vállalati APP ADATAIT**, és kattintson a **MENTÉS**gombra.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Windows 10-es eszköz számára a beállítások a vállalati állam barangoló szolgáltatása az eszköz kell azonosítania Azure AD-identitás. Az Azure Active Directory tartományhoz eszközök a felhasználó elsődleges jelentkezzen be az Azure Active Directory identitás, így további beállítás nem szükséges. A hagyományos a helyszíni Active Directory használó eszközök esetén az informatikai rendszergazda kell [a tartományhoz tartozó eszközök: Azure AD a Windows 10-es élmény csatlakozni](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Szinkronizálási adattárolás
Vállalati állam barangoló adatok egy vagy több [Azure régiók](https://azure.microsoft.com/regions/ ) , amely a legjobban egymás alá igazítja az ország/régió az Azure Active Directory-példány érték helyezkedik el. Vállalati állam barangoló adatok alapján három fő földrajzi régióban particionált: Észak-Amerika EMEA és APAC. A bérlői adatok vállalati állam barangoló helyileg található, a földrajzi területhez tartozik, és nem van replikált régiókban keresztül.  Például a felhasználóknak, akik már meg EMEA országok közül az ország/régió értéket, mint "Franciaország" vagy "Zambia" lesz egy-vagy az Azure régiók Európa belül adataikat.  Azon ügyfelek, akik az ország/régió értékük beállítása a Azure AD egy Észak-amerikai országok, mint "USA" vagy "Kanada" lesz egy vagy több Azure régió az Amerikai Egyesült Államok belül is adataikat.  Állítsa az ország/régió értéket Azure AD egy APAC országok "Ausztrália" vagy "Új-Zéland" lesz egy vagy több Ázsia belül az Azure régiók üzemeltetett adataikat, mint felhasználókat.  Az Amerikai Egyesült Államok egy vagy több Azure régióban Dél-amerikai országok és Antarktisz adatokat fog tárolni.  Az ország/régió érték van beállítva az Azure Active directory létrehozásának folyamatát részeként, és ezt követően nem módosítható. 

Ha további információra kíváncsi, kattintson az adatok tárolási helye, kérjük, fájl [Azure](https://azure.microsoft.com/support/options/)támogatása jegy.

## <a name="manage-enterprise-state-roaming"></a>Vállalati állam központi kezelése
Azure Active Directory a globális rendszergazdák is engedélyezése és letiltása a vállalati állam barangoló az Azure klasszikus portálon.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globális rendszergazda korlátozhatja a beállítások szinkronizálása azokat a biztonsági csoportokat.

A globális rendszergazdák is megtekinthetők felhasználónkénti eszköz szinkronizálása állapotjelentés egy felhasználó az Active Directory-példány **felhasználók** listában, és kattintson az **eszközök** fülre, gomb kiválasztásával **eszköz miatti hibaüzenetet küld beállítások és a vállalati alkalmazás adatainak**megtekintése
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Adatmegőrzés
Adatok keresztül vállalati állam barangoló Azure-bA szinkronizált végtelen időre szóló megmarad, kivéve, ha egy kézi törlési művelet történik, vagy a szóban forgó adatokat kell elavult határozza meg. 

**Explicit törlés:** Az adatok törlődik az Azure rendszergazda törlése egy felhasználó vagy könyvtár vagy a rendszergazda kifejezetten kéri, hogy adatai törlődik.

- **Felhasználó törlése**: egy felhasználó az Azure Active Directory törlésekor a felhasználói fiók barangolási adatait megjelöli a törlésre, és a 90-180 nap közötti is törli. 
- **Címtár-törlés**: azonnali művelet törlése egy teljes címtárban az Azure Active Directory. A beállítások minden adat, amely címtár megjelöli a törlésre és törlődik a 90-180 nap közötti társított. 
- **A kérelem törlés**: az Azure Active Directory-rendszergazdai szeretne manuálisan törölje az adott felhasználó vagy adatok beállításait, ha a rendszergazda is fájl a jegy az [Azure támogatja](https://azure.microsoft.com/support/). 

**Elavult Adattörlés**: adatok, amelyek még nem nyitottak elavult tekinteni egy évvel ("az adatmegőrzési időszak"), valamint is törlődik az Azure. Az adatmegőrzési időszak változhatnak, de nem lesznek 90 napnál. Lehet, hogy az elavult adatok meghatározott Windows-alkalmazás vagy beállításait az összes felhasználó számára. Példa:
 
- Ha eszközök nem fér hozzá egy adott beállításainak gyűjtemény (például az alkalmazások törlődik az eszközről vagy a beállítások csoport, például "Téma" le van tiltva, az összes felhasználó eszközök), majd adott webhelycsoport után az adatmegőrzési időszak elavult válnak, és előfordulhat, hogy törölhető. 
- Ha egy felhasználó beállítások szinkronizálási oktatók eszközön kikapcsolta, majd hívási beállítások adatok elérhető, és az adott felhasználó összes beállítások adatok elavult lesz, és az adatmegőrzési időszak után is törlődik. 
- Ha az Azure Active directory admin kikapcsolja a vállalati állam barangoló a teljes címtárban, majd az összes felhasználó számára, hogy a címtár fogja állítani a szinkronizálást a beállításokat, az összes beállítások adatok minden felhasználó számára elavult válnak és az adatmegőrzési időszak után is törlődik. 

**Törölt adatok helyreállítása**: az adatmegőrzési érték nem módosítható. Az adatok végleges törlése után nem lesz a helyreállítható. Azonban fontos tudni, hogy a beállítások adatok csak az Azure, a végfelhasználói eszköz nem törlődnek. Ha bármilyen eszközről később ismét csatlakozik a vállalati állam barangoló szolgáltatása, a beállítások fog újra kell szinkronizált és Azure-ban tárolt.


## <a name="related-topics"></a>Kapcsolódó témakörök
- [Vállalati állam barangoló – áttekintés](active-directory-windows-enterprise-state-roaming-overview.md)
- [És az adatfájl barangoló – gyakori kérdések](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Beállítások szinkronizálása házirendek és a Mobileszköz-beállításai](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [A Windows 10 központi beállítások hivatkozás](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
