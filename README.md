# Wyoming Satellite with Chromecast Support

A fork of the [Wyoming Satellite](https://github.com/rhasspy/wyoming-satellite) project that adds Chromecast audio output support. This allows you to use any Chromecast-enabled device (like Google Home speakers or Chromecast-enabled TVs) as audio output devices.

## Features

* All original Wyoming Satellite features:
  * Works with [Home Assistant](https://www.home-assistant.io/integrations/wyoming)
  * Local wake word detection using [Wyoming services](https://github.com/rhasspy/wyoming#wyoming-projects)
  * Audio enhancements using [webrtc](https://github.com/rhasspy/webrtc-noise-gain/)
* Added features:
  * Chromecast audio output support using [catt](https://github.com/skorokithakis/catt)
  * Enhanced audio capture options with sox
  * Improved voice activity detection

## Prerequisites

1. System packages:
```bash
sudo apt-get update
sudo apt-get install python3-venv alsa-utils sox
```

2. Python virtual environment setup:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Required Components

1. Voice Activity Detection (VAD):
   * Automatically installed via requirements.txt
   * Uses Silero VAD for improved speech detection
   * Recommended for better interaction

2. Audio Enhancements:
   * WebRTC noise suppression and auto gain control
   * Automatically installed via requirements.txt
   * Helps improve audio quality in noisy environments

3. Cast All The Things (catt):
   * Cast All The Things allows you to send videos from many, many online sources (YouTube, Vimeo, and a few hundred others) to your Chromecast.
   * It also allows you to cast local files or render websites.
   * Automatically installed via requirements.txt

## Running the Satellite

Example command with all features enabled and also a localwakeword instance running:

```bash
source .venv/bin/activate && ./script/run \
    --name 'Boardgame Room Assist' \
    --uri 'tcp://0.0.0.0:10700' \
    --mic-command 'sox -t alsa hw:0,0 -r 48000 -c 2 -t raw -r 16000 -c 1 -b 16 -' \
    --snd-command 'Boardgame Room Display' \
    --wake-uri 'tcp://127.0.0.1:10400' \
    --wake-word-name ok_nabu \
    --vad \
    --mic-auto-gain 5 \
    --mic-noise-suppression 2
```

### Command Line Arguments Explained

* `--name`: Friendly name for the satellite (shown in Home Assistant)
* `--uri`: Network address for the satellite service
* `--mic-command`: Audio capture command
  * Example uses sox to capture from ALSA device 0 (can also use arecord if your mic supports 16000khz and mono sound)
  * Converts from 48kHz stereo to 16kHz mono
* `--snd-command`: Chromecast device name for audio output
* `--wake-uri`: Address of wake word detection service
* `--wake-word-name`: Wake word to use (e.g., 'ok_nabu')
* `--vad`: Enable voice activity detection
* `--mic-auto-gain`: WebRTC auto gain control (0-31 dbFS)
* `--mic-noise-suppression`: WebRTC noise suppression (0-4)

## Troubleshooting

1. Audio Input Issues:
   * List available audio devices: `arecord -l`
   * Test audio capture: `sox -t alsa hw:0,0 test.wav`
   * Adjust input device in --mic-command if needed

2. Chromecast Issues:
   * List available devices: `catt devices`
   * Test casting: `catt -d "Device Name" cast test.mp3`
   * Ensure device name in --snd-command matches exactly

3. Wake Word Detection:
   * Ensure wake word service is running
   * Check wake word service logs
   * Verify wake word name matches available options

## Credits

This project is a fork of the [Wyoming Satellite](https://github.com/rhasspy/wyoming-satellite) by Michael Hansen. The original project provides the foundation for this Chromecast-enabled version.

## License

This project maintains the same license as the original Wyoming Satellite project.
