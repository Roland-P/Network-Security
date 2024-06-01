# Brute-Force
For this simulation, we will perform a simple common type of attack called a Brute Force Attack on an SSH service running on Windows 10 machine. Additionally, we will document the steps and configurations involved. Here’s the detailed plan:

Set up SSH on Windows 10
Perform a Brute Force Attack from Kali Linux
Monitor Traffic Using pfSense, wireshark
Document the Attack and Results

## Network Setup

- **Kali Linux (Attacker)**: 192.168.56.5
- **Windows 10 (Target)**: 192.168.56.7
- **pfSense (Firewall/Monitor)**: 192.168.56.6

## Objective

Simulate a brute force attack on an SSH service running on Windows 10 and monitor the attack.

## Step 1: Setting up SSH on Windows 10

1. **Install SSH Server**: Using Powershell
    ```powershell
    Add-WindowsCapability -Online -Name OpenSSH.Server
    Start-Service sshd
    Set-Service -Name sshd -StartupType 'Automatic'
    New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
    ```
![image](https://github.com/Roland310/Network-Security/assets/171095729/5b1cdec5-7512-4fbb-a725-8f534db28097)

2. **Verify SSH Service**:
   Ensure SSH service is running and reachable from the network.
   <img width="506" alt="image" src="https://github.com/Roland310/Network-Security/assets/171095729/64dddabe-9325-4966-b0e2-18b2baad8315">


## Step 2: Performing Brute Force Attack

1. **Install Hydra on Kali Linux**:
    ```bash
    sudo apt-get update
    sudo apt-get install hydra
    ```

2. **Create User and Password .txt Files**: Verify using CAT
   - `user.txt`:
     ```text
     admin
     user
     test
     ```
   - `pass.txt`:
     ```text
     password123
     123456
     admin
     ```
     
3. **Run Hydra**:
    ```bash
    hydra -L user.txt -P pass.txt ssh://192.168.56.7
    ```
<img width="926" alt="image" src="https://github.com/Roland310/Network-Security/assets/171095729/2f778bed-1927-45b0-a3d0-090ddd951c95">

## Step 3: Monitoring Traffic with pfSense and wireshark

1. **Set up pfSense**: Login to the Web GUI
   - Enabled logging for firewall and system categories.
   - Optionally installed Suricata or snort for detailed monitoring.
   - ![image](https://github.com/Roland310/Network-Security/assets/171095729/0674d183-597b-4b7f-902b-9e6bfd6685cb)


2. **Review Logs**:
   
    <img width="832" alt="image" src="https://github.com/Roland310/Network-Security/assets/171095729/931d1766-f4a4-45a1-b69b-436ae57c03a9">

   

## Results

- **Attack Details**:
   - Hydra successfully attempted multiple SSH logins.
   - Screenshots and logs captured showing the attack process.

- ** Monitoring**:
   - Logs showed connection attempts and potential security alerts.

## Alternatively

Step 1: Scan Network with Nmap
On Kali Linux, scan the network to discover open ports on the Windows 10 host:

nmap -sP 192.168.56.0/24

Step 2: Perform Vulnerability Scan
Use nmap to perform a vulnerability scan on the Windows 10 host:

nmap -sV --script vuln 192.168.56.7

Step 3: Log and Analyze Traffic with pfSense and Splunk
- Configure pfSense to log traffic:
- Set up logging for firewall rules and export logs to Splunk.
- Set up Splunk on Kali Linux:
- Collect and analyze logs from pfSense and the Windows 10 host.

## Conclusion

This project demonstrated a simulated brute force attack on an SSH service and effective monitoring. This setup helps understand the dynamics of network security and intrusion detection.
