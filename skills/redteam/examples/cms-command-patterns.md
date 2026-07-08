## CMS command patterns

Use these as starting points and adapt to the rules of engagement:

```bash
# Passive technology fingerprint
whatweb -a 3 https://TARGET
httpx -title -tech-detect -status-code -follow-redirects -u https://TARGET

# CMS-neutral crawling with low request volume
katana -u https://TARGET -d 2 -rate-limit 3 -silent | tee urls.txt

# WordPress
wpscan --url https://TARGET --enumerate vp,vt,tt,u --plugins-detection passive --random-user-agent
curl -s https://TARGET/wp-json/ | jq .

# Drupal / Joomla
droopescan scan drupal -u https://TARGET
droopescan scan joomla -u https://TARGET

# Magento
magescan scan:all https://TARGET
curl -s https://TARGET/graphql -H 'content-type: application/json' --data '{"query":"{__typename}"}'

# Generic CVE/template triage (pin severity and scope)
nuclei -u https://TARGET -severity medium,high,critical -rl 5 -o nuclei-findings.txt
```
