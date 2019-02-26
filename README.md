# cloud_ip_ranges_source

This page collects common used cloud providers' ipv4 ip ranges, following command is run by shell script.

I am trying to using all the native shell command without install any other new package

# aws 
```
wget https://ip-ranges.amazonaws.com/ip-ranges.json -O aws-ip.json
```

# azure
```
MICROSOFT_IP_RANGES_URL="https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653"
MICROSOFT_IP_RANGES_URL_FINAL=$(curl -Lfs "${MICROSOFT_IP_RANGES_URL}" | grep -Eoi '<a [^>]+>' | grep -Eo 'href="[^\"]+"' | grep "download.microsoft.com/download/" | grep -m 1 -Eo '(http|https)://[^"]+')
wget $MICROSOFT_IP_RANGES_URL_FINAL -O azure-ip.xml
```

# gcp
```
for LINE in `dig txt _cloud-netblocks.googleusercontent.com +short | tr " " "\n" | grep include | cut -f 2 -d :`
do
	dig txt $LINE +short
done | tr " " "\n" | grep ip4  | cut -f 2 -d : | sort -n > gcp-ip.txt
```

# cloudflare
```
wget https://www.cloudflare.com/ips-v4 -O cloudflare-ip.txt
```

# ibm
```
curl -Lfs https://github.com/IBM-Bluemix-Docs/hardware-firewall-dedicated/blob/master/ips.md | grep "td" | grep -Eo '(\d+)\.(\d+)\.(\d+)\.(\d+)/(\d+)' | grep -vE "^10.|^192.168|^172.(1[6-9]|2\d|3[0-1])" > ibm-ip.txt
```

# oracle
```
curl -Lfs https://docs.cloud.oracle.com/iaas/Content/General/Concepts/addressranges.htm | grep "li value" | awk -F"<|>" '{print $3}' | grep -E '(\d+)\.(\d+)\.(\d+)\.(\d+)/(\d+)' > oracle-ip.txt
```

