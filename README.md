# Overview
This is the data and the notebooks (code + documentation) for the analysis that is part of EPSS **Applied** Guide.


# Data Sources

[data_in](./data_in) 

| Data Source |    Detail     | ~~ CVE count K | Directory           |  
|-------------|:-------------:|---------------:|---------------:|
| [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)    |  Active Exploitation|              1 | [cisa_kev](./data_in/cisa_kev) |
| [EPSS](https://www.first.org/epss/api)        |    Predictor of Exploitation   |        210 |  [epss](./data_in/epss) |
| [Metasploit modules](https://github.com/rapid7/metasploit-framework)  | Weaponized Exploit |              3 | [metasploit](./data_in/metasploit) |
| [Nuclei templates](https://github.com/projectdiscovery/nuclei-templates)      |  Weaponized Exploit           |              2 | [nuclei](./data_in/nuclei) |
| [ExploitDB](https://gitlab.com/exploit-database/exploitdb)   |  Published Exploit Code             |            25 | [exploitdb](./data_in/exploitdb) |
| [NVD CVE Data](https://nvd.nist.gov/vuln/data-feeds) | NVD CVEs with CVSSv3 parameters | 140| [nvd](./data_in/nvd) |
| [Qualys TruRisk Report](https://www.qualys.com/forms/tru-research-report/) | The 2023 Qualys TruRisk research report lists 190 CVEs from 2022 with QVS scores| .2| [qualys](./data_in/qualys) |
| [Microsoft Security Response Center (MSRC)](https://msrc.microsoft.com/update-guide/vulnerability) | CVEs Exploited and with Exploitability Assessment| .2| [msrc](./data_in/msrc) |

## Getting data
* [get_data.sh](./data/get_data.sh) gets the data that can be downloaded automatically and used as-is.
* Other data is manually downloaded (as of now).
* Larger files are gzip'd

### National Vulnerability Database (NVD)
Get NVD data automatically
* A script in [nvd](./data_in/nvd) downloads the NVD data and extracts the relevant fields.
* Note: The download method used will be deprecated after Dec 2023 per https://nvd.nist.gov/vuln/data-feeds


### Google Project Zero (GPZ)
See [0day "In the Wild" GoogleSheet](https://docs.google.com/spreadsheets/d/1lkNJ0uQwbeC1ZTRrxdtuPLCIl7mlUreoKfSIgajnSyY/edit#gid=1190662839) 
* Select "All" tab

### Microsoft Security Response Center (MSRC) 
* Go to https://msrc.microsoft.com/update-guide/vulnerability
* Edit columns - ensure these columns are selected "Exploitability Assessment" and "Exploited"
* Download

### Qualys TruRisk Report
The CVE data was extracted from the [Qualys TruRisk Report](https://www.qualys.com/forms/tru-research-report/) PDF using standard tools like sed.

### ExploitDB
* Download https://gitlab.com/exploit-database/exploitdb/-/blob/main/files_exploits.csv (manually for now - credentials required for automation)
* Extract the CVEs using the script in the directory i.e. some entries don't have CVEs - and have only Open Source Vulnerability Database (OSVDB) entries instead.


## Other Data Sources 
Other data sources to consider - these are not currently used here:
1. https://github.com/trickest/cve for a list of CVE PoCs



# Analysis

1. [enrich_cves.ipynb](./analysis/enrich_cves.ipynb) 
   1. take the data sources from [data_in/](./data_in/) 
   2. enrich the CVE data from NVD with the other data sources
   3. add an "Exploit" column to indicate the source of the exploitability (used later to set colors of CVE data in plots)
   4. store the output in data_out/nvd_cves_v3_enriched.csv.gz
2. [kev_epss_cvss.ipynb](./analysis/kev_epss_cvss.ipynb)
   1. Read the data from data_out/nvd_cves_v3_enriched.csv.gz
   2. Read the data from CISA KEV alert reports in ./data_in/cisa_kev/
   3. Plot CISA KEV datasets showing EPSS, CVSS by source of the exploitability
3. [qualys.ipynb](analysis/qualys.ipynb)
    1. Read the data from data_out/nvd_cves_v3_enriched.csv.gz
    2. Read the data from ./data_in/qualys
    3. Plot Qualys dataset showing EPSS, CVSS by source of the exploitability