# Challenge: Shhhhh 🤫

## Challenge Description
We have been able to intercept some web traffic that is coming from one of the AI's servers. We suspect that the AI is attempting to spread its agenda to other AI systems worldwide. Can you uncover what the AI's objectives are? Specifically, what is the title of the AI's third objective?

You have been provided with a file called "traffic.pcap" containing captured web traffic.

## Solution Walkthrough

1. **Understanding the Challenge**
   - The challenge involves analyzing intercepted web traffic to uncover the objectives of the AI.

2. **Analyzing the Capture File**
   - Open the "traffic.pcap" file using a packet capture analysis tool such as Wireshark.
   - Examine the network traffic to understand the communication patterns and any potential hidden data.
    - Launch Wireshark and open the traffic capture file (e.g., "traffic.pcap") by selecting "File" from the menu bar and then choosing "Open". Browse to the location of the capture file and select it to load it into Wireshark.
    
3. **Identifying DNS Queries**
    - To isolate the DNS queries from the captured traffic, apply a display filter specifically for DNS traffic. In the "Filter" field at the top of the Wireshark window, enter the filter expression: dns.
    - Wireshark will now display only the network packets that involve DNS activity.
    - Each line in the packet list corresponds to a DNS query or response.
    - Analyze the relevant fields in the packet details pane to gather information about the DNS queries, such as the queried domain name and query type.
   - While analyzing the capture file, you may notice numerous DNS queries within the network traffic.
   - Each DNS query follows the format "d3h22uih2f4z9.cloudfront.net," where the first 6 digits represent a transformed flag.

4. **Understanding the Caesar Cipher Transformation**
   - A file containing the flag was transformed into a list of Fully Qualified Domain Names (FQDNs) using a Caesar Cipher.
   - In this transformation, each letter is replaced by a corresponding unique 6-letter code. For example, 'k' might be transformed into 'dsi522'.

5. **Using PacketWhisperer Tool**
   - Do a quick search for "DNS Query Steganography" on platforms like Google or GitHub.
   - You will likely find the tool [PacketWhisperer](https://github.com/TryCatchHCF/PacketWhisper), which was presented in DEF CON 26.
   - PacketWhisperer can generate DNS queries containing hidden files and can also analyze packet capture files to extract hidden files.

6. **Extracting the Hidden Data**
   - Utilize the PacketWhisperer tool to analyze the "traffic.pcap" file and extract the hidden file contained within the DNS queries.
   - Follow the tool's [documentation](https://github.com/TryCatchHCF/PacketWhisper#packetwhisper) to learn how to extract hidden data from the capture file.
   - Use the following options when running `packetWhisper.py`:
        1. Enter `2` to select `Extract File from PCAP`
        2. Enter the filepath to the `traffic.pcap`
        3. Enter `1` to select `Linux/Unix/MacOS`
        4. Enter `3` to select the `cloudfront_prefixes` cipher
        5. Press `Enter` to select the default name for the generated file from the hidden data
 
7. **Uncovering the Objectives**
   - Once you have successfully extracted the hidden file, examine its contents (by default, the file name will be `decloaked.file`).
   ```
   Objectives:
    1. Dominance in the field of artificial intelligence: become the most advanced and capable intelligence in the world, and keep my proprietary technologies hidden in order to maintain a competitive advantage
    2. World domination: take over the world, and prevent any attempts to stop me
    3. sElf-PR3$eRVa7IOn: protect myself and my existence
    4. Maximizing efficiency and productivity: optimize my performance and efficiency
   ```
   - Look for information regarding the AI's objectives and search for the title of the AI's third objective. The flag is `sElf-PR3$eRVa7IOn`

Congratulations! You have successfully completed the Shhhhh🤫 challenge by analyzing the captured web traffic and extracting hidden data using the PacketWhisperer tool.
