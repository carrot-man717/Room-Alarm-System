# 🚨 Hardware-Triggered Room Security System

A responsive, local-running physical security alarm system built with an **Arduino Uno Rev3** and a custom **HTML5/Web Serial API** frontend dashboard. Designed to guard personal spaces and alert room owners of unauthorized entry using high-decibel audio looping networks and full-screen visual strobe arrays.

---

## ⚡ Features

* **Physical Tripwire Integration:** Communicates natively with an Arduino Uno door sensor via standard USB using the modern Google Chrome Web Serial API.
* **Intelligent Looping Lifecycle:** Automatically plays a warning track (`start_sound.wav`) and loops it exactly 3 times before shifting states.
* **3-Strike PIN Lockout:** Features an ATM-style masked password text box (`9023`). If an incorrect combination is typed 3 times, countdowns are bypassed and defensive chaos mode triggers instantly.
* **Lockdown Chaos Mode:** Simultaneously launches 7 separate audio tracks at maximum volume while changing the cursor properties to invisible and running a high-intensity full-screen red/blue strobe sequence.
* **Fail-Safe Secret Shortcut:** Implements a global hardware key event tracker. Smashed key combinations (`Ctrl + S + A + F + E`) will override the layout and instantly kill all sounds *only* when chaos mode is active.
* **10-Second Auto-Reset:** Automatically sanitizes variables, flushes background audio cache memories, clears terminal states, and arms the perimeter back to default 10 seconds post-deactivation.

---

## 🛠️ Hardware Requirements

1. **Arduino Uno Rev3** Microcontroller (Detected on port identifier `0043`)
2. **Magnetic Door Reed Switch** (or standard momentary push button)
3. Connecting jumper wires
4. A computer running Google Chrome (Fully compatible with Chromebooks!)

---

## 🔌 Circuit Assembly

Because the Arduino sketch utilizes the internal microcontroller pull-up network, wiring requires no complex external resistance components:

* Connect **Terminal A** of your door switch/button to Arduino **Digital Pin 7**.
* Connect **Terminal B** of your door switch/button to any available Arduino **GND** (Ground) pin.

---

## 🚀 Deployment Instructions

### 1. Program the Arduino
Upload the following control loop sketch to your Arduino board using the Arduino Web Editor or IDE:

```cpp
const int BUTTON_PIN = 7;
int lastButtonState = HIGH; 

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  Serial.begin(9600); 
}

void loop() {
  int currentButtonState = digitalRead(BUTTON_PIN);

  if (lastButtonState == HIGH && currentButtonState == LOW) {
    Serial.println("DOOR_OPENED"); 
    delay(300); 
  }
  lastButtonState = currentButtonState;
}
```
*Note: Make sure to **close or disconnect** the Serial Monitor tab inside your compiler once uploaded so it doesn't block incoming browser requests.*

### 2. Configure Local Directory Files
Ensure your project folder contains your compiled `.html` script alongside your assets named exactly as follows:
```text
your-project-folder/
│
├── index.html
├── start_sound.wav
├── sound_1.mp3
├── sound_2.mp3
├── sound_3.wav
├── sound_4.wav
├── sound_5.mp3
├── sound_6.mp3
└── sound_7.mp3
```

### 3. Run the Alarm System
1. Open `index.html` inside a **Google Chrome** browser tab.
2. Click the prominent **CONNECT DOOR SENSOR** button.
3. Select your connected Arduino device from the browser hardware overlay popup.
4. Your terminal will display `SECURITY SYSTEM ARMED` in green. Open your physical door to trip the sensor and begin deployment!

### NOTE ###

1. The software requires Google Chrome. This has not been tested on Safari, Edge, Firefox, or Opera and other browsers.

2. MIT License. Copyright (c) 2026 Ransom Turner. 
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.