# TryHackMe
  https://tryhackme.com/room/wifihacking101
  
<img src="wifi_hack.png"
     alt="WIFI_HACK_Marker_icon"
     style="float: left; margin-right: 10px;" />

# Task 1  The basics - An Intro to WPA
What type of attack on the encryption can you perform on WPA(2) personal?

Answer-```Brute Force```

Can this method be used to attack WPA2-EAP handshakes? (Yea/Nay)

Answer-```Nay``` Initialization vecors are needed to crack

What three letter abbreviation is the technical term for the "wifi code/password/passphrase"?

Answer-```PSK```  (preshared key)

What's the minimum length of a WPA2 Personal password?

Answer-```8```

# Task 2  You're being watched - Capturing packets to attack
Using the Aircrack-ng suite, we can start attacking a wifi network. This will walk you through attacking a network yourself, assuming you have a monitor mode enabled NIC.

I suggest creating a hotspot on a phone/tablet, picking a weak password (From rockyou.txt) and following along with every stage. To generate 5 random passwords from rockyou, you can use this command on Kali:

command to use is ```head /usr/share/wordlists/rockyou.txt -n 10000 | shuf -n 5 -```

You will need a monitor mode NIC in order to capture the 4 way handshake. Many wireless cards support this, but it's important to note that not all of them do.

To do this ran commands below>>>>

```apt update
apt update
apt install bc -y
git clone https://github.com/cilynx/rtl88x2BU_WiFi_linux_v5.3.1_27678.20180430_COEX20180427-5959.git
cd rtl88x2BU_WiFi_linux_v5.3.1_27678.20180430_COEX20180427-5959
VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
sudo dkms add -m rtl88x2bu -v ${VER}
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 88x2bu
```
I needed dkms
```sudo apt-get install dkms```

Next command failed

```sudo dkms build -m rtl88x2bu -v ${VER}```

kali needs updated>>>

```sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 88x2bu
```
 Injection mode helps, as you can use it to deauth a client in order to force a reconnect which forces the handshake to occur again. Otherwise, you have to wait for a client to connect normally.

How do you put the interface “wlan0” into monitor mode with Aircrack tools? (Full command)

Answer-```airmon-ng start wlan0```

What is the new interface name likely to be after you enable monitor mode?

```wlan0mon```

What do you do if other processes are currently trying to use that network adapter? 

```airmon-ng check kill```

What tool from the aircrack-ng suite is used to create a capture?

```airodump-ng```

What flag do you use to set the BSSID to monitor?

```--bssid```

And to set the channel?

```--channel```

And how do you tell it to capture packets to a file?

```-w```


# Task 3  Aircrack-ng - Let's Get Cracking

```I will attach a capture for you to practice cracking on. If you are spending more than 3 mins cracking, something is likely wrong. (A single core VM on my laptop took around 1min).

In order to crack the password, we can either use aircrack itself or create a hashcat file in order to use GPU acceleration. There are two different versions of hashcat output file, most likely you want 3.6+ as that will work with recent versions of hashcat.

Useful Information

BSSID: 02:1A:11:FF:D9:BD

ESSID: 'James Honor 8'
```
