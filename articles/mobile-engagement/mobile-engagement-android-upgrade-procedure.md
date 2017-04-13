<properties 
    pageTitle="Azure mobil tetszés szerint elmélyedhet Android SDK integrációja" 
    description="Legújabb frissítésekkel és Azure Mobile tetszés szerint elmélyedhet Android SDK eljárások"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="upgrade-procedures"></a>Frissítési eljárások

Ha akkor már van beépül a SDK régebbi verzióját az alkalmazást, akkor a SDK frissítéskor, vegye figyelembe az alábbiakat.

Előfordulhat, hogy végre kell hajtani több eljárás, ha Ön kihagyott a SDK különböző verzióiban. Ha például telepít át 1.4.0 1.6.0 először menete a "feladó 1.4.0 való 1.5.0" kell majd a "from 1.5.0 való 1.6.0" eljárást.

Tekintet nélkül a verziót frissít, ki kell cserélni a `mobile-engagement-VERSION.jar` az újat.

##<a name="from-420-to-421"></a>A 4.2.0 való 4.2.1.

Ebben a lépésben ténylegesen végezhető bármely a SDK verzióját, hogy a biztonság fokozása amikor elér tevékenységek integrálnia.

Célszerű most hozzáadni `exported="false"` összes vannak tevékenységekre.

Vannak tevékenységek kell a következőhöz hasonlóan a `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>A 4.0.0 való 4.1.0

Az SDK most fogópont új jogosultsági modellben Android M.

Ha helyre te104004683et vagy áttekintés értesítések, olvassa el [Ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Az új jogosultsági modell kívül most támogatjuk futásidőben konfigurálása hely funkciók.
Azt is kompatibilisek legyenek a nyilvánvalóan paraméterek hely, de most elavult. Futási idejű konfiguráció használata, távolítsa el az alábbi szakaszok a a ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

és olvassa el [ezt az eljárást frissített](mobile-engagement-android-integrate-engagement.md#location-reporting) futtatókörnyezet konfigurációs használja helyette.

##<a name="from-300-to-400"></a>A 3.0.0 való 4.0.0

### <a name="native-push"></a>Natív leküldéses

Natív leküldéses (GCM/OPA) most is használható az alkalmazás, meg kell adnia a leküldéses marketingkampány bármilyen natív leküldéses hitelesítő adatait.

Ha még nem hajtsa végre [ezt az eljárást](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Vannak integrációs módosították ``AndroidManifest.xml``.

Csere ki:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Szerint

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Már létezik esetleg betöltése képernyőre (ha benne szöveg/webtartalom) tartalmat vagy szavazás elemére.
Hozzáadja az adott munkára 4.0.0 az kampányok van:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Erőforrások

Az új beágyazási `res/layout/engagement_loading.xml` fájlt a merevlemezről a projektben.

##<a name="from-240-to-300"></a>A 2.4.0 való 3.0.0

Az alábbi leírja, hogy miként telepítheti át SDK integrációs-alkalmazásba, Azure Mobile tetszés szerint elmélyedhet hajtott Capptain Társítások által kínált Capptain szolgáltatás. Ha egy korábbi verzióról, olvassa el az első 2.4.0 áttelepítése a Capptain webhely, és majd alkalmazza az alábbi eljárást.

>[AZURE.IMPORTANT] Capptain és Mobile tetszés szerint elmélyedhet nem ugyanazokkal a szolgáltatásokkal, és az alábbi eljárás csak kiemeli az ügyfél alkalmazás áttelepítése. Az alkalmazásban a SDK áttelepítése nem áttelepíti az adatok a Capptain kiszolgálókról Mobile tetszés szerint elmélyedhet kiszolgálókhoz.

### <a name="jar-file"></a>ÜVEG fájl

Csere `capptain.jar` által `mobile-engagement-VERSION.jar` a a `libs` mappát.

### <a name="resource-files"></a>Erőforrásfájl

Minden erőforrásfájl, hogy a megadott (alapértelmezés szerint `capptain_`) az új felhasználókat helyébe van (Ha `engagement_`).

Ha testre szabott azokat a fájlokat, akkor alkalmazza újra a testreszabási új fájlok, **az erőforrás-fájlok összes azonosítók is átnevezett**.

### <a name="application-id"></a>Azonosítója

Most már tetszés szerint elmélyedhet a kapcsolati karakterlánc használja a SDK azonosítók, például az alkalmazás azonosítója konfigurálása.

Kell használnia, `EngagementAgent.init` módszer az alkalmazásindító tevékenység jelennek meg:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

A kapcsolati karakterláncot, az alkalmazás a Azure portálon jelenik meg.

Távolítson el minden hívást `CapptainAgent.configure` mint `EngagementAgent.init` Ez a módszer helyettesíti.

A `appId` már nem konfigurálhatók `AndroidManifest.xml`.

Távolítsa el az ebben a szakaszban a `AndroidManifest.xml` , ha:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API

Minden hívást valamelyik Java osztálynak a SDK van átnevezendő; Ha például `CapptainAgent.getInstance(this)` átnevezendő `EngagementAgent.getInstance(this)`, `extends CapptainActivity` átnevezendő `extends EngagementActivity` stb...

Ha módosította integrálva alapértelmezett ügynök preferencia-sorrend fájlokat, az alapértelmezett fájlnevet ettől kezdve `engagement.agent` és a kulcs `engagement:agent`.

Webhely hirdetmények létrehozásakor a Javascript Iratgyűjtő ettől kezdve `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Módosítások sok van történt, a szolgáltatás már nincs megosztva, és sok címzett nem exportálható eltűnt.

A szolgáltatás nyilatkozat ettől kezdve egyszerűbb; a leképezési szűrő és az összes metaadatai belül a kijelöléséhez eltávolítása és hozzáadása `exportable=false`.

Valamint minden új neve tetszés szerint elmélyedhet használni.

Akkor most néz ki:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Teszt naplók engedélyezni szeretné, ha a metaadatok most át lett helyezve a alkalmazás címkét, és átnevezése:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Minden más metaadatok csak átnevezett, Íme a teljes beállításlista (tanfolyam Átnevezés csak a használt lehetőségekből):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

A Google Play és nyomon követése SmartAd el lett távolítva SDK, ha el szeretné távolítani a csere nélkül van:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

A gyermekektől tevékenységek most már vannak deklarálva jelennek meg:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Ha az egyéni vannak tevékenységek, csak módosítani szeretné a megfelelő vagy a leképezési műveletek `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` vagy `com.microsoft.azure.engagement.reach.intent.action.POLL`.

A közvetítési vevők átnevezett, valamint most hozzáadása `exported=false`. Az alábbiakban a vevők és az új meghatározását, (tanfolyam Átnevezés csak a használt lehetőségekből) teljes listája:

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Nyomon követése a címzett megszűnt, így Önnek nem kell a szakasz eltávolítása:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Megjegyzendő, hogy a közvetített címzett **EngagementMessageReceiver** példányába nyilatkozat megváltozott a `AndroidManifest.xml`. Ennek az oka az API küldéséhez és tetszőleges XMPP üzenetek eltávolítása tetszőleges XMPP személyek és az API eszközök közötti üzenetek küldése és fogadása el lett távolítva. Így kell is a következő visszahívást törlése **EngagementMessageReceiver** Önnél.

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

és

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Törölje a bármely **EngagementAgent** -hívás:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

és

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard konfigurációs is hatással lehet rebranding, például most keresi a szabályok:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
