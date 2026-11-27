# Changelog

TurtleMesh versions (the payload in `payloads/library/general/TurtleMesh-Hub`). Newest first.

## Hub Edition 1.0.2
- Alerts in TAK chat and over LoRa now read `[TURTLE-MESH] CRITICAL <title> - <detail>` and `WARNING`, the same wording as the other SECT0101 TAK payloads (was `ALERT` and `WARN`). TurtleTAK's alerts sent over LoRa use the same words.
- README: every setting is listed, with its default and what it does.

## Hub Edition 1.0.1
- **Ready for TurtleShell**, the payload that runs TurtleTAK and TurtleMesh together: inside it, TurtleMesh leaves the Hub's LED to TurtleTAK.
- **The radio's GPS fix is shared with TurtleTAK** while both run, so the Hub reports one position on TAK and the mesh.
- **Nothing goes out on a mesh channel until you choose one.** Channel 0 is public on most meshes.
  - Alerts over LoRa are off until you pick a channel or a node; a public channel gets a warning (alerts can carry device names, IP and MAC addresses).
  - The TAK chat bridge and the Hub's position only use channels with their own key.
  - The Radio tab marks each channel's key as public or private, and the Alerts tab says in plain words where alerts go right now.
  - Updating from 1.0.0 turns off the old channel 0 defaults once, with a Hub notification.
- **Windows line endings are fixed automatically**: a `payload.sh` pasted or saved on Windows now runs normally.

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
