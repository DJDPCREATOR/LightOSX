# LightOSX by DJDP: desktop app

A standalone version of LightOSX. No browser, no Python, no helper programs: everything runs inside this one app, fully offline.

## What the desktop app adds
- **A rock-solid USB DMX driver.** Serial DMX interfaces (Open DMX, ENTTEC PRO, DMXking, generic FTDI) are driven by a dedicated native thread that keeps re-sending the latest frame at a steady rate, like a hardware controller. Nothing in the user interface (Bluetooth, pads, shows, window dragging) can stall your lights, and it reconnects automatically if the adapter drops.
- **System audio capture.** On Windows, "Capture System Audio" listens to everything the PC plays (loopback); "Capture Tab / Window / Screen" shows a chooser. Pick a different output device in your sound settings (button provided) to listen to that one.
- **Art-Net and sACN built in.** Network DMX nodes work directly (no `bridge.py` needed). Pick the protocol on the interface row.
- **PC RGB lighting** through OpenRGB (keyboards, mice, RAM, motherboards, fans, GPUs, LED strips).
- **Settings saved automatically** to a file (`LightOSX-settings.json`, shown in Settings). With `portable.txt` next to the program, everything stays in the app folder, so you can run it from a USB stick.
- **No throttling when the window is hidden or minimised**, and the screen is kept awake, so lights never stutter mid-set.
- USB DMX (Open DMX, PRO, DMXking), uDMX, MIDI controllers and Bluetooth LED strips work as in Chrome. The app shows a simple chooser when several devices are found.

## What changed in 1.1
- **Refresh rate:** Open DMX and other USB DMX interfaces now start at **40 Hz**. You can type any rate or tap a preset (14, 20, 30, 40, 44) in Settings > DMX interfaces. Lights can respond differently at different rates; if a fixture only behaves fully at a lower one (for example 14 Hz), set that.
- **Crash/freeze protection:** the serial (DMX) code now runs in its own helper process. If it ever crashes or hangs, only that helper stops and the app reconnects by itself. Slow opens (Bluetooth link, serial port) run one at a time and each has a time limit, so connecting an ENTTEC while a Bluetooth strip is connected (or the other way round) cannot lock the app.
- **Log file:** `LightOSX-log.txt` sits next to the settings file (in `LightOSX-data` in portable mode). Help > Open log / settings folder shows it. Send it along with "Copy diagnostics" if something misbehaves.
- **System audio:** Capture System Audio now waits for the app to be ready before asking Windows, retries once with a simpler request, and gives a clear message if no sound comes through.
- **Fixture groups** can be reordered with the ◀ ▶ buttons or by dragging the ⠿ handle.

## Run it
- **Windows:** unzip, double-click `LightOSX DJDP.exe`. (Windows SmartScreen may warn because the app is not code-signed: More info > Run anyway.) Allow the firewall prompt if you use Art-Net or sACN.
- **Linux:** make the AppImage executable (`chmod +x LightOSX*.AppImage`) and double-click it (or run it). For USB DMX adapters add yourself to the serial group: `sudo usermod -aG dialout $USER` (Arch: `uucp`), then log out and in. Bluetooth strips need BlueZ.
- **macOS:** unzip, drag **LightOSX DJDP.app** to Applications. The app is not code-signed: the first time, right-click > Open (or run `xattr -dr com.apple.quarantine "/Applications/LightOSX DJDP.app"`). macOS 13 or newer is needed to capture system audio; otherwise use a loopback device such as BlackHole as a sound input.
- **From source (Windows, macOS, Linux):** install Node.js, then in this folder run `npm install` and `npm start`.
- **Build your own installers:** `npm run dist:win` (portable .exe), `npm run dist:linux` (AppImage and .deb), `npm run dist:mac` (.dmg). Build on the matching operating system.

## PC RGB (OpenRGB)
1. Install OpenRGB (openrgb.org) and open it. Go to the **SDK Server** tab and click **Start Server**.
2. In LightOSX: Settings > DMX interfaces > search "OpenRGB" > Add, then Connect. Choose "All devices" or one device.
3. On the Fixtures tab add a fixture from the brand **PC RGB (OpenRGB)**. It links to the device automatically. Everything that colours that fixture (pads, chases, shows) now colours your PC.

## Bluetooth strips
Add the strip in Settings, then Connect: a list of nearby Bluetooth devices appears. Add a fixture from the brand **Bluetooth (BLE)**; it links to the strip automatically, wherever you patch it. Close the manufacturer's phone app first, since most strips accept only one connection.

## If a USB interface keeps dropping
Interfaces now reconnect by themselves (watch the row: "retrying", then "live"). Still dropping? Plug it straight into the PC (no hub), use a short good cable, disable Windows "USB selective suspend" (Power Options > Advanced > USB settings), and lower the Refresh rate on that row. Use **Copy diagnostics** in Settings and send the text along with a description.

## Files
`main.js` (window, devices, network), `preload.js` (safe bridge), `openrgb.js` (OpenRGB client), `LightOSX-DJDP.html` (the app).

## Screen outputs (added in 1.1)
Fixtures tab > "Add screen / video output" creates a virtual fixture with no DMX address. Its output is a window on any display
(monitor, projector, or a cast / wireless display that your system lists as a display). Chases, pads, groups and the Show timeline
drive its colour, brightness and strobe like any light. Content: fixture colour, audio bars, image / logo, text (or the song playing
in the Show tab) and a looping video. Choose the display on the fixture card and press "Open screen"; Esc closes it in the desktop app.

## Release notes

Version numbers: the number in **Settings > About, app & shortcuts** (and in the window title and the Windows file properties) is LightOSX's own version. The "44.7.0" you may see elsewhere is the Electron engine (the program LightOSX runs inside), not LightOSX itself.
New features raise the middle number (1.16.0 -> 1.17.0), fixes only raise the last one (1.16.0 -> 1.16.1).

### 1.16.0 - 22 Nov 2026
**Chase generator: scenes and layers**
- Say which lights you mean: *"make the tops red and the floor pars on the right strobing green and the lasers sweeping scissors on a neon colour fade at 140 BPM, then when the drop hits ... then after the drop ..."*. Each layer picks its lights by name, group, kind (pars, moving heads, a spotlight, blinders, lasers, screens), side (left/right) or row (tops/floor) of the Stage, or *the stage / every other fixture*, and has its own colours, strobe, tempo, movement, laser pattern and screen mode. Scenes change on *then*, *when the drop hits* (heard in the music) and *after the drop*, or with the new ◀ ▶ buttons. The Scenes panel shows what every layer picked. Lights a scene does not mention stay dark. Pads store the scenes with the look.
**Lasers**
- New laser channel roles for custom profiles, five generic laser profiles, a **Laser settings** panel on the fixture card (control-mode value, maximum power, test pattern, value of each of 16 patterns), and DMX output of pattern, size, X/Y sweep, rotation, speed and colour from the show. Any screen can be a laser: **Laser (projector)** draws beams and shapes.
**Screens**
- **Real size** (cm, m, in, ft): the Stage draws screens in proportion. **Chase cells**: a screen split into virtual fixtures driven by the chase or scene (the generator understands *split the screen into 8 virtual fixtures*). **Video wall**: screens with the same wall name, grid and position show one big picture (also **Make video wall** from ticked screens).
**Audio**
- Any number of extra inputs with level, mute and **Send to speakers**, an output chooser and volume; *Computer sound* as an input. Guide for using a phone as a Bluetooth speaker.
**Interfaces**
- Per-interface BREAK length, mark after break, RTS line and channels per frame.
**Other**
- The preview bar draws a moving head as a wedge toward its pan. Stage rotation, laser, size, wall and cell settings are now saved with the fixtures (rotation was not saved in 1.15.0).

### 1.15.0 - 21 Nov 2026
**Fixtures tab**
- The word "Drag" is gone: just the ⠿ dots (or grab the card anywhere with a mouse). The **Main** button is gone; a fixture set to opposite shows an **◐ Opposite** tag (click to make it normal).
- **▤ Compact / ▥ Expanded:** compact cards show picture and name; **▾** opens all settings of one fixture. Cards and the group settings are smaller.
- Tick several fixtures: **Remove from groups** and **Unpatch (delete)**. An empty group can now be deleted even when no fixture is patched (the board used to show only "Nothing patched yet"), and empty groups are deleted without a question.
- The "follow brightness" tick-box in the pixel-matrix settings now wraps inside its box.
- **Groups move too:** Mirror groups turn the pan of moving heads around, Opposite groups turn pan and tilt around (colour behaved this way already).
**Simple Desk**
- **Universe** selector and a **Faders/page** slider (4 to 512): faders shrink so all of them fit without scrolling. Labels read "U3 · Ch 1-32".
**Stage**
- **Auto-groups:** new **Automatic** (by names, then positions) and **Clusters by position** with a number you type.
- Adjustable **Grid** (default 40 x 20, e.g. 80 x 40), **multi-select** (box, Shift/Ctrl-click, Select all, **Group selected**), **Rotate**, a **beam** in the direction of moving heads and screens, and lit lights take their colour as background.
**DJ Pads**
- New **Edit banks** bar (+ Bank, - Bank, Rename, Pads/bank, Reset rig); **+ Add pad** moved into Edit pads. **Unlimited choke groups** (**＋ New**).
**Settings**
- Panel opens wide by default. **Look** and **Feel** are two choices (four combinations) and the chunky touch sliders are back with the touch look.
**Bluetooth strip:** after Blackout the black frame is now repeated for about 3 seconds, in case one packet is lost while a strobe is running.

### 1.14.0 - 17 Nov 2026
**Pixel-matrix screens**
- New screen type **Pixel matrix** (Shows > Pixel matrix): a grid of glowing pixels, 2 to 128 columns by 2 to 64 rows, round or square with an adjustable gap. Patterns: rainbow, plasma, scanning bar, sparkle, digital rain, fire, checkerboard, ripples, colour wash. Shapes (circle, ring, square, diamond, triangle, heart, star, cross, arrow) that pulse, grow, spin, bounce or stay still. **Picture to pixel grid** (fit, fill, scroll sideways, scroll up) and scrolling text. Colour from the pattern, the fixture colour or both; brightness and speed.
- Optional **DMX pixel output**: the pixels are also sent as RGB channels from an address you choose (rows from the top left, optional zig-zag, optional 170 pixels per universe) for real LED matrices. Blackout and the pad fade apply to it.
**Cloud sync and Art-Net**
- **Cloud sync (GitHub Gist)** in Settings > Strobe, AI & backup: upload your settings to a private Gist and load them on another computer, with optional automatic upload and a start-up check for a newer copy. The token is kept only on that computer, never in backups or the cloud copy; big items (song library) can be left out to stay under GitHub's size limit.
- **Art-Net bridge over WebSockets** (html version): the browser keeps one WebSocket open to the helper instead of one HTTP request per frame. bridge.py (stdlib only) and bridge.js both speak WebSocket and HTTP, so old and new files work together; the page falls back to HTTP by itself and reconnects.
**Tested:** every matrix pattern and shape draws, a picture becomes the right pixel colours, text and DMX output (including 170 per universe) work; both bridges answered WebSocket frames (small, odd, 300 byte and ping) and legacy HTTP with correct Art-Net packets in the Linux test; the browser client streamed, fell back and reconnected; cloud sync was exercised against a simulated GitHub (private Gist, update, truncated download, error messages, token never in the copy). **Not tested against the real GitHub or on Windows.**

### 1.13.0 - 17 Nov 2026
**Stage and chase**
- **Spread order** in Chase Manager: patch order, or follow the positions of your lights on the Stage tab (left to right, right to left, back to front, front to back, centre outwards, outside inwards), so colours travel across the stage.
- **Auto-groups...** on the Stage tab: make groups from where the lights stand (left | right, back | front, columns, rows, corners, inner | outer, or automatic clusters) and choose how they act together (independent, mirror, alternate, opposite); optionally set the spread order to match.
**Audio and Show**
- **Second audio input:** mix another input (for example the room microphone) into the same analysis as the first source, with its own level.
- **Smooth meters:** band meters, threshold lines, VU bars and spectrum now run on their own frame loop at the screen's rate with fast attack and slower fall (about 51 frames per second measured in the test) instead of stepping with the DMX and interface rates.
- **Ambient channel** in the Show tab: a looping audio or video file under the show with fade, volume, a lower level while a song plays, "while the show plays / always" and a test button.
**Tested in the Linux build:** auto-groups (left/right, front/back opposite, k-means), spread order ranks and colours, second input with Chromium fake devices, meter frame rate, ambient channel (fade, always mode, test, persistence). Not tested with a real microphone or on Windows.

### 1.12.0 - 16 Nov 2026
**DJ Pads**
- **Several choke groups per pad:** a pad can be in any of the groups 1 to 4 (tap the numbered squares in Customize pads, or use Choke groups and tap pads). Switching it on switches off every pad that shares any of its groups. Pads from 1.7 to 1.11 keep their single group.
- **Named banks:** double-click a bank button (or ✎ Edit pads > ✎ Rename bank) to name it. Default names: **A Colours & effects, B More colours, C Moving heads**.
- **New defaults:** bank A now holds all the colours (28 named colours next to blackout, white, strobes and the chases), bank B is a 32-hue rainbow and bank C holds every moving-head control in 32 pads (8 patterns in normal, fast, slow and wide versions, plus Heads centre). Existing pad sets just gain the missing moving-head pads.
- **Reset rig** now resets all three banks (it only did A and B), the bank names and the pads-per-bank layout.
- **Import and export pads:** ⬆ Export pads (ticked pads, a bank or everything) and ⬇ Import pads in the Edit pads bar. Imported pads get new ids, so nothing is overwritten.
- **Touch:** a swipe up or down scrolls the page and no longer presses pads. A finger or pen press fires after a split second of resting or on a tap; the mouse is instant.
**Settings**
- **Number of DMX universes** (1 to 8, default 4) under the interface list: limits the universe lists, the DMX map and the Stage lists; universes in use are always kept.
- **Look & size:** the old Automatic / Touch / Mouse choice is gone. **Buttons and sliders** (Bigger or Compact, with a hint which suits touch and mouse) and **Look** (Touch look or Mouse look) are now two separate choices, so you can have the touch look at the mouse size or the mouse look at touch size. Your old choice carried over as the size; the default is compact with the touch look.
**Tested in the Linux build:** multi-group choke matrix, default pads (96, three banks), bank rename, Reset rig, export and import round trip, tap / quick tap / swipe with emulated touch, universe count, size and look classes. Not tested on a real touch screen or on Windows.

### 1.11.0 - 16 Nov 2026
**Fixtures tab redesigned again (your feedback on 1.9.0)**
- **No more separate editor and list.** Every group is a section and every fixture is a **card in a grid** inside it, with **everything editable right on the card**: name, picture, colour, DMX address, group, master dimmer, clone, find (hold ◎), delete, moving-head Follows, and the full screen settings for screens. Nothing to open first.
- **Bigger pictures:** the icon or photo box on every card is larger (48 px, 54 px in touch size) and the picture fills it.
- **Touch drag and drop works:** press the **⠿ Drag** handle on a card and drag it onto a group (the handle was not receiving touch moves before because of how it was drawn). With a mouse you can also grab the top rows of a card. **⇢ Move** on every card opens a group menu as a second way that always works. Tick several cards to move them together.
- Touching a card outlines its channels in the **DMX 512 map** and switches the map to its universe. The map sits under the groups and can be folded.
- Group sections are full width, one under the other, so the cards have room.
**Tested in the Linux build (real mouse and emulated touch events):** inline address editing, mouse drag from the card header onto a group, touch drag by the handle onto a group, DMX map outline, screen cards. Not tested on a real touch screen or on Windows.
**Still on your list:** multiple choke groups per pad, named pad banks with new defaults and a full Reset rig, importing/exporting pads, touch scrolling that ignores swipes on pads, number of DMX universes, Look & size choices, stage groups that travel colours, pixel-matrix screens, second audio input, ambient channel, cloud sync. Ableton Link is not built in.

### 1.10.0 - 16 Nov 2026
**Fixes for what you found in 1.9.0**
- **RGB lights in a group with dimmer fixtures (the virtual master dimmer problem):** a group used to copy the first fixture's channel values by name, so a light without a Dimmer channel (RGB, a Bluetooth strip) never got the brightness of a first fixture that had one, and its preview, master dimmer and real output stayed at full. Now every fixture of a group shows the **look** of the group's first fixture, written through its own channels, so brightness reaches every light and the master dimmer works on all of them. Checked in the app: an RGB light in a group led by a dimmer fixture follows its dimmer exactly, and its master dimmer at 50% halves the output.
- **Groups that follow each other:** two groups set to Mirror now show exactly the same colour on every fixture, and Opposite shows the opposite colour (green against magenta in the test). The Spread slider no longer separates groups that follow each other; other groups still use it.
- **Blackout and Bluetooth strobe:** the strip kept flashing during a blackout (or "blackout on no audio") because its strobe helper still believed the look wanted a strobe. Blackout now stops it. (Not tested on your strip.)
- **💡 Find** now shows the fixture white even over Blackout and the silence blackout.
- **Settings:** the tab contents no longer look cut off at the top; the tabs have a clearer look.
- **About:** the "engine number" now shows the Electron version, not LightOSX's own.
- **Pads:** the "+AI" button in the corner of the pads is gone.
**Stage**
- Lights now sit on a background of the colour they are putting out. **Screens show up on the stage** (as a small monitor with their colour). New **View size** slider shrinks the whole stage picture for small screens; the old Size slider is called **Icon size**.
**Look**
- **DMX 512 map** is about twice as dense (44 columns where there were 22).
- **Developer mode** (Settings > Look & size): the DMX statistics bar at the bottom and the FPS / Loop numbers in the top bar only show when it is on (off by default).
- **Fit the app to the window** is on by default (you can still switch it off).
**Audio**
- A note in the Audio tab and in the error message explains the Rekordbox "device in use" error: choose a different Windows sound output than the Pioneer / AlphaTheta controller (Ctrl+Win+V).
**User guide**
- New **USER-GUIDE.md** in every package explains every feature and the hidden tricks. It is replaced (not added to) with each version; these release notes keep the history.
**Tested in the Linux build:** group mirror and opposite, RGB in a dimmer-led group, find over blackout and silence blackout, developer mode, DMX map columns, stage view size. Not tested on a touch screen, a Bluetooth strip or Windows.
**Still on your list (not in this version):** fixtures as a grid with all editing inside the group and touch drag (the touch drag fix), multiple choke groups per pad, named pad banks with new defaults and a full Reset rig, importing and exporting pads, touch scrolling that ignores swipes, number of DMX universes setting, the Look & size choices (bigger buttons / compact and touch look / mouse look), stage groups that travel colours, pixel-matrix screens, second audio input, ambient channel, cloud sync. Ableton Link is not built in (it needs Ableton's own library and I cannot prove it works).

### 1.9.0 - 15 Nov 2026
**Fixtures and Stage are now two separate tabs**
- **Stage** has its own tab (Shift+3) with everything it had: several stages, universes, background picture, texts, snap, size, auto-arrange. Double-click a light on the stage to jump to it in the Fixtures tab.
- **Fixtures** (Shift+2) is one workspace instead of sub-tabs, all on one screen:
  - **Group board:** every group is a card (plus a card for fixtures not in a group). Each fixture is a tile with its live colour, name, profile and address. **Drag a tile onto a group to move it**, onto **＋ New group** to start a new group, or onto "Not in a group" to take it out. Mouse: grab the tile anywhere. Touch and pen: grab the ⠿ handle (the → button on every tile also opens a "move to group" menu). Tick several tiles first to move them together. A group card can be folded, renamed (click its name), reordered (drag its ⠿), and has **＋ Add** (tick the fixtures that belong) and ✕. The **⚙** on a card opens the group behaviour: Follows (mirror or opposite another group), ½ Alternate, ⇄ Flip, All main, ◀ ▶ order.
  - **Fixture editor** on the right: everything the old editor had (name, icon or photo, address, master dimmer, screen settings, moving-head follow, colour, group, locate, clone, delete). Picking a fixture also switches the DMX map to its universe and outlines its channels.
  - **DMX 512 map** underneath, always in view (fold it away with its title): the same level-bar grid with drag-to-readdress.
  - Also on the board: 💡 hold to flash a fixture, ● / ◐ main or opposite, red "overlap" tags with a one-click fix, ☑ Select all, ⇕ Fold all, search, bulk move or delete.
- The tab row is Simple Desk, Fixtures, Stage, Audio, Chase, DJ Pads, Show, MIDI Control. **Shift+number now goes 1 to 8** in that order (Audio moved from 3 to 4, and so on). The shortcut list in Settings says so.
**Removed**
- The MIDI clock tempo sync from 1.8.0 is gone again (the box in the MIDI tab and its code), as requested. Ableton Link is still not built in.
**Notes**
- Tested in the Linux build with real mouse drags: a fixture dropped on a group moved into it, one dropped on ＋ New group made a new group, selecting a tile opened its editor and outlined it in the DMX map, and the Stage tab opened with the lights on it. Not tested on a touch screen or on Windows.
- Still open: pixel-matrix screens, second audio input, choppy meters, ambient channel in Show, cloud sync, Art-Net over WebSockets.

### 1.8.0 - 15 Nov 2026
**Fixtures tab, redesigned**
- Four big sub-tabs: **🎛 Rig**, **🗂 Groups**, **🎭 Stage**, **🔢 DMX map**, with counts on Rig and Groups. The tab you used last opens next time. Everything from before is still there (add fixtures, screens, custom profiles, search, bulk select, drag onto groups, master dimmer, locate, follow/mirror/opposite, Stage with several stages, DMX map).
- **Groups is its own tab** now: all your groups side by side (order, rename, delete, "Follows", Alternate, Flip, All main, move a fixture in). Clicking a group name in the Rig list jumps there and highlights it; **＋ Group** opens it ready to rename.
- **Live colour:** the little swatch of every fixture in the Rig list shows the colour it is really putting out right now (grey when dark), so you can see at a glance what is on.
- **💡 button on every row:** hold it to flash that fixture white and find which light it is, without opening it.
- **Overlap warning:** fixtures that share DMX channels get a red "overlap" tag and a banner with **Move them to free channels**.
- **☑ Select all** ticks every fixture in the list (or the ones that match your search) for bulk move or delete. The search box and Select all hide on Stage and DMX map.
**Settings in sub-tabs**
- Six tabs instead of one long page: **DMX & interfaces**, **Look & size**, **Strobe, AI & backup**, **Home Assistant**, **This device**, **About & shortcuts**. Typing in "Find a setting…" still searches everything. The last tab is remembered.
**Tempo sync (MIDI clock)**
- MIDI Control tab > **🎚 Tempo sync**: follow a MIDI clock (Ableton Live, other DJ/music software, drum machines) for the chase tempo, optionally from one chosen input, and optionally start/stop the chase with the sender's Play and Stop. In Live: switch on Sync for a MIDI output port (loopMIDI on Windows, IAC on a Mac) and pick that port here.
- **Ableton Link itself is not built in.** Link needs Ableton's own network library, which cannot run in a single offline html and could not be built for all systems here. Live passes the Link tempo on as MIDI clock, so it works through that.
**Small**
- The top tab row is a little narrower again so all seven tabs fit on a normal screen.
- Tested in the Linux build: Settings tabs and search, the four Fixtures sub-tabs, Groups jumps, overlap detect and fix, live swatch colour, and tempo sync with simulated clock (128 and 100 BPM followed, Play starts, Stop stops, other inputs ignored). Not tried with Ableton, a real MIDI port, or on Windows.

### 1.7.0 - 15 Nov 2026
**DJ Pads**
- **Choke groups.** Every pad can be put in choke group 1, 2, 3 or 4 (or None): in DJ Pads press **✎ Edit pads**, then **🎨 Customize pads** (tap a pad, pick "Choke group") or **🔗 Choke groups** (pick a group number, then tap all the pads that belong to it; pick None to take pads out). Switching on a pad switches off every other pad in its group, for Hold and Latch pads alike, from mouse, touch, keys, MIDI and the Home Assistant API. Releasing a Hold pad never brings back a pad it choked. Pads in no group, or in another group, layer as before. It works across banks, Show pads cannot be grouped, and the groups are saved with the pads in the settings file. Groups A-D from 1.6.0 became groups 1-4.
- **Edit pads.** Customize pads, Resize pads, Hold/Latch, Choke groups and Delete pads now sit behind one **✎ Edit pads** button, so nothing can be changed by accident while you play. Closing it switches any edit mode off.
- **Fade time.** New **Fade** slider (0.0 to 5.0 s) next to the pad columns: a Latch colour or scene pad crossfades the lights when you switch it on or off, instead of snapping. Hold pads, macros and Blackout always snap. Saved in the settings.
- Tested in the Linux build: choke matrix with Hold and Latch pads (including release of a Hold pad and layering), 1.6.0 groups migrating to 1-4 after a restart, Edit pads closing edit modes, and the fade ramp measured on the DMX output (about 1 s smooth rise and fall, instant with Fade at 0). Not tried with a MIDI controller or on Windows.

### 1.6.1 - 15 Nov 2026
**Fixes**
- Pad and transport shortcuts (pad keys 1-8 / Q-I / A-K / Z-comma, [ ], =, backslash, Space, Delete, Shift+number, the Show keys) are now ignored whenever you are typing in a box, a drop-down or an editable text, including while an input method is composing. Checked in the app: the same keys do nothing inside an input, while pad keys still work outside.
- Hold / Latch wording is the same everywhere (DJ Pads, MIDI help text, pad tooltips and messages, Home Assistant docs). The Home Assistant API now also accepts the action **latch** (flip on or off); the older word **toggle** still works.
- The DMX frame clock of the single-file html was checked: it already runs on an inline Web Worker timer, so browsers do not throttle it while another program is in front (nothing needed changing; not tested in a real background browser tab).

### 1.6.0 - 14 Nov 2026
**New**
- **Pad groups ("only one pad on at a time").** DJ Pads > **🔗 Pad groups**: pick a group letter (A to H), then tap the pads that belong to it (tap again to take a pad out). When you switch on a pad of a group, the other pads of that group switch off. It works the same for mouse, touch, keyboard keys, MIDI and the Home Assistant API. Grouped pads show a coloured letter badge in the corner. Pads in no group behave as before, and a pad can only be in one group. "Empty this group" and "Remove all groups" are in the same panel. Show pads cannot be grouped (they fire once). Groups are saved with the pads.
- Tested in the Linux build with real clicks: three pads put in group A, switching each one on switched the previous one off, and a pad outside the group stayed on. Not tried with a MIDI controller.
- Not in this version (still open): pixel-matrix screens, second audio input, ambient channel in Show, cloud sync, Fixtures sub-tab redesign, choppy audio meters.

### 1.5.3 - 14 Nov 2026
**New**
- **Virtual dimmers page on the Simple Desk.** Fixtures without a dimmer channel (RGB, RGBW...) get a fader each on a new last desk page, "☀ Virtual dimmers" (pick it in the page list). It is the same master dimmer as in the Fixtures tab. Drag it, or map it to a MIDI fader with MIDI learn like any desk fader (it shows as "Virtual dimmer: name"). The lit part of the fader shows the live brightness of the fixture, so it moves with chases and audio like the other dimmer faders. Double-click the label to switch the dimmer off (100%).
- **Import progress.** Importing a settings file now shows a "Importing settings..." window with steps, and after the restart a "Loading your imported settings..." window stays until the app is ready, so it no longer looks frozen.
**Notes**
- The master dimmer itself was tested in the app: dragging the slider changes the output, and it caps the brightness while a chase plays (unless the "let looks take over" box is ticked). If an RGB fixture still ignores it, tell me which fixture and what is driving it (chase, pad, audio, Show).
- Not tested on real hardware, and MIDI learn for the new faders was not tried with a real controller.

### 1.5.2 - 13 Nov 2026
**Fix**
- "Audio error: Could not start video source" when pressing Capture System Audio or Capture App/Window (desktop app). The capture needs a video source only to switch the system sound on, and the video is thrown away at once. The app now uses LightOSX's own window as that source (the one that worked for you), and the screen as the second try; Capture App/Window falls back to the same path if the source you picked cannot start. The log (LightOSX-log.txt) shows a line "capture source: ..." for each try.
- Linux: a second, separate fault is fixed too: the capture request sent an invalid audio setting on Linux and was refused. The list of capture sources is also asked again once if it comes back empty.
- Not tested on Windows hardware: I could only check the new code path on Linux. If the error still appears, send LightOSX-log.txt.

### 1.5.1 - 13 Nov 2026
**Fixes**
- Tab row (Simple Desk, Fixtures & Patch, Audio, Chase, DJ Pads, Show, MIDI) is taller and bigger again; the 1.5.0 slim version was too small.
- Audio reactions: the reaction you choose for each frequency band is now remembered separately from the temporary reactions that pads, scenes and chases set. Before, a pad or chase could overwrite the band reactions and the overwritten ones were what got saved.
- Screen windows (desktop app): a screen opens full screen on the display you chose for it. If none is chosen (or the chosen one is not connected) it opens on the display where the mouse pointer is.
- Start-up: a MIDI controller that reports its current knob or switch positions right after it connects no longer presses pads (this was the most likely reason Blackout and the purple pad were on at start-up). The first reading of each mapped control within 4 seconds after MIDI connects is only noted, never treated as a press.
- Not tested on real hardware: the start-up fix could not be reproduced without your controller, so please tell me if Blackout or the purple pad still turn on by themselves (and send LightOSX-log.txt plus the MIDI monitor line).
- Still open from this round (not done in 1.5.1): the virtual master dimmer of RGB fixtures, the Fixtures sub-tab redesign, pixel-matrix screens, second audio input, ambient channel in Show, cloud sync, choppy audio meters and the app-capture freeze.

### 1.5.0 - 10 Nov 2026
**Faster and lighter**
- The lights' timing loop no longer burns processor time while waiting for the next frame (desktop app). Measured idle processor use of the window roughly halved.
- The screen is redrawn 30 times per second instead of every monitor refresh (60-144), the status numbers update twice a second and only when they change, and the DMX grid is only drawn while you are looking at it. New setting: **Interface refresh** (Smooth 60 / Balanced 30 / Power saver 20 with animations off).
- Settings panel scrolls without repainting the whole page.
- Saving a large song library is lazy (it no longer re-saves after every song), long lists show only a part of the library.
**Strobe**
- Strobe is back to the true Hz you set (default **Exact Hz**). The "Even beat" style rounds the speed to whole frames, which made fast strobes run faster than set; it is still available as an option.
**Fixes**
- Audio reactions per band are saved at once and again when you close the app.
- Shift+number always switches tabs. The pad mode (latch/hold) is changed with Alt+pad key or Shift+click, never with a Shift+number any more.
- Group editing: the handle to change the order of groups is at the far left of the group box.
- Tab row is slimmer.
- Screen fixtures: the option check boxes are separate pills and no longer look like part of the Output Hz box.
- The Flash and Tap Strobe buttons in the top bar were removed.
**Stage**
- Dragging a light follows the mouse all the time.
- Each light shows the icon or photo chosen in the Rig, its name and its live colour.
- Several stages, each with its own universes (one or more, or all, and several stages may share a universe).
- Upload your own picture as the stage background (per stage). Text labels can be added, moved, edited and removed.
**Home Assistant and network control** (desktop app)
- Settings > Home Assistant & network control: a small local web API with a secret key. Press pads, black out, start or stop the chase, set the master and send a scene prompt. One button copies a ready-made Home Assistant YAML.
**Versions**
- The Windows file properties and the macOS app now say 1.5.0 instead of the engine number. This file now holds all release notes.

### 1.4.0 - 9 Nov 2026
- Fixtures tab redesigned a third time: every fixture has a group button (one tap menu), each group has "Add" with a checklist, rows can be dragged onto a group, a Stage view, and moving heads can follow another head (same, mirrored, flipped, opposite).
- Settings regrouped (DMX first; Strobe + AI generator + backup combined; About + App + Shortcuts combined); Touch / Mouse / Automatic control sizing; top bar buttons stay together.
- Audio bands can press a pad; noise gate that learns the room; grand master flash and tap strobe (removed again in 1.5.0); one clock per frame for movement and fades.
- MIDI: monitor shows the device and result, warns about mappings whose control no longer exists, "Rescan & enable all inputs".
- USB DMX: identical interfaces are told apart by serial number or USB port, never guessed; "Identify" button flashes an interface white.
- Bluetooth: Web Bluetooth switched on for Linux; at startup the strip used last time (same name) is reconnected automatically.
- Show: "Repeats: step up each time" makes second drops bigger. Much faster with thousands of songs.

### 1.3.0 - 6 Nov 2026
- Desk "Snapshot to Pad": save the fader positions into a pad; pressing the pad moves all faders back.
- Moving-head patterns (circle, square, triangle, figure 8, diamond, left-right, up-down, random) as pads, chase panel and per-step settings; Show lane "Moving heads"; chase generator understands movement words.
- Per-step "arrives with fade" in chases; Show clips with intensity and fade-in; colour popup with checkboxes; Show generator options for colour fade and intensity.
- Fixed the Show scroll loop (playhead lost then jumping), pinned tab row on by default, MIDI list shows the device, touch-friendly Rig.

### 1.2.0 - 5 Nov 2026
- Linux: fixed the AppImage that could never start, modern AppImage runtime, portable folder with launcher and README.
- Fixtures tab "Rig" (list and editor), Show colour option, version shown in Settings, first queue song reopens after restart.
- (4 Nov) Part detection rework (breaks before drops, beat-accurate boundaries, tempo changes), Show wheel scroll and zoom, Play now, new timeline without audio, pinned bars, fit-to-window sizing, MIDI start-up choice, SVG transport icons.
- (4 Nov) Local AI model for the scene generator, audio auto-thresholds, master dimmer fix, Show blackouts and "every song" regenerate.

### 1.1.0 - 18 Oct to 2 Nov 2026
- DMX: Open DMX refresh rate setting, helper process for serial output, Enttec fixes, LightOSX-log.txt, Art-Net and sACN (desktop), multi-universe, BLE LED strips with strobe hardening.
- Audio: system and app capture, auto-level and beat detection, band reactions, live song-structure tracker.
- Show: Rekordbox-style timeline with analysis, library and queue, long tracks in pieces, saved file paths, pad actions.
- Screens (virtual video fixtures): colour, bars, image, text, video, gradient, strobe, clone, fit modes, display matching.
- Scene generator (offline language engine), pad customisation, fixture icon picker, groups, per-fixture master dimmer, strobe rewritten with exact Hz and Even beat styles, vertical tabs, Ctrl+W on screen windows.

### 1.0.0 - October 2026
- First single-file web app with DMX output, fixtures, chase generator, DJ pads, simple desk, audio detection and MIDI mapping, plus the first desktop (Electron) builds for Windows, macOS and Linux.
