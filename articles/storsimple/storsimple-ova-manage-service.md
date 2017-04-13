<properties 
   pageTitle="A StorSimple kezelő szolgáltatás StorSimple virtuális tömb telepítése |} Microsoft Azure"
   description="Megtudhatja, hogyan hozhat létre, és törölje a StorSimple-kezelő szolgáltatást az Azure klasszikus portálon, és megtudhatja, hogy miként kezelheti a szolgáltatás regisztrációs billentyűt."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>A StorSimple kezelő szolgáltatás StorSimple virtuális tömb terjesztése

## <a name="overview"></a>– Áttekintés

A StorSimple kezelő szolgáltatás fut, a Microsoft Azure-ban, és több StorSimple eszköz csatlakozik. Miután létrehozta a szolgáltatást, használhatja a Microsoft Azure klasszikus portálon, a böngészőben futó Eszközkezelés. A nyomon követheti a StorSimple kezelő szolgáltatás egy egyetlen központi helyen, ezáltal a felügyeleti terhet minimalizálása a csatlakoztatott eszközök.

A kezelő StorSimple céloldal használatával kezelheti a StorSimple tároló StorSimple Manager szolgáltatások listája. Az egyes StorSimple Manager szolgáltatásokhoz StorSimple Manager lapon látható az alábbi adatokat:

- **Név** – a nevet, amelyet van beállítva a StorSimple kezelő szolgáltatás létrehozásakor jött létre automatikusan. A szolgáltatás neve nem módosíthatók, a szolgáltatás létrehozását követően.

- **Állapot** – a szolgáltatást, amely lehet **aktív**, **létrehozása**vagy **Online**állapotának.

- **Hely** – az StorSimple eszköz telepítik földrajzi helyét.

- **Előfizetés** – a számlázási előfizetés társított a szolgáltatásban.

A leggyakoribb feladatok a StorSimple Manager lap keresztül elvégezhető a következők:

- A szolgáltatás hozzon létre
- A szolgáltatás törlése
- A szolgáltatás regisztrációs kulcs beszerzése
- A szolgáltatás regisztrációs kulcs újragenerálása

Ebben az oktatóanyagban végre az alábbi műveleteket ismerteti. Ez a cikk tárolt adatok esetén csak StorSimple virtuális tömbök alkalmazható. További információt a StorSimple 8000 sorozat lépjen a [StorSimple kezelő szolgáltatás telepítése](storsimple-manage-service.md).

## <a name="create-a-service"></a>A szolgáltatás hozzon létre

A **Gyors létrehozása** beállítás használatával hozzon létre egy StorSimple kezelő szolgáltatás, ha a StorSimple eszköze telepíteni szeretné. Szolgáltatás létrehozására van szükség:

- Egy Enterprise Agreement-előfizetés
- Microsoft Azure active tároló fiók
- A számlázási információkat, amellyel hozzáférés kezelése

Is választhat egy alapértelmezett tároló fiókot létrehozni, a szolgáltatás létrehozásakor.

Egyetlen szolgáltatás több is kezelheti. Egy eszközt azonban több szolgáltatás nem időtartamát. A nagy vállalati beállíthatja, hogy több szolgáltatás példányainak készült különböző előfizetések, szervezetek vagy akár telepítési helye.  

> [AZURE.NOTE] Különálló példányai StorSimple kezelő szolgáltatás StorSimple 8000 sorozat eszközök és StorSimple virtuális tömbök kezeléséhez szükséges.

A következő lépésekkel szolgáltatás hozzon létre.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>A szolgáltatás törlése

Töröl egy szolgáltatást, mielőtt győződjön meg arról, hogy nincsenek csatlakoztatott eszközök azt használja. Ha a szolgáltatást használja, a csatlakoztatott eszközök inaktiválása Az inaktiválás művelet Server alkalmazás az eszközt, és a szolgáltatás közötti kapcsolat, de a felhőben az eszköz adatainak megőrzése. 

> [AZURE.IMPORTANT] Miután a szolgáltatás törlődik, a művelet nem vonható vissza. 

A következő lépésekkel törli a szolgáltatást.

### <a name="to-delete-a-service"></a>A szolgáltatás törlése

1. **StorSimple kezelő szolgáltatás** lapján jelölje be a szolgáltatást, amely a törölni kívánt.

1. Kattintson a **Törlés** az oldal alján.

1. A jóváhagyást kérő értesítésnél kattintson az **Igen** gombra. Eltarthat néhány percig, amíg a szolgáltatás törlődik.

## <a name="get-the-service-registration-key"></a>A szolgáltatás regisztrációs kulcs beszerzése

Miután létrehozott egy szolgáltatás sikeresen, szüksége lesz az StorSimple eszköz regisztrálhatja szolgáltatással. Az első StorSimple eszköz regisztrálni, szüksége lesz a szolgáltatás regisztrációs billentyűt. További eszközök regisztrálása egy meglévő StorSimple szolgáltatást, szüksége lesz a regisztrációs kulcsa és a szolgáltatás adatok titkosítókulcs (amely létrejön az első eszközön regisztráció során). Adatok titkosítókulcs kapcsolatos további tudnivalókért olvassa el a [felhasználói felület helyi webről szolgáltatás adatok titkosítókulcs első](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)című témakört. 

A következő lépésekkel beszerzése a szolgáltatás regisztrációs billentyűt.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

A szolgáltatás regisztrációs kulcs ne egy megbízható helyre. Szüksége lesz a következő kulcsot, valamint a szolgáltatás adatok titkosítási kulcs, további eszközök regisztrálása ezt a szolgáltatást. A szolgáltatás regisztrációs kulcs megszerzése, után szüksége lesz állítsa be az eszközt, a Windows Powershellen keresztül StorSimple kapcsolat.

## <a name="regenerate-the-service-registration-key"></a>A szolgáltatás regisztrációs kulcs újragenerálása

Szüksége lesz egy szolgáltatás regisztrációs kulcs újragenerálása, ha fő Forgatás elvégzéséhez szükséges, vagy ha megváltozott a szolgáltatás-rendszergazdák listája. A kulcs követező létrehozásakor, az új kulcs csak használatos további eszközök regisztrálásakor. Az eszközök, amelyek már regisztráltak a folyamat nem érinti.

Hajtsa végre az alábbi lépéseket a szolgáltatás regisztrációs kulcs újbóli.

### <a name="to-regenerate-the-service-registration-key"></a>A szolgáltatás regisztrációs kulcs újbóli

1. A **StorSimple kezelő szolgáltatás** lapon kattintson a **Regisztráció billentyűt**.

1. Kattintson a **Szolgáltatás regisztrációs kulcs** párbeszédpanelen **újragenerálása**.

1. A megerősítést kérő üzenet jelenik meg. Kattintson az **OK gombra** az újbóli folytatásához.

1. Új szolgáltatás regisztrációs kulcs fog megjelenni.

1. Másolja a következő kulcsot, és mentse az új eszközök regisztráció ezt a szolgáltatást.

1. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-ova-manage-service/image7.png) Ez a párbeszédpanel bezárásához.


## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan az [első lépések](storsimple-ova-deploy1-portal-prep.md) a egy StorSimple virtuális tömböt.
    
- További tudnivalók [az StorSimple eszköz](storsimple-ova-web-ui-admin.md)felügyelete.

 
