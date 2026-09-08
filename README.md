# Snort Intrusion Protection System (IPS) Setup Guide
## Overview
This is a tutorial I wrote up about setting up Snort on a VirtualBox Ubuntu Machine. This is a slightly older tutorial of mine, but it has significantly better referencing and additional information as this was for my university coursework.

This Guide may be revised in the future to further improve it and bring it into line with my other guides. As this was originally made in Microsoft Word, there may be some stylistic differences as I have greater freedom with Markdown.

## What did I learn?
At the time, I had very little knowledge of how to use Ubuntu, this meant that I frequently made mistakes such trying to trying to `nano` directories (i.e, open them like a text file) instead of `cd`ing into them and frequently confusing `cat` and `nano`.

Over the course of making the tutorial, I gained confidence in my abilities to navigate through Linux and could confidently navigate through directories and edit files in the terminal without using a GUI.

Without further ado, you can find the guide below:

### What is Snort?
Snort is an Intrusion Prevention System (IPS) which has 3 primary uses: A packet sniffer (like tcpdump), a packet logger and an intrusion detection system (Snort, 2025a). This makes it a very versatile tool that can be used for large and small organisations alike (Cox and Gerg, 2004), making it accessible to businesses who have the relevant knowledge to deploy it.

For more information about what Snort is capable of, visit the link [here](https://www.geeksforgeeks.org/computer-networks/what-is-snort/)  (Gluttony777, 2022).

Snort has 2 “editions”, the Community Ruleset which is maintained by the community and is free to use, and the Subscriber Ruleset which maintained by CISCO itself (Snort, 2025a). In this tutorial, we will be using the community ruleset because it is free – and this allows us to try features and functions before deciding if it is the right solution for your needs.
For more information about the Snort Licenses, please visit [this link](https://www.snort.org/license)  (Snort, 2025b)

### Why should you use Snort?
Snort has a community version which can be used for free, this means that it can be tested for its feasibility before choosing whether to adopt it within a business – this can save money for your business as money is not wasted on an IPS that you may not want to use in the end. Furthermore, as it is maintained by a larger company (CISCO), so larger businesses can benefit from faster updates to rulesets by subscribing – as it is developed in house and is not made by the community.

### Prerequisites
- Virtualbox Ubuntu Installation
    - 8GB RAM (Recommended)
        - 4GB RAM (Minimum)
    - 4 CPU Cores
    - 25GB Storage

Further reading and documentation can be found on the [Snort Manuals](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/) (Snort, 2025c)

### 1) Ensure Everything is Up-to-date
Firstly, we want to make sure our software and packages are up to date. To do this, run the command ``sudo apt update && sudo apt upgrade``.
``sudo apt update`` Gets an updated record (catalog) of packages on your system, and makes you aware of whether they need to be updated/installed/removed
``sudo apt upgrade`` Acts on the record's instructions
The ``&&`` allows us to run two commands without having to type them out individually

When prompted, press `Y` and enter to continue.

An example can be found below (Figure 1):

![An example screenshot after running both bommands](/images/fig1.png)

### 2) Install and use ifconfig (for older systems)
NOTE: `net-tools (ifconfig)` is depreciated, and has been replaced by `iproute2`, seperate instructions for that can be found below `(b)`, but if you want to use `net-tools (ifconfig)` follow `(a)`

(a) Run the command `sudo apt install net-tools`

Once complete, run the command `ifconfig` to find your IP address, typically this is found next to the `enp0s3`. See Below (Figure 2):

![An example image of me running `ifconfig`](/images/fig2.png)

(b) `iproute2` should be installed by default, so simply run the command `ip addr`and you should get a result similar to the above

Now, note down the IP address of our machine. In my case, it would be `10.0.2.15`

NOTE: The IP address `127.0.0.1` is also for ourself, but is only for applications that need to callback. So you do not need to worry about this.

### 3) Install Snort
Now we want to install Snort. Run the command `sudo apt-get install snort -y`.
This command is similar to the one we used before for `net-tools`, but `-y` parameter says yes to the prompts  we are given automatically.

Now, you should get a prompt like shown below. You want to enter the IP Address you were given before, however the last decimal should be `0`, and then add `/24`. For example, my example from before would become `10.0.2.0/24`. See below (Figure 3)

__For further explanation of the /24, see the bottom of this instruction.__

![An example of me putting 10.0.2.0/24 when prompted](/images/fig3.png)

Now that we have given our network addresses, you should see the following output:

![The result after installing Snort](/images/fig4.png)

#### **Further explanation of the /24**
Firstly, we will need to understand what an IP Address looks like in binary, for example `255.255.255.255`.
| First 8 Bits | Second 8 Bits | Third 8 Bits |  Fourth 8 Bits | 
| ------------- | ------------- | ------------- | ------------- |
| `11111111` | `11111111` | `11111111` | `11111111` |

Each bit reads right to left, see below how to write `255` in binary:
| 128 | 64 | 32 |  16 | 8 | 4 | 2 |  1 | 
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| **1** | **1** | **1** | **1** | **1** | **1** | **1** | **1** |

Now, when we use a **subnet mask** (i.e /24). We say that the first 24 bits are used to identify the network.
Because the first 24 bits are used to identify the network, the last 8 bits can be used to find the specific device.

The last 8 Bits can add up to 256, so __in theory__ we can have up to 256 devices on a network. However, `x.x.x.1` is usually reserved for a router.

In human terms, we can break this down into Streets and Addresses: 
- For the example given `10.0.2.15` we can say: "I am on the street `10.0.2` and my address is `.15`
- What we are doing by putting `/24` in Snort, is saying "I want to read all mail from addresses `.0-.255` on street `10.0.2`






