Hello, my LinkedIn and GitHub colleagues! 🌐🔒

**Follow me on LinkedIn: https://www.linkedin.com/in/lukas-apynis**  

I would like to share important insights into the latest targeted attack against Lithuanian state institutions.  
In investigating this virus, it has been revealed that it is unique compared to other malware.  
**Although it has similarities with the AgentTesla malware family.**  

### **🕵️‍♂️ Key findings from technical analysis:**

The malware was designed to operate silently.
It includes espionage features to monitor the compromised system.

### **🛡️ Security Challenges:**  

* The uniqueness of this virus can pose significant challenges in identifying and neutralising it.
* Security professionals should be vigilant and constantly update security measures and take preventive actions.
* Organisations must educate their colleagues about cyber security and its dangers.
* Unique, never-before-seen viruses or zero-days can be detected by advanced security solutions such as Sandbox, EDR/XDR tools.

### **🔗 Full virus analysis and operational description:**  

**We detected this malware in December 2023.**   
**Everything started with a phishing attack from a newly registered domain: _beldiaspara[.]info_  
Create date: 2023-12-27 00:00:00.**

**What makes this attack unique is that all domains associated with this malware are under Cloudflare, Inc.  
All communication is done through HTTPS.**  

This makes it harder for SOC teams or security teams to detect this kind of malware.

The email subject was written in the Russian language when translated: _Permission to hold an event_.  

The content of the letter was also in strange Russian language, so we translated it like this:  
_Good afternoon!_  
_Belarusian Diaspora in Lithuania requests permission to hold the event_  
_14.01.2024._  
_Thank you in advance_  

**Original Email Subject:** _Разрешение на проведение мероприятия_  
**Original Email:**  
_Добрый день!_  
_Белорусская диаспора в Литве просит разрешение на проведение мероприятия_  
_14.01.2024 года_  
_Заранее благодарны_  
**Attachment:**  
_Запрос.doc_  

### **🕵️‍♂️ Technical analysis:**   

As I said before everything started with a phishing attack!   

**This phishing email had an attachment named: _Запрос.doc_**  

When analyzing _Запрос.doc_ inside the file is encrypted VBA Macros.
I also found a keyword "Document_Open" which indicates: "Runs when the Word or Publisher document is opened"

**When a document is opened we see that we need to fill up a form:**  

![Screenshot 2024-01-04 085450](https://github.com/Wortexz/ltspyware/assets/26935578/1ab363f7-b7c3-444a-ae89-12a7b9701ddf)

**This is done intentionally because to fill in a form a user needs to click on "Enable Content".**  
**When the user clicks on "Enable Content" VBA Macro silently is launched.**  

**Installation/Persistence:**  
VBA Macros downloads/drops: "certweb32.exe" which is "PE32 executable Mono/.Net assembly for MS Windows".  
**File Location: [%APPDATA%\Microsoft\Windows\certweb32.exe]**  

**When Macro is executed these domains/IP addresses are contacted:**  
* _104.21.2.153 - Cloudflare, Inc._  
* _clfeed.online - Cloudflare, Inc._  
* _152.199.19.161 - Edgecast Inc._  
* _172.67.129.87 - Cloudflare, Inc._  
* Country: US
  
**Most likely: Legitimate, likely compromised domains/IPs misused.**  



### **👾 About the main malware/spyware (certweb32.exe):**  
**File Location: [%APPDATA%\Microsoft\Windows\certweb32.exe]**  

**File description: _Company: Autodesk_**  
**Compilation Timestamp: _6 Aug 2064 04:47:24_**  
This is mostly done intentionally to confuse the security experts.

**Environment Awareness:**  
The malware collects information about the system (Hardware, Software, and Network).

**Anti-Detection/Stealthyness:**  
* Malware queries process information: _"certweb32.exe" queried SystemProcessInformation at..._*
* Evasive: Possibly tries to implement anti-virtualization techniques using MAC address detection

**Anti-Reverse Engineering:**  
  
* Uses a .NET obfuscator/packer to hide its code: "MPRESS" / "Obfuscar" obfuscator
* Contains the ability to execute applications in hidden mode (Dotnet)
* Dotnet file contains encryption/decryption functions
  
## **TTPs**
* **Execution Native API [T1106]** - Creates guarded memory regions (anti-debugging trick to avoid memory dumping) details "certweb32.exe" is allocating memory with PAGE_GUARD access rights.
* **Command and Control Application Layer Protocol [T1071]** - Establishes an encrypted HTTPS connection
* **Modify Registry [T1112]** - Defense Evasion
* **Execution Command and Scripting Interpreter & PowerShell [T1059.001]** - Data downloaded by powershell script
* **Execution Command and Scripting Interpreter & PowerShell [T1059.001]** - Powershell is sending data to a remote host
* **Application Layer Protocol [T1071]** - Powershell is sending data to a remote host

**Data downloaded by powershell script (partial code shown):**  
![Screenshot_1](https://github.com/Wortexz/ltspyware/assets/26935578/87d5a742-5f2b-4134-8035-b6b8106b25ee)

**Powershell is sending data to a remote host (partial code shown):**  
![Screenshot 2024-01-09 114318](https://github.com/Wortexz/ltspyware/assets/26935578/91596b34-5156-462d-b608-ca3a29f1aa70)

**Establishes an encrypted HTTPS connection:**  
![Screenshot 2024-01-09 114433](https://github.com/Wortexz/ltspyware/assets/26935578/4be41605-153f-4270-8c9f-3115ea1f6018)


**The main malware process: _certweb32.exe_ network communication:**  
* _104.21.2.153 - Cloudflare, Inc._ 
* _clfeed.online - Cloudflare, Inc._
* _172.67.129.87 - Cloudflare, Inc._  

## ★ **Malware Features**
* Command & Control - C2
* Exfiltration: Password Stealing / info stealer

## **Original, obfuscated code & XOR:**  
![Screenshot_5](https://github.com/Wortexz/ltspyware/assets/26935578/a0b1da71-bf39-40a3-b014-b847558eddf5)
![Screenshot_6](https://github.com/Wortexz/ltspyware/assets/26935578/30409fe1-c53c-4463-93ba-773a69fe4fa6)



## **Unpacking obfuscated code, cleaning, and patching:**    
**Assembly information:**  
![Screenshot_3](https://github.com/Wortexz/ltspyware/assets/26935578/65f1b595-3890-4768-a09d-5b28fb795d1a)
**One of the modules**  
![Screenshot_2](https://github.com/Wortexz/ltspyware/assets/26935578/6300bcb0-3dae-4f81-9fd3-cc7b38391b8b)


**ESET Detections:**  
* _Запрос.doc_ - VBA/TrojanDropper.Agent.CXW
* _certweb32.exe_ - A Variant Of MSIL/Small.II
