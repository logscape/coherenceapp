## Coherence Data Sources

The coherence app uses the following data sources to pull different types of log data produced by Coherence

 * coh-report-data	- 	This datasource points to where the Coherence Report data is located. This will typically include cache utilization, service status and resource usage of the Coherence nodes
 * coh-logs	-	This datasource points to the Coherence application logs. 
 * coh-app	-	By default this data source points to the output of the CoherenceApp. 


## Datasource - JMXReporter 
---
Point your Datasource to the folder where the jmx reports are being generated
**DataSource**: coh-report-data
 ![](/images/ds-jmxreporter-0.png)

 * Use the line break rule: "SingleLine"
 * Rolling Convention: Enabled

 ![](/images/ds-jmxreporter-1.png)


## Datasource - Coherence Log4j files
---

Point your datasource to the folder where Coherence application log data is stored. See the Oracle Web Site and the [prerequistes page](/docs/prerequisites.md) for the Coherence Log4j configuration 
**DataSource**:coh-logs

 * Line break rule: Year
 * Rolling convention: Enabled




## Datasource - Coherence GC Logs
---

The folder where your GC Logs are stored. 
**DataSource**:coh-gc-logs

 * Line break Rule: SingleLine
 * Rolling Convention: Disabled




