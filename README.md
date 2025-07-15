# SmartSchool-InternetOfThings
SmartSchool IoT is a classroom-scale prototype that connects Arduino sensor nodes and actuators to a Node.js MQTT hub and a PostgreSQL web dashboard for real-time gas-sensor logging, complete with Cisco Packet Tracer and Tinkercad simulations.

> *End‑to‑end smart‑classroom prototype* – collect sensor data with Arduino, upload logs to a Node.js/Express dashboard backed by PostgreSQL, and validate the network flow with Cisco Packet Tracer & Tinkercad simulations.

---

## ✨  Key Features

| Area                                                                              | What it does                                                                                                                                                                                                    |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Gas Safety*                                                                    | Upload .txt files produced by an MQ‑2 gas sensor; the server parses lines like Gas sensor = 437, stores each value with a timestamp in PostgreSQL and shows them in a sortable table. fileciteturn3file6 |
| *Live Dashboard*                                                                | EJS views + vanilla CSS served by Express at http://localhost:3000 display the latest readings and upload status messages. fileciteturn4file68                                                             |
| *Database Layer*                                                                | Automatic CREATE TABLE IF NOT EXISTS gas_readings (…) on first run; all inserts wrapped in BEGIN … COMMIT for atomicity.                                                                                    |
| *Simulations*                                                                   | ‑ *Cisco Packet Tracer*: full network topology (Wi‑Fi AP, gateway, classroom LAN).                                                                                                                            |
| ‑ *Tinkercad*: micro‑level wiring for Arduino Uno + MQ‑2 sensor + LCD + buzzer. |                                                                                                                                                                                                                 |
| *Extensibility*                                                                 | Codebase leaves room for MQTT ingestion, additional sensors (temp, PIR, light) and real‑time charts via WebSockets.                                                                                             |

---

## 🗂  Repository Structure


📦 smartschool-iot
 ├── server/               # Express + PostgreSQL code
 │   ├── app.js            # (the file you see below)
 │   ├── views/            #   └── index.ejs
 │   └── public/           # CSS & JS assets
 ├── pkt/                  # Cisco Packet Tracer project (.pkt)
 ├── tinkercad/            # Links or .csv exports for circuits
 ├── arduino/              # *.ino sketches that generate log files
 └── README.md             # (this file)


---

## 🏗️  How It Works (architecture in words)

1. *Arduino Edge Node* collects analogue readings from an MQ‑2 gas sensor, timestamps them and writes them to an SD card or sends them over Serial.
2. The *log file* (sensor_log_YYYYMMDD.txt) is downloaded and uploaded to the *Express web app* via the dashboard form.
3. The *server* (Node.js 20 +) uses *Multer* to store the upload temporarily, extracts every Gas sensor = NNN line with a regex and bulk‑inserts the numbers into *PostgreSQL*.
4. After commit, the temp file is deleted and the browser is redirected with a success or error flash message.
5. The *index page* re‑queries gas_readings and renders a table with EJS every time / is hit.
6. Network behaviour (DNS, DHCP, NAT) and device connectivity were first validated in *Cisco Packet Tracer, while sensor wiring was prototyped in **Tinkercad Circuits*.

---

## 🛠️  Tech Stack

| Layer           | Tech                                      |
| --------------- | ----------------------------------------- |
| MCU & Firmware  | Arduino C/C++ (Uno)                       |
| Backend         | Node.js 20, Express 4, Multer 1           |
| Database        | PostgreSQL 15, pg node driver           |
| Front‑end       | EJS templates + vanilla CSS               |
| Simulators      | Cisco Packet Tracer 8, Autodesk Tinkercad |
| Version Control | Git / GitHub                              |

---

## 🚀  Quick Start

bash
# Clone & install deps
   git clone https://github.com/<your‑user>/smartschool‑iot.git
   cd smartschool‑iot/server
   npm install

# Configure DB (edit .env or hard‑code pool settings)
   createdb iot

# Run the app
   node app.js
   # → visit http://localhost:3000


*Default credentials* are set to user=postgres  password=diva  database=iot. Change them in app.js or via environment variables before deploying.

---

## 🧩  Extending the Project

* *Real‑time MQTT ingest* – subscribe to a topic and write to DB on the fly.
* *WebSocket chart* – stream new readings without refreshing the page.
* *Additional sensors* – create new tables (temp_readings, pir_events) and reuse the existing upload‑and‑parse pattern.

---

## 📜  License

MIT  – see LICENSE file.

## 🙏  Acknowledgements

Course Internet of Things, Faculty XYZ. Hardware kits sponsored by Lab 4. Cisco Packet Tracer tutorials by PacketCoder.
