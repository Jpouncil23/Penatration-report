# Penetration Report — Shellshock Vulnerability

[![Type](https://img.shields.io/badge/Type-Penetration%20Test-red)](https://github.com)
[![Severity](https://img.shields.io/badge/Severity-Critical-darkred)](https://github.com)
[![Status](https://img.shields.io/badge/Status-Complete-success)](https://github.com)

## Objective

The objective of this report is to analyze the Shellshock vulnerability, a critical security flaw in Bash that allows attackers to execute arbitrary commands and potentially gain full system access. This report covers the impact of Shellshock, explores attack vectors like CGI web servers and OpenSSH, and goes over its potential use in data extraction, network exploitation, and denial-of-service attacks. I also provide remediation steps including applying security patches, restricting Bash access, monitoring network traffic, and implementing intrusion detection systems to reduce the risk.

## Tools Used

| Tool | Purpose |
|------|---------|
| **Nmap** | Network scanning and host/port discovery |
| **Nikto** | Web vulnerability scanning |
| **Attacker VM** | Machine used to launch the attack |
| **Vulnerable VM** | Target machine running the vulnerable Bash version |

## Skills Learned

- Vulnerability analysis and assessment
- Risk assessment and severity rating
- Ethical hacking practices
- Threat intelligence research
- Identifying and exploiting attack vectors

---

## Vulnerability Description

Red Hat describes this vulnerability as:

> "A flaw was found in the way Bash evaluated certain specially crafted environment variables. An attacker could use this flaw to override or bypass environment restrictions to execute shell commands."

---

## Executive Summary

Through my findings on the Shellshock vulnerability, this would be considered a critical vulnerability due to the fact that you are able to execute commands within the Bash shell and potentially gain full access to the system. Attack vectors include CGI web servers, OpenSSH servers, DHCP clients, Qmail servers, and IBM HMC restricted shells. Shellshock can also be used to extract sensitive information, exploit network devices, and launch DOS attacks. What made this so concerning was the fact that Bash is used in a huge number of systems that were potentially at risk.

---

## Remediation Steps / Recommendations

- Apply patches and updates
- Update Bash version to 4.3 or higher
- Restrict access to Bash for users that don't need it
- Regularly update and review web servers and CGI scripts
- Stay informed on the latest vulnerabilities and patches
- Increase network defenses for devices that use Bash
- Monitor network traffic for the string `() {`
- Monitor logs for evidence of attempted or successful command execution
- Sanitize user input and remove unnecessary characters
- Implement Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS)

---

## Exploitation Walkthrough

### Step 1 — Host Discovery

I start my attack by using Nmap with the `-sn` flag to ping for available hosts. The target IP is 192.168.100.6.

![Nmap Host Discovery](https://github.com/user-attachments/assets/533abd99-c487-46d8-9c77-86342866af46)

### Step 2 — Port Scanning

Now that I have my target IP, I use Nmap to scan for open ports on 192.168.100.6. I'll be targeting the HTTP service on port 80.

![Nmap Port Scan](https://github.com/user-attachments/assets/04112c0d-bbc1-4289-be4f-afb12dae8c9f)

### Step 3 — Vulnerability Scanning

Since I know there's an open HTTP service, I use Nikto to scan for vulnerabilities on 192.168.100.6. The scan confirms that the target is susceptible to the Shellshock vulnerability.

![Nikto Vulnerability Scan](https://github.com/user-attachments/assets/c31c8dec-ac20-442b-98b3-9127d9da4ddb)

### Step 4 — Identifying the Vulnerable Path

The Nikto results show the HTTP service is vulnerable to Shellshock and give me the CGI path. Putting the full path together brings me to: `http://192.168.100.6/cgi-bin/status`

![CGI Path Discovery](https://github.com/user-attachments/assets/57c699e9-8c28-4b09-aeb5-72c1a47f33e0)

### Step 5 — Crafting the Exploit

Using the information from Nikto, I researched the Shellshock vulnerability and found that I can gain access to the Bash shell by injecting a command through the user-agent header using curl:

```bash
curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'" \
http://192.168.100.6/cgi-bin/status
```

![Shellshock Exploit Syntax](https://github.com/user-attachments/assets/9dff3c65-3e61-4589-ae1e-a298819158ed)

### Step 6 — Understanding the Attack Vector

I'm exploiting the Shellshock vulnerability through the CGI attack vector. The web server passes the user-agent header to Bash as an environment variable, and because of the Shellshock flaw, the injected command gets executed.

![CGI Attack Vector](https://github.com/user-attachments/assets/a3e68ae2-d451-42d8-a6fa-5b3d8700c6df)

### Step 7 — Command Execution

Here you can see the exploit in action. I'm able to execute any command within the Bash shell on the target machine, fully demonstrating the Shellshock vulnerability.

![Successful Command Execution](https://github.com/user-attachments/assets/a4b7cabf-839f-442c-a961-41be7375b6fe)

---

## Author

**Justin**
IT Professional

---

*Last Updated: January 2025*
