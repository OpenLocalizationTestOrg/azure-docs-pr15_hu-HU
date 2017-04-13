<properties
    pageTitle="Speciális Azure Mobile tetszés szerint elmélyedhet Android SDK jelentéskészítési lehetőségei"
    description="Megtudhatja, hogy miként végezze el a speciális jelentéskészítés az Azure Mobile tetszés szerint elmélyedhet Android SDK analytics rögzítéséhez"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Speciális Android rendszerű tetszés szerint elmélyedhet a jelentéskészítés

> [AZURE.SELECTOR]
- [Univerzális Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-advanced-reporting.md)

Ez a témakör ismerteti az Android alkalmazásban további jelentéskészítő forgatókönyvek. A beállításokkal alkalmazhat az alkalmazást, az [Első lépések](mobile-engagement-android-get-started.md) oktatóprogram során létre.

## <a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Ha befejezte az oktatóprogram szándékosan közvetlen és egyszerű, de vannak, speciális beállítások választhat.

## <a name="modifying-your-activity-classes"></a>Módosítása a `Activity` osztályok

Az [első lépések oktatóprogram](mobile-engagement-android-get-started.md)kellett végezze el az összes volt, hogy a `*Activity` alosztályok örökölhet a megfelelő `Engagement*Activity` osztályok. Ha például a régi tevékenység bővített `ListActivity`, kiterjesztése tenné `EngagementListActivity`.

> [AZURE.IMPORTANT] Használatakor `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy bármelyik hívása `requestWindowFeature(...);` történik, mielőtt a hívást `super.onCreate(...);`, egyéb esetben e az összeomlást fordul elő.

Ezek az osztályok megtalálhatja a `src` mappát, és bemásolhatja őket a projectbe. Az osztályok is szerepelnek a **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatív módszer: hívás `startActivity()` és `endActivity()` manuálisan

Ha nem szeretne a többszörös vagy nem a `Activity` osztályok, helyette indítása és a tevékenység befejezéséhez hívja fel a `EngagementAgent`meg közvetlenül a módszereket.

> [AZURE.IMPORTANT] Az Android SDK soha nem felhívja a `endActivity()` módszer, akkor is, ha az alkalmazás be van zárva (Android-eszközön, alkalmazások nem soha nem zárul). Így célszerű *erősen* ajánlott, ha fel szeretne hívni a `startActivity()` módszer a `onResume` visszahívást az *összes* a tevékenységek, és a `endActivity()` módszer a `onPause()` *összes* a visszahívás a tevékenységek. Ez a csak úgy, hogy meggyőződhessen róla, hogy a program nem kiszivárogtatott munkamenetek. A munkamenet kiszivárogtatott van, ha a tevékenységek szolgáltatás soha nem kapcsolat bontása a tetszés szerint elmélyedhet kódmentes (mivel ez a szolgáltatás maradjon csatlakoztatott, mindaddig, amíg a munkamenet folyamatban).

Lássunk egy példát:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Ez a példa hasonlít a `EngagementActivity` osztály és a változatok, amelynek forráskód van megadva, a `src` mappát.

## <a name="using-applicationoncreate"></a>Application.onCreate() használata

Bármilyen kódot elhelyeztem `Application.onCreate()` és másik alkalmazásban visszahívást az alkalmazás folyamatok, beleértve a tetszés szerint elmélyedhet szolgáltatás futtatása. Előfordulhat, hogy a nem kívánt oldal effektusok, például a felesleges memória hozzárendelések és a beszélgetésekben a tetszés szerint elmélyedhet folyamatban, vagy az ismétlődő közvetítési vevők vagy a szolgáltatások.

Ha Ön felülbírálása `Application.onCreate()`, javasoljuk, hogy a következő kódrészletet hozzáadása az elején a `Application.onCreate()` függvény:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Is ugyanezt a célt szolgálja `Application.onTerminate()`, `Application.onLowMemory()`, és `Application.onConfigurationChanged(...)`.

Is bővítheti `EngagementApplication` helyett bővítése `Application`: a visszahívás `Application.onCreate()` jelent, a folyamat ellenőrzése és hívások `Application.onApplicationProcessCreate()` csak ha az aktuális folyamat nem az adott levő tetszés szerint elmélyedhet szolgáltatást, a többi visszahívást ugyanazokat a szabályokat alkalmazni.

## <a name="tags-in-the-androidmanifestxml-file"></a>Címkék a AndroidManifest.xml fájlban

A szolgáltatás címkébe a AndroidManifest.xml fájlban a `android:label` attribútum lehetővé teszi a felhasználóknak a "Services szolgáltatást futtató" a telefon képernyőjén megjelenő, válassza ki a tetszés szerint elmélyedhet szolgáltatás nevét. Ajánlott beállítást az attribútum `"<Your application name>Service"` (például `"AcmeFunGameService"`).

Adja meg a `android:process` attribútum biztosítja, hogy a tevékenységek szolgáltatás fut a saját (futó folyamat tetszés szerint elmélyedhet ugyanezt az eljárást, az alkalmazás lehetővé teszi a fő/felhasználói felületi szálon esetleg kisebb válaszol).

## <a name="building-with-proguard"></a>A ProGuard épület

A alkalmazáscsomag a ProGuard hoz létre, ha szeretne rögzíteni néhány osztályok. A következő konfigurációs kódtöredékének használható:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
