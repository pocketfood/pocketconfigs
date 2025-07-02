Sure! Here’s a step-by-step guide on how to generate OpenVPN client configurations once you have your server set up and the necessary certificates and keys prepared.

### Steps to Generate OpenVPN Client Configs

1. **SSH into Your OpenVPN Server:**
   Connect to your OpenVPN server via SSH.

   ```bash
   ssh username@your_server_ip
   ```

2. **Navigate to Easy-RSA Directory:**
   If you used Easy-RSA to set up your PKI (Public Key Infrastructure), navigate to its directory. This is often found in `/etc/openvpn/easy-rsa/`, but the path might vary depending on your setup.

   ```bash
   cd /etc/openvpn/easy-rsa/
   ```

3. **Source the Variables File:**
   Load the Easy-RSA variables file to ensure you're working with the right settings.

   ```bash
   source vars
   ```

4. **Build the Client Certificate and Key:**
   Use the Easy-RSA management commands to build the client certificate and  key. Replace `client_name` with your desired client name.

   ```bash
   ./build-key client_name
   ```

   You’ll be prompted for various input fields. You can just press Enter for defaults or fill them in as needed.

5. **Copy the Client Files:**
   After generating the client key and certificate, you need to copy these files, along with the CA certificate, to your local machine or the client device.

   The necessary files are typically found in the `keys` directory within the Easy-RSA directory:

   ```bash
   cp keys/client_name.crt keys/client_name.key keys/ca.crt /path/to/your/client/configs/
   ```

   Replace `/path/to/your/client/configs/` with the desired path.

6. **Create the Client Configuration File:**
   You can create a `.ovpn` file using a text editor on the server. Here is a sample configuration you can use:

   ```ini
   client
   dev tun
   proto udp
   remote your_server_ip your_server_port
   resolv-retry infinite
   nobind
   persist-key
   persist-tun
   ca ca.crt
   cert client_name.crt
   key client_name.key
   cipher AES-256-CBC
   comp-lzo
   verb 3
   ```

   Replace `your_server_ip` and `your_server_port` with your OpenVPN server's public IP address and port (usually `1194`), and ensure that the paths for `ca`, `cert`, and `key` are accurate.

7. **Transfer the Client Config:**
   Transfer the `.ovpn` file and related certificates/keys to your client device. You can use SCP, SFTP, or any file transfer method you're comfortable with.

   ```bash
   scp your_config.ovpn user@client_ip:/path/to/save/
   ```

8. **Connect Using OpenVPN Client:**
   On the client machine, use the OpenVPN client to connect:

   ```bash
   openvpn --config /path/to/your/client/config.ovpn
   ```

9. **Verify Connection:**
   Check the OpenVPN logs for any errors, and verify that the connection is established correctly.

### Additional Notes:
- Make sure you have the OpenVPN client installed on your client device.
- Depending on your network setup, you might need to adjust firewall rules on the server to allow incoming OpenVPN connections.
- If the client is a Windows machine, the OpenVPN GUI can be used to easily import the `.ovpn` file.

That’s it! You should now be able to connect to your OpenVPN server from your client device. If you have any issues or need further assistance, feel free to ask!
