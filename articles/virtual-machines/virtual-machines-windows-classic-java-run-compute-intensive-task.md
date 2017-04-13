<properties
    pageTitle="A virtuális a számítási igényű Java-alkalmazások |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy Azure virtuális gépen futó másik Java-alkalmazás figyelhető számítási igényű Java-alkalmazások."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Hogyan Java virtuális gépen számítási igénylő feladat futtatása

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Az Azure-virtuális gép számítási igénylő feladatok kezelésére is használhatja. Például egy virtuális gép feladatok kezelésére és eredmények nyújtásához ügyfél gépek vagy mobilalkalmazások. Ez a cikk elolvasása, után megértéséhez, hogy miként hozhat létre egy másik Java-alkalmazás figyelhető számítási igényű Java alkalmazást futtató virtuális gép lesz.

Ebben az oktatóanyagban feltételezi, hogy hogyan hozhat létre Java konzol alkalmazások, a Java alkalmazásba importálhatja a tárak és hozhat létre egy Java archívum (üveg). Nem ismeri a Microsoft Azure tekinti.

Ismerkedhet meg:

* Bemutatja, hogyan hozhat létre virtuális gép az egy Java fejlesztési Kit (JDK) már telepítve van.
* Hogyan lehet távolról jelentkezzen be a virtuális géphez.
* Hogyan lehet szolgáltatás bus névtér létrehozása.
* Hogyan lehet egy számítási igényű feladatot hajt végre Java-alkalmazás létrehozása.
* Hogyan lehet a számítási igénylő tevékenység előrehaladását figyeli Java-alkalmazás létrehozása.
* Hogyan lehet Java alkalmazást.
* Hogyan szüntetheti meg az Java-alkalmazásokat.

Ebben az oktatóanyagban fogja használni a Traveling üzletkötő probléma a számítási igényű feladatot. Az alábbi képen a számítási igénylő feladat futtatása Java-alkalmazás.

![A solver Traveling üzletkötő probléma][solver_output]

Az alábbi képen az Java-alkalmazás figyelése a számítási igényű feladatot.

![A probléma Traveling üzletkötő ügyfél][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Virtuális gép létrehozása

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2. Kattintson az **Új**, kattintson a **számítja ki**, **virtuális gép**kattintson, és válassza **A gyűjtemény**.
3. **Virtuális gép kép kiválasztása** párbeszédpanelen jelölje be a **JDK 7 Windows Server 2012-ben**.
Ne feledje, hogy **JDK 6 Windows Server 2012-ben** elérhető abban az esetben, ha van régebbi alkalmazások, amelyek nem JDK 7 futtathatja.
4. Kattintson a **Tovább**gombra.
4. A **virtuális gép konfigurációs** párbeszédpanel:
    1. Adja meg a virtuális számítógép nevét.
    2. Adja meg a virtuális gép használandó méretét.
    3. A **Felhasználónév** mezőbe írja be a rendszergazda nevét. Ne feledje, hogy ez a név és a jelszót, akkor ezután írja be, amikor távolról jelentkezzen be a virtuális gép használandó őket.
    4. Az **Új jelszó** mezőbe írjon be egy jelszót, és adja meg újra a **Megerősítés** mezőben. Ez az a rendszergazdai fiók jelszava.
    5. Kattintson a **Tovább**gombra.
5. A következő **virtuális gép konfigurációs** párbeszédpanelen:
    1. **Felhőbeli szolgáltatástól**használja az alapértelmezett **létrehozása egy új felhőszolgáltatásba**.
    2. **Felhőalapú szolgáltatás DNS-** név az értéknek egyedinek kell lennie a cloudapp.net keresztül. Szükség esetén módosítsa ezt az értéket, hogy Azure azt jelzi, hogy egyedi.
    2. Adja meg a régió, a affinitás csoport vagy a virtuális hálózat. Ebben az oktatóanyagban alkalmazásában adja meg, például a **Nyugati USA -beli**terület.
    2. **Tárterület-fiókot**jelölje be **az automatikusan generált tárterület-fiók használata**.
    3. **Elérhetőség beállítása**jelölje be a **(nincs)**.
    4. Kattintson a **Tovább**gombra.
5. A végső **virtuális gép beállításai** párbeszédpanelen:
    1. Fogadja el az alapértelmezett végpont bejegyzéseket.
    2. Kattintson a **kész**gombra.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>A virtuális géphez távolról bejelentkezés

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2. Kattintson a **virtuális gépeken futó**.
3. Kattintson a nevére a virtuális gépen jelentkezzen be a kívánt.
4. Kattintson a **Csatlakozás**gombra.
5. A virtuális gépen való kapcsolódáshoz szükség szerint megválaszolása a képernyőn megjelenő utasításokat. Amikor a rendszer kéri a rendszergazda nevét és jelszavát, használja a virtuális gép létrehozásakor megadott értékeket.

Figyelje meg, hogy az Azure Service Bus funkciókhoz a Egerben CyberTrust legfelső szintű tanúsítvány telepítve legyen a JRE **cacerts** áruházból részeként. A tanúsítvány automatikusan tartalmazza az a Java futtatókörnyezet (JRE) ebben az oktatóanyagban által használt. A tanúsítvány nem rendelkezik a JRE **cacerts** áruházban, ha [felvétele] című témakör tartalmaz egy tanúsítványt az Java hitelesítésszolgáltató tanúsítvány tárolásához[ add_ca_cert] kapcsolatban tudnivalókat azt (, valamint a tanúsítványok megtekintése a cacerts áruházban).

## <a name="how-to-create-a-service-bus-namespace"></a>Hogyan hozhat létre egy szolgáltatás bus névtere

Azure Service Bus sorban várakozó használatának megkezdéséhez, először létre kell hoznia egy szolgáltatás névtere. Egy szolgáltatás névtere tartalmazó tároló nyújt a megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül.

A szolgáltatás névtere létrehozása:

1.  Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2.  A bal alsó navigációs ablakban a Azure klasszikus portál **szolgáltatás Bus, a hozzáférés-vezérlés és a gyorsítótár**gombra.
3.  Az Azure klasszikus portál bal felső munkaablakban kattintson a **Szolgáltatás Bus** csomópontra, és kattintson az **Új** gombra.  
    ![Szolgáltatási Bus csomópont képernyőképe][svc_bus_node]
4.  **Egy új szolgáltatás Namespace létrehozása** párbeszédpanelen adja meg a **Namespace**, és győződjön meg arról, hogy az egyedi, kattintson az **Elérhetőség ellenőrzése** gombra.  
    ![Hozzon létre egy új Namespace képernyőképe][create_namespace]
5.  Miután gondoskodhat arról, hogy a névtér neve áll rendelkezésre, válassza az ország vagy régió, amelyben a névtér kell tárolni, és kattintson a **Namespace létrehozása** gombra.  

    A létrehozott névtér az Azure klasszikus portál fog megjelenni, és aktiválása egy kis időt vesz igénybe. Várja meg, amíg az Állapot oszlop értéke **az aktív** , mielőtt a következő lépéssel folytatná.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Alapértelmezett felügyeleti hitelesítő adatait a névtér beszerzése

Adatkezelési műveletek, például várólista, hozza létre az új névtér végrehajtásához kell szerezze be a névtér management hitelesítő adatait.

1.  A bal oldali navigációs területen kattintson a **Szolgáltatás Bus** csomópontra elérhető névtér listájának megjelenítéséhez.
    ![Rendelkezésre álló névtér képernyőképe][avail_namespaces]
2.  Jelölje ki az imént létrehozott a listában látható névtér.
    ![Namespace lista képernyőképe][namespace_list]
3.  A jobb oldali **Tulajdonságok** ablaktáblában az új névtér tulajdonságait sorolja fel.
    ![Tulajdonságok ablak képernyőképe][properties_pane]
4.  Az **Alapértelmezett kulcs** el van rejtve. A **Nézet** gombra kattintva megjelenítheti a hitelesítő adatokat.
    ![Alapértelmezett kulcs képernyőképe][default_key]
5.  Jegyezze le a **Kibocsátó alapértelmezett** és az **Alapértelmezett kulcs** ezt az információt az alábbi műveletek elvégzését a névtér a program használata.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Hogyan hozhat létre, amely egy számítási igényű feladatot hajt végre Java-alkalmazások

1. A számítógépen fejlesztését (amelyek nem kell az Ön által létrehozott virtuális számítógépen) a [Azure SDK Java](https://azure.microsoft.com/develop/java/)letöltése.
2. Ez a szakasz végén példa kód használatával Java console-alkalmazás létrehozása. Ebben az oktatóanyagban használjuk **TSPSolver.java** Java nevet. Módosítsa a **a\_szolgáltatás\_bus\_névtér**, **a\_szolgáltatás\_bus\_tulajdonosa**, és **a\_szolgáltatás\_bus\_kulcs** a szolgáltatás használatához a helyőrzők bus **névtér**, **Alapértelmezett kibocsátó** és **Kulcs alapértelmezett** értékét, illetve.
3. Kódolási, exportálása egy runnable Java archívumba (üveg) az alkalmazás és a szükséges tárak package be a létrehozott üveg. Ebben az oktatóanyagban **TSPSolver.jar** használjuk a létrehozott üveg neveként.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Hogyan hozhat létre, amely a számítási igénylő tevékenység előrehaladását figyeli Java-alkalmazások

1. A fejlesztői számítógépen példa kód használatával Ez a szakasz végén Java console-alkalmazás létrehozása. Ebben az oktatóanyagban használjuk **TSPClient.java** Java nevet. Ahogy korábbi verziójában, módosítsa a **a\_szolgáltatás\_bus\_névtér**, **a\_szolgáltatás\_bus\_tulajdonosa**, és **a\_szolgáltatás\_bus\_kulcs** a szolgáltatás használatához a helyőrzők bus **névtér**, **Alapértelmezett kibocsátó** és **Kulcs alapértelmezett** értékét, illetve.
2. Az alkalmazás egy runnable üveg exportálni, és a szükséges tárak csomag be a létrehozott üveg. Ebben az oktatóanyagban **TSPClient.jar** használjuk a létrehozott üveg neveként.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Hogyan kell futtatni a Java-alkalmazások
Futtassa a számítási igénylő alkalmazás, először hozzon létre a várakozási sorban található, majd a út közben Saleseman probléma megoldására, amelyek az aktuális legjobb továbbítására hozzáadja a szolgáltatás bus várakozási sorban található. Míg a számítási igényű fut (vagy később), futtassa az ügyfél, a szolgáltatás bus sorból eredmények megjelenítéséhez.

### <a name="to-run-the-compute-intensive-application"></a>A számítási igényű alkalmazásnak a futtatására

1. Jelentkezzen be a virtuális géphez.
2. Hozzon létre egy mappát, ahová futtatható az alkalmazást. Ha például **c:\TSP**.
3. **C:\TSP**, **TSPSolver.jar** másolása
4. Hozzon létre egy **c:\TSP\cities.txt** a következő tartalommal nevű fájlt.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. A parancssorablakban c:\TSP könyvtárak értékre.
6. Győződjön meg róla, a PATH környezeti változó szerepel-e a JRE bin mappát.
7. A szolgáltatás bus várakozási sorban található, a Szolgáltató a solver függvényérték futtatása előtt hozzon létre kell. A következő parancsot a szolgáltatás bus várólista létrehozásához.

        java -jar TSPSolver.jar createqueue

8. Most, hogy a sor hoz létre, a Szolgáltató a solver függvényérték futtathatja. Például a következő parancsot a solver található 8 városok futtatásához.

        java -jar TSPSolver.jar 8

 Ha nem adja meg a számot, a 10 városok fog futni. A solver aktuális legrövidebb útvonalak talál, mint azt hozzáadja őket a sor.

> [AZURE.NOTE]
> Minél nagyobb a megadott számú, a már a solver fog futni. Ha például fut 14 városok eltarthat néhány percig, és 15 városok használatának megkezdése több óráig is eltarthat. 16 vagy több városok növelni vezethet (végül hét, hónap és év) futtatókörnyezet napokban. Ez a város nő számát a solver által kiértékelt variációinak gyors növekedése miatt.

### <a name="how-to-run-the-monitoring-client-application"></a>A felügyeleti ügyfélalkalmazás futtatása
1. Jelentkezzen be a számítógépen, ahová az ügyfélalkalmazás futtatható. Ez nem kell lennie az **TSPSolver** alkalmazást futtató ugyanarra a gépre annak ellenére, hogy is.
2. Hozzon létre egy mappát, ahová futtatható az alkalmazást. Ha például **c:\TSP**.
3. **C:\TSP**, **TSPClient.jar** másolása
4. Győződjön meg róla, a PATH környezeti változó szerepel-e a JRE bin mappát.
5. A parancssorablakban c:\TSP könyvtárak értékre.
6. A következő parancsot.

        java -jar TSPClient.jar

    Tetszés szerint adja meg, hogy hány perc legyen a várakozási sorban található ellenőrzése a parancssori argumentum átadása által alvó. Az alapértelmezett meddig alhatnak időszak ellenőrzéséhez a várakozási sorban található érték 3 perc, mellyel, ha nincs parancssori argumentum átadott **TSPClient**. Ha szeretne egy másik értéket használja a meddig alhatnak intervallum, például egy perc, futtassa a következő parancsot.

        java -jar TSPClient.jar 1

    Az ügyfél mindaddig, amíg azt láthatja, hogy várólista "Kész" üzenetet fog futni. Fontos tudni, hogy ha többszöri előfordulása a solver futtatása az ügyfél nélkül, akkor előfordulhat, hogy futtassa az ügyfél többször teljesen kiüríteni a sor. Azt is megteheti a sor törlése, és ezután ismét létrehozni. A sor törlése, a következő **TSPSolver** (nem **TSPClient**) parancsot.

        java -jar TSPSolver.jar deletequeue

    A solver fog futni, amíg be nem fejezi az összes útvonal vizsgálata.

## <a name="how-to-stop-the-java-applications"></a>Hogyan szüntetheti meg az Java-alkalmazások
A solver és ügyfélalkalmazásokban nyomja le **A Ctrl + C** való kilépéshez, ha be szeretné fejezni a rendes befejezés előtt.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
