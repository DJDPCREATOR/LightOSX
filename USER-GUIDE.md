# LightOSX by DJDP - User guide
This guide matches **LightOSX 1.14.0** and is replaced by every new version. (The release notes keep the history of what changed; this guide only says how things work *now*.)

## 1. Getting started
- **One program, four forms:** the single offline html file (any Chrome / Edge / Brave browser), and the desktop app for Windows, macOS and Linux. Everything works in all of them unless a line below says "desktop only".
- **Fastest start:** plug in your DMX interface (or use no interface to try things out), open **Fixtures**, press **＋ Add fixtures**, pick your light, then open **DJ Pads** and press a pad.
- **Tabs (Shift+1 to Shift+8 jumps between them):** Simple Desk, Fixtures, Stage, Audio & EQ Detection, Chase Manager & Generator, DJ Pads, Show, MIDI Control. **Shift+Left / Shift+Right** go to the previous / next tab.
- **Top bar:** the connection pill (click it for interface settings), **Blackout** (all lights off, press again to bring them back), the full-screen button (F11 does the same) and the gear for **Settings**. The **Preview** bar shows what every light is doing right now.
- **Typing is safe:** while you type in any box, none of the keyboard shortcuts fire.

## 2. Connecting lights (Settings > DMX & interfaces)
- **ENTTEC Open DMX USB** and other serial adapters: pick the port in the list. On Linux add yourself to the `dialout` group once.
- **Art-Net, sACN (desktop app):** enter the IP (or leave the broadcast / multicast default) and the universe number.
- **Art-Net from the html version:** browsers cannot send network packets, so download the small helper (bridge.py for Python, or bridge.js for Node) from the interface card and run it. LightOSX streams the frames to it over a **WebSocket** (fast and gentle on Wi-Fi) and falls back to plain HTTP if the WebSocket cannot connect. Older helper files keep working over HTTP.
- **Bluetooth LED strips (ELK-BLEDOM and similar):** add a Bluetooth output, pick the strip. The desktop app reconnects to the strip it used last time by name. In the Fixtures tab put the strip's fixture on that output with the fixture's **link** option.
- **Several identical USB adapters:** use **Identify** on an adapter card; that adapter flashes white so you know which lights it drives. LightOSX remembers each adapter by serial number or USB port.
- **Universes:** every interface carries one universe (512 channels). **Number of DMX universes** (under the interface list, 1 to 8, default 4) sets how many universes the lists, the DMX map and the Stage offer; universes already in use are always kept.
- **Simulation:** with no interface the app still runs fully; the preview and Stage show what would be sent.

- **Settings of one interface (1.16.0):** each interface in the list has a **Settings of this interface only** fold-out: BREAK length, mark after break, RTS line and channels sent per frame. Empty fields follow the general settings under Diagnostics, so one picky interface can be tuned without touching the others. They apply the next time you connect. Refresh rate stays next to the name.

## 3. Fixtures tab (one workspace)
- **＋ Add fixtures:** choose brand, model and mode (or **Custom profile** to describe your own channels), the DMX address and how many copies. **＋ Screen** adds a virtual fixture that shows on a monitor, projector or cast display. **＋ Group** adds a group.
- **Groups and fixture cards:** every group is a section; every fixture is a **card in a grid** inside its group, and you edit everything right on the card, with no separate editor: name (✎ or double-click), icon or photo (tap the big picture), colour, DMX address, group, master dimmer, clone, **hold ◎ to find** it, delete, and for moving heads the **Follows** settings. Screen cards show their screen settings (display, what they show, tint, output Hz, strobe, open screen).
- **Move a fixture to another group:** drag its card by the **⠿** dots onto a group (on a touch screen press the handle and drag; with a mouse you can also grab the top rows of the card). Drop on **＋ New group** to start a new group, or on "Not in a group" to take it out. The **⇢ Move** button on every card opens a "move to group" menu, which always works on touch screens. Tick several cards first to move them together.
- **Group buttons:** click the name to rename, ▾ folds the section, **＋ Add** lets you tick which fixtures belong, **✕** deletes the group (its fixtures stay), drag ⠿ to reorder groups. **⚙** opens the group behaviour:
  - **Follows** another group (or "Not in a group") as **Mirror** (shows exactly the same look) or **Opposite** (shows the opposite colour). Groups that follow each other get no spread between them: the Spread slider in Chase Manager does not separate them.
  - **½ Alternate / ⇄ Flip / All main** set which fixtures of the group are **main** or **opposite** inside the group; a fixture set to opposite shows a **◐ Opposite** tag on its card, and clicking the tag makes it normal again.
  - **Movement follows too (1.15.0):** a group that follows another group does the same to moving heads as to colour. **Mirror** turns the pan around (left becomes right), **Opposite** turns pan and tilt around. A fixture set to opposite inside its group moves oppositely too.
  - Everything in one group shows the **same look** (the look of the group's first fixture), whatever each fixture's channels are.
- **Master dimmer (☀):** a per-fixture cap on how bright it may get. Fixtures without a real dimmer channel (RGB, RGBW) get one on their card and a fader on the last Simple Desk page, **☀ Virtual dimmers**, which you can also map to a MIDI fader. Double-click that fader's label to switch the cap off.
- **Find (◎, hold):** while you hold it the fixture shows full white, even over a blackout or the silence blackout.
- **Overlap warning:** fixtures that share DMX channels get a red "overlap" tag and a banner with **Move them to free channels**.
- **DMX 512 map** (below the groups): every channel of the chosen universe, with live level bars in the fixture's colour. Touching a fixture card outlines its channels and switches the map to its universe. Drag a patched cell to readdress its fixture. Fold the panel away by clicking its title.
- **▤ Compact / ▥ Expanded (1.15.0):** in Compact, every fixture is a small card with its picture, name and colour; press its **▾** arrow to open all its settings (name, address, group, master dimmer, screen settings), **▴** to close it. Expanded shows all settings on every card.
- **Several fixtures at once:** tick their boxes, then use the bar that appears: choose a group to move them, **Remove from groups** (they go to *Not in a group*), or **Unpatch (delete)** to delete them from the patch. A group can now be deleted even when it is empty and nothing is patched; empty groups are deleted without a question.
- **Search, ☑ Select all, ⇕ Fold all, Clear patch** are in the bar at the top.

- **Lasers (1.16.0):** add one of the **Generic laser** profiles (4, 7, 10, 14 or 20 channels), or make your own in the custom profile editor with the new laser roles (control mode, pattern, size, X, Y, rotation, speed, colour select, wave). Every make numbers its channels differently, so open **Laser settings** on the fixture card: set the DMX value that switches the laser to DMX control, the most power you allow, a test pattern, and the DMX value of each pattern (16 patterns, spread evenly until you change them). The generator and the chases drive the laser's pattern, size, position sweep, speed and colour from the show. These generic profiles are not copies of any one maker's manual, so check the values against yours before a gig; none of this has been run on a real laser.
- **A projector as a laser:** on a screen choose **Laser (projector)** under *Shows*. It draws laser beams and shapes on black in the fixture colour, and chases and scenes can change its pattern. It is a picture of a laser, not a laser beam.

## 4. Stage tab
- Drag your lights (and screens) to where they really are. Each light shows its icon or photo on a background of the colour it is putting out right now.
- **Several stages:** the list at the top switches stages; **＋ Stage**, **Rename**, **Delete**. **Universes:** choose which universes a stage shows (nothing ticked = all). **Background...** sets your own picture per stage; **＋ Text** adds labels (double-click a text to edit or remove it).
- **Icon size** scales the lights and names; **View size** shrinks the whole stage picture so it fits a small screen (the layout stays the same). **Snap** aligns to a grid. **Grid** (columns × rows, default 40 × 20) sets the size of the grid behind the stage, for example 80 × 40 for a finer one; each stage remembers its own. **Auto-arrange** lays the lights out in rows.
- **Auto-groups...** makes groups from where your lights stand: Left | Right, Back | Front, three columns, four columns, three rows, four corners, Inner | Outer, **Automatic** (it decides how many groups there are and who is in them: first by names such as "Floor Par 1", "Floor Par 2" and "Top Par", then by where the lights stand), or **Clusters by position** where you type how many clusters you want (2 to 24, found by k-means on the positions). Choose how the new groups act together: **Independent** (colours can travel from group to group with Spread), **Mirror**, **Alternate** (mirror and opposite take turns) or **Opposite**. Tick *set the Spread order to follow the stage* so colours travel across the stage. Existing groups stay unless you tick *Delete my existing groups first*.
- **Select several lights (1.15.0):** drag a box round them on an empty part of the stage, Shift- or Ctrl-click lights, or press **☑ Select all**. Drag one selected light and they all move together. **Group selected** puts them in a new group. Esc clears the selection.
- **Rotate (1.15.0):** the Rotate slider and **⟳ 90°** turn the selected lights (their pictures turn too).
- **Preview bar (1.16.0):** at the top of every tab a moving head only lights the side it points to (a wedge towards its pan), instead of the whole dot.
- **Direction of the light (1.15.0):** a **moving head** draws a beam in the direction its pan and tilt point (pan sweeps around from the front, tilt sets how far the beam reaches), and a **screen** only shines out of its front, turned by Rotate. This is a guide in an overhead picture, not a measurement. Lights that are on also take their live colour as the background of their picture.
- Double-click a light to open the Fixtures tab.

## 5. Simple Desk
- One fader per DMX channel. Pick the **Universe** (1 to 8; each is 512 channels), then page through it. **Faders/page** (4 to 512) is a slider: the more faders per page, the narrower they get so they all fit on screen without scrolling; fewer faders are bigger. At 512 per page a whole universe is on one page. Faders show what is being sent; moving one holds that channel at your value, and **double-clicking the fader's channel label releases it** again.
- **📸 Snapshot to Pad** saves the fader positions into a pad that moves all faders back.
- **Custom pages** (★) gather the channels you care about. The last page, **☀ Virtual dimmers**, has the master dimmers of fixtures that have no dimmer channel.
- Any fader can be mapped to a MIDI control (see MIDI Control).

## 6. DJ Pads
- Press a pad (mouse, touch, or the key shown on it). Pads can be a **colour**, a **scene** (made by the generator), a **macro** (changes the chase or audio reactions), a **moving-head pattern**, a **Show** action or **Blackout**.
- **Touch:** a finger swipe up or down scrolls the page and never presses a pad. A pad fires when you rest your finger on it (after a fraction of a second) or tap it. Mouse presses are instant.
- **Hold / Latch:** a Hold pad is on only while pressed; a Latch pad switches on with one press and off with the next. Change it with **Alt+pad key**, **Shift+click**, or **✎ Edit pads > ⇄ Hold/Latch** then tap the pad.
- **Default banks (32 pads each):** **A "Colours & effects"** (blackout, white, strobes, chases and 28 named colours), **B "More colours"** (a rainbow of 32 hues) and **C "Moving heads"** (every moving-head pattern in normal, fast, slow and wide versions, plus Heads centre). **Reset rig** brings all three banks, their names and the 32-pads-per-bank layout back.
- **Bank names:** double-click a bank button, or press **✎ Edit pads > ✎ Rename bank**, to give the bank you are looking at your own name.
- **✎ Edit pads** opens the editing tools (they are hidden while you perform so nothing changes by accident): **🎨 Customize pads** (name, sub-text, colour, picture, choke groups), **⤡ Resize pads**, **⇄ Hold/Latch**, **✎ Rename bank**, **⬆ Export pads**, **⬇ Import pads**, **🔗 Choke groups** and **⌫ Delete pads** (or hover a pad and press Delete).
- **Choke groups:** put a pad in one or several of the groups 1 to 4 (in Customize pads tap the numbered squares, or use 🔗 Choke groups and tap pads). Switching a pad on switches off every other pad that shares any of its groups, for Hold and Latch pads alike. Pads in no group layer as usual. Releasing a Hold pad never brings back a pad it switched off. A coloured badge with the group numbers shows on the pad.
- **Export / import pads:** **⬆ Export pads**, tap the pads you want (they light up) and press *Export ticked pads* (one pad gives a file named after it), or *Export this bank* or *Export all pads*. **⬇ Import pads** adds the pads of such a file at the end (new pads get new ids, so nothing is overwritten) and jumps to the bank they landed in. The file keeps names, colours, pictures, modes, choke groups and scenes.
- **Fade (0.0 to 5.0 s):** Latch colour and scene pads crossfade the lights instead of snapping. Hold pads and Blackout always snap.
- **Banks:** `[` and `]` switch banks; **Columns** and **Pads/bank** change the layout. **=** (or the Full screen button) shows only the pads, edge to edge.
- **Keyboard:** the pad keys are 1-8, Q-I, A-K, Z-comma rows. They are ignored while you type in a box.

- **Edit banks (1.15.0):** the **✎ Edit banks** button shows the bank tools: **+ Bank**, **− Bank**, **Rename bank**, **Pads/bank** and **Reset rig**. **Edit pads** now also holds **+ Add pad** next to Delete pads.
- **Choke groups (1.15.0):** there is no limit. In the choke-group tools, **＋ New** adds the next group number; a pad can be in several groups.

## 7. Audio & EQ Detection
- Pick the source: **Mic / Sound Input** (any input device, including line-in, Stereo Mix, virtual cables, Linux "Monitor of ..."), **Capture System Audio** (everything the computer plays) or **Capture Tab / Window / Screen** (one browser tab or program; the desktop app uses the whole system sound).
- **Capturing Rekordbox or another DJ program on Windows:** Rekordbox does not let other programs listen while Windows plays through your Pioneer / AlphaTheta controller (FLX4, DDJ...). Press **Ctrl+Win+V**, choose a different sound output (speakers or headphones) for the app, then capture. The error "device in use" means exactly this.
- **Second input:** tick **Second input**, choose another input (for example the room microphone) and set its level. It is mixed into the same analysis as the first source, so you can listen to the system sound and a microphone together. If it is the same device as the first input the app says so.
- **Meters:** the band meters, threshold lines, VU bars and spectrum move smoothly at the screen's own rate (fast attack, slower fall) while the Audio tab is open.
- **Six bands** (sub, bass, low-mid, mid, high-mid, high), each with a **reaction** you choose (colour, pulse, flash, blinder, press a pad, ...) and a **threshold** that can follow the room automatically. Your chosen reaction per band is saved.
- **Blackout when the sound stops** (silence blackout) switches the lights off after silence; the 💡 find button still works through it.
- **Song structure tracker, BPM detection and auto-level** run in the background; a tapped tempo or the BPM lock decides the chase speed.

- **More audio inputs (1.16.0):** under the audio settings press **+ Add input** as often as you like. Each input has its own device (or *Computer sound*), level, mute, and a **Send to speakers** box. All of them are mixed into the sound analysis. Pick an **Output** and a volume to play the ticked inputs through those speakers (and tick *Send the main input to speakers too* if you want that as well).
- **Phone as a Bluetooth speaker:** pair the phone with this computer and play on the phone; on Windows 10 and 11 its sound plays on the computer (some Bluetooth adapters need the *Bluetooth audio receiver* feature or a free receiver app, and the app cannot switch that on). Then add an input with *Computer sound* (or the phone, if it shows up in the list) and tick *Send to speakers*. Not tested with a real phone.

## 8. Chase Manager & Generator
- A chase is a list of steps (colour, level, strobe, optional fade and moving-head pattern) with **BPM**, **beats per step**, **wave**, **depth**, **fade**, **spread**, **direction** and **master**.
- **Spread across fixtures** shifts the chase from fixture to fixture. Groups that follow each other ignore it between them.
- **Spread order** chooses which fixture comes first: *Patch order* (DMX addresses) or the positions on the **Stage tab**: left to right, right to left, back to front, front to back, centre outwards, outside inwards. With a spread above zero the colours then travel across the stage in that direction. The choice is saved.
- **Generator:** type what you want ("slow blue fade with circle movement", "fast red strobe") and the offline language engine builds a chase. The desktop app can use a local AI model (Settings > Strobe, AI & backup).
- **Strobe:** the default **Exact Hz** keeps the Hz you set; "Even beat" rounds to whole frames.
- **Tap tempo:** the Space bar (outside the Show tab) or the Tap button.

- **Scenes and layers (1.16.0):** name the lights you mean and the generator gives each its own look. For example: *"make the tops red and the floor pars on the right strobing green and the lasers sweeping scissors on a neon colour fade at 140 BPM, then when the drop hits they sync up in colours, go in a red mood with white blinders, and then after the drop goes into a yellow blue crash mix"*, or *"make the stage light up red, then have a green spotlight move left to right while every other fixture on the stage is either red or flashing orange"*.
  - **Who is meant:** *tops* (lights named or grouped "top", else the back of the Stage), *floor pars* (named "floor", else the front of the Stage), *on the right / left* (by where they stand on the Stage tab), *pars / washes, moving heads, a spotlight* (one moving head), *blinders, lasers, screens*, *the stage / everything*, and *every other fixture / the rest*. Lights a scene does not mention stay dark. Name your lights and groups sensibly and place them on the Stage tab to help it.
  - **What each layer can do:** its own colours and fades, strobe, speed (BPM), movement (moving heads: *left to right*, circles, and so on), laser patterns (*scissors, sweep, fan, circle, wave, star, spiral, zigzag, grid, tunnel, dots, heart, square, triangle*) and screen modes (*gradient, bars, pixel matrix*, or *split the screen into 8 virtual fixtures*).
  - **Scenes:** *then* makes the next scene; *when the drop hits* waits for the drop the app hears in the music (Audio tab running with musical dynamics on) and *after the drop* follows it. Under the result, the **Scenes** panel shows every scene and layer with how many lights each picked, and has **◀ ▶** to move between scenes by hand and **Auto on drop** to turn the listening off. Scenes without a drop cue take turns every 16 beats. If no blinder is patched, *white blinders* flash all the ordinary lights white on the beat.

## 9. Show
- A Rekordbox-style timeline: load a song, the analysis finds beats and parts (intro, drop, break...), and the generator writes lighting lanes (colours, strobe, moving heads, blackouts) on the timeline. Edit the clips by hand, add songs to the **queue**, press **Play now**.
- **Space** play / pause, **Left / Right** seek 5 s, **comma / period** previous / next timeline; the mouse wheel scrolls and zooms the timeline.
- "Repeats: step up each time" makes the second drop bigger than the first.
- **Ambient channel:** open *Ambient channel* in the Show tab, choose an audio or video file and it loops under the show. It fades in and out with the show (the **Fade** slider), plays *while the show plays* or *always*, has its own **Volume** and drops to the **While a song plays** level under your songs. **▶ Test** plays it by hand. A video can show a preview in the panel; to put it on a projector add a Screen fixture set to Video and pick the same file. The desktop app remembers where the file is; the html version needs the file chosen again after a restart.

## 10. MIDI Control
- Turn MIDI on (the desktop app does it for you). Press **Learn** on any pad, fader or button, move a control, done. The monitor shows the device and the result and warns about mappings whose control no longer exists. **Rescan & enable all inputs** finds controllers you plugged in later.
- You can map: pads, a pad's Hold/Latch switch, Simple Desk faders and the ☀ virtual dimmers, grand master, tap tempo and more.

## 11. Screens (virtual video fixtures)
- **＋ Screen** in Fixtures. A screen shows a colour, bars, a **pixel matrix**, an image, text, a video, a gradient, a strobe or a clone of another screen, and follows the lights' colour and dimmer. In the desktop app it opens fullscreen on the display you choose, or on the display where the mouse is if you did not choose one. **Ctrl+W** closes a screen window. Screens appear on the Stage.

### Pixel matrix screens
- Set a screen's **Shows** to **Pixel matrix**. It draws a grid of glowing pixels (2 to 128 columns, 2 to 64 rows, round or square, with an adjustable gap) on the screen window, and on the Stage as a small monitor.
- **Patterns:** rainbow, plasma, scanning bar, sparkle, digital rain, fire, checkerboard, ripples, colour wash. **Shapes** drawn over the pattern or alone: circle, ring, square, diamond, triangle, heart, star, cross, arrow, each pulsing, growing, spinning, bouncing or staying still.
- **Picture to pixels:** *Choose picture...*, then fit the whole picture, fill (crop), scroll it sideways or scroll it upwards. **Text:** tick *Scroll the text* and type it in the text box.
- **Colour:** the pattern's own colours, the fixture colour (pads and chases recolour it), or the pattern tinted by the fixture colour. **Brightness** and **Speed** sliders. *Follow the fixture's brightness* lets chases, pads and the master dimmer dim the matrix (off by default so a matrix always shows).
- **Real pixel hardware:** tick *Send the pixels as DMX* and give the first address: 3 channels per pixel (RGB), rows from the top left, optional zig-zag rows and a new universe every 170 pixels. Blackout and the pad fade apply to it.

- **Real size (1.16.0):** type how big the screen is in the real world (cm, m, in or ft). The Stage tab draws it in proportion, so a 2 m wide screen looks twice as wide as a 1 m one, with the right shape.
- **Chase cells (virtual fixtures):** choose **Chase cells** under *Shows*, then how many cells across and down. Each cell gets its own look from the running chase or scene, spread across the screen, like a row of virtual fixtures. The chase generator can set it up for you (*"split the screen into 8 virtual fixtures"*).
- **Video wall:** give several screens the same **Wall name** with the wall's columns and rows and each screen's column and row (or tick the screens in the Fixtures tab and press **Make video wall**). The screen at column 0, row 0 decides what the whole wall shows, and every screen displays its own part of that one picture, as if they were one big display. Open each screen on its own monitor. Not tested with real monitors.

## 12. Settings
- **DMX & interfaces**, **Look & size**, **Strobe, AI & backup**, **Home Assistant**, **This device**, **About & shortcuts** are tabs; "Find a setting..." searches all of them.
- **Look & size:** **Look** (Touch look: round and roomy with chunky sliders and big round handles, or Mouse and keyboard look: tight, flat, slim sliders) and **Feel** (Touch feel: big buttons and sliders that are easy to hit with a finger, or Mouse and keyboard feel: compact) are separate, so you get four combinations: touch look with touch feel, mouse look with mouse feel, touch look with mouse feel, or mouse look with touch feel. The Settings panel opens wide by default; the arrow in its header makes it narrower. Also **fit the app to the window automatically** (on by default), vertical tabs, pinned bars, interface refresh (Smooth / Balanced / Power saver) and **Developer mode**.
- **Developer mode** shows the DMX statistics bar at the bottom (frames, dropped, errors, break and MAB timing, RTS, frame size) and the FPS / Loop readout at the top. Leave it off for performing.
- **Backup:** export a copy of all settings and import it again. Importing shows a progress window and restarts the app.
- **Cloud sync (GitHub Gist)** (Strobe, AI & backup tab): make a token at github.com/settings/tokens with only the **gist** permission and paste it in. **Upload now** makes a private Gist with your settings (the Gist id fills in), **Download...** loads it on another computer (after you confirm), *Upload automatically* sends a copy a minute after settings change, and *At start-up, ask me* offers a newer cloud copy. *Leave out big items* skips very large parts such as the song library so the copy stays under GitHub's size limit. The token stays on that computer only: it is not in backups or in the cloud copy.
- **Home Assistant & network control (desktop app):** a small local web API with a secret key. Pad actions are `on`, `off` and `latch`. Press **Copy Home Assistant setup (YAML)** and paste it into Home Assistant's configuration.

## 13. Hidden features and small tricks
- **Shift+Left / Right** change tab, **Shift+number** jumps to a tab.
- **Alt+pad key** or **Shift+click** a pad: switch Hold / Latch.
- **Hover a pad + Delete:** remove it.
- **Hold ◎** on a fixture card: find it (beats every blackout).
- **Double-click a light** on the Stage: open it in Fixtures.
- **Double-click the ☀ label** of a virtual dimmer fader: switch the cap off.
- **Ctrl+W** closes a screen window; **F11** is full screen.
- **Save pads' look:** use Customize pads to give pads names, colours and pictures.
- **Settings file:** your settings live in one JSON file in the desktop app (and in the browser's storage for the html); **Export copy** makes a backup you can import on another computer.
- **LightOSX-log.txt** in the app's folder records what the program did; send it with a problem report.

## 14. If something does not work
- **Lights do not respond:** check the connection pill, the universe number and the DMX address. On Linux add yourself to `dialout`. Turn Blackout off.
- **Bluetooth strip does not connect (Linux):** install and start BlueZ and switch the adapter on.
- **"Audio error: device in use":** see the Rekordbox note in section 7.
- **Everything is too big or too small:** Settings > Look & size, "Fit the app to the window automatically" and the size slider.
- **Report a problem:** quote the version number from Settings > About & shortcuts and attach LightOSX-log.txt.
