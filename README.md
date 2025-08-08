# ESPP

**Easy to Deploy Frontend Static Playlist Player**

ESPP is a lightweight, frontend-only music player designed for static hosting (like GitHub Pages).  
Just drop your music and covers into the right folders, and it's ready to go.

> 🛑 **Note:** All content in the player is fully public.  
> Use this only for openly shareable audio. Do **not** use it for private or copyrighted material.

## Features

- 🔊 Audio playlist support
- 🌙 Light/Dark theme toggle
- 🎵 Cover art display
- 💻 Fully static – no backend required
- 🚀 GitHub Pages friendly

## Getting Started

```bash
npm install
npm run dev
````

To build for production:

```bash
npm run build
```

Then deploy the `dist/` folder to any static host – such as GitHub Pages.

## Usage

1. Clone this repo:

   ```bash
   git clone https://github.com/JeongHan-Bae/espp.git
   cd espp
   ```

2. Replace the example content:

    * Place your `.mp3` files in `public/music/`
    * Place cover images in `public/images/`
    * Edit `public/playlist.json` to define your playlist

   Only the audio source (`filename`) is required; `cover` and `artist` are optional.

3. Deploy!

If you're using GitHub Pages, set the `dist/` folder as the source for Pages after running `npm run build`.

## License

MIT
