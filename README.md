# Home Lab Phase 3

## Basic Network Functionality Testing

Here we will test basic network communication between all devices, including existing devices and those that have been set up and configured in the previous steps. Using a simple command line interface (CLI) tool called ‘nmap’ we will determine if a host is working, also referred to as ‘up’. This proves that communication is taking place. A faster tool called ‘ping’ could be used, but all the devices are set to ignore incoming pings and not send out replies. This is for security reasons and helps devices from being easily discovered by outside agents. Blocking pings doesn’t prevent attackers from discovering your hosts, but it does add obscurity because ping sweeps are often one of the first steps in gathering information about a possible target.

- Right click the Windows start button on the taskbar.
- Click ‘Terminal’:
  - This opens a terminal window, command prompt, or command line interface (CLI). It may be referred to by different names.
- Click inside the window with the mouse.
- Type `nmap <IP address>` without the quotes:
  - Replace `<IP address>` with the IP of each device you want to test.

The command line input and resulting output can be seen in the list below. The last two lines are important. This shows what IP the result is for, if the host is ‘up’, and the length of the latency. This is the round-trip time (RTT). Meaning how long it took for packets to be sent to the destination and return to the nmap source.

### Subnet 1 (home.lan) 192.168.1.0 /24

- PFSENSE LAN PORT:
  - `C:\Windows\System32>nmap 192.168.1.1`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:19 Eastern Daylight Time
  - Nmap scan report for 192.168.1.1
  - Host is up (0.0028s latency).

- ASUS ROUTER:
  - `C:\Windows\System32>nmap 192.168.1.2`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:20 Eastern Daylight Time
  - Nmap scan report for 192.168.1.2
  - Host is up (0.0049s latency).

- PC1:
  - `C:\Windows\System32>nmap 192.168.1.182`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:08 Eastern Daylight Time
  - Nmap scan report for 192.168.1.182
  - Host is up (0.0030s latency).

- PC2:
  - `C:\Windows\System32>nmap 192.168.1.135`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:07 Eastern Daylight Time
  - Nmap scan report for 192.168.1.135
  - Host is up (0.0031s latency).

- LAPTOP:
  - `C:\Windows\System32>nmap 192.168.1.116`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:10 Eastern Daylight Time
  - Nmap scan report for 192.168.1.116
  - Host is up (0.063s latency).

### Subnet 2 (lab.lan) 192.168.2.0 /24

- PFSENSE OPT PORT - LAB GATEWAY:
  - `C:\Windows\System32>nmap 192.168.2.1`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:10 Eastern Daylight Time
  - Nmap scan report for 192.168.2.1
  - Host is up (0.0016s latency).

- NETGEAR SWITCH:
  - `C:\Windows\System32>nmap 192.168.2.2`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:10 Eastern Daylight Time
  - Nmap scan report for 192.168.2.2
  - Host is up (0.014s latency).

- GMKtec M5 PRO - LAB PC:
  - `C:\Windows\System32>nmap 192.168.2.102`
  - Starting Nmap 7.94 ( https://nmap.org ) at 2024-09-24 17:10 Eastern Daylight Time
  - Nmap scan report for 192.168.2.102
  - Host is up (0.0000030s latency).

## Research Network Performance Analysis Tools

First, our internet connection speed will be tested. These speed statistics are measured in download, upload, and ping. The following testing servers are some of the most popular choices for testing internet connection speeds. Each has its own strengths, weaknesses, and features listed below in the table below:

<p align="center">
  <br/>
  <img src="https://imgur.com/qn5ndyc.png" height="80%" width="80%" alt="Table1.1"/><br /><br />
</p>

- **Ookla** – A powerful and popular option that has servers all over the world (some others may not and are limited depending on your region). Very good for general performance testing.
- **Cloudflare** – Useful for checking connection performance to Cloudflare’s network. This is especially important if you are using Cloudflare services.
- **Fast.com** – Analyzes connection speed and performance to Netflix servers specifically. This can be useful information for how other streaming services will perform on the network.
- **LibreSpeed** – Open-source option that is highly customizable. Provides privacy advantages and is beneficial for those that want to have more control over their testing environment.

### iPerf3

iPerf3 is another network performance testing tool that focuses on the bandwidth, throughput, and latency between two devices on the same network. It is a powerful and flexible tool. Full documentation can be found at [iPerf Documentation](https://iperf.fr/iperf-doc.php#3doc). Use and documented results of both internet speed testing and network performance testing can be found in the next section.

## Network Performance Testing

Internet speed testing is a straightforward process. Simply traveling to the associated website and clicking the ‘GO’ button will begin the testing. It should be noted that you should close as many applications as possible before beginning the testing (especially applications that use an internet connection like Spotify, Chrome, or other web browsers). 

We will use two in our testing; both Ookla and LibreSpeed. The results from the speed tests are listed in the table below.

<p align="center">
  <br/>
  <img src="https://imgur.com/EWfthxJ.png" height="80%" width="80%" alt="Table1.1"/><br /><br />
</p>

These are comparable results for either device being used, whether on ethernet or wireless, which are both on separate subnets. This reflects no large discrepancies between the choice of using ethernet or wireless and connecting to subnet 1 (home LAN) or subnet 2 (lab LAN).

While these speeds are not as fast as some top internet access speeds, they perform well for our purposes. I believe we are somewhat limited by the performance of the Netgate SG-1100, which has variable reports on throughput of 500 – 1000 Mbits, depending on sources. We should also consider the replacement of some runs of ethernet cabling that was pre-existing on the network before we began this project. 

Averaging these together, we get results for our two devices listed in Mbps below in the table below.

<p align="center">
  <br/>
  <img src="https://imgur.com/tvTpjPk.png" height="80%" width="80%" alt="Table1.1"/><br /><br />
</p>

## LAN Performance Testing with iPerf3

Moving on to the LAN performance between devices, we now use iPerf3. We are only testing two devices that exist in the lab currently: our Lab PC and the Laptop.

- Go to [iperf.fr](https://iperf.fr).
- Click on the **Download iPerf binaries** tab.
- Two links are available; we will use the GitHub link. Click on the link.
- On the right side of the page under **Releases**, click on the green **latest tag**.
- Click on the link to download the zip file.
- Unzip this file.
- Enter the unzipped folder.
- While in the folder, click the address bar:
  - Type `cmd` in the address bar and press Enter.
- This will open a command prompt window:
  - Notice the prompt refers to the iPerf application:
    ```
    C:\Users\bjg91\Downloads\iperf-3.17.1-win64-dynamic-auth>
    ```
- Type `iperf3.exe -s -p 6000` and press Enter:
  - This creates a server (or sender) on port 6000, sending messages used for measuring performance.
  - Next, we will set up the receiver of these messages on another device. But first, we need the IP address of the sender machine, so:
- Open a new terminal window and type `ipconfig` and press Enter.
- Look for an entry that is named **Ethernet** or **Ethernet adapter**.
- Note this **IPv4 address** down or leave the window open for now.
- On the second device (Lab PC), open a terminal window:
  - Follow the same steps of downloading iPerf3, unzipping the file, entering the unzipped folder, and typing `cmd` in the address bar and pressing Enter.
- Type `iperf3.exe -c <IP address> -p 6000` and press Enter in the newly opened terminal window (on the second device – the receiver – which is the Lab PC).
  - Allow the testing to run until it completes, which will only take a matter of seconds.

The results for the iPerf3 test are shown in the image of Figure 1 below:

<p align="center">
  <br/>
  <img src="https://imgur.com/ORtFHtH.png" height="80%" width="80%" alt="Table1.1"/><br /><br />
</p>

The iPerf3 program performs many small transfers of UDP packets to test performance. At the bottom right of the figure above we can see the bitrate of 949 Mbits/sec. This is very close, but just less than, a benchmark of 1Gbs for local network speeds between devices within the lab subnet. These speeds are adequate for our purposes.
