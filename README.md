# Fullscreen Camera

A one-file static page that shows a USB webcam fullscreen. No tests, no ads, no
build step, no dependencies — just `getUserMedia` piped into a `<video>` element.

Live at **<https://dulani.github.io/fullscreenwebcam/>**

## Use

1. Open the page.
2. Click **Start camera**, allow camera access once.
3. That's it. Camera + fullscreen in one click.

On later visits the feed starts by itself (Chrome remembers the permission for
the origin), so the only click left is the one that enters fullscreen —
browsers require a user gesture for that and there's no way around it.

| Key | Action |
| --- | --- |
| `f` | toggle fullscreen (or double-click the video) |
| `r` | rotate 180° — for tent / A-frame mode on a convertible |
| `m` | mirror horizontally |
| `z` | fill the screen — crops the edges instead of letterboxing |
| `n` | next camera |
| `Esc` | exit fullscreen |

Your camera choice, mirror, and fill settings are remembered in `localStorage`.
The toolbar and mouse cursor fade out after 2.5s of no input.

## Deploy to GitHub Pages

```sh
git init
git add index.html README.md
git commit -m "Fullscreen camera"
git branch -M main
git remote add origin git@github.com:dulani/fullscreenwebcam.git
git push -u origin main
```

Then **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`**.
It goes live at <https://dulani.github.io/fullscreenwebcam/> within a minute or
two. (No `.nojekyll` needed — nothing here starts with an underscore.)

HTTPS is required for camera access, and GitHub Pages serves HTTPS — so it just
works. (`file://` will *not* work: Chrome treats it as an insecure origin and
refuses camera access. For local testing use `python3 -m http.server`, which is
fine because `http://localhost` counts as secure.)

## Notes

- Requests 1920×1080 @ 30fps as *ideal*, so the browser picks the closest mode
  your webcam actually supports rather than failing.
- `audio: false` — no mic access requested, no echo, nothing extra to permit.
- Holds a screen wake lock while streaming so the Chromebook won't dim.
- If the webcam is unplugged and replugged, the page reconnects to it.
- Default is `object-fit: contain`, which letterboxes rather than distorting or
  cropping. Press `z` if you'd rather fill the screen.
- **Flip** (`r`) is a 180° *rotation*, not a vertical mirror. Use it when a
  convertible laptop is folded into tent / A-frame mode, where the display
  itself is upside down. It turns the toolbar along with the video, so both read
  correctly. A literal vertical flip (`scaleY(-1)`) would leave all text as
  mirror-writing — which is why it isn't that. **Flip** and **Mirror** are
  independent and compose.

## Credits

Built by [Dulani Woods](https://github.com/Dulani) with
[Claude Code](https://claude.com/claude-code) (Opus 5).
