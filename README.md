# TurtleMesh – Meshtastic payloads for the Hak5 [LAN Turtle](https://hak5.org/products/lan-turtle-hub)

<p align="center"><img src="docs/logo.png" width="260" alt="TurtleMesh Hub Edition: the turtle operator in Meshtastic green with the M-Powered badge"></p>

[![Buy Me a Coffee](https://img.shields.io/badge/Buy_me_a_coffee-support-FFDD00?logo=buymeacoffee&logoColor=black)](https://www.buymeacoffee.com/sect0101)

Community payloads that put a **Meshtastic** radio on the **Hak5 LAN Turtle** family. The LAN Turtle runs the radio, watches the mesh, sends alerts over LoRa, and can bridge the mesh to the **TAK** ecosystem (ATAK, WinTAK, iTAK, WebTAK, CloudTAK).

**Payloads here are written in Bash + Python for the LAN Turtle Hub. They are community payloads and are not supported or guaranteed by Hak5.** See [Legal](#legal).

Also see [TurtleTAK](https://github.com/SECT0101/TurtleTAK) for TAK on the same Hub, [TurtleShell](https://github.com/SECT0101/TurtleShell) to run both from one payload, and [PagerMesh](https://github.com/SECT0101/PagerMesh) for Meshtastic on the WiFi Pineapple Pager. The radio code comes from PagerMesh.

---

## Payloads

| Edition | Device | Version | Description |
|---|---|---|---|
| [TurtleMesh Hub Edition](payloads/library/general/TurtleMesh-Hub/) | LAN Turtle Hub | 1.0.2 | A Meshtastic radio over Wi-Fi or USB, with mesh chat, a node list and an offline map; radio settings and channels, traceroute and position requests; mesh watch alerts (new nodes, watched nodes going quiet, low battery, detection sensors); alerts over LoRa, including TurtleTAK's while it runs; direct-message commands from any node; an optional mesh ↔ TAK bridge, including Meshtastic ATAK plugin users; a private MQTT proxy to your own broker; location from the radio's GPS, a USB GPS, your phone, a site or the public IP; and a web console on port 8485. |

## Repository layout

```
payloads/
└── library/
    └── general/
        └── TurtleMesh-Hub/
            ├── payload.sh      # the whole payload: Bash for the Hub, with the Python service and web console inside
            └── README.md       # setup, features, location, LED, settings, files
docs/
├── logo.png                    # TurtleMesh Hub Edition
└── web-*.jpg                   # web console screenshots (demo data)
.github/
├── FUNDING.yml                 # Buy Me a Coffee (Sponsor button)
└── workflows/                  # shellcheck on every push; payload.sh attached to every release
```

Your settings, certificates, node labels and chat history live on the Hub under `/root/turtlemesh/`, outside the payload, so updating the payload never touches them.

## Requirements

- LAN Turtle Hub with Python 3
- A Meshtastic radio on the same network (Wi-Fi), or on the Hub's dynamic USB port (native-USB radios such as the Heltec Wireless Tracker)
- Optional: a TAK Server and a client certificate, or TurtleTAK on the same Hub

## Legal

Use these payloads only on networks and radio channels you own or are authorized to use, and follow your local radio rules. You are responsible for complying with all applicable laws and policies. Meshtastic® is a registered trademark of Meshtastic LLC. This project is not affiliated with or endorsed by Meshtastic, Hak5 or the TAK Product Center.

## License

[GPL-3.0](LICENSE). Leaflet is bundled under BSD-2-Clause; see [THIRD-PARTY-LICENSES.txt](THIRD-PARTY-LICENSES.txt).
