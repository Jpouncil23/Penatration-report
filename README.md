# Penatration-report (Shellshock Vulnarability)


## Objective
The objective of this report is to analyze the Shellshock vulnerability, a critical security flaw in Bash that allows attackers to execute arbitrary commands and potentially gain full system access. This report assesses the impact of Shellshock, explores attack vectors such as CGI web servers and OpenSSH, and highlights its potential use in data extraction, network exploitation, and denial-of-service attacks. Additionally, it provides remediation strategies, including applying security patches, restricting Bash access, monitoring network traffic, and implementing intrusion detection systems to mitigate the risk
### Skills Learned
- Vulnerability analysis 
- Risk assessment 
- Ethical Hacking Practices
- Threat intelligence
- Assessing attack vectors

### Tools Used

- Nmap
- nikto
- Attacker VM
- vulnarble VM 

## Vulnerability Description
Red hat describes this vulnerability as “A flaw was found in the way Bash evaluated certain specially crafted environment variables. An attacker could use this flaw to override or bypass environment restrictions to execute shell commands” 
## Executive summary
Through my findings of the shell shock vulnerability this would be considers a critical vulnerability due to the fact that you are able to execute commands within the bash shell and potentially gain full access. Through attack vectors such as CGI web server, open SSH server,  DHCP clients, Qmail server, and IBM HMC restricted shell. Shell shock can also be used to extract sensitive information , exploit network devices and launch DOS attacks. What made this so concerning was the fact that bash is utilized in a multitude of systems that potentially were at risk.
## Remediation steps / Recommendations
- Apply patches and updates
- Update bash version to 4.3 and higher
- Restrict access to bash for users that don't need it
- regularly updating and reviewing web servers and cgi scripts  
- Staying informed of the latest vulnerabilities and patches
- increase network defense for devices that use bash
- Monitoring network traffic for the string “() {“
- Monitor logs for evidence of attempted or successful command execution
- sanitizing user input and removing un-needed characters
- Implement Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS)
## Exploitation Walkthrough
1. I initiate my attack by first using nmap using the -sn to ping for available hosts only. The IP being targeted is 192.168.100.6
![image](https://github.com/user-attachments/assets/533abd99-c487-46d8-9c77-86342866af46)
2. now that I have my targeted IP I use nmap to scan for open ports on 192.168.100.6 .I will be targeting the http service on port 80
![image](https://github.com/user-attachments/assets/04112c0d-bbc1-4289-be4f-afb12dae8c9f)
3. since I know there is an open http service I can use use the tool nikto to scan for vulnerabilities on the IP address 192.168.100.6 and find that the targeted IP is susceptible to the shellshock vulnerability
![image](https://github.com/user-attachments/assets/c31c8dec-ac20-442b-98b3-9127d9da4ddb)
4. I have my results and see that http service running is susceptible to shell shock vulnerability with the path . Putting the full path in we come across this site http://192.168.100.6/cgi-bin/status
![image](https://github.com/user-attachments/assets/57c699e9-8c28-4b09-aeb5-72c1a47f33e0)
5. By utilizing the information discovered on nikto, I can conduct research on the Shellshock vulnerability. Using this syntax I can gain access into the bash shell by executing any command I want. curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd' " \http://localhost:8080/cgi-bin/vulnerable. For my machine the syntax will look more like this 
curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd' " \http://192.168.100.6/cgi-bin/status
![image](https://github.com/user-attachments/assets/9dff3c65-3e61-4589-ae1e-a298819158ed)
6. I am exploiting the shell shock vulnerability through the CGI attack vector that you can read up on here
![image](https://github.com/user-attachments/assets/a3e68ae2-d451-42d8-a6fa-5b3d8700c6df)
7. now for the execution of the syntax. Here you can see that I can execute any command within the bash shell showcasing the shell shock vulnerability

![image](https://github.com/user-attachments/assets/a4b7cabf-839f-442c-a961-41be7375b6fe)












