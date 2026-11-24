# TurtleMesh Hub Edition

<p align="center"><img src="../../../../docs/logo.png" width="260" alt="TurtleMesh Hub Edition"></p>

Puts a **Meshtastic** radio on the **Hak5 LAN Turtle Hub**.

The Hub connects to a Meshtastic radio over Wi-Fi or USB and serves a web console for mesh chat, nodes, the map and radio settings. It watches the mesh and sends alerts over LoRa. With a TAK Server, it puts the mesh on the TAK map.

> Meshtastic® is a registered trademark of Meshtastic LLC. TurtleMesh is a community payload, not affiliated with Meshtastic and not supported by Hak5.

## Features

| Feature | What it does |
|---|---|
| **Radio** | Any Meshtastic radio over **Wi-Fi / Ethernet** (TCP 4403) or **USB serial** on the Hub's dynamic USB port. It reconnects on its own. |
| **Mesh chat** | Every channel, plus direct messages to any node, from the web console. Delivery status is shown for messages you send. |
| **Nodes** | Every node heard: name, hardware, role, battery, SNR, hops, last heard, and range and bearing from the Hub. Give nodes labels, **traceroute** them, or **request a position**. Download the packet log as CSV or pcap. |
| **Map** | Mesh nodes, TAK contacts and the Hub on the web map (Leaflet, built in). Tiles are cached on the Hub (USB storage when attached), and **Save this area offline** stores USGS maps for use without internet. |
| **Hub on the mesh** | Off until you pick a channel (Radio > *Hub position on the mesh*). Then the Hub's position goes out every `MESH_POS_INTERVAL` seconds (15 min), on a channel with its own key only. |
| **Radio settings** | Long and short name; channels (name, role, new random key, default key or no encryption) and the share link; LoRa, device, position, telemetry, network, Bluetooth, MQTT, display and power settings; restart. |
| **Mesh watch** | Learns which nodes belong for `MESH_LEARN_MIN` minutes (60), then raises **New mesh node** for any node not seen before. **Watch** a node to get **Node quiet** after `NODE_QUIET_MIN` minutes of silence (and **Node back**) and **Low battery** under `LOW_BATTERY` %. A quiet sensor node is critical. **Radio link lost / restored** too. |
| **Sensors** | Detection-sensor packets (a radio running Meshtastic's detection-sensor module, like a door contact or PIR) become alerts. Mark a node as a **Sensor** and its text messages become alerts too. |
| **Alerts over LoRa** | Off until you choose where they go (Alerts tab): a channel, or a DM to one node (`LORA_ALERT_TO`). A public channel works but gets a warning, since alerts can carry device names, IP and MAC addresses. Alerts look like `[TURTLE-MESH] WARN Node quiet: Gate 2 - Not heard on the mesh for 61m`. While **TurtleTAK** runs on the same Hub, its alerts (new devices, ARP spoofing, link tamper) go out too. |
| **DM commands** | Any node can direct-message the Hub's radio: `status`, `alerts`, `where`, `nodes`, `help`. Answers are short enough for LoRa. `MESH_COMMANDS_FROM` limits who can ask. |
| **Mesh ↔ TAK** | Optional. Mesh nodes show on TAK as markers. One mesh channel and TAK All Chat Rooms carry each other's messages, once you pick a channel with its own key (a public channel can't be bridged). Meshtastic **ATAK plugin** users (TAK packets on the mesh) show on the TAK Server with their callsign, team and role. Alerts go to TAK as markers and chat lines. TAK users can DM the Hub the same commands. |
| **MQTT proxy** | The Hub carries the radio's MQTT traffic to your own broker (`MQTT_HOST`), so a USB radio without Wi-Fi can still use MQTT. *Set up the radio for it* turns on the radio's MQTT client proxy. |
| **Alert settings** | Each alert type on or off; the lowest level for TAK markers, TAK chat lines and LoRa; and quiet hours (for example `22-06`). |
| **Web console** | Status, Map, Mesh Chat, Nodes, Alerts, Radio, TAK, Settings and Setup on port **8485**, on every interface: USB-C host, RJ45, Wi-Fi and VPN. Password protected. |

## Screenshots

Web console, with demo data.

| Status | Map |
|---|---|
| <img src="../../../../docs/web-status.jpg" width="420" alt="Status tab"> | <img src="../../../../docs/web-map.jpg" width="420" alt="Map tab"> |
| **Mesh Chat** | **Nodes** |
| <img src="../../../../docs/web-chat.jpg" width="420" alt="Mesh Chat tab"> | <img src="../../../../docs/web-nodes.jpg" width="420" alt="Nodes tab"> |
| **Alerts** | **Radio** |
| <img src="../../../../docs/web-alerts.jpg" width="420" alt="Alerts tab"> | <img src="../../../../docs/web-radio.jpg" width="420" alt="Radio tab"> |
| **TAK** | **Setup** |
| <img src="../../../../docs/web-tak.jpg" width="420" alt="TAK tab"> | <img src="../../../../docs/web-setup.jpg" width="420" alt="Setup tab"> |

## Setup

1. Copy `payload.sh` to the Hub as its payload (web UI editor, `scp`, or USB) and run it.
2. The first run creates `/root/turtlemesh/`. It posts the web console's **first password** as a Hub notification and writes it to `/root/turtlemesh/web-password.txt`. Change it in Setup, then delete that file.
3. Open `http://172.16.84.1:8485` from the USB-C host, or the Hub's RJ45, Wi-Fi or VPN address, and sign in.
4. **Radio** tab:
   - **Wi-Fi radio**: turn on Wi-Fi in the radio (Meshtastic app > Settings > Network), put it on the same network as the Hub, and enter its IP address.
   - **USB radio**: plug it into the Hub's **dynamic USB port** (the one nearest the USB-C cable) with a data cable. TurtleMesh claims and powers the port. Radios with native USB (ESP32-S3 boards like the Heltec Wireless Tracker, and nRF52 boards) work with the Hub's built-in `cdc-acm` driver and show up as `/dev/ttyACM0`. Radios with a CP210x or CH34x USB chip (Heltec V3, T-Beam and others) need a driver the stock Hub firmware may not have; use Wi-Fi for those. If a USB radio doesn't connect, the Radio card says why.
5. Optional:
   - **TAK** (Setup tab). With **TurtleTAK** on the same Hub and `TAK_FROM_TURTLETAK: 1` (the default), TurtleMesh uses TurtleTAK's server and certificates. Otherwise, connect the same way as ATAK: a **connection package**, the **certificates** (client and truststore, each with its own password), or **enroll** with a TAK username and password. No openssl needed.
   - The Hub's **location** (Setup, or click the map), for range and bearing and for its position on the mesh.
   - **Alerts over LoRa** (Alerts tab) and the **MQTT proxy** (Settings, then TAK > *Set up the radio for it*).
6. *Run TurtleMesh at boot* is on by default.

The Hub runs one payload at a time. TurtleMesh reads TurtleTAK's server settings whenever TurtleTAK is installed, but relays TurtleTAK's alerts over LoRa only while TurtleTAK is running. To run both, use [TurtleShell](https://github.com/SECT0101/TurtleShell): one payload that runs TurtleTAK and TurtleMesh together, shares the radio's GPS fix with TurtleTAK, and leaves the LED to TurtleTAK.

Running the payload again restarts the service with that version. The service and web page in `/root/turtlemesh/app/` are rewritten on every run, and your settings, certificates, node labels and chat history are kept.

## Location

In **Auto**, TurtleMesh uses the first of these that has a usable position:

1. **Radio GPS**: the radio's own GPS fix (a Heltec Wireless Tracker, T-Beam, RAK with GPS...), while it's newer than `RADIO_GPS_MAX_AGE` (30 min). The radio then sends its own position on the mesh, so TurtleMesh doesn't send a second one.
2. **USB GPS** on the Hub's dynamic USB port (`GPS_DEVICE`).
3. **My phone (ATAK)**: your ATAK position from the TAK Server (`MY_ATAK` callsign).
4. **My phone (mesh)**: a mesh node you pick (`MY_NODE`), like the radio paired to your phone. The Meshtastic app shares the phone's GPS through it, with no TAK Server needed.
5. A **saved site**, then a **manual** lat/lon (Setup, or click the web map).
6. **Network**: public IP location, city level only. TAK shows a 25 km circle.

Phone positions count while they're newer than `ANCHOR_MAX_AGE` (30 min). Pick a **primary source** in Setup > Location to put it first; the rest stay as fallback in the same order.

## Mesh channels and privacy

Channel 0 on most meshes is public: it uses no key, the default key or a well-known "simple" key, so anyone with Meshtastic can read it. TurtleMesh sends nothing on any channel until you choose one:

- **Alerts over LoRa**: off until you choose a channel or a node (Alerts tab). A public channel is allowed, with a warning.
- **TAK chat bridge** and **Hub position**: only channels with their own key. Public channels are shown but can't be picked.
- The Radio tab marks each channel's key as *public* or *private*. Use *new random key* there to make a private channel, then share it with the share link.

Updating from 1.0.0 turns off the old channel 0 defaults once and posts a Hub notification, so you can pick channels.

## LED

| LED | Meaning |
|---|---|
| Magenta solid | Starting |
| Yellow double | Connecting to the radio |
| Green single | Radio connected (and TAK, if set up) |
| Cyan single | Radio connected, TAK not connected |
| Red slow | No radio set up |
| Cyan inverse (60 s) | Warning alert |
| Red fast (60 s) | Critical alert |

Set `LED: 0` to keep the LED dark.

## Settings

Every setting is in `/root/turtlemesh/default.cfg` as `KEY: value`. Change them in the web console's **Settings** tab or over SSH. The most used ones:

| Key | Default | |
|---|---|---|
| `RADIO_CONN` | `wifi` | `wifi` or `usb` |
| `RADIO_HOST` / `RADIO_PORT` | / `4403` | Wi-Fi radio address |
| `RADIO_DEV` | `auto` | USB serial device |
| `CALLSIGN` | `TURTLE-MESH` | Name on TAK and in alerts |
| `LOC_MODE` | `auto` | Primary location source: `auto`, `radio`, `gps`, `atak`, `mesh`, `site`, `manual` or `ip` |
| `MY_ATAK` / `MY_NODE` | | Your ATAK callsign / your mesh node (`!a1b2c3d4`) for the phone sources |
| `MESH_POS_CHANNEL` / `MESH_POS_INTERVAL` | `-1` / `900` | Channel for the Hub's position (`-1` = off; channels with their own key only) and seconds between positions |
| `MESH_LEARN_MIN` | `60` | Minutes of learning before *New mesh node* alerts |
| `NODE_QUIET_MIN` / `LOW_BATTERY` | `60` / `20` | Watched node limits |
| `LORA_ALERTS` / `LORA_ALERT_MIN` | `0` / `warning` | Alerts over LoRa and the lowest level sent |
| `LORA_ALERT_CHANNEL` / `LORA_ALERT_TO` | / | Channel for alerts (none until you choose), or a `!node` id for DMs |
| `LORA_ALERT_SOURCES` | `turtletak,turtlemesh` | Whose alerts go out over LoRa |
| `MESH_COMMANDS_FROM` | | Blank = any node, or a list of `!node` ids |
| `TAK` / `TAK_FROM_TURTLETAK` | `1` / `1` | TAK on, and use TurtleTAK's server and certificates |
| `BRIDGE_NODES` / `BRIDGE_ATAK_PLUGIN` | `1` / `1` | Mesh nodes and ATAK plugin users on TAK |
| `BRIDGE_CHAT_CHANNEL` | `-1` | Mesh channel bridged to TAK All Chat Rooms (`-1` = off; channels with their own key only) |
| `MQTT_PROXY` / `MQTT_HOST` / `MQTT_PORT` | `0` / / `1883` | MQTT proxy to your broker (`MQTT_TLS`, `MQTT_USER`, `MQTT_PASSWORD`) |
| `ALERTS_ON` | all types | Alert types that are raised |
| `QUIET_HOURS` | | For example `22-06` |
| `MAP_SOURCE` / `TILE_CACHE_MB` | `usgs_topo` / `500` | Web map and its tile cache limit |
| `WEB_PORT` | `8485` | Web console port (TurtleTAK uses 8484; never 1471, the Hub's own UI) |

## Files

| Path | |
|---|---|
| `/root/turtlemesh/default.cfg` | Settings |
| `/root/turtlemesh/certs/` | TAK certificates and connection packages. Never share them. |
| `/root/turtlemesh/nodes.json` | Node labels, watch and sensor flags |
| `/root/turtlemesh/known_nodes.json` | Nodes that belong to the mesh |
| `/root/turtlemesh/mesh_chat.jsonl` | Mesh chat history |
| `/root/turtlemesh/tiles/` | Saved map tiles (`/usb/turtlemesh-tiles/` when USB storage is attached) |
| `/root/turtlemesh/logs/` | `turtlemesh.log` (also in the web console) and `console.log` |
| `/root/turtlemesh/app/` | The service and web page, rewritten on every run |

## Credits

The radio code (Meshtastic protocol, radio settings, packet log) comes from [PagerMesh](https://github.com/SECT0101/PagerMesh). The web map uses [Leaflet](https://leafletjs.com) (BSD-2-Clause), bundled in the payload. USGS map tiles are from The National Map.

## Requirements

- LAN Turtle Hub with Python 3. The Hub runs Bash and Python payloads.
- A Meshtastic radio with Wi-Fi or USB serial (built for a Heltec V3 over Wi-Fi).
- For TAK: a network path to your TAK Server. `openssl` is **not** needed.
