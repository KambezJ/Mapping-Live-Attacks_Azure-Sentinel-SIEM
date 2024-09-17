<h1>Using a SIEM tool to map live cyber attacks</h1>

<h2>Description</h2>
In this lab we will map live cyber attacks using the Microsoft Sentinel SIEM tool by creating a VM honeypot, exposing it to the internet, and importing RDP failure logs into Microsoft's Log Analytics Workspace. The Powershell script in this repository parses out Windows Event Log information for failed RDP attacks and uses a third-party API to collect geographic information about the attackers' location. This consists of setting up Microsoft Sentinel (SIEM) and connecting it to a live virtual machine acting as a honeypot. We will observe live attacks (RDP Brute Force) coming in from around the world, and use a custom PowerShell script to look up the attackers' Geolocation data and plot them on a Microsoft Sentinel (SIEM) Map.


<h2>Tasks that will be performed</h2>

- <b>Use custom PowerShell script</b> to extract metadata from Windows Event Viewer to be forwarded to a third-party API to derive geolocation data.
- <b>Configure Log Analytics Workspace in Azure</b> to ingest custom logs containing geographic information (Latitude, Longitude, State/Province, and Country).
- <b>Configure Custom Fields in Log Analytics Workspace</b> to map geodata in Azure Sentinel.
- <b>Configure Azure Sentinel</b> (Microsoft's cloud SIEM) workbook to display global attack data (RDP brute force) on world map according to physical location and magnitude of attacks.



<h2>Environments, Languages and Utilities Used</h2>

- <b>Windows 11</b> (22H2)
- <b>PowerShell ISE:</b> Extract RDP failed logon logs from Windows Event Viewer.
- <b>Microsoft Sentinel:</b> Map the locations where cyber attacks are coming from.
- <b>Microsoft Log Analytics Workspace:</b> Import RDP failure logs from VM acting as a honeypot.
- <b>KQL:</b> Code used to extract desired RawData and map the data on a Microsoft Sentinel map.
- <b>Remote Desktop:</b> Access and work on the honeypot.
- <b>IPgeolocation.io:</b> IP Address to Geolocation conversion API.



<h2>Program walk-through:</h2>

<p align="center">
<b>Step 1: Set up a Virtual Machine and open traffic to all ports to expose it to the internet. <br/>
<img src="https://i.imgur.com/igCbH9R.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 2: Create a Log Analytics Workspace(LAW) to ingest logs from the Virtual Machine.  <br/>
<img src="https://i.imgur.com/tJwMZdY.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 3: Open Microsoft Defender, turn on "CSPM", turn on "Servers", and turn off "SQL servers on machines". <br/>
<img src="https://i.imgur.com/6dpl1zP.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 4: Navigate to the Data Collection tab for Microsoft Defender, and ensure that the "ALL EVENTS" option is selected for event and log collection.  <br/>
<img src="https://i.imgur.com/aMONENz.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 5: Navigate to Log Analytics Workspace(LAW), select the "virtual machines" tab, and connect it to the VM we made.  <br/>
<img src="https://i.imgur.com/P2TfFkO.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 6: Open Microsoft Sentinel (the SIEM tool) and connect it to the workspace we made.  <br/>
<img src="https://i.imgur.com/AlHiMsw.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 7: Open the Remote Desktop and connect to the Virtual Machine (VM) Public IP address.  <br/>
<img src="https://i.imgur.com/DHFJD7u.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 8: Turn off Windows Defender Firewall (for domain, private, and public profiles) on the remote desktop to allow response to ICMP echo requests (meaning people can discover it on the internet faster).  <br/>
<img src="https://i.imgur.com/plDX073.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 9: Ping the Virtual Machines IP address using the command line on your physical computer.  <br/>
<img src="https://i.imgur.com/jtI861P.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 10: Retrieve the API key from IPGeoLocation.io   <br/>
<img src="https://i.imgur.com/MKbUIKl.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 11: Write a custom script to Windows Powershell ISE (This script looks through the system event log and grabs IP address + Geodata for all failed RDP logons), and paste the custom API key that was retrieved in step 10.  <br/>
<img src="https://i.imgur.com/HFKQubO.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 12: Run the PowerShell script and watch as the honeypot is attacked by threat actors from all around the world.  <br/>
<img src="https://i.imgur.com/FQGt59P.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 13: Copy all log data from the failed_rdp folder on the VM (collected using the Powershell script).  <br/>
<img src="https://i.imgur.com/Mmx0nHG.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 14: Save the log data copied during the previous step onto our ACTUAL computer (not VM) using Notepad.  <br/>
<img src="https://i.imgur.com/TXIz0vt.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 15: Create a custom log on Log Analytics Workspace using the log data file saved in the previous step as a collection path.  <br/>
<img src="https://i.imgur.com/qBUE0P1.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 16: Wait about 30 minutes for log data to load in LAW and then insert custom code to extract latitude, longitude, Source host/IP address, destination host, username, country, province or state, and timestamp.  <br/>
<img src="https://i.imgur.com/P3HfbfH.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 17: Open the Sentinel dashboard to see if data is present. This dashboard shows us the alerts, security events, etc from the log data we uploaded.  <br/>
<img src="https://i.imgur.com/1lDhHtK.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 18: Create a new workbook in Sentinel and add a log query.  <br/>
<img src="https://i.imgur.com/8FNh519.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 19: Write KQL code to extract and plot all information onto the workbook map.  <br/>
<img src="https://i.imgur.com/a0aEnEV.png" height="80%" width="80%" alt="Program Walk-Through Steps"/>
<br />
<br />
<b>Step 20: Watch the map and see how geodata is mapped as logons are failed by threat actors around the world. In this case, we see activity from Russia and Mexico. These results are only after one hour.  <br/>
<img src="https://i.imgur.com/dqDZ0f7.png" height="80%" width="80%" alt="Results after 1 hour"/>
<br />
<b>These are the results after 6 hours.  <br/>
<img src="https://i.imgur.com/EBuk316.png" height="80%" width="80%" alt="Results after 6 hours"/>
<br />
<br />
<b>These are the results of a separate VM honeypot I created, after 24 hours of being live and exposed to attacks
<img src="https://i.imgur.com/VbRDyj2.png" height="80%" width="80%" alt="Results after 24 hours of a seperate experiment"/>
