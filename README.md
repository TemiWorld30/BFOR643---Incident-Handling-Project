c

### 3.2 CyberChef
**Author: Komali**

`CyberChef` is a tool used to analyze, decode and encode data. When we looked at the email sample, it stated that the file was encoded in Base64. This encoding is used to trasmit binary data over systems that only support text and is used to hide malicious URLs/ scripts from security tools. 

After opening: [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/), a portion of the email was pasted into the input field. The content seemed like random characters and looked like it was encoded as Base64. `From Base64` is an operation that helped decode this encoded string, so it was dragged under recipes. After clicking the button `BAKE`, it decoded the encoded string to reveal a malicious link which will be discussed in detail under `RESULTS` section.

### 3.3 VirusTotal
**Author: Komali**

`VirusTotal` is a cloud-based malware analysis platform that scans and verifies if any files/URLs/IP Addressses are flagged as malicious. It is very useful for analysis as it scans these against 80+ antivirus engines and provides a detailed report. 

`Indicators of Compromise (IOCs)` such as IP Address, the link embedded into the encoded Base64 string were submitted to this tool. After scanning each IOC, there were 0 flags for any malicious content. However, it is important to use multiple tools to verify if any emails are malicious as there was email authenticaion failures when analyzing email headers.

Multiple tools must be used together to determine if an email is malicious or not. Since this is a sample, VirusTotal did not flag any of the embedded links, but in real life, VirusTotal can be used to verify if any embedded content is malicious or not. 

## 4. Development Environment

### VM Setup
**Author: David**

## To Run This Project (Step-By-Step Guide)

### 4.2 Download and Install UTM

**Step 1:** Visit [https://mac.getutm.app/](https://mac.getutm.app/)

**Step 2:** Click the download button for macOS

**Step 3:** Open the downloaded `.dmg` file

**Step 4:** Drag UTM to your Applications folder

**Step 5:** Open Applications folder and launch UTM

**Step 6:** Grant any necessary permissions when prompted

---

### 4.3 Deploy Kali Linux on UTM

**Step 7:** Follow Kali Linux's official UTM installation guide:
[https://www.kali.org/docs/virtualization/install-utm-guest-vm/](https://www.kali.org/docs/virtualization/install-utm-guest-vm/)

This guide will walk you through:
* Downloading the Kali Linux ARM64 image for UTM
* Creating a new virtual machine in UTM
* Allocating CPU, RAM, and storage resources
* Importing the Kali Linux image

---

### 4.4 Setup and Configure Kali Linux

**Step 8:** Follow Kali Linux's installation and setup guide:
[https://www.kali.org/docs/installation/hard-disk-install/](https://www.kali.org/docs/installation/hard-disk-install/)

This guide will walk you through:
* Initial Kali Linux boot and configuration
* Partitioning and disk setup
* User account creation
* System updates and package configuration
* Desktop environment setup

---

### 4.5 Login to Kali Linux

**Step 9:** At the Kali Linux login screen, enter:
* **Username:** `kali`
* **Password:** `kali`

**Step 10:** Press Enter to login

---

### 4.6 Use CyberChef and VirusTotal

**Step 11:** Open a web browser in Kali Linux

**Step 12:** Visit CyberChef at [https://cyberchef.io/](https://cyberchef.io/)

**Step 13:** Use CyberChef for your analysis tasks

**Step 14:** Visit VirusTotal at [https://www.virustotal.com/](https://www.virustotal.com/)

**Step 15:** Use VirusTotal for your scanning and analysis tasks

---

## 5. Results

### 5.1 CyberChef
**Author: Komali**

The portion of the email has content encoded in Base64 which converts data into 64 ASCII characters. Base64 encoding itself is not malicious but it is commonly used to embedded malicious scripts/URLs into the final string to bypass security tools.

Using CyberChef, the Base64 string was analyzed and decoded. The encoded string was pasted into the input field and `From Base64` operation was applied to the recipes. After hitting the button `BAKE`, it decoded the string and revealed the malicious URL:

` <a href="https://blog1seguimentmydomaine2bra.me/">Clique aqui</a></font> `

This URL does not seem to be related to Bradesco Bank (the bank it is claiming to be) and the anchor text of `Clique aqui`
(Click here) , seems to be very generic and suspicious.

<img width="1435" height="790" alt="Screenshot 2026-04-22 at 9 58 34 PM" src="https://github.com/user-attachments/assets/26c8943a-889e-4a0a-8802-3d894c2bc896" />

### 5.2 VirusTotal
**Author: David**

`VirusTotal` is a cloud-based malware analysis platform that aggregates detection data from 80+ antivirus engines, URL/domain blacklists, and IP reputation databases. By submitting Indicators of Compromise (IOCs) such as URLs, domains, and IP addresses, analysts can quickly determine if they have been flagged as malicious by multiple security vendors.

For this phishing investigation, three primary IOCs were submitted to VirusTotal for analysis:

`IOC 1 - URL Scanned`
<img width="1435" height="790" alt="image" src="https://github.com/user-attachments/assets/ccee722e-584d-4033-a25e-c6474a89df11" />

The malicious URL decoded from the Base64 string (`https://blog1seguimentmydomaine2bra.me/`) was submitted to VirusTotal for analysis. This URL returned 0 detections across all antivirus engines, indicating it was either newly registered or not yet flagged by security vendors at the time of scanning.

`IOC 2 - IP Address Scanned`
<img width="1435" height="790" alt="image" src="https://github.com/user-attachments/assets/0c5808b2-781b-47f4-b930-0d244acbb06b" />

The originating IP address (`137.184.34.4`) extracted from the email headers was scanned through VirusTotal. This IP traces back to DigitalOcean, LLC, a cloud hosting provider in San Jose, California. The scan revealed 0 malware detections, but the geographic mismatch (email claims to be from a Brazilian bank but originates from California) is a significant red flag for phishing.

`IOC 3 - Domain Scanned`
<img width="1435" height="790" alt="image" src="https://github.com/user-attachments/assets/ea57a60e-f7bd-4756-b8e2-276db9f5d8ad" />

The suspicious domain (`blog1seguimentmydomaine2bra.me`) was submitted for analysis. VirusTotal returned 0 flags across all indicators, demonstrating why multiple analysis tools must be used in conjunction. A domain may not be flagged as malicious by traditional antivirus engines, but other indicators—such as email authentication failures, domain registration details, and sender mismatches—reveal its malicious intent.

**Key Finding:** While VirusTotal did not flag any of the IOCs as malicious, this emphasizes the importance of using a multi-layered approach to threat analysis. Traditional malware detection engines may miss sophisticated phishing attacks that leverage newly registered domains and cloud infrastructure. Email authentication analysis (SPF/DKIM/DMARC failures) and sender verification remain critical in identifying phishing emails that evade signature-based detection.

## 6. Documentation

`GitHub Sample`
<img width="1047" height="688" alt="Screenshot 2026-04-22 at 10 04 58 PM" src="https://github.com/user-attachments/assets/eabf5e5e-afbf-4dad-9b83-976b374557f9" />




`IOC 1 - Suspicious IP Address`
<img width="550" height="128" alt="width-550" src="https://github.com/user-attachments/assets/15a8008d-87ee-49de-b8ba-e4f5d5136f83" />

IP Addresses reveal the location the email originates from and gives users insight into whether you can trust this sender.
The IP Address listed is `137.184.34.4` and when we searched up this, it traces back to a cloud server in the San Jose area called `DigitalOcean,LLC`. This is suspicious as real banks don't send emails from random cloud servers from outside their country of origin. Since this email claims to be from Bradesco Bank, a bank in Brazil, it raises a red flag that the sender is traced back to California.




`IOC 2 - Failed Email Authentications`
<img width="918" height="431" alt="Screenshot 2026-04-22 at 10 24 25 PM" src="https://github.com/user-attachments/assets/594e59e4-465e-4c2a-b1f5-9a6416c50d39" />

Email Authentication helps users understand and verify if they can even trust the sender of the email. There are a few email authentication protocols that should be passed, if the sender is valid. These protocols include `SPF`, `DKIM`and `DMARC` and work together to prevent any spoofing or phishing.

`SPF (Sender Policy Framework)` checks to see if the sender is using any of the authorized IP Addresses established by the domain owner.  When this fails it means the sender is not authorized to send the email on behalf of the domain. 

`DKIM (DomainKeys Identified Mail)` is a digital signature to the email that makes sure the email hasn’t been altered. Failed DKIMs are one of the reasons why emails are marked as spam and the sender is probably not who they claim to be.

`DMARC (Domain based Message Authentication Reporting and Conformance)` is a security protocol that uses both `SPF` and `DKIM` to verify the senders identity. Failed DMARC means this is likely a phishing attack.




`IOC 3 - Sender Identity `
<img width="873" height="403" alt="Screenshot 2026-04-22 at 10 25 05 PM" src="https://github.com/user-attachments/assets/1abdfd07-395c-4d9d-9fa8-e923536ca1d5" />

Confirming if the sender identities match is a crucial step in verifying if the email is a phishing email or not. This can be done by checking who this email is coming from `From: BANCO DO BRADESCO LIVELO<banco.bradesco@atendimento.com.br>` and the return path `Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06`.

The first red flag here is that the return path does NOT match the sender domain at all. The second red flag is when we checked the official website of the back, the official domain of the bank is `@bradescobank.com` , not  `@atendimento.com.br`.




`IOC 4 - Base64 Encoding`
<img width="873" height="422" alt="Screenshot 2026-04-22 at 10 25 21 PM" src="https://github.com/user-attachments/assets/09afbefc-1839-44c3-9286-a23513953528" />

`Base64` is a binary to text encoding scheme that turns binary data into a sequence of 64 ASCII (askey) characters. This encoding is used to transmit binary data over systems that only support text (such as email). This encoding is often used to transform code into a `Base64 format` to hide these malicious URLs/scripts from security tools.




`CyberChef Decoded output`
<img width="1435" height="790" alt="Screenshot 2026-04-22 at 9 58 34 PM" src="https://github.com/user-attachments/assets/84996d9a-88d2-4ac9-bc85-75b72795aeac" />

After decoding the encoded string by using the T `From Base64` operation,it revealed the malicious URL:

` <a href="https://blog1seguimentmydomaine2bra.me/">Clique aqui</a></font> `

Embedded URLs are something to look out for as these can be very malicious if you accidentally open them. There is even the anchor text of `Clique aqui`, which prompts the user the click a button. 




`Sample Email Layout`
<img width="979" height="1606" alt="05002d64-4971-4ba8-aca0-c78b9238df4c" src="https://github.com/user-attachments/assets/e94c8d77-daa6-4c73-a234-e78a2f8bc417" />

This sample email is what a user would potnetially see on their end of the screen when they received this email. This was generated using `ChatGPT` using the decoded Base64 content. This was done to visualize what a user would potentially see on their end when they received an email. This was generated purely for visualization puposes.


Below is also a visualization of how the email would look (the decoded content from CyberChef was directly pasted below).
Please **DO NOT** click any links below *







<!DOCTYPE html><html lang="en"><head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"><body style="background-color:rgb(241, 241, 241);">
 
 <p style="text-align:center;">
 <font face="Arial" size="2">Para visualizar as imagens deste email.`<a href="https://blog1seguimentmydomaine2bra.me/">Clique aqui`</a></font>
 </p>
 
 
 <meta http-equiv="X-UA-Compatible" content="IE=edge">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <link rel="preconnect" href="https://fonts.gstatic.com">
 <link href="https://fonts.googleapis.com/css2?family=Signika:wght@300;500;700&amp;display=swap" rel="stylesheet">
 <title>Pontos Livelo</title>
 </head>
 <body style="background-color:#eeeeee;">
 <div id="bg" style="width: 602px; margin: 0 auto; padding: 15px;background-color: #fff;">
 <div id="bg" style="width: 100%; margin: 0 auto; padding: 0px 15px 15px 15px; border: 2px solid #e50091;box-sizing: border-box;">
 <div style="text-align: center; margin-bottom: 30px;">
 <img src="header.png" alt="">
 </div>
 <div style="text-align: center;">
 <img src="icone-superior.png" alt="">
 </div>
 <div style="text-align: center;">
 <h1 style="font-family: 'Signika', sans-serif; font-weight: 700;color: #190f55;font-size: 26px;padding-top: 0px;margin-top: 0px;">Banco do Bradesco (Livelo). </h1>
 </div>
 <div>
 <p style="font-family: 'Signika', sans-serif; font-weight: 300; color: #707070; font-size: 16px; line-height: 18px;">Você possui <strong style="color:#190f55;">Pontos Livelo com seu cartão Banco do Bradesco</strong> disponíveis para resgate que expiram HOJE, evite a perda destes pontos realizando agora mesmo o resgate da sua Pontuação Visa Infinite.</p>
 </div>
 <div style="margin-bottom:30px;">
 <p style="font-family: 'Signika', sans-serif; font-weight: 300; color: #707070; font-size: 16px; line-height: 18px;">Você Clientes <strong style="color:#190f55;">Banco do Bradesco</strong> acumulam pontos livelo todas as vezes que utilizam seus cartões na função débito ou crédito, é rápido e fácil de acumular.</p>
 </div>
 
 <div style="background-color:#FF0080; border-radius:20px;margin-bottom: 40px;">
 <table width="100%" cellspacing="0" cellpadding="0">
 <tr>
 <td width="60%" style="padding-left:20px;padding-top: 30px; padding-bottom: 30px;">
 <p style="font-family: 'Signika', sans-serif; font-weight: 300; color: #ffff; font-size: 14px; line-height: 18px; margin:0px;padding:0px;"><span style="font-weight: 500;">Troque seus pontos por milhas aéreas</span> </p>
 <p style="font-family: 'Signika', sans-serif; font-weight: 300; color: #ffff; font-size: 14px; line-height: 18px; margin:0px;padding:0px;"><span style="font-weight: 500;">Descontos de até 35% na fatura do cartão</span> </p>
 <p style="font-family: 'Signika', sans-serif; font-weight: 300; color: #ffff; font-size: 14px; line-height: 18px; margin:0px;padding:0px;"><span style="font-weight: 500;"></span></p>
 </td>
 <td width="40%" style="padding-right:20px;">
 <div style="border-left: 1px solid #fff; padding-left:40px;padding-top: 0px;padding-bottom: 0px;">
 <h2 style="font-family: 'Signika', sans-serif; font-weight: 700;color: #fff;font-size: 36px;padding: 0px;margin: 0px;">92.990</h2>
 <p style="font-family: 'Signika', sans-serif; font-weight: 300;color: #fff;font-size: 10px;padding: 0px;margin: 0px;">MIL PONTOS ACUMULADOS EXPIRAM HOJE</p>
 </div>
 </td>
 </tr>
 </table>
 </div>
 <div style="text-align: center;margin-bottom: 70px;">
 <a style="padding:10px 40px;border-radius:20px;text-decoration: none;color: #fff;font-family: 'Signika', sans-serif; font-weight: 500;font-size: 16px;background: linear-gradient(to top,#FF0080,#00b5fc);background-color: #FF0080;" href="https://blog1seguimentmydomaine2bra.me/">Resgatar Agora</a>
 </div>
 
 <div>
 <p style="font-family: 'Signika', sans-serif; font-weight: 300; color: #707070; font-size: 12px; line-height: 18px;"><img src="icone-rodape.png" style="float: left;;" alt="">Resgate agora mesmo antes que eles expirem! Aproveite, Troque seus pontos por milhas aereas, Descontos de ate 35% no cartão ou milhares de premios em nosso Catalogo.</p>
 </div>
 </div>
 </div>
 </body>
 </html>
 

 

`Live Demo`
https://github.com/user-attachments/assets/ebff0b2f-9bfb-4dba-883c-17d84022fa9f


`IOC 1 - URL Reputation Analysis`
<img width="1435" height="790" alt="VirusTotal URL Scan" src="https://github.com/user-attachments/assets/ccee722e-584d-4033-a25e-c6474a89df11" />

When a URL is submitted to VirusTotal, it is scanned against 80+ antivirus engines and URL blacklists to determine if it has been flagged as malicious. In this case, the URL (`https://blog1seguimentmydomaine2bra.me/`) returned 0 detections. However, this does not necessarily mean the URL is safe. Newly registered phishing domains often bypass antivirus engines for the first 24-48 hours. This demonstrates why analysts must use multiple tools in conjunction—VirusTotal alone cannot identify newly created malicious domains. Additional analysis such as domain registration age, WHOIS data, and SSL certificate details must be examined to determine true intent.




`IOC 2 - IP Address Geolocation and Hosting Provider`
<img width="1435" height="790" alt="VirusTotal IP Address Scan" src="https://github.com/user-attachments/assets/0c5808b2-781b-47f4-b930-0d244acbb06b" />

When an IP address is scanned in VirusTotal, the tool provides geolocation data and identifies the hosting provider. In this case, the IP address (`137.184.34.4`) is hosted by DigitalOcean, LLC in San Jose, California. VirusTotal returned 0 malware detections for this IP. However, the geolocation is a significant red flag. Legitimate financial institutions send emails from dedicated servers within their country of origin, not from shared cloud hosting providers in different regions. This geographic mismatch strongly suggests the email is spoofed or phishing, even though the IP itself has not been flagged as malicious by antivirus engines.




`IOC 3 - Domain Hosting Infrastructure`
<img width="1435" height="790" alt="VirusTotal Domain Scan" src="https://github.com/user-attachments/assets/ea57a60e-f7bd-4756-b8e2-276db9f5d8ad" />

Domain scanning through VirusTotal provides critical information about hosting infrastructure, DNS resolution, and nameservers. The domain (`blog1seguimentmydomaine2bra.me`) returned 0 detections across all antivirus and reputation databases. However, several indicators of compromise are visible through infrastructure analysis. The deceptive domain name (containing "seguimento" and "domaine" to appear Brazilian) combined with the use of a `.me` top-level domain instead of `.com` or `.br` are suspicious. VirusTotal's infrastructure data reveals hosting history and nameserver information that, when analyzed together with other IOCs, strengthens the conclusion that this is a phishing domain designed to evade detection.

---

### ✒️ Authors
- Rachael Oyenola
- David Matute-Jimenez
- Komali Kollapudi
