# Elastic Stack: The Basics

---

Elastic Stack (ELK), initially for storing, searching, and visualizing large datasets in performance monitoring, has become a key 
tool in security operations center (SOC) for log investigations, functioning similarly to a Security Information and Event Management 
(SIEM) with strong search and visualization capabilities. Components comprise Elasticsearch, a full-text search and analytics engine 
handling JSON documents with RESTful API interaction; Logstash, a data processor with configurations split into input (specifying 
ingestion sources), filter (normalizing data), output (routing to Kibana, listening ports, Elasticsearch databases, files), supporting 
various plugins; Beats, lightweight host agents shipping specific data—Winlogbeat for Windows event logs, Packetbeat for network 
traffic flows—to Elasticsearch; Kibana, a web interface for real-time data analysis, investigation, and dashboard creation.

Workflow sees Beats gathering from multiple agents, Logstash parsing/normalizing into field-value pairs for storage in Elasticsearch 
as the searchable database, Kibana rendering visualizations like time charts or infographics.

Discover tab presents ingested logs, search bar, normalized fields, term/time-based filters. Elements include log rows with event info 
and field-values, fields pane listing parsed fields with top five values and occurrence percentages for filter addition/removal, 
index pattern selection (e.g., vpn_connections for VPN logs), search bar for queries, time filter for durations, timeline chart 
displaying event counts over time with selectable bars highlighting spikes, top bar for saving/opening/sharing searches, add filter 
for specific field restrictions.

Kibana Query Language (KQL) supports free text searches matching complete terms ("United States" but not "United"), wildcards 
("United*" for suffixes like Nations), logical operators—AND combining terms ("United States" AND "Virginia"), OR for alternatives 
("United States" OR "England"), NOT excluding ("United States" AND NOT "Florida"). Field-based searches use field:value syntax 
(Source_ip:238.163.231.224 AND UserName:Suleman), with fields populated on click.

Visualization tab generates tables, pie charts, bar charts; dragging fields creates correlations (Source_Country with Source_IP), 
saves with titles/descriptions.

Dashboards aggregate saved searches and visualizations for oversight, created by adding from library, adjusting placement, saving.

Lab connects to ELK instance at http://MACHINE_IP with Analyst/analyst123 credentials.

I find the timeline's spike detection useful for spotting anomalies in VPN logs, streamlining initial triage.

---

### Key Takeaways
- Navigate to Dashboard tab, click Create dashboard
- Click Add from Library
- Select visualizations and saved searches to add
- Adjust layout
- Save dashboard
- Click field in Discover, select visualization
- Choose type (table, pie, bar)
- Drag fields for correlations
- Save with title/description
- Click Add filter under search bar
- Select field, apply inclusion/exclusion via +/-
- View timeline, select bars for time-specific logs
- Search "United States" for exact matches
- Use "United*" for partial wildcard matches
- Combine with AND/OR/NOT ("United States" AND "Virginia", "United States" OR "England", "United States" AND NOT "Florida")
- Use field:value (Source_ip:238.163.231.224 AND UserName:Suleman) for precise queries
- Deploy Beats for data collection (Winlogbeat Windows events, Packetbeat network flows)
- Configure Logstash input/filter/output
- Store normalized data in Elasticsearch
- Visualize in Kibana
- Input defines sources
- Filter normalizes
- Output routes to Kibana, ports, Elasticsearch, files
- Elasticsearch searches/analyzes JSON via RESTful API
- Kibana analyzes/investigates/visualizes real-time streams with dashboards

---

