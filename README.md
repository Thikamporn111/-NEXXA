# NEXXA

A 3D simulator for **Nexxa**, an ESP32 line-following robot that has a phone screen as its face. You can tell it to fetch items by typing or by talking to the AI, and you can also ask the AI general questions.

## Features

- **3D model**: a two-layer acrylic chassis, ESP32-WROOM, L298N motor driver, 5-channel IR sensors, an ultrasonic sensor, a short servo arm with a gripper (right side), and a rear cargo tray. A phone mounted in the center is the face, with eyes that blink and look in the direction it is turning.
- **Line-following track**: black tape with junctions J0–J5, item shelves at A, B, C and D, a DROP delivery point, and HOME as a parking spot to wait for charging (no dock).
- **Telemetry and Serial Monitor**: IR sensor values, left/right PWM, battery, plus a log like the one an ESP32 would print over serial.
- **AI chat as a phone app**:
  - type commands, or **tap the microphone and speak in Thai**
  - replies can be read aloud
  - the AI plans the commands GOTO / PICK / PLACE / STOP and the robot carries them out
  - it also answers general questions, such as science, homework, or ESP32 code

## How to run

The whole project is a single file, `index.html`, with no build step.

- **Run on your own machine**: you need a `localhost` address for the microphone to work.
  ```bash
  npx serve .
  # or
  python3 -m http.server 8000
  ```
  Then open `http://localhost:8000` in **Chrome or Edge**.
- **GitHub Pages**: go to Settings → Pages → Deploy from branch → `main` / root.
  - On a free GitHub account, Pages only works for **public** repos. If the repo is private, use localhost instead.

## Connecting the AI

The page picks the AI in this order:

1. **On claude.ai** (opened as an Artifact): it uses Claude automatically.
2. **On other sites**: tap the **AI** button in the phone app and paste an Anthropic API key, created at [console.anthropic.com](https://console.anthropic.com).
   - The key is stored only in that browser's `localStorage`. It is never written into the code or sent to the repo.
   - The default model is `claude-haiku-5-5`. You can change it on the same screen.
   - The key is called directly from the browser, so only use it on your own machine. Never put the key in code you share.
3. **No AI connected**: it uses a built-in command interpreter, which understands only robot commands such as "ไปหยิบขวดน้ำที่ B มาส่งที่ DROP" (fetch the water bottle from B and deliver it to DROP).

## Voice commands

- Uses the Web Speech API (`th-TH`). This works on Chrome and Edge, on both desktop and Android.
- It requires `https` or `localhost`, and you must allow microphone access.
- When you speak a command, Nexxa reads its reply aloud automatically. The "อ่านออกเสียง" (read aloud) button makes it also read replies to typed messages.

## Example commands

- ไปหยิบกล่องสีแดงที่ A มาส่งที่ DROP — fetch the red box from A and deliver it to DROP
- หยิบของจุด C กับ D ไปไว้ที่ DROP — take the items at C and D to DROP
- ตอนนี้ถืออะไรอยู่? — what are you holding right now?
- กลับบ้าน — go home
- หยุด — stop
- ทำไมท้องฟ้าเป็นสีฟ้า? — why is the sky blue? (general question, needs the AI)
