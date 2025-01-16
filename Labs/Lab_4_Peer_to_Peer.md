# Lab4:  Peer-to-Peer Internet Sharing

## Aim:
Connect two computers in a peer-to-peer network and share the Internet connection between them in a lab environment.

---

## Materials Required:
1. **Two Computers**:
   - One as the host computer (sharing the Internet).
   - The other as the client computer (accessing the shared Internet).
2. **Ethernet Cable**:
   - Use a crossover Ethernet cable for direct connection.
   - Alternatively, modern computers with auto-sensing ports can use a regular Ethernet cable.
3. **Administrator Access**:
   - Ensure both computers have administrative rights.
4. **Active Internet Connection** on the Host Computer.
5. **Lab Report Template**:
   - A document to record observations, screenshots, and results.

---

## Steps for Setting Up Peer-to-Peer Internet Sharing:

### On the Host Computer (Internet Sharing):
1. **Log In as Administrator:**
   - Log in to the host computer with an account that has administrative privileges.

2. **Enable Internet Connection Sharing (ICS):**
   - Go to **Start** > **Control Panel** > **Network and Internet Connections**.
   - Click **Network Connections**.
   - Right-click the connection used to access the Internet (e.g., Wi-Fi or Ethernet) and select **Properties**.
   - Go to the **Advanced** tab.
   - Under **Internet Connection Sharing**, check the box: **Allow other network users to connect through this computer’s Internet connection**.
   - If a warning appears about setting the LAN adapter’s IP address to `192.168.0.1`, click **Yes**.

3. **Connect the Ethernet Cable:**
   - Plug one end of the Ethernet cable into the host computer’s LAN port and the other into the client computer’s LAN port.

4. **Verify IP Configuration on the Host:**
   - Go to **Start** > **Control Panel** > **Network and Internet Connections** > **Network Connections**.
   - Right-click **Local Area Connection**, select **Properties**, and ensure the following settings under **Internet Protocol (TCP/IP)**:
     - IP Address: `192.168.0.1`
     - Subnet Mask: `255.255.255.0`
     - Leave Gateway and DNS blank.

---

### On the Client Computer:
1. **Log In as Administrator:**
   - Log in to the client computer with an account that has administrative privileges.

2. **Configure the IP Address:**
   - Go to **Start** > **Control Panel** > **Network and Internet Connections** > **Network Connections**.
   - Right-click **Local Area Connection** and select **Properties**.
   - Select **Internet Protocol (TCP/IP)** and click **Properties**.
   - Set the following static IP configuration:
     - IP Address: `192.168.0.2`
     - Subnet Mask: `255.255.255.0`
     - Default Gateway: `192.168.0.1` (Host’s IP Address).
     - Preferred DNS Server: `192.168.0.1` (or the DNS provided by your ISP).
   - Click **OK** to save settings.

3. **Test the Connection:**
   - Open a browser and try to access any website to verify the Internet connection.
   - Alternatively, use the `ping` command to check connectivity with the host computer (e.g., `ping 192.168.0.1`).

---

## Testing and Validation:

1. **Ping Test:**
   - On the client computer, open Command Prompt (`cmd`) and type:
     - `ping 192.168.0.1` (host’s IP address).
   - Observe successful replies from the host computer.
   - Optionally, type `ping google.com` to confirm Internet access.

2. **Website Access:**
   - Open a browser on the client computer and access a website (e.g., `www.google.com`).
   - Capture a screenshot showing the website loaded successfully.

3. **IP Configuration Check:**
   - On both computers, run `ipconfig` in Command Prompt and capture screenshots showing:
     - Host’s IP as `192.168.0.1`.
     - Client’s IP as `192.168.0.2`.

4. **Shared Folder Test (Optional):**
   - Share a folder on the host computer and access it from the client computer to validate the peer-to-peer connection.

---

## Documentation and Reporting:
1. **Screenshots:**
   - Include screenshots of all configurations, tests, and results.
2. **Lab Report:**
   - Document the steps.
   - Provide proof of successful Internet sharing with evidence from testing.
3. **Attach in the Google Classroom**

---

## Troubleshooting:
- **If the Client Cannot Connect to the Internet:**
  1. Verify the Ethernet cable is securely connected.
  2. Recheck the IP configurations on both computers.
  3. Ensure Internet Connection Sharing is enabled on the host computer.
  4. Restart both computers and repeat the steps.

- **If Slow Connection or Packet Loss Occurs:**
  - Ensure no other devices are connected to the host’s Internet.
  - Check for outdated drivers on the network adapters.