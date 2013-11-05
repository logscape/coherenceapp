## Application Overrides

Edit your jmxUrl and point it to your Coherence JMXReporter 

	jmxUrl=service:jmx:rmi://localhost:16331/jndi/rmi://localhost:16332/server
	bundle.defaults.resourceSelection=type contains Coh

The Overrides file contains system wide properties. 

 **jmxUrl** -  The url to our Coherence Reporter.  The bulk of Coherence Metrics come from this source
 **bundle.defaults.resourceSelection - Specifies which Indexer,Forwarder or IndexerStore the CoherenceApp will run on. For each distinct Coherence environment you will want to run a Single Agent.  You can use the resourceSelection to specifiy which agent this is. Here are some example values

 1. hostName equals raid0-lon-rh0	
 2. type containsAny Coh  - This will run on any agent containing the substring Coh. E.g CohIndexer, ProdCohForwarder etc


