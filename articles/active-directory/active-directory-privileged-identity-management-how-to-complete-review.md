<properties
   pageTitle="Hogyan kell elvégezni egy access-Véleményezés |} Microsoft Azure"
   description="Miután az Azure Active Directory Yammerhez Identitáskezelés elkezdett egy access-véleményezés, megtudhatja, hogy miként fejezze be, és az eredmény megjelenítése"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Hogyan kell elvégezni az Azure Active Directory Yammerhez Identitáskezelés egy access áttekintése


Kiemelt szerepkör-rendszergazdák áttekintheti a hozzáférési jogosultsággal rendelkező egyszer [biztonsági Véleményezés el van indítva](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure Active Directory jogosultsággal rendelkező Identitáskezelés (személyes Információkezelő) fog automatikusan küldjön egy üzenetet, tekintse át a hozzáférésüket felhasználók kéri. Ha a felhasználó nem kapott e-mailben, elküldheti őket a képernyőn megjelenő utasításokat [egy biztonsági Véleményezés módjáról](active-directory-privileged-identity-management-how-to-perform-security-review.md)az.

A biztonsági Véleményezés letelte, vagy az összes felhasználót befejezése után azok önálló tekintse át, miután kövesse a jelen cikkben a Véleményezés bátran a találatokat.

## <a name="manage-security-reviews"></a>Biztonsági véleményezések kezelése

1. Az [Azure-portálra](https://portal.azure.com/) , és válassza az **Azure Active Directory Yammerhez Identitáskezelés** alkalmazás az irányítópulton.
2. Jelölje ki az **Access áttekinti a** szakasz az irányítópulton.
3. Jelölje ki a használni kívánt access ellenőrzése.

Az access Véleményezés Részletek lap a lehetőség egy szám adott Véleményezés kezelésére szolgáló.

![Személyes Információkezelő access Véleményezés gombok - képernyőképe][1]

### <a name="remind"></a>Emlékeztetheti

Ha egy access-Véleményezés állította be, hogy a felhasználók maguk áttekintése, értesítést küld a **emlékeztetése** gombra. 

### <a name="stop"></a>állj

Az összes access véleményezések van egy záró dátumot, de a korai befejezés, használhatja a **Leállítás** gombra. Ha az azok a felhasználók által most még nem ellenőrzött, nem tudja a véleményezés befejezése után. Nem lehet indítsa újra a véleményezés, miután van állítva.

### <a name="apply"></a>Alkalmazása

Az access véleményezés befejezése után vagy mert eléri a befejezési dátumot, vagy manuálisan, leállította az **Alkalmaz** gombra végrehajtja a Véleményezés eredményét. A felhasználói hozzáférés megtagadva a áttekintése, az a lépés, amely a szerepkör-hozzárendelés eltávolítja.  

### <a name="export"></a>Exportálás

Ha szeretne manuálisan alkalmazza a biztonsági Véleményezés eredményét, exportálhatja a Véleményezés. Az **Exportálás** gombra a CSV-fájl letöltésének elindul. Az Excel vagy más programot, amely a CSV-fájlok megnyitása az eredmények kezelheti.

### <a name="delete"></a>Törlése

Ha Ön nem minden további ellenőrzése érdekli, törölheti azt. A **Törlés** gombra a Véleményezés eltávolítja a személyes Információkezelő alkalmazást.

> [AZURE.IMPORTANT] Most fog nem figyelmeztetés jelenik meg, akkor fordul elő, törlés előtt, ezért ügyeljen arra, hogy a Véleményezés törölni kívánt.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
