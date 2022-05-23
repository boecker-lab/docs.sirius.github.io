---
permalink: /admins/
title: Administrator/Technical information
---

SIRIUS is a software framework for MS/MS data analysis of small molecules. For more details on the 
included tools and methods please have a look into the [user documentation]({{ "/" | relative_url }}).
SIRIUS is written in JAVA, but **no** separate JRE is needed. A compatible JRE is included in each SIRIUS release.
For further information on the installation process and hardware requirements, see [Installation]({{ "/install/" | relative_url }}).

## Offline vs Online features
SIRIUS is both, a classical Desktop application that performs computations on the local machine it is executed on, but
it is also a client for web services such as 
[CSI:FingerID]({{ "/cli/#csifingerid-identifying-molecular-structures" | relative_url }}) and 
[CANOPUS]({{ "/cli/#canopus-predicting-compound-classes-without-identification" | relative_url }}) 
which are seamlessly integrated so that they feel like classical desktop applications from a user perspective. 
This is true for both, the CLI and GUI.

{% capture fig_img %}
![Foo]({{ "/assets/images/sirius-infrastructure.svg" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Basic Architecture Diagram of SIRIUS and its web services.</figcaption>
</figure>

#### Why are some features implemented as web service?
The structure and compound class prediction for the given mass spectrometry data relies on complex infrastructure which we
found not appropriate to be executed on Laptop or Workstation computers. Therefore, the predictions are done on a scalable cloud 
infrastructure. This means the web service is about computations in the cloud and not about storing 
data in the cloud. Further, this allows us to improve the methods and fix bugs silently in the background without the 
need to install an update on the user's computer.

### Offline features
* Data import, result export and result storage. 
* Result visualization (GUI), even for Webservice based results. Once results are successfully stored they are available offline. 
* SIRIUS molecular formula estimation (fragmentation tree computation and Isotope pattern analysis)
* ZODIAC network based molecular formula reranking
* Passatutto decoy spectra generation
* Most of the little standalone helper sub-tools in the CLI

### Online features
* Chemical Structure database based features such as CSI:FingerID structure database search 
and the restriction of formula candidates to a database during SIRIUS molecular formula estimation.
* CSI:FingerID fingerprint prediction
* COSMIC confidence score computation
* CANOPUS compound class prediction

## Connections needed to run online features
Two servers need to be reachable for SIRIUS to work properly; 
the login server (`https://auth0.bright-giant.com`) and the web service itself.
Whereas the login server is the same for the commercial and the non-commercial (provided by FSU Jena) versions of the 
web service, the URL of the web service itself might vary for different subscriptions. Further, there are a few additional 
URLs that might be requested for debugging and error reporting purposes, e.g. if the web service or the login server 
are not reachable. These URLs are **optional** and are **not** needed for SIRIUS to work properly. However, error 
messages regarding connection issues might be misleading if the optional URLs are blocked.

### Non-Commercial Version (FSU Jena)
#### Mandatory:
* License Server: `https://gate.bright-giant.com`
* Login Server: `https://auth0.bright-giant.com`
* Web Services: `https://www.csi-fingerid.uni-jena.de`
#### Optional:
* Check Internet: `https://www.google.com`

### Commercial Version (Bright Giant GmbH)
#### Mandatory:
* License Server: `https://gate.bright-giant.com`
* Login Server: `https://auth0.bright-giant.com`
* Web Services: `https://csi.bright-giant.com`    

**Note:** Users with a deticated hosting subscription need to replace the web service URL with the URL of their custom subscriction (usually `<companyname>.csi.bright-giant.com`).

#### Optional:
* Check Internet: `https://www.google.com`

### Change internet connection check URL
If you are using SIRIUS from somewhere where Google is not reachable you can replace it by some other URL 
outside your institution that is reliable enough to act as an internet connection check.
To do so, add or replace the following entry in `<USER_HOME>/.sirius-<X.Y>/sirius.properties`:
```
de.unijena.bioinf.fingerid.web.external=https://my.custom.url/
```
The connection check expects an HTTP response value `OK` (e.g. 200) to be successful

## Proxy Settings
If your institution uses a proxy server to connect to the internet you need to configure the proxy server within SIRIUS. 
See [Network Settings]({{ "//gui/#settings" | relative_url }}) for details.

## Security and Data Usage

#### Data in transit
All communication between the SIRIUS software on your local computer and its web services uses HTTPS and is TLS encrypted.

#### Data at rest
**No data is stored at rest.** The data sent to our servers are only stored on our servers for computation and only until 
the results have been fetched by the SIRIUS client on your local computer. So the data of a single 
computation job just stays a couple of minutes on our servers until the result has been transferred to your local computer.
After transfer the result data is also deleted immediately.       
**Special case:** In case you are using a subscription with a compound limit (e.g. Bright Giant Shared subscription) 
a hash of the input spectra is stored for about 24h by the web service, so that computations can be counted correctly. 
Note, that this hash can **never** be used to reconstruct the spectrum, it just allows us to check whether a spectrum has 
already been computed in a previous job. If you are using a subscription without a compound limit 
(e.g. FSU Jena version or Bright Giant Private subscription), this hash is not needed and will not be stored.

#### What data is transferred to the Web Services?
For every instance (compound) to be analyzed, SIRIUS sends the mass spectra, the locally computed fragmentation tree, 
the parameters for the computation and a unique session key to the web services. The only personal data that is sent to 
the web service are the necessary account data (user id and email address) as Json Web Token (JWT).

#### How is it ensured that only I can fetch the results of my computation jobs?
Each time SIRIUS is started on your local computer it generates a unique session key that is used to submit the 
computation jobs to the server. This key is never stored; it only exists in System Memory at runtime. Only the SIRIUS 
process that knows this key can fetch the results. Results are deleted from the server immediately after the client has 
fetched them. If a client with a specific session key loses its connection to the server for longer than a few hours, 
all corresponding jobs are permanently deleted.

#### What kind of statistics are collected?
We only count the number, time and type of computation jobs to be able to scale our infrastructure appropriately

## SIRIUS workspace - Configs, Logs and Caches
SIRIUS stores its configuration files, logs and caches in the SIRIUS workspace directory at `<USER_HOME>/.sirius-<X.Y>` 
where `X.Y` is are **Major** and **Minor** numbers of the SIRIUS version string. When using SIRIUS in VMs or containers it might 
be worth storing this directory persistently to preserve configuration and benefit from performance improvements due 
to structure database caching. It is also the default location for custom databases.

**Directory structure:**
* **csi_fingerid_cache**
  * **rest-cache** - structure database cache default location (changeable)
  * **custom** - custom database default location  (changeable)
  * **version** - db version file
* **custom.config** - Config file to override default parameters of tools in CLI and GUI
(see, [config tool](https://boecker-lab.github.io/docs.sirius.github.io/cli/))
* **logging.properties** - SIRIUS logging configuration file
* **sirius.properties** - SIRIUS configuration file
* **version** - SIRIUS version that created this content
* **sirius.log.0** - log file
* **sirius.log.1** - log file
* **sirius.log.2** - log file
* ...


## Release Policy
For SIRIUS as well as for the backend (web service) we use a "rolling release" alike  strategy and semantic versioning. 
This means updates and fixes are rolled out as soon as they are declared to be stable. Sometimes even a single bug fix. 
New releases of *vanilla* SIRIUS are available [here](https://github.com/boecker-lab/sirius/releases). 
SIRIUS versions preconfigured by Bright Giant can be found [here](https://github.com/bright-giant/sirius/releases).

## Versioning
Following the Apache Maven convention we distinguish between *stable* (`x.y.z`) and  *SNAPSHOT* (`x.y.z-SNAPSHOT`) builds.
The `x.y.z-SNAPSHOT` build can be seen as a development version until `x.y.z` is released, so that `x.y.z.SNAPSHOT` < `x.y.z` holds.
Development (`x.y.z-SNAPSHOT`) builds are usually not publicly available.

### What does the SIRIUS Version numbers mean?
**`x`:** Major change in the application:
* CLI commands changed or removed
* Input formats changed in a not backward compatible manner
* Output formats changed so that it may break existing workflows
* Might not be backward compatible with results from previous versions

**`y`:** Minor change or Major backend changes:
* Major change in the backend (web service) so that it gets incompatible with the client 
(see, [below]({{ "/admins/#major-web-service-changes-and-end-of-life" | relative_url }}))
* New tools, methods or functionality have been introduced
* New CLI command(s) added (backwards compatible)
* Additional inputs or outputs (backwards compatible)
* Backward compatible with results from previous versions
* Update of the included JRE or native libs

**`z`:** Build number that will be increase with every new build:
* Bug fixes, corrections of typos,...
* Minor improvements without the risk to break anything e.g. new or corrected tooltips, additional documentation,... 

### Upgrade Recommendations
From a *system administrator's* or *system integration* perspective, **Minor** and **Build** number changes are safe 
to upgrade and should not break anything. From a *user's perspective*, a **Minor** update might change (usually improve) 
the results of the methods. The corresponding entry in the [changelog]({{ "/changelog/" | relative_url }}) will contain 
the relevant information.

### Multiple versions alongside
* Multiple different **Minor** versions can be safely used beside each other without interfering each other.
* Different **Build**s of the same **Minor** version should not be used alongside each other since they would share 
configs, logs and caches which can cause unexpected behaviour.

  
### Major web service changes and end-of-life
When a **Major** web service update is released, this also causes an update of the SIRIUS client to a new **Minor** 
version (`x.y+1.0`). The previous version (`x.y.z`) will become a legacy version that will only be fully functional for a 
limited amount of time (usually at least 3 Month). In the past, sometimes even longer due to user requests to keep them 
alive a bit longer. However, unless we just have a **Minor** version change for the SIRIUS client, upgrading is not 
different from other **Minor** version changes. Just keep in mind that previous version might reach its end-of-life.


