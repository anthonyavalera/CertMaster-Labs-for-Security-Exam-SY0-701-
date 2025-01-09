# APPLIED LAB: Using Network Sniffers

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.9 Given a scenario, use data sources to support an investigation.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Using Wireshark

In this exercise, you will use Wireshark to capture and examine network traffic.

Connect to the KALI virtual machine and sign in as root with the password Pa$$w0rd.

Launch Wireshark.

Expand this hint for guidance.
Select the Kali Applications icon. It is a blue square with a white stylized dragon, located to the far left on the Kali top taskbar.

Select the search field at the top of the expanded Applications menu.

In the search field, enter wireshark.

The Wireshark application should be displayed as the only result. Select Wireshark from the application list.

Maximize the Wireshark window and initiate network frame collection on the eth0 interface.

Expand this hint for guidance.
Select the Maximize button on the Wireshark window located to the far-right on the header. The icon is immediately to the left of the close X. It will look like a blank/black circle until your mouse cursor is over it, then it displays a white square.

Notice all of the available interfaces listed in Wireshark that could be the focus of a network traffic capture. Wait a few seconds and see activity on the EKG-like line presented for each listed interface. If you don’t already know which interface to select, you can use activity levels to help make that decision.

Locate and double-click the eth0 interface.

This will initiate the collection of network frames on that interface and open the primary Wireshark three-pane display.

While most of the discussion around network sniffing mentions packets, it is more accurate to discuss frames. However, you will see the term packet used often, even in the Wireshark interface. Network sniffing utilities, such as Wireshark, capture Ethernet frames. Once captured, the contents of the Ethernet frame header and payload can be examined. The payload of Ethernet is often IPv4 or IPv6. The network containers of IP are called packets (operating at OSI Layer 3, the Network layer), while the network containers of Ethernet are called frames (operating at OSI Layer 2, the Data Link layer). At OSI Layer 4, the Transport layer, two protocols are common: TCP and UDP. TCP’s network containers are called segments, while UDP’s network containers are called datagrams.

...less
Continue to capture traffic while visiting the website www.structureality.com using Firefox.

Expand this hint for guidance.
On the Kali top taskbar, select Firefox. The icon looks like an orange fox curled over a globe.

Enter www.structureality.com in the Firefox address bar.

Stop the network capture.

Expand this hint for guidance.
Switch back to Wireshark by selecting it from the Kali top taskbar. It should be presented as a tab with the green Wireshark shark fin logo (indicating an active capture is underway) and labeled Capture from eth0.

Select the Stop capturing packets icon on the Wireshark toolbar. This icon looks like a red square.

Select the first packet from the capture in the top Packet List pane.

By selecting a captured frame from the top window of Wireshark (i.e., the Packet List pane in the default layout), the middle (i.e., the Packet Details pane in the default layout) and bottom (i.e., the Packet Bytes pane in the default layout) windows are focused on that single frame. Notice the middle window allows you to expand and explore the headers of all of the protocols involved in this frame. For example, this could include Ethernet, IPv4, TCP, and HTTP. Notice the bottom window is a hexadecimal presentation of the raw data of the frame and an American Standard Code for Information Interchange (ASCII) interpretation of that data.

The middle pane, known as "Packet Details" in Wireshark, is used to expand and look at the header values of the captured frames. Any header element that is captured in plaintext can be reviewed here. Wireshark will add relative and relevant information to the header data. Any such added information will be contained in square brackets. For example, [Stream Index: 2] indicates that the selected frame is part of the second stream contained in the current capture. Such added data is not directly contained in the captured frames. Wireshark will also perform interpretations or provide details to help explain the values in header fields. These interpretations or explanations are contained in parentheses. For example: Flags: 0x012 (SYN, ACK), which is an explanation that the hex flag value of 0x012 represents the flags of synchronization and acknowledgment. Such interpretations help to clarify the meaning or purpose of the values contained in the captured frame.

...less
Use Wireshark to examine the captured frames and use a simple display filter to display the HTTP traffic collected and attempt to locate the initial request from Kali to www.structureality.com. The Kali virtual machine is using the IPv4 address of 10.1.16.66.

If you allow several minutes of time to expire between the initiation of the network capture, the accessing of the www.structureality.com URL, and then stopping the capture, the initial GET request packet could be deep in the capture and hard to find manually. If so, try to redo the steps faster: restart the capture by selecting the Start capturing packets icon (it looks like a blue shark fin) from the Wireshark tool, re-load the www.structureality.com page, then quickly return to Wireshark to stop the capture.

Expand this hint for guidance.
At the top of the Wireshark window, just below the toolbar, is the display filter field. In this field, enter http. Be sure to press Enter on your keyboard or select the Apply display filter button on the far right end of the field, which looks like an arrow.

This simple display filter will hide all of the captured frames except for those containing HTTP content. HTTP is an Application layer (OSI layer 7) protocol used by web services.

The captured frames being displayed will all have HTTP listed in the Protocol column. This column indicates the highest OSI level protocol discoverable in each frame.

Scroll to the top of the display filtered results to see the initial packets. The initial packets in a capture will have the lowest value in the No. column. The No. column is the relative frame number of the captured network container. The first frame captured is assigned 1, the second frame is assigned 2, etc.

The source address of the request is 10.1.16.66. There should be a few frames with that address listed in the Source column. The top-most frame with that IP address source is most likely the request packet.

Select the displayed frame which has a Source address of 10.1.16.66, a Protocol of HTTP, and an Info statement of GET / HTTP/1.1.

By selecting a captured frame from the top window of Wireshark, the middle and bottom windows are focused on that single frame.

Notice the middle window allows you to expand and explore the headers of all the protocols involved in this frame. In this example, this includes Ethernet II, Internet Protocol Version 4 (i.e., IPv4), Transmission Control Protocol (i.e., TCP), and Hypertext Transfer Protocol (i.e., HTTP).

Notice the bottom window is a hexadecimal presentation of the raw data of the frame and an ASCII interpretation of that data.

Look in the ASCII interpretation to see if you can recognize the URL requested. You should be able to find www.structureality.com in the 5th-8th line (those hex offsets labeled as 0040 – 0070), but it might be broken across two lines.

What is the IP address of the www.structureality.com site?

Use Wireshark to determine if any DNS communications were captured.

Expand this hint for guidance.
Delete the current display filter by selecting the Clear display filter button located at the far right of the display filter field. The button will be a dark grey X over a light gray square until your mouse cursor hovers over it, then it will turn red.

Select the display filter field, then enter dns. Be sure to press Enter or select the Apply display filter arrow.

Now, the display shows communications containing the DNS protocol.

Select the Clear display filter button.

Leave the Wireshark and Firefox windows open.

This exercise has shown you the basics of frame/packet capture, use of simple display filters, and examining the contents of captured data. This process can be enhanced using several of the features of Wireshark, including display filters, capture filters, following a TCP stream, and detailed packet analysis.

Check your work
Confirm that you captured network traffic with Wireshark.
Confirm that you used simple display filters to find content in the captured network traffic.

### Using Display Filters

In this exercise, you will use Wireshark's display filters to locate specific packets within a capture.

Connect to the KALI virtual machine and, if needed, sign in as root with the password Pa$$w0rd.

Using Wireshark, start a new capture, then visit dvwa.structureality.com, then stop the capture and look at the summary of captured traffic.

Expand this hint for guidance.
Start a new Wireshark capture by selecting the Start capturing packets button on the Wireshark toolbar. The button looks like a shark fin and is the leftmost button on the toolbar.

On the Unsaved packets… pop-up window, select Continue without Saving.

Switch to Firefox and visit dvwa.structureality.com.

Select the Stop capturing packets icon on the Wireshark toolbar. This icon looks like a red square.

Use a Wireshark display filter to display only captured frames that include the IPv4 address of 10.1.16.66 as a destination

Expand this hint for guidance.
Select the Apply a display filter field and type ip.

The final period is necessary.

Notice that a presentation of the sub-elements of the ip. filter are displayed in a drop-down window.

In the drop-down window, select ip.dst.

Continuing to type into the display filter field, type in from your keyboard ==10.1.16.66.

The display filter should now be ip.dst==10.1.16.66.

Press Enter on your keyboard or select the Apply display filter button on the far right end of the field, which looks like an arrow.

It is important to understand the comparison operators used in both display and capture filters. For an exact match, use a double equal sign (i.e., ==). For a not-equal comparison, use a bang (i.e., exclamation point) followed by an equals sign (i.e., !=). Other operators include: greater than (>), less than (<), greater than or equal to (>=), less than or equal to (<=), and contains (typically used to match a protocol, field, slice, or string).

...less
You should now see a display of the captured frames that include the IPv4 address of 10.1.16.66 as the destination in the header.

Create a new display filter that shows captured frames that have a TTL value below 128.

Expand this hint for guidance.
Remove the current display filter by selecting the Clear display filter button on the far right of the display filter field. This button looks like a capital X and turns red when your mouse cursor is over it.

Select the Apply a display filter field and type in ip.

The final period is necessary.

Notice that a presentation of the sub-elements of the ip filter are displayed in a drop-down window.

Continuing to type into the display filter field, type in from your keyboard t.

In the drop-down window, select ip.ttl.

Continuing to type into the display filter field, type in from your keyboard <128.

The display filter should now be ip.ttl<128.

Press Enter on your keyboard or select the Apply display filter button.

Now, the displayed captured frames are only those with a TTL value below 128.

Alter the display filter to include ARP frames.

Expand this hint for guidance.
Select the Apply a display filter field by clicking in the empty space in the field to the right of the current display filter.

Enter or arp to add to the display filter already present. Be sure to add a space before the word or.

The display filter should now be ip.ttl<128 or arp.

Press Enter on your keyboard or select the Apply display filter button.

Now, the displayed captured frames are those with an IP TTL value less than 128 or which are ARP communications.

If this display filter was defined as ip.ttl<128 and arp, then no packets would be displayed. Because ARP communications do not contain IP payloads, so, it is not possible for any frame to contain both an IP header (with a TTL value) and ARP at the same time. Therefore, the OR relation must be used in these types of conditions.

Create a new display filter to show frames that do not contain the IPv4 address of 10.1.16.66.

Expand this hint for guidance.
Select the Clear display filter button.

Select the Apply a display filter field.

Enter ip.addr!=10.1.16.66.

The results of this display filter are the captured frames that do not contain a source or destination address of 10.1.16.66.

The results could be empty. Meaning there were no captured packets without the IPv4 address of 10.1.16.66.

Use the Display Filter Expression syntax window to create a display filter to display only TCP packets with the FIN flag set.

Expand this hint for guidance.
Select the Clear display filter button.

The display filter express interface is accessed by selecting Analyze from the Wireshark menu, then selecting Display Filter Expression.

It can take up to 10 seconds for the Wireshark - Display Filter Expression window to appear.

Notice the massive list of protocols with expandable content listed in the Field Name area.

In the Search: field, enter tcp.

This search term will reduce the number of protocols in the Field Name area significantly, but you will still need to scroll to locate and then select TCP – Transmission Control Protocol.

Select the arrow to the left of the TCP entry to expand its contents.

Scroll down to locate and select tcp.flags.fin.

Verify that the Relation field has highlighted the double-equals relation (i.e., ==), the Value (Boolean) is set to 1, and the Predefined Values is set to Set.

At the bottom of the Display Filter Expression window is the constructed filter field. It should be displaying tcp.flags.fin == 1.

Select OK to insert the constructed filter into the display filter field.

Select Apply display filter.

The displayed frames should all have an Info statement that includes FIN, which indicates that the FIN flag is set or enabled in those captured frames.

Many of the displayed frames with a FIN flag set will also have other flags set as well, such as ACK. The display filter used displays frames with any presence of the condition, not the exclusive presence of the condition.

What is the IPv4 source address of the system that first used a FIN flag from the captured traffic?

There are a staggering number of possible filter field values to choose from. The more specific your protocol field value matches, the more precise your filtering results will be in gaining easy access to the portions of a communication relevant to your search parameters.

Modify the current display filter to remove frames that have the ACK flag set.

Expand this hint for guidance.
Select the display filter field and click the blank area to the right of the current filter to place the cursor at the end of the current filter.

Enter and. Note: there is a space before and after the word and.

Open the Display Filter Expression.

If you already know the structure of a protocol element, you can quickly locate it by entering it into the Search: field. Enter tcp.flags.ack in the Search: field.

Notice the Field Name is reduced to a small number of results. Double-click TCP to expand its contents.

Select tcp.flags.ack – Acknowledgment from the expanded contents of TCP (it should be the only item).

Look at the constructed filter. It should be tcp.flags.ack == 1.

Select Not Set in the Predefined Values area so the constructed filter reads tcp.flags.ack == 0.

Select OK.

The resultant display filter should be tcp.flags.fin == 1 and tcp.flags.ack == 0.

Select Apply display filter.

The results may be empty.

This will likely result in no packets being displayed, as it is not common to have a FIN flag without an ACK flag.

The Display Filter Expression tool is used to craft elements of complex filters, which are then added to any existing filter already defined in the display filter field.

Edit the display filter to display frames with FIN set but SYN not set.

Expand this hint for guidance.
Select the display filter field and click the blank area to the right of the current filter to place the cursor at the end of the current filter.

Use the arrow keys on the keyboard to position the cursor after the ack flag term.

Use backspace to delete the ack flag term, then type in syn.

The resultant display filter should be tcp.flags.fin == 1 and tcp.flags.syn == 0.

Select Apply display filter.

The displayed frames will be those with the FIN flag set but without the SYN flag set.

These tasks of creating and modifying display filters demonstrate that you can type in display filters manually, craft them using the Display Filter Expression tool, combine multiple conditions with logical expressions (i.e., AND and OR), and edit existing filters directly in the display filter field.

Select the Clear display filter button.

If the capture is still running, select the Stop capturing packets button.

Leave the Wireshark and Firefox windows open.

Check your work
Confirm that you used display filters.
Confirm that you altered display filters.
Confirm that you created complex display filters.

### Follow a TCP stream

In this exercise, you will use Wireshark's follow stream function to look at the packets composing a TCP and HTTP conversation.

Connect to the KALI virtual machine and, if needed, sign in as root with the password Pa$$w0rd.

Using Wireshark, continue with the traffic captured from the previous exercise.

Use a display filter to locate the first frame of the communication with dvwa.structureality.com, then access the follow TCP Stream analysis.

Expand this hint for guidance.
Remove any existing display filter from the Wireshark filter field by selecting the Clear display filter icon on the far right of the field.

In the display filter field, enter tcp contains "dvwa.structureality.com", then select Apply display filter.

Select the first displayed frame result.

Select Analyze from the menu, then select Follow, then select TCP Stream.

The Follow TCP Stream window opens.

Notice the presentation is of the TCP segment and its payload. However, the payload is usually compressed, so it is unreadable in this initial format/presentation.

Notice the presentation of the TCP segments is color coordinated with red for the client and blue for the server and presented in communication/chronological order.

Close the Follow TCP Stream window.

Open a Follow HTTP Stream from a frame containing the TCP request for plainenglish.co.uk.

Expand this hint for guidance.
Remove any existing display filter from the Wireshark filter field by selecting the Clear display filter icon on the far right of the field.

In the display filter field, enter http, then select Apply display filter.

Select the first displayed frame result.

Select Analyze from the menu, then select Follow, then select HTTP Stream.

The Follow HTTP Stream window opens.

Notice this display is similar to that of the Follow TCP Stream window. However, the compressed and/or encoded HTTP payload is now visible in ASCII/plaintext.

Attempt to locate in the HTTP Stream the segment from the web server which contains the HTML line of:

<h1>Welcome to Damn Vulnerable Web Application!</h1>
Expand this hint for guidance.
In the Find: field at the bottom of the Follow HTTP Stream window, enter vulnerable, then select Find Next.

The search should auto-scroll down to where the first keyword match occurs.

If the result does not match the HTML code of: <h1>Welcome to Damn Vulnerable Web Application!</h1>, then seledt Find Next again.

Repeat until you locate the specified HTML code line.

What color and from which side of the conversation is the HTML code line of: <h1>Welcome to Damn Vulnerable Web Application!</h1>?

client
blue
red
server
Close the Follow HTTP Stream window.

Close all windows.

The ability to use Follow HTTP Stream is limited by the use of encrypted HTTPS communications. If the captured frames of an HTTP session are encrypted by TLS, then the option to access the Follow HTTP Stream window is not available. You can still follow the TCP stream, as the TCP headers are still in plaintext when the payload of TCP is HTTPS (i.e., TLS-encrypted web traffic). However, if the TCP headers are encrypted, as would occur over most VPNs and wireless encryption, then following TCP Stream would not be an available option.

...less
Check your work
Confirm that you looked at a TCP stream.
Confirm that you searched for a string in a TCP stream.
Confirm that you looked at an HTTP stream.
Confirm that you searched for a string in an HTTP stream.
