# Adobe CQ Exploitation Knowledge

Locating Hosts:
--------

  * Nerdy Data: [CQ_Analytics](https://search.nerdydata.com/search/#!/searchTerm=CQ_Analytics)
  
Links:
-----------

* http://crxdelight.com/

Directories
----------

Source: http://cq-ops.tumblr.com/post/21045033313/frequently-used-cq-urls

Assuming the base URL of the “author” instance to be this (need to edit to fit yours):

**Main:**

  * Admin Port
    * http://cqauthor1.example.com:4502
  * Main (Welcome) Page
    * /libs/cq/core/content/welcome.html
  * Login page
  	* /libs/cq/core/content/login.html
  	* [inurl:/libs/cq/core/content/login.html]
  * Website Administrator
    * /siteadmin
  *  Digital Asset Manager
    * /damadmin
  * Notification Inbox
    * /libs/cq/workflow/content/inbox.html

**Search:**

  * Search UI
    * /crx/explorer/ui/search.jsp?Path=&Query=
  * Query Debugger
    * /libs/cq/search/content/querydebug.html


**Client Context:**

  * /etc/clientcontext/default/content.html
 
**Translator Settings:**

  * /libs/cq/i18n/translator.html

**Administration:**

Administration Tools:

/miscadmin

Backup:

/libs/granite/backup/content/admin.html

Mobile Administration:

/miscadmin#/etc/mobile

Site Templates (Blueprints):

/miscadmin#/etc/blueprints

Site Designs:

/miscadmin#/etc/designs

Tag Management:

/libs/cq/tagging/content/tagadmin.html

User Segment Management:

/miscadmin#/etc/segmentation

Multi-Site Manager Rollout Configurations:

/miscadmin#/etc/msm/rolloutconfigs

Digital Asset Manager (DAM):

/damadmin#/content/dam

Feed Import Management:

/miscadmin#/etc/importers

Cloud Services:

/etc/cloudservices.html



PACKAGES

Local Package Repository

/crx/packmgr

Cloud-based Package Repository

/crx/packageshare



DEVELOPMENT

CRX DE Lite:

/crx/de



TROUBLESHOOTING

Code Profiling:

/system/console/profiler

Disk Performance Benchmarking

/system/console/diskbenchmark



WORKFLOW

Workflow Models:

/libs/cq/workflow/content/console.html

Notification InBox:

/libs/cq/workflow/content/inbox.html



REPLICATION

Replication Management:

/etc/replication.html

Activate (Replicate) a tree:

/etc/replication/treeactivation.html

Agents on Author:

/etc/replication/agents.author.html

Agents on Publish:

/etc/replication/agents.publish.html

Dispatcher Flush Agent

/etc/replication/agents.publish/flush.html



REPORTS

Display Client Libraries in Use by Component:

/libs/cq/ui/content/dumplibs.html

Author Activity Audit:

/etc/reports/auditreport.html

Disk Usage Report:

/etc/reports/diskusage.html

Disk Storage by DAM Assets:

/etc/reports/diskusage.html?path=/content/dam

User Report:

/etc/reports/userreport.html



REPOSITORY

Content Explorer

/crx/explorer/browser/index.jsp

Node Type Administration

/crx/explorer/nodetypes/index.jsp

Maintenance Tools:

/system/console/jmx/com.adobe.granite%3Atype%3DRepository



SYSTEM ADMINISTRATION

Clustering Administration:

/libs/granite/cluster/content/admin.html

Apache Felix Administration Console:

/system/console

OSGi Bundle Configuration:

/system/console/configMgr

JVM Runtime Properties:

/system/console/jmx/java.lang%3Atype%3DRuntime

JVM Memory usage:

/system/console/memoryusage

System Information:

/system/console/vmstat

Product Version and License Information:

/system/console/productinfo

Performance Profiler:

/system/console/profiler

Disk Performance Benchmark Tool:

/system/console/diskbenchmark

List of Backups:

/libs/granite/backup/content/admin.html

MIME Type List:

/system/console/mimetypes

Components, Licences, EULAs:

/system/console/licenses
```

**other unsorted**

 /crx/explorer/index.jsp  - CRX explorer
/crx/de/index.jsp – CRXDE Lite url
/libs/cq/search/content/querydebug.html – Query debug tool
/libs/granite/security/content/admin.html – New user manager standalone ui  [5.6 only?]
/libs/cq/contentsync/content/console.html – Content sync console
/system/console/bundles – Felix web admin console
/system/console/jmx/com.adobe.granite.workflow%3Atype%3DMaintenance - Felix web admin console JMX / Workflow maintenance tasks
/system/console/jmx/com.adobe.granite%3Atype%3DRepository - Felix web admin console JMX / Repository maintenance tasks
Params
wcmmode=DISABLED - This handy publisher parameter turns off CQ authoring features so you can preview a page cleanly
/system/console/depfinder – This new 5.6 tool will help you figure out what package exports a class and also prints a Maven Dependency for the class.

/libs/cq/security/userinfo