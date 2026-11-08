# mcloud on Windows — tester checklist

Thank you for testing! mcloud is a Spicetify theme that makes Spotify look like Apple Music. Its maintainer only has a
Mac, so a few things have never been seen on a real Windows PC. This takes about 30–40 minutes.

**What to send back:** the answers in the "Report" boxes (copy this file and fill them in, or just number your answers),
plus the screenshots asked for. **Keep personal data out of every screenshot:** crop away the left sidebar's playlist
names, your profile picture/name (bottom left), and the song bar at the bottom if it shows what you are playing. Use
`Win + Shift + S` (Snipping Tool) to grab just the area asked for.

Please also tell us once, at the top: Windows version (10 or 11), screen resolution, Spotify version (Spotify → the
"…" menu top left → Help → About Spotify), and Spicetify version (`spicetify -v` in PowerShell).

---

## 0. Install the test version

You need Spotify from spotify.com (not the Microsoft Store) and [Spicetify](https://spicetify.app/docs/getting-started).
Open **PowerShell (not as administrator)** and run, replacing `REF` with the name you were given
(for the window buttons alone: `claude/issue-200-traffic-lights-8edbb2`):

```powershell
& ([scriptblock]::Create((iwr -useb https://raw.githubusercontent.com/mcloudHQ/mcloud/main/install.ps1))) -Ref REF
```

Spotify restarts with the theme. The installer prints the commit it installed: copy that line into your report.

Set mcloud to the defaults first: open **Settings** (click your name bottom left → **mcloud**, or `Ctrl + Alt + ,`) and
check **General → Appearance = Dark**, **Glass effect = Balanced**. Play some music you like at low volume; the player
must show a song for parts 1 and 2.

To go back to your old look at the end:

```powershell
& ([scriptblock]::Create((iwr -useb https://raw.githubusercontent.com/mcloudHQ/mcloud/main/install.ps1))) -Uninstall
```

---

## 1. Liquid Glass at 125% and 150% display scaling

Windows' scaling: **Settings → System → Display → Scale**. Do the steps below twice: once at **125%**, once at
**150%**. After changing the scale, **quit Spotify completely** (right-click its tray icon → Exit) and start it again.

1. Open a long playlist (100+ songs). Scroll it top to bottom with the mouse wheel at a normal pace, then fast.
2. Go to **Home** and scroll down and up the same way.
3. Look closely at the **toolbar** at the top (back/forward, search field) and the **floating player** at the bottom:
   their edges, the thin bright line around them, and the corners.
4. Optional, for the frame rate: start Spotify from PowerShell with
   `& "$env:APPDATA\Spotify\Spotify.exe" --remote-debugging-port=9229`, open Google Chrome, go to `chrome://inspect`,
   click **inspect** under Spotify, press `Ctrl + Shift + P` in the DevTools window, type **Show frame rendering stats**,
   Enter. Scroll again and note the FPS number. Then set **Glass effect = Eco** in mcloud Settings and scroll again.

**Pass:** scrolling looks smooth (about 60 fps, no jumps or stutter you can notice); the glass edges sit exactly on the
edge, with no thin line drawn one pixel off, no gap, and no doubled or blurry edge compared with 100%.

> **Report 1** (for 125% and for 150%): smooth / some stutter / bad stutter, on playlist and on Home; FPS with
> Balanced and with Eco if you did step 4; any edge or line out of place.
> **Screenshots:** the toolbar (top of the window, without the sidebar) and the floating player, at 100%, 125% and
> 150%. Zoom-cropped is best.

---

## 2. Spotify's menus, and the top strip of the window

1. Right-click a song in a playlist. Then hover **Add to playlist** so its submenu opens. Do this over colourful
   album covers (Home or an artist page), at 125% and at 150%.
2. Look at the menu's rounded edges: the thin bright rim should follow the edge exactly, and the corners should look
   clean (not cut, not doubled).
3. Click your name bottom left. Then click the round profile/account button at the **top** of the window if there is one,
   and Spotify's **"…"** menu top left. Each menu item should react to clicks, including the first items near the top of
   the window.
4. Drag the window by its top strip (the empty area at the top, between the buttons), and double-click that strip.
   Then check that every button inside that top strip still clicks: back/forward, search field, the toolbar's icons.

**Pass:** menus look clean at both scales; every menu item and toolbar button at the top of the window reacts to the
first click; the window drags from the empty top area and double-click maximises/restores it.

> **Report 2:** menus OK / problem (which); any item or button near the top that did not react; dragging and
> double-click OK?
> **Screenshots:** a song's right-click menu with the Add to playlist submenu open, at 125% and 150% (crop to the menu).

---

## 3. Sidebar text weight

On the Mac the sidebar's rows (Search, Home, Browse, Recently Added, …) use regular weight; on Windows they are a bit
bolder. We want to know which reads better on Windows.

1. Start Spotify with the debugging port as in 1.4 and open DevTools from `chrome://inspect`.
2. Take a screenshot of the sidebar's **top part only** (Search down to Songs; no playlist names).
3. In the DevTools **Console**, paste this and press Enter:
   `document.head.insertAdjacentHTML('beforeend','<style>#am-sidebar .am-nav-row:not(.am-selected){font-weight:400}</style>')`
4. Take the same screenshot again. (Restarting Spotify undoes it.)

> **Report 3:** which looks better: the first (500, bolder) or the second (400, lighter)? Does the lighter one look thin
> or blurry anywhere?
> **Screenshots:** both, same crop.

---

## 4. macOS window buttons (red, yellow, green) — issue #200

New setting: mcloud can replace Windows' minimise/maximise/close buttons with macOS-style ones. It has never run on
Windows, so every item here is new.

Before you start, take a screenshot of the window's **top-right corner** and **top-left corner** as they are now.

1. Open mcloud **Settings → General → Window buttons**. You should see three choices: **Windows**, **macOS, left**,
   **macOS, right**. (If the row is missing, report it and skip this part.)
2. Pick **macOS, left**. Within a second or two:
   - Windows' three buttons (top right) should disappear, and so should Spotify's **"…"** menu (top left).
   - Three small circles (red, yellow, green) should appear in the top-left corner, over the sidebar.
3. Hover the circles: all three should show their symbols (×, −, and two small triangles).
4. Click **yellow**: the window minimises to the taskbar. Bring it back from the taskbar.
5. Click **green**: the window maximises (fills the screen, taskbar still visible); the triangles now point inwards.
   Click green again: it goes back to its earlier size. Also try **Alt + click** on green: full screen (no taskbar);
   green again or `Esc` leaves it.
6. Click another app so Spotify is in the background: the circles should turn grey; hovering them brings the colours
   back.
7. Drag the window by its top empty area; drag it to the left/right screen edge (snap) and to the top (maximise).
   Double-click the top empty area.
8. Change Spotify's zoom with `Ctrl + +` and `Ctrl + -`, then `Ctrl + 0`. The Windows buttons must stay hidden.
9. Open a music video or Spotify's full-screen view if you have one (the player's full-screen button), then leave it.
   The Windows buttons must stay hidden.
10. Pick **macOS, right**: the circles move to the top-right corner and nothing overlaps them (search field, toolbar
    icons).
11. Click **red**. Note what happens: does Spotify close completely, or hide to the tray? Compare with your Spotify
    setting **"Close button should minimise the Spotify window"** (Spotify's own Settings; type "close" in its search
    field), trying it once on and once off if you can.
12. Start Spotify again: the circles should be back and the Windows buttons hidden from the start (a short flash of the
    old buttons while Spotify starts is fine; tell us how long it lasts).
13. Pick **Windows** again: Windows' buttons and the "…" menu come back, the circles go.
14. Repeat steps 2–6 at **125%** and **150%** scaling: the circles should be crisp and round, not blurry or egg-shaped.

> **Report 4:** for each step 2–14: OK, or what happened instead. Especially: did the "…" menu go away too (step 2)?
> Did anything at the top stop reacting to clicks? Red: closed or tray (step 11)? Do you miss Windows 11's snap
> layouts pop-up (hovering the maximise button)? Would you keep the macOS buttons on, and on which side?
> **Screenshots:** the top-left corner with the circles (normal and while hovering), the top-right corner with
> **macOS, right**, and the top-right corner with **Windows** picked again. Crop to the corners.

---

## 5. Anything else

Anything that looked broken, out of place, or slow while you tested, with a screenshot (cropped). Thanks a lot!
