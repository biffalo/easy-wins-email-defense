# easy-wins-email-defense
### <img src="/_resources/2023-10-27%2014_52_29.png" alt="2023-10-27 14_52_29.png" width="706" height="382">

### Collection of resources/ideas/recommendations for reducing attack service for Microsoft 365/Microsoft Entra ID (AzureAD). The goal of these are to be EASY, low breakage, and cheap or free.

# Conditional Access Policies

📝PLEASE NOTE: All of these require at least 1 Entra ID P2 license. You can browse direct to the Entra/AzureAD admin center at [THIS link](https://entra.microsoft.com/).

## 🌍Conditional Access - Block Countries you don't have users in

By default. Authenticated access to a 365/Azure tenant is permitted from ANYWHERE. Make a named location and fill it with countries your users are not expected to login from and include it in the policy and select "block" under the "grant" section. To avoid locking yourself out if something goes sideways, you could exclude a breakglass admin account etc. This doesn't stop attackers from just using a VPN in the US, but this offers a solid reduction in attack surface.  
<img src="/_resources/2023-10-27%2014_37_07.png" alt="2023-10-27 14_37_07.png" width="686" height="377">

## 🪪Conditional Access - Require MFA for all Logins

Most orgs appear to utilize "Security Defaults" on their 365/Azure tenants. While that is better than nothing, that doesn't mean a user (malicious or otherwise) will get an MFA challenge for every M$ service. Check out [THIS](https://rootsecdev.medium.com/azure-ad-security-defaults-mfa-bypass-with-graph-api-86a5d6f57d4a) link as a example.  

The solution here is to require MFA for all cloud apps no matter what. Create a CA rule and apply it to all cloud apps, all users. You'll want to exclude your ADSync/ADConnect service account in a sep policy and lock that down to only being allowed to login from the IP ranges where your DCs reside. Under "Conditions" select Grant and require multifactor authentication. For bonus points you can optionally require that the device be hybrid joined which will further reduce our attack surface here.  
<img src="/_resources/2023-10-27%2014_49_17.png" alt="2023-10-27 14_49_17.png" width="966" height="476">

## 📱Conditional Access - OS/Platform Blocking

If you don't have Macs/iPhones in your environment, Macs/iPhones shouldn't be logging in. Don't let them 😎. Obviously adapt this to suit the OSs/Platforms used in your environment. Create a CA rule and apply to all apps/users. Select conditions > device platforms. Action is block.  
<img src="/_resources/2023-10-27%2015_01_44.png" alt="2023-10-27 15_01_44.png" width="778" height="539">

# 😶‍🌫️Blocking Consent Phishing

Block consent phishing, by disallowing users to register apps or limiting apps/permissions they can register.  
Head to Entra/AzureAD admin center > Enterprise Apps > Security > Consent & Permissions. Set both options here to do not allow. Optionally you can define a set of low risk permissions for verified apps that users can register without approval.  

<img src="/_resources/2023-10-30%2007_27_14.png" alt="2023-10-30 07_27_14.png" width="825" height="490">




# 🎭Sign-in Risk Policy

Microsoft has a neat feature buried in AzureAD/Entra ID > Security > Identity Protection. It will evaluate each login against a black box of factors, but those that are known are IP reputation, impossible travel, odd sign in properties etc. Enable sign in risk in blocking mode for low and above. You can remediate any false positives using the reporting section. You can also utilize sign-in risk using conditional access policies if you'd prefer a more granular implementation.  
<img src="/_resources/2023-10-27%2015_10_55.png" alt="2023-10-27 15_10_55.png" width="493" height="541">

# 🥷🏻Spam/Phishing/Malware Filtering

Microsoft's default spam/phishing/malware filtering settings leave a bit to be desired. We'll cover a few easy ways below to quickly improve that. These are almost exclusively handled in the security admin center. Direct link [HERE](https://security.microsoft.com/).  

## 🎣Anti-Phishing Improvements

Go to Policies & Rules > Threat Policies > Antiphishing . Modify the default phishing rule as shown in the below screenshot.  
<img src="/_resources/2023-10-27%2015_20_01.png" alt="2023-10-27 15_20_01.png" width="443" height="354">

## ⌨️Block Typosquat Domains

Go to Policies & Rules > Threat Policies > Antispam (outbound) and edit the default policy. In another tab generate a list of typosquat domains for your domain using a service like https://domaincheckplugin.com/typo . Scroll down to the bottom of the antispam outbound settings and click the blocked domains list. Add your generated list here and save.  
<img src="/_resources/2023-10-27%2015_28_00.png" alt="2023-10-27 15_28_00.png" width="422" height="357">

## 🦠Blocking Risky Email Attachments

There are a myriad of email attachments that have a high likelihood of being malicious. You can block them entirely by going to Policies & Rules > Threat Policies > Antimalware. Edit the default policy, enable the common attachments filter, and add the extensions below. I would recommend leaving the action as quarantine so you have the option to recover any legit emails that get caught. 

|          |          |          |          |
|----------|----------|----------|----------|
| bat      | hta      | jse      | scr      |
| chm      | htm      | mshtml   | vbe      |
| cmd      | iso      | one      | vbs      |
| com      | js       | ps1      | vhd      |
| exe      | lnk      | rar      | wsf      |
|          | zip      |          | wsh      |


## 🧑‍💻Alerting on AiTM/Proxy Phishing

You can generate a canary token for AzureAd/Entra that will alert if you a proxy or cloned login page loads custom CSS from a domain that isn't microsoft. More info about setup can be found [HERE](https://blog.thinkst.com/2024/01/defending-against-the-attack-of-the-cloned-websites.html)
    

That's all for now. More will be added as needed. Thanks for taking the time to get this far 🤩!
