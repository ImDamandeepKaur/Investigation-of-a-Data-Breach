# Investigation-of-a-Data-Breach
Objective: Investigation of a Data Breach on a Renowned  Website.

**Prerequisites**

- VirtualBox/VMware for virtualization
- TryHackMe account (TryHackMe)
- Kali Linux (For penetration testing)
- Docker (For creating a fake bank environment)
- Nessus (For vulnerability scanning)
- TSK (The Sleuth Kit) (For forensic analysis)
- Tripwire (For integrity checking)

**Offensive Security (Penetration Testing)**
1. Using Gobuster for Hidden Pages
- Gobuster is used to find hidden pages on a vulnerable website.
  gobuster dir -u http://fakebank.thm -w wordlist.txt
- This discovered a /bank-transfer page, which allowed unauthorized access.
- The attacker transferred $2000 from a fake bank account.

**Defensive Security (Prevention & Response)**
2. Setting Up a Security Operations Center (SOC)
- Monitors networks for vulnerabilities, policy violations, and unauthorized access.
- Uses Threat Intelligence to gather information on potential attackers.
  
3. Digital Forensics and Incident Response (DFIR)
- File System Analysis: Recover deleted or modified files.
- System Memory Analysis: Detect malware that runs in memory.
- Log Analysis: Check system and network logs for anomalies.
  
**Practical Setup**
4. Creating a Fake Bank for Investigation
- Installing Docker
    sudo apt-get install -y docker.io
    sudo systemctl start docker
    sudo systemctl enable docker
    docker --version
- Setting Up FakeBank
    mkdir fakebank && cd fakebank
    vi Dockerfile  # Create a Dockerfile  
    vi bank_demo_app.php  # Create a fake banking app  
    vi init.sql  # Set up a MySQL database  
- Running the FakeBank Containers
  sudo docker build -t fakebank .
    sudo docker network create fakebank-network
    sudo docker run -d --name fakebank-mysql --network fakebank-network -e MYSQL_ROOT_PASSWORD=mypassword -v $(pwd)/init.sql:/docker-entrypoint-initdb.d/init.sql:ro mysql:5.7
    sudo docker run -d --name fakebank-web --network fakebank-network -p 8080:80 fakebank
- Access the fake bank at
      http://localhost:8080/bank_app_demo.php?id=1
  
5. Vulnerability Scanning with Nessus
- Installing Nessus
    sudo dpkg -i Nessus.deb
    sudo systemctl enable nessusd
    sudo systemctl start nessusd
- Open Nessus in a browser
    https://localhost:8834
- Register and activate Nessus Essentials for vulnerability scanning.

6. Digital Forensics with The Sleuth Kit (TSK)
- Installing TSK
    sudo apt-get install sleuthkit
- Extracting Filesystem from Docker Containers
    docker export fakebank-web -o fakebank_web_filesystem.tar
    docker export fakebank-mysql -o fakebank_mysql_filesystem.tar
    mkdir -p /path/to/extracted_web
    tar -xvf fakebank_web_filesystem.tar -C /path/to/extracted_web

- Analyzing File System Activity
  fls -r fakebank_web_filesystem.img | grep "access"
  istat fakebank_web_filesystem.img [inode]
- Helps in tracing unauthorized activities and understanding security flaws

7. Integrity Checking with Tripwire
   sudo apt-get install tripwire
    sudo tripwire --init
    sudo tripwire --check
- Detects unauthorized changes in system files.

Conclusion
This project simulates a real-world data breach, providing hands-on experience in offensive and defensive security strategies. By using forensic tools, SOC monitoring, and security assessments, we can identify vulnerabilities and mitigate cyber threats effectively.




