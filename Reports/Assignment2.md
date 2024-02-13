# Assignment 2

## Summary

According to my own analysis, and then also the analysis of professionals in the field, this file is certainly a type of ransomware, namely, KeyPass ransomware. I detected this by various strings found in the file indicating - string indicating both the functionality as ransomware and the variant as KeyPass. After hashing the file and searching for the hash in VirusTotal, it was confirmed that this file is indeed KeyPass ransomware, with 50+ detections and various reports written on this specific hash. The easiest and most reliable indicator of compromise is found in the existence of files with the `.KEYPASS` extension, but it is possible that network connections to a `kronus.pp.ua` domain also indicate compromise.

## Type of Malware

This malware targets the Windows Operating System. This is known through the following indicators:
- The File Type is said to be `Portable Executable 32` by CFF Explorer.
- The first two bytes of the file are `0x4D5A`, which indicates a Windows Executable.
- In the Import Directory found in CFF Explorer we encounter various DLL files, which are Windows-specific library files.

This malware seems to be Ransomware. This following features are indicators of the program being ransomware:
- Under the Resource Editor in CFF Explorer, we see Bitcoin icons.
- After running FLOSS on the file and extracting all of the strings, we found a ransom note string within the file:
  ```
  Attention!
  All your files, documents, photos, databases and other important files are encrypted and have the extension: .KEYPASS
  The only method of recovering files is to purchase an decrypt software and unique private key.
  After purchase you will start decrypt software, enter your private key and it will decrypt all your data. 
  Only we can give you this key and only we can recover your files.
  You need to contact us by e-mail BM-2cUMY51WfNRG8jGrWcMzTASeUGX84yX741@bitmessage.ch send us your personal ID and wait for futher instructions.
  [...]
  Price for decryption is $300.
  [...]
  ```
- After running the program in an isolated system, the program does encrypt files and ask for payment for the decryption key.

## Signatures

Hashes: \
MD5: `6999C944D1C98B2739D015448C99A291` \
SHA-1: `D9BEB50B51C30C02326EA761B5F1AB158C73B12C` \

I entered these hashes into VirusTotal and VirusTotal did flag the hash as belonging to malware. Most scanners reported from VirusTotal indicate a type of "Trojan" or "Ransom". All these reports confirm my discoveries in the the [Type of Malware](#type-of-malware) section, including the prediction of the file being a ransomware program and it being a Windows Executable.

VirusTotal Analysis: https://www.virustotal.com/gui/file/35b067642173874bd2766da0d108401b4cf45d6e2a8b3971d95bf474be4f6282

## Indicators of Compromise

There are a indicators of compromise to check if this malware has already infected their system, including:
- Any files that have the extension `.KEYPASS`, as all encrypted files will have this extension.
- The following domains were found within the file, so any connections to these domains/sites my be an indicator of compromise:
  - `hxxp://kronus.pp.ua/upwinload/get.php`
  - `hxxp://onus.pp.ua/upwinload/get.php`
- Likewise, these emails addresses were found, so any communication with these domains could be an indicator:
  - `keypassdecrypt@india.com`
  - `BM-2cUMY51WfNRG8jGrWcMzTASeUGX84yX741@bitmessage.ch`


## Clues about Origin

From extracting strings from the file, the renamed file extensions, and the various VirusTotal reports and community notes, this ransomware seems to be
KeyPass ransomware. After some brief research on this Keypass Ransomware, I discovered it was originally made public and known in August 2018. 

Interestingly enough, this ransomware is thought to be originally created with the idea of using it in targetted attacks. According to a SecureList article (https://securelist.com/keypass-ransomware/87412/), this is believed because of a GUI that is coded within the program that "can be shown after pressing a special button on the keyboard." This GUI allows the user to change the used encryption key, folders to encrypt, and other information of the ransom note. This GUI is most likely the reason that the `GDI32.dll` library, a GUI library, was found in the Important Directory of the executable.

Also something of note, according to a Kaspersky article on KeyPass (https://usa.kaspersky.com/blog/keypass-ransomware/15924/), "a peculiar feature of KeyPass is that if for some reason the computer is not connected to the Internet when the malware starts working [...] it uses a hard-coded key, meaning that the files can be decrypted without any trouble". This is curious because if the key is hard coded, as mentioned, the user could decrypt their files after finding this key (if they happened to not be connected to the internet). This would seem to be a flaw in the logic for the authors of this ransomware, but perhaps this is further indication that this is supposed be used in targetting individuals -- as if it were only used on some individuals, the reverse engineering community probably wouldn't get their hands on on the executable and this aspect of the encryption wouldn't be discovered.

But according to the earlier SecureList article, if the victim's computer is connected to the internet, the ransomware wil connect to its C2 server to receive an encryption key to encrypt the victim's computer with. But, according to this article, "The data is transferred over plain HTTP in the form of JSON.", so any network captures would contain the decryption key, which might allow a victim to decrypt their files without any ransom.

There doesn't seem to be much, if any, information about the authors or origin of this KeyPass ransomware. Nor any other motivations for the spread/creation of this malware (besides the profit from the ransoms). In the domains discovered in the file, the domains `kronus.pp.ua` and `onus.pp.ua` were found -- maybe indicating a Ukrainian source (.ua is the Ukraine extension) but this is pure speculation.

## YARA Rule

```
rule detectKeyPass 
{
    strings:
        $a = "KEYPASS"
        $b = "onus.pp.ua" nocase
        $c = "keypassdecrypt@india.com" nocase

    condition:
        $a and $b and $c

}
```

I think that generally this will not detect false positives. The chances that a file contains both of those specific domains AND the string `KEYPASS` is quite unlikely. Those domains are already seemingly uncommon (I had never once been to india.com nor any .ua site). Likewise, the string `KEYPASS` is not a common word, and also to see it in all capitals is also more unlikely. So by and-ing of all these unlikely features, I don't think there will be many, if any, false positives.

