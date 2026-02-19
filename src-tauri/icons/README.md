# Icons

This directory should contain the application icons for Tauri.

Required files:
- `32x32.png` - 32x32 pixels PNG icon
- `128x128.png` - 128x128 pixels PNG icon
- `128x128@2x.png` - 256x256 pixels PNG icon (for high DPI)
- `icon.icns` - macOS icon bundle
- `icon.ico` - Windows icon

You can generate these icons from a source image using:

```bash
pnpm tauri icon /path/to/source-image.png
```

The source image should be at least 1024x1024 pixels and square.
