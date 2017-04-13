<properties 
    pageTitle="Egy Linux virtuális Apache Qpid Proton-C telepítése |} Microsoft Azure"
    description="Hogyan hozhat létre egy CentOS Linux virtuális Azure virtuális gépeken futó használatával és összeállítása és telepítse a Apache Qpid Proton-C tárat."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Egy Linux Azure virtuális Apache Qpid Proton-C telepítése

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Ebből a szakaszból megtudhatja, hogyan hozhat létre egy CentOS Linux virtuális Azure virtuális gépeken futó használatával és a letöltési, összeállítása és telepítse a Apache Qpid Proton-C tárat a Python és PHP nyelvi kötések együtt. Lépések végrehajtása után lesz a Python és PHP minták, ez az útmutató részét képező futtatásához.

Az első lépés az [Azure klasszikus portal][]segítségével végezhető el. Az alábbi képernyőfelvételen jeleníti meg, hogyan egy "scott-centos" nevű CentOS virtuális jön létre:

![Kattintson egy Linux Azure virtuális proton][0]

-Kiépítése után a portálon eredménye a következő:

![Kattintson egy Linux Azure virtuális proton][1]

Annak érdekében, hogy jelentkezzen be a számítógépre, a a végpontot port ismernie kell a SSH. Az újonnan létrehozott virtuális kijelölése, és kattintson a **Végpontok** lapon ezt az értéket az [Azure klasszikus portál][] szerezhet be. Az alábbi képernyőfelvételen jelzi, hogy a számítógép a nyilvános SSH port 57146.

![Kattintson egy Linux Azure virtuális proton][2]

A számítógép SSH használatával is csatlakozhat. Ebben a példában a gitt eszközt, ahogy az alábbi képernyőfelvételen:

![Kattintson egy Linux Azure virtuális proton][3]

A Python és PHP alkalmazásai ebben a példában a Proton ügyfél tárai Apache. A tárak [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)letölthető érhetők el. A terjesztési csomag fontos fájl telepítése a függőségeket, és hozza létre a Proton lépéseit ismerteti. Az alábbiakban összefoglaljuk a lépéseket:

1.  A yum konfigurációs fájl szerkesztése (/ etc/yum.conf) és a frissítések kizárása kernel fejlécek meg Megjegyzés (\# kizárása = kernel\*). Ez a a ÖET fordító telepítéséhez szükséges.

2.  Telepítse a csomagokat:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Töltse le a Proton tár:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Bontsa ki a Proton kódot a terjesztési archívumból:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Össze, és telepítse a kód fontos fájl származik, az alábbi lépésekkel:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Következő lépés elvégzése után Proton telepítve a számítógépen és használatra kész.

## <a name="next-steps"></a>Következő lépések

Készen áll a további információt? Látogasson el a következő hivatkozásra:

- [Szolgáltatás Bus AMQP – áttekintés][]

[Szolgáltatás Bus AMQP – áttekintés]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure klasszikus portál]: http://manage.windowsazure.com


