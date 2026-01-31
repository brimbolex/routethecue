# routethecue
RouteTheCue is a modular, headless show-control router. It exists to route cues between people and systems on stage — before timing, context, or clarity get lost.

# routethecue
RouteTheCue is a modular, headless show-control router. It exists to route cues between people and systems on stage — before timing, context, or clarity get lost.

Built for Linux / Raspberry Pi, designed for live use, and extended through modules.


What it does

RouteTheCue connects and transforms messages across:
	•	OSC (iPad apps, controllers, ESP/Arduino)
	•	MIDI (USB-MIDI via ALSA, RTP-MIDI via rtpmidid)
	•	DMX over IP
	•	sACN / E1.31
	•	Art-Net

Core features:
	•	patchbay-style routing (in → out)
	•	mapping / transforms (scale, clamp, curve, merge)
	•	preset / scene switching (“one button changes everything”)
	•	status + metrics (drops, fps, sources alive/dead)
	•	remote control via HTTP/JSON and/or OSC control

 Design philosophy

This project is built for joy, but also for real shows:
	•	keep the core small and stable
	•	make protocol support modular
	•	avoid GUI dependencies (headless first)
	•	prefer predictable timing over “fastest possible”
	•	make failure modes explicit (fallbacks, last-good-state)

Architecture overview

Core vs Modules

The router consists of:

Core
	•	routing graph (nodes + edges)
	•	internal message types
	•	scheduler (DMX frame tick)
	•	presets (atomic switch)
	•	control API (HTTP/OSC)
	•	config loading + validation

Modules
	•	protocol endpoints (OSC, ALSA MIDI, sACN, Art-Net, …)
	•	each module registers its endpoints + config schema

Internal message types

RouteTheCue avoids “protocol ↔ protocol spaghetti” by using internal types:
	•	ControlValue — key/path + typed value (OSC-like)
	•	MidiEvent — note/cc/pc/clock/mtc + timestamp
	•	DmxFrame — universe + 512 slots + priority + timestamp

Modules only convert:
Protocol ↔ Internal Types
Mappings/Transforms operate only on internal types.

Networking notes (real-world)
	•	multiple protocols can share one NIC (Linux sockets handle this)
	•	stability depends on multicast/broadcast discipline
	•	recommended setup:
	•	keep DMX traffic wired
	•	keep Wi-Fi devices (iPad/ESP) in a control segment or use unicast for DMX