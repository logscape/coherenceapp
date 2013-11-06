## Prerequisites

### Set up Coherence JMX Reporting 


Here are some useful links on enabling Management Reporting in Coherence:

   - [Management Reporitng](http://docs.oracle.com/cd/E18686_01/coh.37/e18682/intro.htm#CEGGICFE)
   - [JMX Reporting](http://docs.oracle.com/cd/E18686_01/coh.37/e18682/reporter.htm#CHDECBIE)

Oracle JMX Reporting provides insight into the health of your Coherence Cluster by providing operational metrics on your Caches, Nodes and Services. 

+ Instruct the management node to use Java RMI. The example below is a basic setting:

		-Dtangosol.coherence.management.report.autostart=true
		 -Dcom.sun.management.jmxremote.ssl=false
		 -Dcom.sun.management.jmxremote.authenticate=false
		 -Dtangosol.coherence.management=all

+ Your reporting node needs to be configured to enable all the valid reports. The example below points to the report-all.xml found in the coherence-3.XX.jar. Update this to reflect your environment

		: -Dtangosol.coherence.management.report.configuration=config/report-all.xml


+ The others nodes should all be running with the following system properties: -Dtangosol.coherence.management=local-only -Dtangosol.coherence.management.remote=true
 
+ Cache configuration needs to have local-scheme/unit-calculator set to binary (to report byte use) - as shown in the coherence-cache-config.xml - and the proxy service names should contain proxy.

	An explanation of the contents of the Coherence log file data can be found [here](http://coherence.oracle.com/display/COH35UG/Analyzing+Reporter+Content).


### Set up Coherence for GC logging

Metrics on  memory utilization of your Cohereence  Cluster  is provided by enabling ConcurrenctMarkSweep for the garbage collector. This follows [Oracle Best Practices](http://coherence.oracle.com/display/COH35UG/Best+Practices#BestPractices-HeapSizeConsiderations)

Set up the following system property:

	-XX:+UseConcMarkSweepGC.

### Set up Coherence to log to file 

Coherence should be configure to redirect output from stdout to a file. Add the following system property

	-Dtangosol.coherence.log=logs/coh_jmx_node.log 

	-Dtangocol.coherence.log=log4j 

Make sure that your log4j.properties file is in the classpath

#### Updating your Coherence logging format. 

The Coherence logging format is defined using [logging-config](http://coherence.oracle.com/display/COH35UG/logging-config) element in your configuration. By default it is set a format that looks similar to this:

		2013-09-19 06:39:11.133 Oracle Coherence GE <Info> (thread=Cluster, member=1): Loaded included POF configuration from "jar:file:/home/logscape/coherence/prod/market-data/lib/coherence.jar!/coherence-pof-config.xml"
		2013-09-19 06:39:11.212 Oracle Coherence GE <D5> (thread=Invocation:Management, member=1): Service Management joined the cluster with senior service member 1

Override your logging-config to this:

	        <logging-config>
	                <!--message-format>{date}/{uptime} {product} {version} &lt;{level}&gt; (thread={thread}, member={member}): {text}
	                </message-format-->

	                <message-format>{date}/{uptime} {product} {version} &lt;{level}&gt; (thread={thread}, member={member}, role={role}, location={location}): {text}
	           </message-format>
	        </logging-config>

To get the ROLE and LOCATION attributes inserted into your log data. The output of your Coherence log4j logs should now look like this:


		2013-09-19 06:54:37.360/0.876 Oracle Coherence GE 3.6.1.0 <Info> (thread=main, member=n/a, role=, location=): Loaded Reporter configuration from "jar:file:/home/logscape/coherence/prod/market-data/lib/coherence.jar!/reports/report-group.xml"

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

