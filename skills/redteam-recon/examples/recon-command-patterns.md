## Command patterns

```bash
# Passive DNS and CT pivots
subfinder -d TARGET -all -silent | tee subdomains.txt
amass enum -passive -d TARGET -o amass-passive.txt
curl -s 'https://crt.sh/?q=%25.TARGET&output=json' | jq -r '.[].name_value' | sort -u

# Low-rate HTTP validation
httpx -l subdomains.txt -title -tech-detect -status-code -follow-redirects -rate-limit 5 -o httpx.txt

# Focused crawl after scope approval
katana -list live-urls.txt -d 2 -rate-limit 3 -silent -o crawled.txt

# Service discovery only when CIDR scanning is allowed
nmap -sV -Pn --top-ports 100 --max-rate 50 -oA nmap-top TARGET
```
