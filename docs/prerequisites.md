## Prerequisites

### Set up Coherence JMX Reporting 

Oracle JMX Reporting provides insight into what your Coherence Cluster is doing 

1. You should use for management/reporting purposes a com.tangosol.net.management.MBeanConnector, passing -rmi as argument, and using with the following system properties:

	 -Dtangosol.coherence.management.report.autostart=true
	 -Dcom.sun.management.jmxremote.ssl=false
	 -Dcom.sun.management.jmxremote.authenticate=false -Dtangosol.coherence.management=all

2. Your reporting node needs to be configured to use the report-all.xml default reporter configuration (contained in coherence.jar). This is done by setting the following system property: -
	Dtangosol.coherence.management.report.configuration=config/report-all.xml
3. The others nodes should all be running with the following system properties: -Dtangosol.coherence.management=local-only -Dtangosol.coherence.management.remote=true
 
4. Cache configuration needs to have local-scheme/unit-calculator set to binary (to report byte use) - as show in the coherence-cache-config.xml - and the proxy service names should contain proxy.

	An explanation of the contents of the Coherence log file data can be found [here](http://coherence.oracle.com/display/COH35UG/Analyzing+Reporter+Content).

### Set up Coherence to log to file 
Metrics on  memory utilization of your Cohereence  Cluster  is provided by enabling ConcurrenctMarkSweep for the garbage collector. This follow [Oracle Best Practices](http://coherence.oracle.com/display/COH35UG/Best+Practices#BestPractices-HeapSizeConsiderations)

Set up the following system property:

	-XX:+UseConcMarkSweepGC.

### Set up Coherence for GC logging
oiCoherence can redirect output from stdout to a file. Add the following system property

	-Dtangosol.coherence.log=myLogFile.log.

	-Dtangocol.coherence.log=log4j 

Make sure that your log4j.properties file is in the classpath


### Set Coherence Garbage Collection
One of the major causes of system lock-ups in a Coherence cluster is a major Garbage Collection event. This endangers the 
reliability of the data stored in your Coherence cluster. It is important to monitor the Garbage Collector and the Heap Utilisation of your 
coherence services.

Set the following properties:

	-verbosegc -XX:+PrintGCDetails     (this enables gc logging )

	-Xloggc:xxxx.log                      (redirects the output to the log)

	-XX:+PrintGCDateStamps              -  this flag time stamps your logs ( highly recommended )

Sample output from a gc log file:

	2011-11-21T13:42:27.624+0000: 14356.771: [GC 14356.771: [ParNew: 104960K->4875K(118016K), 0.0051590 secs] 129566K->29482K(1035520K), 0.0052400 secs] [Times: user=0.04 sys=0.00, real=0.00 secs]

