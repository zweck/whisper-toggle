# whisper-toggle

Single-keybinding speech-to-text for Linux. Press once to record, press again to transcribe and paste.

## Quick Start

```bash
# Install from AUR
yay -S whisper-toggle

# Run interactive setup
whisper-toggle-setup

# Use: press your keybinding (default Super+`) to start/stop recording
```

## Dependencies

| Package | Purpose | Required |
|---------|---------|----------|
| bash | Runtime | Yes |
| sox | Audio recording | Yes |
| curl | Server API calls | Yes |
| jq | JSON parsing | Yes |
| libnotify | Desktop notifications | Yes |
| libpulse | PulseAudio support for sox | Yes |
| whisper.cpp | Speech recognition engine | Yes |
| xsel | Clipboard (X11) | X11 only |
| xdotool | Auto-paste (X11) | X11 only |
| wl-clipboard | Clipboard (Wayland) | Wayland only |
| ydotool | Auto-paste (Wayland) | Wayland only |
| pciutils | GPU detection in setup | Optional |

## Configuration

Config file: `~/.config/whisper-toggle/whisper-toggle.conf`

| Variable | Default | Description |
|----------|---------|-------------|
| `BACKEND` | `server` | `server` (on-demand whisper-server) or `cli` (whisper-cli) |
| `WHISPER_SERVER` | (auto-detect) | Path to `whisper-server` binary |
| `WHISPER_CLI` | (auto-detect) | Path to `whisper-cli` binary |
| `WHISPER_MODEL` | `~/.local/share/whisper-toggle/models/ggml-small.en.bin` | Path to GGML model |
| `WHISPER_PORT` | `58080` | Port for server backend |
| `WHISPER_DEVICE` | `0` | GPU device index (`-1` for CPU-only) |
| `WHISPER_THREADS` | `4` | CPU threads for inference |
| `WHISPER_LANGUAGE` | `en` | Language code or `auto` |
| `AUTOPASTE` | `1` | Auto-paste after transcription |
| `SILENCE_DURATION` | `3.0` | Seconds of silence before auto-stop |
| `SILENCE_THRESHOLD` | `3` | Silence detection sensitivity (%) |

## Keybinding Setup

### i3
```
bindsym $mod+grave exec --no-startup-id whisper-toggle
```

### sway
```
bindsym $mod+grave exec whisper-toggle
```

### Hyprland
```
bind = $mainMod, grave, exec, whisper-toggle
```

### GNOME
Configured via `whisper-toggle-setup` using gsettings, or manually in Settings > Keyboard > Custom Shortcuts.

### KDE
System Settings > Shortcuts > Custom Shortcuts > Add > Command/URL, set trigger and action to `whisper-toggle`.

## Backends

### Server (recommended)
Starts `whisper-server` on demand when you begin recording. The model loads while you speak, so there's minimal delay. The server is killed after each transcription — no persistent daemon.

### CLI
Runs `whisper-cli` directly after recording finishes. Simpler but slower since the model loads after you stop speaking.

## Models

| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| tiny.en | 75 MB | Fastest | Basic |
| base.en | 142 MB | Fast | Good |
| small.en | 466 MB | Moderate | Very good |
| medium.en | 1.5 GB | Slow | Excellent |
| large-v3-turbo | 1.6 GB | Moderate | Excellent |
| large-v3 | 3.1 GB | Slowest | Best |

Models are downloaded to `~/.local/share/whisper-toggle/models/` by the setup wizard.

## GPU Setup

The setup wizard detects GPUs via `lspci` and lets you choose which device to use. For NVIDIA, ensure you have CUDA-enabled whisper.cpp. For AMD, ensure ROCm or Vulkan support.

## Troubleshooting

**No sound recorded**: Check that PulseAudio/PipeWire is running and `rec` (sox) can access your microphone.

**Server timeout**: The whisper-server has 30 seconds to start. Large models on CPU may need more time — try a smaller model or enable GPU.

**Nothing pastes**: On X11, install `xsel` and `xdotool`. On Wayland, install `wl-clipboard` and `ydotool`. Ensure `ydotoold` is running for Wayland.

**Double triggers**: The 500ms debounce should prevent this. If your WM sends multiple key events, increase the debounce or use `exec --no-startup-id` (i3).

Check logs at `/tmp/whisper-toggle.log` for diagnostics.

## License

MIT
