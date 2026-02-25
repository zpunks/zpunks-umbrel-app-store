## Setting Up the Blockstream Satellite API Key

To ensure your Blockstream Satellite service works correctly, you need to set the `SATELLITE_API_KEY` environment variable with your API key. Follow these steps:

1. **Access Umbrel OS Command Line via SSH**:
   - Open a terminal on your local machine.
   - Connect to your Umbrel node using SSH:
     ```sh
     ssh umbrel@umbrel.local
     ```
   - You may need to replace `umbrel.local` with the actual IP address or hostname of your Umbrel node.

2. **Set the Environment Variable**:
   - Once connected, set the `SATELLITE_API_KEY` environment variable:
     ```sh
     export SATELLITE_API_KEY=your_actual_api_key
     ```

3. **Run Docker Compose**:
   - Navigate to the directory containing your `docker-compose.yml` file:
     ```sh
     cd /path/to/your/docker-compose-file
     ```
   - Start the services with Docker Compose:
     ```sh
     docker-compose up -d
     ```

Replace `your_actual_api_key` with your actual Blockstream Satellite API key.

## Example Docker Compose File

Ensure your `docker-compose.yml` file includes the following configuration:

```yaml
version: "3.7"

services:
  app_proxy:
    environment:
      APP_HOST: blockstream-satellite_server_1
      APP_PORT: 8080  # Ensure this matches the port used by Blockstream Satellite

  server:
    image: blockstream/satellite:latest
    user: "1000:1000"
    init: true
    volumes:
      - blocksat-cfg:/root/.blocksat/
    devices:
      - /dev/dvb/adapter0:/dev/dvb/adapter0
    network_mode: host
    cap_add:
      - NET_ADMIN
      - SYS_ADMIN
    environment:
      - SATELLITE_API_KEY=your_api_key  # Replace with your API key
      - BLOCKSAT_DVB_ADAPTER=/dev/dvb/adapter0

volumes:
  blocksat-cfg:
