# Changelog

TurtleMesh versions. Newest first.

## Hub Edition 1.0.0
- First release for the LAN Turtle Hub, as one `payload.sh`: Bash for the Hub commands, with a Python 3 service and a web page written from it.
- A Meshtastic radio over Wi-Fi / Ethernet (TCP 4403) or USB serial on the dynamic USB port, reconnecting on its own.
- Mesh chat on every channel and by direct message, with delivery status.
- Node list with labels, battery, SNR, hops, range and bearing; traceroute and position requests; packet log as CSV or pcap.
- Web map with mesh nodes, TAK contacts and the Hub, and offline USGS tiles.
- The Hub's position on the mesh, from the first usable of: the radio's own GPS, a USB GPS, your phone (ATAK or a mesh node), a saved site, a manual position or the public IP. A primary source can be picked.
- Radio settings: names, channels and the share link, LoRa / device / position / telemetry / network / Bluetooth / MQTT / display / power settings, restart.
- Mesh watch alerts: new mesh node after a learning window, watched node quiet / back, low battery, radio link lost / restored.
- Detection-sensor packets, and texts from nodes marked as sensors, raise alerts.
- Alerts over LoRa on a channel or as a DM, including TurtleTAK's alerts while it runs.
- DM commands from any node (and from TAK): `status`, `alerts`, `where`, `nodes`, `help`.
- Optional mesh ↔ TAK bridge: nodes as markers, a mesh channel ↔ TAK All Chat Rooms, Meshtastic ATAK plugin users on the TAK Server, alerts as markers and chat.
- TAK setup like ATAK (connection package, certificates with their own passwords, or enrollment), no openssl needed, or TurtleTAK's server and certificates.
- MQTT proxy from the radio to your own broker.
- Alert settings: each type on or off, the lowest level for markers, chat and LoRa, and quiet hours.
- Web console on port 8485, on every interface.
