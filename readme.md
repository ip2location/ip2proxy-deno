# IP2Proxy Deno Module

This module allows user to query an IP address if it was being used as VPN anonymizer, open proxies, web proxies, Tor exits, data center, web hosting (DCH) range, search engine robots (SES) and residential (RES). It lookup the proxy IP address from **IP2Proxy BIN Data** file. This data file can be downloaded at

* Free IP2Proxy BIN Data: https://lite.ip2location.com
* Commercial IP2Proxy BIN Data: https://www.ip2location.com/database/ip2proxy

As an alternative, this module can also call the IP2Proxy Web Service. This requires an API key. If you don't have an existing API key, you can subscribe for one at the below:

https://www.ip2location.com/web-service/ip2proxy

## Installation

To use this module, download the latest release of the module to your local, and then copy the *mode.ts* file and the whole *src/* folder into your project root.

## QUERY USING THE BIN FILE

## Methods
Below are the methods supported in this class.

|Method Name|Description|
|---|---|
|open|Open the IP2Proxy BIN data for lookup.|
|close|Close and clean up the file pointer.|
|getPackageVersion|Get the package version (1 to 11 for PX1 to PX11 respectively).|
|getModuleVersion|Get the module version.|
|getDatabaseVersion|Get the database version.|
|isProxy|Check whether if an IP address was a proxy. Returned value:<ul><li>-1 : errors</li><li>0 : not a proxy</li><li>1 : a proxy</li><li>2 : a data center IP address or search engine robot</li></ul>|
|getAll|Return the proxy information in an object.|
|getProxyType|Return the proxy type. Please visit <a href="https://www.ip2location.com/database/px10-ip-proxytype-country-region-city-isp-domain-usagetype-asn-lastseen-threat-residential" target="_blank">IP2Location</a> for the list of proxy types supported|
|getCountryShort|Return the ISO3166-1 country code (2-digits) of the proxy.|
|getCountryLong|Return the ISO3166-1 country name of the proxy.|
|getRegion|Return the ISO3166-2 region name of the proxy. Please visit <a href="https://www.ip2location.com/free/iso3166-2" target="_blank">ISO3166-2 Subdivision Code</a> for the information of ISO3166-2 supported|
|getCity|Return the city name of the proxy.|
|getISP|Return the ISP name of the proxy.|
|getDomain|Return the domain name of the proxy.|
|getUsageType|Return the usage type classification of the proxy. Please visit <a href="https://www.ip2location.com/database/px10-ip-proxytype-country-region-city-isp-domain-usagetype-asn-lastseen-threat-residential" target="_blank">IP2Location</a> for the list of usage types supported.|
|getASN|Return the autonomous system number of the proxy.|
|getAS|Return the autonomous system name of the proxy.|
|getLastSeen|Return the number of days that the proxy was last seen.|
|getThreat|Return the threat type of the proxy.|
|getProvider|Return the provider of the proxy.|

## Usage

```javascript
import { IP2Proxy } from "./mod.ts";

let ip2proxy = new IP2Proxy();

if (ip2proxy.open("./IP2PROXY-IP-PROXYTYPE-COUNTRY-REGION-CITY-ISP-DOMAIN-USAGETYPE-ASN-LASTSEEN-THREAT-RESIDENTIAL-PROVIDER.BIN") == 0) {
	ip = '199.83.103.79';
	
	console.log("GetModuleVersion: " + ip2proxy.getModuleVersion());
	console.log("GetPackageVersion: " + ip2proxy.getPackageVersion());
	console.log("GetDatabaseVersion: " + ip2proxy.getDatabaseVersion());
	
	// functions for individual fields
	console.log("isProxy: " + ip2proxy.isProxy(ip));
	console.log("ProxyType: " + ip2proxy.getProxyType(ip));
	console.log("CountryShort: " + ip2proxy.getCountryShort(ip));
	console.log("CountryLong: " + ip2proxy.getCountryLong(ip));
	console.log("Region: " + ip2proxy.getRegion(ip));
	console.log("City: " + ip2proxy.getCity(ip));
	console.log("ISP: " + ip2proxy.getISP(ip));
	console.log("Domain: " + ip2proxy.getDomain(ip));
	console.log("UsageType: " + ip2proxy.getUsageType(ip));
	console.log("ASN: " + ip2proxy.getASN(ip));
	console.log("AS: " + ip2proxy.getAS(ip));
	console.log("LastSeen: " + ip2proxy.getLastSeen(ip));
	console.log("Threat: " + ip2proxy.getThreat(ip));
	console.log("Provider: " + ip2proxy.getProvider(ip));
	
	// function for all fields
	let all = ip2proxy.getAll(ip);
	console.log("isProxy: " + all.isProxy);
	console.log("proxyType: " + all.proxyType);
	console.log("countryShort: " + all.countryShort);
	console.log("countryLong: " + all.countryLong);
	console.log("region: " + all.region);
	console.log("city: " + all.city);
	console.log("isp: " + all.isp);
	console.log("domain: " + all.domain);
	console.log("usagetype: " + all.usageType);
	console.log("asn: " + all.asn);
	console.log("as: " + all.as);
	console.log("lastSeen: " + all.lastSeen);
	console.log("threat: " + all.threat);
	console.log("provider: " + all.provider);
}
else {
	console.log("Error reading BIN file.");
}
ip2proxy.close();

```

## QUERY USING THE IP2PROXY PROXY DETECTION WEB SERVICE

## Methods
Below are the methods supported in this class.

|Method Name|Description|
|---|---|
|open(apiKey, apiPackage, useSSL = true)| Expects 2 or 3 input parameters:<ol><li>IP2Proxy API Key.</li><li>Package (PX1 - PX11)</li></li><li>Use HTTPS or HTTP</li></ol> |
|lookup(myIP, callback)|Query IP address. This method returns an object containing the proxy info. <ul><li>countryCode</li><li>countryName</li><li>regionName</li><li>cityName</li><li>isp</li><li>domain</li><li>usageType</li><li>asn</li><li>as</li><li>lastSeen</li><li>threat</li><li>proxyType</li><li>isProxy</li><li>provider</li><ul>|
|getCredit(callback)|This method returns the web service credit balance in an object.|

## Usage

```javascript
import { IP2ProxyWebService } from "./mod.ts";

let ws = new IP2ProxyWebService();

let ip = "8.8.8.8";
let apiKey = "YOUR_API_KEY";
let apiPackage = "PX11";
let useSSL = true;

ws.open(apiKey, apiPackage, useSSL);

ws.lookup(ip, (err, data) => {
	if (!err) {
		console.log(data);
		
		ws.getCredit((err, data) => {
			if (!err) {
				console.log(data);
			}
		});
	}
});

```

### Proxy Type

|Proxy Type|Description|
|---|---|
|VPN|Anonymizing VPN services|
|TOR|Tor Exit Nodes|
|PUB|Public Proxies|
|WEB|Web Proxies|
|DCH|Hosting Providers/Data Center|
|SES|Search Engine Robots|
|RES|Residential Proxies [PX10+]|

### Usage Type

|Usage Type|Description|
|---|---|
|COM|Commercial|
|ORG|Organization|
|GOV|Government|
|MIL|Military|
|EDU|University/College/School|
|LIB|Library|
|CDN|Content Delivery Network|
|ISP|Fixed Line ISP|
|MOB|Mobile ISP|
|DCH|Data Center/Web Hosting/Transit|
|SES|Search Engine Spider|
|RSV|Reserved|

### Threat Type

|Threat Type|Description|
|---|---|
|SPAM|Spammer|
|SCANNER|Security Scanner or Attack|
|BOTNET|Spyware or Malware|