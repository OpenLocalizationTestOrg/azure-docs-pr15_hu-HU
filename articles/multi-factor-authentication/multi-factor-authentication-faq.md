<properties
    pageTitle="Azure többtényezős hitelesítés – gyakori kérdések"
    description="Itt a gyakori kérdéseket és válaszokat Azure többtényezős hitelesítés kapcsolódó listáját. Többtényezős hitelesítés, amely több mint egy felhasználónevet és jelszót igényel egy felhasználó személyazonosság ellenőrzésének metódusát. Egy felhasználónak bejelentkezés, és a tranzakciók biztonsági további réteget biztosít."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Azure többtényezős hitelesítés – gyakori kérdések


Ez a cikk az Azure többtényezős hitelesítés és az többtényezős hitelesítés szolgáltatás, beleértve a számlázási modell és használhatóság kapcsolatos kérdések használatával kapcsolatos gyakori kérdésekre ad választ.

## <a name="general"></a>Általános

**Kérdés: hogyan Azure többtényezős hitelesítést kiszolgáló felhasználói adatok kezeléséhez?**

Többtényezős hitelesítés kiszolgálóval a felhasználói adatok csak a helyszíni kiszolgálókon vannak tárolva. Nincs állandó felhasználói adatok a felhőben vannak tárolva. Amikor a felhasználó kétlépcsős hitelesítési hajt végre, többtényezős hitelesítést kiszolgáló adatokat küld a hitelesítést az Azure többtényezős hitelesítés felhőszolgáltatásba. Többtényezős hitelesítés Server és a többtényezős hitelesítés felhőalapú szolgáltatás közötti kommunikáció vagy használja a Secure Sockets Layer (SSL) átviteli réteg Security (TLS) kimenő 443-as port fölé.

Amikor hitelesítési kérések elküldi a felhőbeli szolgáltatástól adatgyűjtés hitelesítési és használati jelentések. Adatmezők kétlépcsős hitelesítési naplók szerepel a következők:

- **Egyedi azonosító** (vagy felhasználói nevet vagy a helyszíni többtényezős hitelesítést kiszolgáló azonosító)
- **Utónév és Vezetéknév** (nem kötelező)
- **E-mail cím** (nem kötelező)
- **Telefonszám** (Ha a hanghívás vagy SMS-hitelesítés használatával)
- **Eszköz jogkivonat** (Ha a mobilalkalmazásban hitelesítéssel)
- **Hitelesítési mód**
- **Hitelesítés eredménye**
- **Többtényezős hitelesítés kiszolgáló neve**
- **Többtényezős hitelesítés kiszolgáló IP**
- **Ügyfél IP** (ha elérhető)

A választható mezők többtényezős hitelesítést kiszolgáló beállíthatók.

Az ellenőrzés eredménye (sikeres vagy megtagadását), és az OK, ha megtagadva, a hitelesítési adatok tárolja, és hitelesítési és használati jelentések érhető el.


## <a name="billing"></a>A számlázás

A legtöbb számlázási kérdések [többtényezős hitelesítést árak lap](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)hivatkozva is érkezni.

**Kérdés: telefonhívások vagy szöveges üzenetek fel szervezeten használatos hitelesítést végezni a felhasználók számára?**

Szervezetek nem terheli az egyes telefonhívások elhelyezni, vagy a szöveg keresztül Azure többtényezős hitelesítés felhasználóknak küldött üzeneteket. Telefon tulajdonosai a telefonhívások vagy szöveges üzenetet kapnak, a személyes phone szolgáltatás szerint kell fizetnie.

**Kérdés: hatással van a felhasználónkénti számlázási modell díjat többtényezős hitelesítés használatára konfigurált felhasználók számát vagy a felhasználók, akik ellenőrzések végrehajtásához száma alapján?**

Számlázási alapuló többtényezős hitelesítést használ felhasználók számát.

**Kérdés: hogyan működik a számlázás többtényezős hitelesítést?**

Amikor a "felhasználó" vagy "egy"hitelesítés modell, Azure MFA felhasználási-alapú erőforrás-e. Minden díjat is számlát kapni a szervezet Azure előfizetéséhez, hasonlóan a virtuális gépeken futó, a webhelyek, a stb.

A licenc modell használata esetén Azure többtényezős hitelesítés licencet vásárolt, és majd felhasználókhoz rendelt, mint az Office 365- és egyéb előfizetés termékek.

**Kérdés: van-e rendszergazdáknak ingyenes verziója Azure többtényezős hitelesítést?**

Bizonyos esetekben az Igen gombra. Többtényezős hitelesítés Azure rendszergazdáknak áttérhet Azure MFA funkciók csak egy részhalmazát kínál. Ezt az ajánlatot nem kapcsolódó felhasználási-alapú Azure többtényezős hitelesítés szolgáltató Azure Active Directory-példányok Azure globális rendszergazdák csoportjának tagjai vonatkozik. Többtényezős hitelesítés-szolgáltató használata frissíti a rendszergazdák és a felhasználó a címtárban ki vannak beállítva, hogy a teljes verzióját Azure többtényezős hitelesítés többtényezős hitelesítés használatával.

**Kérdés: van-e az Office 365-felhasználóknak ingyenes verziója Azure többtényezős hitelesítést?**

Bizonyos esetekben az Igen gombra. Többtényezős hitelesítés Office 365-höz készült Azure MFA szolgáltatások áttérhet csak egy részhalmazát kínál. Ezt az ajánlatot felhasználók, akiknek van hozzárendelve, amikor egy felhasználási-alapú Azure többtényezős hitelesítés szolgáltató nem kapcsolódik az Azure Active Directory megfelelő példány Office 365-licenc vonatkozik. Többtényezős hitelesítés-szolgáltató használata frissíti a rendszergazdák és a felhasználó a címtárban ki vannak beállítva, hogy a teljes verzióját Azure többtényezős hitelesítés többtényezős hitelesítés használatával.

**Kérdés: van lehetőség a szervezeten válthat felhasználónkénti és a hitelesítési felhasználás számlázási modellek bármikor?**

Szervezete számlázási modell úgy dönt, amikor létrehoz egy erőforrás. A számlázási modell után az erőforrás már kiépítve nem módosítható. Létrehozhat azonban az eredeti cseréje másik többtényezős hitelesítés erőforrás. Felhasználói beállításainak és a beállítási lehetőségek nem átkerülnek az új erőforrás.

**Kérdés: van lehetőség a szervezeten Váltás a felhasználás számlázási és licenc modell bármikor?**

Licencek egy mappába, amely már rendelkezik egy felhasználónkénti Azure többtényezős hitelesítés szolgáltató felvétele után felhasználási-alapú számlázási csökken által birtokolt licencek számát. Többtényezős hitelesítést használ, a minden felhasználónak van a hozzárendelt licencek, ha a rendszergazda törölheti az Azure többtényezős hitelesítés szolgáltató.

Hitelesítés felhasználás számlázási licenc modell nem használható együtt. Ha egy hitelesítési többtényezős hitelesítés szolgáltató könyvtárában van csatolva, a szervezet összes többtényezős hitelesítés ellenőrzése kérelem, függetlenül a tulajdonosa licencekre számlázva.

**Kérdés: eladja a szervezeten kell használni, és szinkronizálja identitások Azure többtényezős hitelesítést használ?**

Ha egy szervezet felhasználási-alapú számlázási modell, Azure Active Directory nincs szükség. Többtényezős hitelesítés szolgáltató csatolása könyvtárában nem kötelező. A szervezeti könyvtár nem kapcsolódik, a Azure többtényezős hitelesítést Server vagy az Azure többtényezős hitelesítést SDK helyszíni telepítheti.

Azure Active Directory oka az, a licenc modell működéséhez szükséges licenceket hozzáadni a címtárhoz, amikor vásárolhat, és a felhasználó a címtárban osztani.


## <a name="usability"></a>Használhatósági

**Kérdés: Mit jelent a felhasználó? Ha nem kapnak, a válasz, a telefonján, vagy telefonon nem érhető el a felhasználó számára**

Amennyiben a felhasználó úgy állította be a biztonsági másolat telefon, kell próbálja meg újra, és jelölje ki a telefont, amikor a rendszer kéri a bejelentkezési lapon. A felhasználó nincs beállítva egy másik módszert, ha a szervezet rendszergazdája frissítheti a felhasználó elsődleges telefon rendelt szám.


**Kérdés: Mit jelent a rendszergazda? Ha a felhasználó kapcsolatba lép a rendszergazdát, hogy a felhasználó már nincs hozzáférésük fiók**

A rendszergazda által mintaüzenetet felhasználva hajtsa végre a regisztrációs folyamat újra alaphelyzetbe állíthatja a felhasználói fiókot. További tudnivalók a [felhasználó-és eszközbeállítások Azure többtényezős hitelesítést a felhőben](multi-factor-authentication-manage-users-and-devices.md).

**Kérdés: milyen nem rendszergazda tenni, ha egy felhasználó phone alkalmazás jelszavak használó elvész vagy ellopják?**

A rendszergazda törölheti a felhasználói alkalmazás jelszavak, megakadályozva ezzel az illetéktelen hozzáférést. A felhasználó rendelkezik egy új eszköz, miután a felhasználó is hozza létre újból a jelszavakat. További tudnivalók a [felhasználó-és eszközbeállítások Azure többtényezős hitelesítést a felhőben](multi-factor-authentication-manage-users-and-devices.md).

**Kérdés: Mi a teendő, ha a felhasználó nem tud bejelentkezni a nem böngésző alkalmazások?**

A felhasználók, akik többtényezős hitelesítés használatára van beállítva jelentkezzen be az egyes nem böngésző-alkalmazásokat az alkalmazás jelszó szükséges. A felhasználóknál kell (Törlés) bejelentkezési adatok törlése, indítsa újra az alkalmazást, és jelentkezzen be a felhasználó nevét, és az alkalmazás jelszó használatával.

További információk alkalmazás jelszavakat és más [alkalmazás jelszavakkal súgó](multi-factor-authentication-end-user-app-passwords.md)létrehozásával kapcsolatban.


>[AZURE.NOTE] Az Office 2013 ügyfélprogramhoz a modern hitelesítési
>
> Az Office 2013-ügyfélprogramok (beleértve az Outlook) új hitelesítési protokollok támogatja. Többtényezős hitelesítés támogatása az Office 2013-ban beállítható. Miután beállította az Office 2013-ban, alkalmazás jelszavak nem szükségesek az Office 2013 ügyfélprogramhoz. További tudnivalókért lásd: az [Office 2013 modern hitelesítési nyilvános előzetes bejelentése](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**Kérdés: Mit jelent a felhasználó? Ha a felhasználó nem kapja meg szöveges üzenet, vagy ha a felhasználó kétirányú szöveg üzenetre válaszol, de az igazolás időtúllépés történik**

A szöveges üzenetek olvasása és válaszok az kétirányú SMS nyugtát nem biztos, mert tényező ellenőrizhetetlen hatással lehet a szolgáltatás megbízhatóságát. Ezek a tényezők a cél ország, a mobilszolgáltatónál és a jel erőssége tartalmazza.

A felhasználók, akik nehézséget biztos, hogy az SMS-EK fogadására kell válassza ki az mobile alkalmazást vagy a telefonhívás helyette. A mobilalkalmazásban kaphatnak értesítések, mind a mobil-és Wi-Fi kapcsolat. Ezenkívül a mobilalkalmazás hozhat létre ellenőrzési kódok, akkor is, ha az eszköz nincs jel egyáltalán rendelkeznek. A Microsoft Authenticator alkalmazás [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)érhető el.

Szöveges üzenetek kell használnia, ha a kétirányú SMS, amikor csak lehetséges, hanem egy SMS használata ajánlott. Egyirányú SMS megbízhatóbb, és megakadályozza, hogy felhasználók egy másik ország küldött szöveges üzenet megválaszolása a globális SMS díjak hozni.


**Kérdés: van lehetőség hardver tokenek Azure többtényezős hitelesítést kiszolgálóval?**

Azure többtényezős hitelesítést kiszolgálót használ, külső megnyitott hitelesítés (elfogadható) időalapú egyszeri jelszó (TOTP) tokenek importálása, és használhatja őket a két lépésből álló tartomány ellenőrzéséhez.

ActiveIdentity tokenek, amelyek elfogadható TOTP tokenek, ha a titkos kulcs fájl elhelyezése egy CSV-fájlt, és importálása az Azure többtényezős hitelesítést kiszolgáló is használhatja. Az Active Directory összevonási szolgáltatások (ADFS), távoli hitelesítési betárcsázós felhasználói szolgáltatás (sugár) során az ügyfél rendszer access kérdés-válasz dolgozhat, és az Internet Information Server (IIS) űrlapalapú hitelesítés használható elfogadható tokenek.

Importálhatja a külső részből álló elfogadható TOTP tokenek, a következő formátumokban:  
- Hordozható szimmetrikus kulcs tárolója (PSKC)  
- Ha a fájl tartalmaz dátumértékké, egy titkos kulcs alap 32 formátumban és az egyik időszakra CSV  

**Kérdés: van lehetőség Azure többtényezős hitelesítést kiszolgáló biztonságos Terminálszolgáltatások?**

Igen, de ha sok használata a Windows Server 2012 R2 vagy újabb, csak a távoli asztali átjáró (asztali).

A Windows Server 2012 R2 biztonsági módosításokat megváltoztak, hogy a helyi biztonsági szervezet (LSA) biztonsági csomag a Windows Server 2012-ben és korábbi verziói Azure többtényezős hitelesítést kiszolgáló csatlakozik. Terminálszolgáltatások a Windows Server 2012-ben és korábbi verziói esetén lehetőség van [egy alkalmazást a Windows-hitelesítés biztonságos](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Ha a Windows Server 2012 R2 rendszer használata esetén szüksége RD átjáró.

**Kérdés: Miért kellene felhasználó kapott többtényezős hitelesítés hívás egy névtelen hívó Hívóazonosító beállítása után?**

Többtényezős hitelesítés hívások kerülnek a nyilvános telefonhálózaton keresztül, amikor előfordul, hogy azok résztvevőkhöz keresztül a carrier, amely nem támogatja a hívó azonosítója. Emiatt Hívóazonosító nincs garantált, annak ellenére, hogy a többtényezős hitelesítés rendszer mindig küldi.


## <a name="errors"></a>Hibák

**Kérdés: milyen felhasználók tegye, ha "hitelesítési kérelem nincs a fiókjához aktivált" hibaüzenet megtekinthetik az értesítések mobilalkalmazás használata esetén?**

Tájékoztatja őket a kövesse az alábbi eljárást a fiók eltávolítása a mobilalkalmazásban, majd állítsa be újra:

1. Nyissa meg [az Azure portál profilt](https://account.activedirectory.windowsazure.com/profile/) , és jelentkezzen be a szervezeti fiókjával.
2. Válassza a **további biztonsági ellenőrzése**.
4. A meglévő fiók eltávolítása a mobilalkalmazásban.
5. Kattintson a **Konfigurálás**gombra, és a mobilalkalmazás dolgozóját, hogy a képernyőn megjelenő utasításokat.


**Kérdés: milyen felhasználók tegye, ha azokat a 0x800434D4L hibaüzenet jelenik meg alkalmazás nem böngészőben történő feliratkozás során?**

Jelenleg a felhasználó további biztonsági ellenőrzés csak használható alkalmazások és szolgáltatások, hogy a felhasználó hozzáférhet a böngészőn keresztül. A Windows PowerShell, például a helyi számítógépen telepített (más néven *ügyfélprogramok alkalmazások*) nem tallózó alkalmazások nem működik a későbbi intézkedést igénylő további biztonsági ellenőrzése. Ebben az esetben a felhasználó megjelenhet, az alkalmazás 0x800434D4L hibát.

Megoldás ezt nem szeretné, hogy külön felhasználói fiók szükséges az admin kapcsolatos és a nem rendszergazdai műveletek. Később postaládák a rendszergazdai fiók és a nem rendszergazdai fiókjával hivatkozás, így akkor is jelentkezzen be az Outlook a nem rendszergazdai fiókjával. További információt a problémáról, megtudhatja, hogyan lehet [adni a rendszergazda a megnyitásához és a felhasználó postafiók tartalmának megtekintéséhez](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Következő lépések

Ha itt nem megválaszolja a kérdést, kérjük, hagyja meg a lap alján a megjegyzések. És Íme néhány további lehetőségeket a Súgó használata:


**Kérdés: hogyan érhetem el a segítségre Azure többtényezős hitelesítést?**

- Keresse meg a [Microsoft támogatási Tudásbázis](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) a gyakori műszaki problémák megoldásait.

- Keresse meg és tallózással keresse meg a technikai kérdések és válaszok a Közösség, vagy a saját felteheti [Azure Active Directory-fórumok](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Ha Ön a régi PhoneFactor ügyfele, és olyan kérdése van, vagy a jelszó alaphelyzetbe állítása a segítségre van szüksége, támogatási eset használja a [jelszó alaphelyzetbe állítása](mailto:phonefactorsupport@microsoft.com) hivatkozásra.

- Forduljon a támogatási [Azure többtényezős hitelesítést Server (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947)támogatásával. Kapcsolatfelvétel, esetén hasznos, ha minél több információt a problémáról a lehető is. Adatokat adhat meg a lapot, ahol a hiba, az adott hibakóddal, az adott munkamenetben azonosító és a felhasználó, aki a hiba látta azonosítója látta tartalmazza.
