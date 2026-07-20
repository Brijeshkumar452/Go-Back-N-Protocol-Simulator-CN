# Go-Back-N Protocol Simulator

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

An interactive, browser-based visualization of the **Go-Back-N (GBN) Automatic Repeat reQuest (ARQ) protocol**, built to show sliding-window flow control, cumulative acknowledgements, timeouts, and retransmissions actually happening — rather than just being described in a textbook.

Developed during the **PS-1 Internship at The LNM Institute of Information Technology (LNMIIT), Jaipur**.

🔗 **Live Demo:** *(add your GitHub Pages / hosted link here)*
📸 **Screenshot:** *(add a screenshot or GIF of the simulation here)*

---

## 📖 Overview

Go-Back-N is a sliding-window ARQ protocol used for reliable data transfer over unreliable channels. This project turns the algorithm into a live, clickable animation so that sender/receiver state, packet flight, acknowledgements, and timeout-driven retransmissions can be watched in real time.

The simulation lets you:
- Send new data packets from the sender within a fixed window size.
- Watch packets travel across an animated channel to the receiver.
- Watch the receiver send back cumulative ACKs.
- Deliberately **kill** (drop) a packet or ACK mid-flight to simulate loss.
- Observe the sender's timeout timer and the Go-Back-N retransmission behaviour when a loss occurs.
- Speed up, slow down, pause, and reset the simulation at any point.

---

## ✨ Features

| Feature | Description |
|---|---|
| **Sliding Window Sender** | Window size = 5; sender cannot transmit beyond `base + windowSize`. |
| **Cumulative Acknowledgement** | Receiver accepts only in-order packets and sends a cumulative ACK for the highest in-order packet received. |
| **Timeout & Retransmission** | A single timer per window; on timeout, **all** unacknowledged packets in the window are retransmitted — true Go-Back-N behaviour, not Selective Repeat. |
| **Packet Loss Simulation** | Pause the simulation, select any in-flight packet/ACK, and click "Kill Packet/Ack" to simulate loss on the channel. |
| **Speed Control** | Speed up (to 8×) or slow down (to 0.25×) to study fast or slow-motion behaviour. |
| **Live Event Log** | A running log of every sender-side (S) and receiver-side (R) event — sends, ACKs, discards, timeouts, retransmits. |
| **Colour-Coded Slots & Packets** | Distinct colours for unsent, sent, acknowledged, received, and lost packets, with a legend for quick reference. |
| **Canvas-Based Animation** | Packets are drawn and animated frame-by-frame on an HTML5 `<canvas>` using `requestAnimationFrame`. |

---

## 🛠️ Tech Stack

- **HTML5** — structure and semantic layout
- **CSS3** — styling, layout (flexbox), and the slot/legend UI
- **JavaScript (ES6+)** — all simulation logic, state management, and rendering
- **HTML5 Canvas API** — real-time packet animation

No frameworks, build tools, or external dependencies — it runs as a single self-contained HTML file.

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Brijeshkumar452/Go-Back-N-Protocol-Simulator-CN.git
cd Go-Back-N-Protocol-Simulator-CN
```

### 2. Open the Project

This project is a standalone HTML application. No installation, build tools, or external dependencies are required.

Open the `Go_Back_N_Protocol.html` file in any modern web browser, such as:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox
- Brave

You can either:
- Double-click the HTML file, or
- Drag and drop it into your browser, or
- Right-click the file and choose **Open with** → Your preferred browser.

### 3. Run the Simulation

- Configure the simulation parameters.
- Click **Start** to begin the simulation.
- Observe packet transmission, acknowledgements, sliding window movement, timeouts, and retransmissions in real time.

---

## 🎮 How to Use

| Control | Action |
|---|---|
| **Send New** | Sends the next data packet, if the window isn't full. |
| **Pause / Resume** | Freezes the animation so you can click and select an in-flight packet/ACK. |
| **Kill Packet/Ack** | Simulates loss of the currently selected packet/ACK (enabled only while paused, with something selected). |
| **Faster / Slower** | Adjusts simulation speed between 0.25× and 8×. |
| **Reset** | Clears all state and restarts the simulation from scratch. |

**Colour Legend**

| Colour | Meaning |
|---|---|
| 🟦 Light Blue | Packet in transit / sent |
| 🟥 Red | Packet received by receiver |
| 🟨 Yellow | ACK in transit |
| 🟦 Dark Blue | ACK received by sender |
| 🟩 Green | Currently selected packet/ACK |

---

## 🧠 How It Works (Under the Hood)

- **State (`mkWorld`)** — a single world object tracks `base`, `nextseqnum`, `expectedseqnum`, per-slot status arrays, in-flight packets, and per-slot timers.
- **Animation loop (`loop` → `tick`)** — driven by `requestAnimationFrame`; on every frame it advances in-flight packets, resolves arrivals, and counts down timers.
- **`onDataArrived`** — receiver logic: accepts only the exact expected sequence number, discards anything else, and re-sends the last cumulative ACK.
- **`onAckArrived`** — sender logic: on receiving a cumulative ACK, slides `base` forward and restarts the timer for the next unacknowledged packet.
- **`onTimeout`** — the heart of Go-Back-N: when the timer for the base packet expires, **every** unacknowledged packet in the current window is retransmitted.
- **Rendering (`refreshSlots`, `drawCanvas`)** — redraws the sender/receiver slot grids and animated packets on the canvas from the current world state every frame.

---

## 💪 Strengths

- **Correctly models true Go-Back-N behaviour** — a single cumulative timer and full-window retransmission on timeout, which is what differentiates GBN from Selective Repeat.
- **Fully interactive loss simulation** — unlike static diagrams, users can inject real packet/ACK loss and watch the protocol self-correct.
- **Zero dependencies** — a single HTML file with inline CSS/JS, so it's portable, easy to host, and easy to read end-to-end.
- **Clear visual feedback** — colour-coded states, a timer progress bar per slot, and a running text log make it easy to correlate what's on screen with the underlying protocol events.
- **Adjustable simulation speed** — useful both for quick demos (fast) and closely studying edge cases like timeouts (slow).

---

## ⚠️ Limitations

- **Fixed configuration** — window size (5), total sequence slots (20), and timer durations are hardcoded constants rather than user-configurable inputs.
- **Single sender–receiver pair** — no support for multiple simultaneous senders/receivers or a shared/contended channel.
- **No corruption simulation** — models packet *loss* but not bit-level *corruption* (e.g., checksum failures), which is also part of real ARQ protocols.
- **No Selective Repeat comparison mode** — since GBN and Selective Repeat are often taught together, a side-by-side comparison isn't currently available.
- **Not responsive on very small (mobile) screens** — the slot grid and canvas use fixed pixel widths, so layout can overflow on narrow viewports.
- **No persistence** — the log and simulation state reset on page refresh; there's no way to export or save a run.
- **Educational simplification** — real-world factors like variable RTT, bandwidth-delay product, and congestion control are intentionally left out to keep the focus on the core ARQ mechanism.

---

## 🔭 Future Improvements

- [ ] Make window size, sequence space, and timeout duration configurable via UI inputs.
- [ ] Add a Selective Repeat mode toggle for direct comparison with Go-Back-N.
- [ ] Add packet corruption (not just loss) as a second failure mode.
- [ ] Make the layout responsive for mobile/tablet screens.
- [ ] Add exportable session logs (CSV/JSON) for lab report use.
- [ ] Add unit tests for the core protocol logic (`onDataArrived`, `onAckArrived`, `onTimeout`).

---

## 📚 What I Learned

This project was built to strengthen my understanding of core Computer Networks concepts during my PS-1 internship, including:
- Sliding window flow control and how window size affects throughput.
- The difference between cumulative acknowledgement (GBN) and per-packet acknowledgement schemes.
- Timer-based retransmission strategies and their trade-offs.
- Translating a textbook state-machine algorithm into working, event-driven JavaScript.
- Building smooth, frame-based animations using the Canvas API and `requestAnimationFrame`.

---

## 🙌 Acknowledgements

Inspired by classic Go-Back-N protocol animations used in networking courses (e.g., the Pearson/Kurose & Ross companion animations), rebuilt from scratch as an original implementation for this project.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE). Feel free to fork, study, and build on it.

---

## 👤 Author

**Brijesh Kumar Koiri**
B.Tech Computer Science & Engineering (Artificial Intelligence)
JK Lakshmipat University
PS-1 Internship — LNMIIT, Jaipur
