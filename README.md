#1 - Question: At what time (UTC, including year) did the portscanning activity from IP address 123.150.207.231 start?
<pre>
$ grep -E '123\.150\.207\.231|ntp|rtc' SWT-syslog_messages
Aug 29 07:07:40 gw kernel: platform rtc_cmos: registered platform RTC device (no PNP device found)
Aug 29 07:07:40 gw kernel: rtc_cmos rtc_cmos: rtc core: registered rtc_cmos as rtc0
Aug 29 07:07:40 gw kernel: rtc0: alarms up to one day, 114 bytes nvram
<b>Aug 29 07:07:40</b> gw kernel: rtc_cmos rtc_cmos: setting system clock to <b>2013-08-29 11:07:08 UTC (1377774428)</b>
Aug 29 07:07:44 gw ntpd[1115]: ntpd 4.2.4p8@1.1612-o Fri Feb 22 11:23:27 UTC 2013 (1)
Aug 29 07:07:44 gw ntpd[1116]: precision = 15.532 usec
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #0 wildcard, 0.0.0.0#123 Disabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #1 wildcard, ::#123 Disabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #2 lo, ::1#123 Enabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #3 eth0, fe80::a00:27ff:fe53:38ee#123 Enabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #4 eth1, fe80::a00:27ff:fe3f:710#123 Enabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #5 lo, 127.0.0.1#123 Enabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #6 eth0, 98.252.16.36#123 Enabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on interface #7 eth1, 172.16.62.1#123 Enabled
Aug 29 07:07:44 gw ntpd[1116]: Listening on routing socket on fd #24 for interface updates
Aug 29 07:07:44 gw ntpd[1116]: kernel time sync status 2040
Aug 29 07:07:44 gw ntpd[1116]: frequency initialized 1.726 PPM from /var/lib/ntp/drift
Aug 29 07:07:46 gw named[1004]: error (unexpected RCODE SERVFAIL) resolving 'g.ntpns.org/A/IN': 108.161.191.2#53
Aug 29 07:12:03 gw ntpd[1116]: synchronized to 50.22.155.163, stratum 2
Aug 29 07:12:03 gw ntpd[1116]: time reset +2.185132 s
Aug 29 07:12:03 gw ntpd[1116]: kernel time sync status change 2001
Aug 29 07:19:35 gw ntpd[1116]: synchronized to 50.22.155.163, stratum 2
Aug 29 07:24:40 gw ntpd[1116]: synchronized to 50.23.135.154, stratum 2
Aug 29 07:33:31 gw ntpd[1116]: synchronized to 158.37.91.134, stratum 2
Aug 29 07:48:34 gw ntpd[1116]: synchronized to 50.22.155.163, stratum 2
Aug 29 07:55:44 gw ntpd[1116]: synchronized to 50.23.135.154, stratum 2
Aug 29 08:15:02 gw ntpd[1116]: synchronized to 158.37.91.134, stratum 2
<b>Aug 29 09:58:55</b> gw kernel: FW reject_input: IN=eth0 OUT= MAC=08:00:27:53:38:ee:08:00:27:1c:21:2b:08:00 SRC=123.150.207.231 DST=98.252.16.36 LEN=44 TOS=0x00 PREC=0x00 TTL=41 ID=35517 PROTO=TCP SPT=38553 DPT=3306 WINDOW=1024 RES=0x00 SYN URGP=0
--SNIP--
Aug 29 10:10:10 gw kernel: FW reject_input: IN=eth0 OUT= MAC=08:00:27:53:38:ee:08:00:27:1c:21:2b:08:00 SRC=123.150.207.231 DST=98.252.16.36 LEN=44 TOS=0x00 PREC=0x00 TTL=41 ID=38705 PROTO=TCP SPT=38553 DPT=16001 WINDOW=1024 RES=0x00 SYN URGP=0
Aug 30 11:13:40 gw ntpd[1116]: ntpd exiting on signal 15
<b>
Answer: Aug 29 09:58:55 2013 + 4 hours is Aug 29 13:58:55 2013</b>
</pre>
#2 - Question: What IP addresses were used by the system claiming the MAC Address 00:1f:f3:5a:77:9b?
<pre>
$ tshark -r nitroba.pcap -V -Y "eth.src == 00:1f:f3:5a:77:9b" -T fields -e ip.src | sort -u
169.254.20.167
169.254.90.183
192.168.1.64
$ tshark -r nitroba.pcap -V -Y "eth.dst == 00:1f:f3:5a:77:9b" -T fields -e ip.dst | sort -u
192.168.1.64
<b>
Answer: 169.254.20.167, 169.254.90.183 and 192.168.1.64</b>
</pre>
#3 -  Question: What IP (source and destination) and TCP ports (source and destination) are used to transfer the “scenery-backgrounds-6.0.0-1.el6.noarch.rpm” file?
<pre>
$ tshark -r ftp-example.pcap -V -Y "ip.src == 192.168.75.29 && ip.dst == 149.20.20.135" -T fields -e tcp.seq -e ip.src -e ip.dst -e tcp.srcport -e tcp.dstport -e ftp.request.command -e ftp.request.arg -e ftp.response
--SNIP--
Nov 23, 2013 03:39:37.716671000 EST     192.168.75.29   149.20.20.135   37028   21      RETR    scenery-backgrounds-6.0.0-1.el6.noarch.rpm      0
Nov 23, 2013 03:39:37.806416000 EST     192.168.75.29   149.20.20.135   37028   21
Nov 23, 2013 03:39:37.821201000 EST     <b>192.168.75.29   149.20.20.135   51851   30472</b>
Nov 23, 2013 03:39:37.821383000 EST     <b>192.168.75.29   149.20.20.135   51851   30472</b>
--SNIP--
<b>
Answer:
Source IP: 192.168.75.29
Destination IP: 149.20.20.135
Source Port: 51851
Destination Port: 30472</b>
</pre>
#4 - Question: How many IP addresses attempted to connect to destination IP address 63.141.241.10 on the default SSH port?
<pre>
$ grep '63.141.241.10:22 ' IVS-netflow-2014-05-23/nfcapd.201405230000.txt | awk -F: '{print $3}' | awk '{print $4}' | sort -u | wc -l
49
<b>
Answer: 49 Unique IP addresses attempted to connect to the default SSH port</b>
</pre>
#5 - Question: What is the byte size for the file named "Researched Sub-Atomic Particles.xlsx"
<pre>
--SNIP--
640801  10.3.58.7       10.3.58.6       \users\public\temp\system7\Researched Sub-Atomic Particles.xlsx 16396
19990   10.3.58.6       10.3.58.7       \users\public\temp\system7\Researched Sub-Atomic Particles.xlsx 16396
640877  10.3.58.7       10.3.58.6       \users\public\temp\system7\Researched Sub-Atomic Particles.xlsx 16396   <b>13625</b>
20094   10.3.58.6       10.3.58.7       \users\public\temp\system7\Researched Sub-Atomic Particles.xlsx 16396
--SNIP--
<b>
Answer: 13625 bytes</b>
</pre>
#6a - Question: The traffic in this Snort IDS pcap log contains traffic that is suspected to be a malware beaconing. Identify the substring and offset for a common substring that would support a unique Indicator Of Compromise for this activity.
<pre>
$ tshark -r snort.log.1340504390.pcap -Y "tcp.len > 0 && tcp.dstport == 1951" -T fields -e data.data > snort.payload
$ for i in \`seq 1 32\`;do echo Offset:$((i-1)) Unique values:$(awk -F: "{print \$$i}" snort.payload | sort | uniq -c | sort -n | wc -l);done;
Offset:0 Unique values:1
Offset:1 Unique values:1
Offset:2 Unique values:42
Offset:3 Unique values:256
Offset:4 Unique values:1
Offset:5 Unique values:1
Offset:6 Unique values:1
Offset:7 Unique values:1
Offset:8 Unique values:1
Offset:9 Unique values:1
Offset:10 Unique values:1
Offset:11 Unique values:36
Offset:12 Unique values:36
Offset:13 Unique values:36
Offset:14 Unique values:35
Offset:15 Unique values:36
Offset:16 Unique values:36
Offset:17 Unique values:36
Offset:18 Unique values:36
Offset:19 Unique values:36
Offset:20 Unique values:36
Offset:21 Unique values:36
Offset:22 Unique values:36
Offset:23 Unique values:36
Offset:24 Unique values:36
Offset:25 Unique values:36
Offset:26 Unique values:36
Offset:27 Unique values:36
Offset:28 Unique values:36
Offset:29 Unique values:36
Offset:30 Unique values:36
Offset:31 Unique values:1
<b>
Answer: Offsets 4-10 are fixed with the value ULQENP2</b>
</pre>
#6b - Bonus Question: Identify the meaning of the bytes that precede the substring above.
<pre>
$ cut -d: -f1-4 snort.payload
4f:e6:c2:74
4f:e6:c2:75
4f:e6:c2:77
4f:e6:c2:78
4f:e6:c2:78
4f:e6:c2:78
4f:e6:c2:79
--SNIP--
4f:e6:eb:b0
4f:e6:eb:b1
4f:e6:eb:b5
4f:e6:eb:b5
4f:e6:eb:c0
<b>Answer: Offsets 0-3 appear to be a timestamp/counter as the values seem to increment</b>
Bonus: I wrote a snort signature for detecting the beacons.
$ snort -c SANS-DFIR-2015.rules -r snort.log.1340504390.pcap -l . --daq pcap --daq-dir /usr/lib/daq -A console
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.24:42124 -> 184.82.188.7:1951
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.51:43407 -> 184.82.188.7:1951
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.62:53407 -> 184.82.188.7:1951
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.99:53270 -> 184.82.188.7:1951
--SNIP--
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.193:55426 -> 184.82.188.7:1951
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.189:45839 -> 184.82.188.7:1951
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.153:42877 -> 184.82.188.7:1951
[**] [1:1000001:0] DFIR-2015 Malware beacon [**] [Priority: 0] {TCP} 10.3.59.166:45140 -> 184.82.188.7:1951
</pre>
