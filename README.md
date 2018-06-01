# antminer-monitoring
## Features
Monitoring hashrate and temperature of antminer S9.
## Approach
External checks are used to run python scripts which retrieve information from bmminer api. 
## Prerequirements
On the zabbix server: Python

On antminer: bmminer api should be allowed
## Installation
* Copy get_hashrate and get_temp to zabbix external script directory. For Ubuntu 16.04 it is /usr/lib/zabbix/externalscripts. Make sure scripts have executable permission.
* Import template zbx_antminer_template.xml.
* Add hosts. They should have valid ip\dns. Hosts must be accesible from the zabbix server.
* Assign template to hosts.
## Details
Hashrate is taken from parameter "GHS av". I have not found description for it. It seems to be average for a short period of time 1-5m.
Temperature shows the highest value of parameters starting with "temp". In tested antminer there were about 20 parameters starting with "temp", only 6 of them had values. 
## Troubleshooting
* To test if antminer bmminer api is available you can run:
`echo -n "stats" | nc ip_or_dns 4028`
If get no response make sure bmminer api is enabled. It is configured in /config/bmminer.conf. 
You need to have "stats" in api-groups.
`"api-groups" : "A:stats:lcd:pools:devs:summary:version:noncenum",`
api-allow is responsible for source of requests. This works fine, but you may want to have more strict settings.
`"api-allow" : "A:0/0,W:*",`
