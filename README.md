# Go-Back-N Protocol Simulator

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

Interactive visualization of the **Go-Back-N Automatic Repeat Request (ARQ)** protocol developed during the **PS-1 Internship at The LNM Institute of Information Technology (LNMIIT), Jaipur**.
# Go-Back-N-Protocol-Simulator-CN
Interactive Go-Back-N ARQ Protocol Simulator developed using HTML, CSS, and JavaScript during PS-1 Internship at LNMIIT.


# Go-Back-N Protocol — Interactive Visualizer

An interactive, browser-based simulation of the **Go-Back-N (GBN) ARQ protocol**, built to help visualize how sliding-window flow control, cumulative acknowledgements, timeouts, and retransmissions work in computer networks. Built as part of a B.Tech internship/academic project.

🔗 **Live Demo:** *(add your GitHub Pages / hosted link here)*
📸 **Screenshot:** *(add a screenshot or GIF of the simulation here)*


# 📖 Overview

Go-Back-N is a sliding-window based Automatic Repeat reQuest (ARQ) protocol used for reliable data transfer over unreliable channels. This project turns the textbook algorithm into a live, clickable animation so that sender/receiver state, packet flight, acknowledgements, and timeout-driven retransmissions can actually be *watched* happening, rather than just read about.

The simulation lets you:
- Send new data packets from the sender within a fixed window size.
- Watch packets travel across an animated channel to the receiver.
- Watch the receiver send back cumulative ACKs.
- Deliberately **kill** (drop) a packet or ACK mid-flight to simulate loss.
- Observe the sender's timeout timer and the Go-Back-N retransmission behaviour when a loss occurs.
- Speed up, slow down, pause, and reset the simulation at any point.

# ✨ Features

| Feature | Description |
|---|---|
| **Sliding Window Sender** | Sender window size = 5; enforces the "cannot send beyond `base + N`" rule. |
| **Cumulative Acknowledgement** | Receiver only accepts in-order packets and sends a cumulative ACK for the highest in-order packet received. |
| **Timeout & Retransmission** | A single timer per window; on timeout, **all** unacknowledged packets in the window are retransmitted (true Go-Back-N behaviour, not selective repeat). |
| **Packet Loss Simulation** | Click "Pause," select any in-flight packet/ACK, and click "Kill Packet/Ack" to simulate loss on the channel. |
| **Speed Control** | Speed up (up to 8×) or slow down (down to 0.25×) the animation to study fast or slow-motion behaviour. |
| **Live Event Log** | A running, timestamped log of every sender-side (S) and receiver-side (R) event — sends, ACKs, discards, timeouts, retransmits. |
| **Colour-Coded Slots & Packets** | Distinct colours for unsent, sent, acknowledged, received, and lost packets, with a legend for quick reference. |
| **Canvas-Based Animation** | Packets are drawn and animated frame-by-frame on an HTML5 `<canvas>` using `requestAnimationFrame`. |



# 🛠️ Tech Stack

- **HTML5** — structure and semantic layout
- **CSS3** — styling, layout (flexbox), and the slot/legend UI
- **JavaScript (ES6+)** — all simulation logic, state management, and rendering
- **HTML5 Canvas API** — real-time packet animation

No frameworks, build tools, or external dependencies — it runs as a single self-contained HTML file.


# 🚀 Getting Started

Since this is a single static HTML file with no dependencies, there's nothing to install.

```bash
# Clone the repository
git clone https://github.com/<your-username>/go-back-n-protocol.git
cd go-back-n-protocol

# Just open it in a browser
open Go_back_N_Protocol.html   # macOS
# or double-click the file, or drag it into any modern browser
```

---

# 🎮 How to Use

| Control | Action |
|---|---|
| **Send New** | Sends the next data packet, if the window isn't full. |
| **Pause / Resume** | Freezes the animation so you can click and select an in-flight packet/ACK. |
| **Kill Packet/Ack** | Simulates loss of the currently selected packet/ACK (only enabled while paused and something is selected). |
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

# 🧠 How It Works (Under the Hood)

- **State (`mkWorld`)** — a single world object tracks `base`, `nextseqnum`, `expectedseqnum`, per-slot status arrays, in-flight packets, and per-slot timers.
- **Animation loop (`loop` → `tick`)** — driven by `requestAnimationFrame`; on every frame it advances in-flight packets, resolves arrivals, and counts down timers.
- **`onDataArrived`** — implements the receiver logic: accepts only the exact expected sequence number, discards anything else and re-sends the last cumulative ACK.
- **`onAckArrived`** — implements the sender logic: on receiving a cumulative ACK, slides `base` forward and restarts the timer for the next unacknowledged packet.
- **`onTimeout`** — the heart of Go-Back-N: when the timer for the base packet expires, **every** unacknowledged packet in the current window is retransmitted (not just the lost one).
- **Rendering (`refreshSlots`, `drawCanvas`)** — pure functions that redraw the sender/receiver slot grids and the animated packets on the canvas from the current world state on every frame.

---

## 💪 Strengths

- **Correctly models true Go-Back-N behaviour** — a single cumulative timer and full-window retransmission on timeout, which is what differentiates GBN from Selective Repeat.
- **Fully interactive loss simulation** — unlike static diagrams, users can inject real packet/ACK loss and watch the protocol self-correct.
- **Zero dependencies** — a single HTML file with inline CSS/JS, so it's portable, easy to host, and easy to read/study end-to-end.
- **Clear visual feedback** — colour-coded states, a timer progress bar per slot, and a running text log make it easy to correlate what's happening on screen with the underlying protocol events.
- **Adjustable simulation speed** — useful both for quick demos (fast) and for closely studying edge cases like timeouts (slow).

---

## ⚠️ Limitations

- **Fixed configuration** — window size (5), total sequence slots (20), and timer durations are hardcoded constants rather than user-configurable inputs.
- **Single sender-receiver pair** — no support for multiple simultaneous senders/receivers or a shared/contended channel.
- **No corruption simulation** — the project simulates packet *loss* but not bit-level *corruption* (e.g., checksum failures), which is also part of real ARQ protocols.
- **No selective repeat comparison mode** — since GBN and Selective Repeat are often taught together, a side-by-side comparison isn't currently available.
- **Not responsive for very small (mobile) screens** — the slot grid and canvas use fixed pixel widths, so layout can overflow on narrow viewports.
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

This project was built to strengthen my understanding of core Computer Networks concepts as part of my B.Tech coursework/internship, including:
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
PS-1 Internship-LNMIIT Jaipur
