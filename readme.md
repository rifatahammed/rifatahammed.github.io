# RIFFF's Castle — portfolio site

A single-file portfolio: a small retro 3D castle (three.js) that you walk through, with every section shown as an old-Windows style window, plus a plain "no game" version of the same content for phones and for people who just want to read.

Everything lives in **`index.html`**. There is no build step, no framework, no other files to keep in sync.

---

## 1. Deploying

1. Put `index.html` at the root of your GitHub Pages repository (`rifatahammed/rifatahammed.github.io`).
2. Commit and push. GitHub Pages serves it at `https://rifatahammed.github.io/` within a minute or two.
3. Keep `assets/images/my-avatar.png` in the repo (the avatar URL points there) — or change the `avatar` field, see below.

Things the page loads from the internet (nothing else):

| What | From | Needed for |
|---|---|---|
| three.js r128 | cdnjs.cloudflare.com | the 3D castle only (the plain site never downloads it) |
| Pixelify Sans font | fonts.googleapis.com | the pixel look; falls back to Tahoma/Verdana |
| SoundCloud player | w.soundcloud.com | the jukebox and the music embeds |
| GitHub API | api.github.com | reading guestbook notes |

To preview locally just open `index.html` in a browser. (The guestbook and jukebox need an internet connection; everything else works offline.)

---

## 2. Changing the content

Open `index.html` in any text editor and search for this banner (around line 165):

```
/* =========================================================
   CONTENT — edit this block to change what the site says
   ========================================================= */
const D={
```

The `D` object is the **only place** you normally need to touch. Both the 3D boards and the plain site are generated from it, so one edit updates both.

Rules of thumb when editing:

- Text goes inside quotes: `'like this'`. If the text itself contains an apostrophe, either use double quotes `"I'm here"` or escape it `'I\'m here'`.
- Items in a list are separated by commas. A trailing comma after the last item is fine; a *missing* comma between items breaks the page.
- Order matters: items appear in the order you list them.
- After editing, open the page once. If it shows a blank screen, you have a typo — press F12 → Console and the browser will point at the line.

### 2.1 Identity

```js
name:'Rifat Ahammed', handle:'RIFFF', role:'UE Dev, Game Designer', location:'Bangladesh, Asia',
avatar:'https://rifatahammed.github.io/assets/images/my-avatar.png',
emails:['reardj007@gmail.com','rifatahammed@yahoo.com'],
```

- `avatar` can be any image URL, or a relative path like `'assets/images/my-avatar.png'` if the image is in the same repo. It is shown at 96×96, pixelated.
- The **first** email is the one used by the guestbook's "Send by email" button.

### 2.2 Social buttons

```js
social:[
  {title:'Facebook',url:'https://www.facebook.com/rifatahammed'},
  {title:'LinkedIn',url:'https://www.linkedin.com/in/rifat-ahammed'},
  ...
],
```

Shown as buttons on the plain-site header and as links in the Contact window. Add or remove lines freely.

### 2.3 About

```js
about:[
  'First paragraph…',
  "Second paragraph…"
],
```

Each string is one post in the **About me** window. The post titles ("Hiiiiiiiiiiii", "We are all NPCs") are set a little further down, search for `S.about=` if you want to rename them.

### 2.4 What I'm doing

```js
doing:[
  {title:'itch.io', note:'Leisure game projects to play with FnF', url:'https://rifff.itch.io/'},
  ...
],
```

`note` is optional. Appears as a linked list in the **What I'm doing** window.

### 2.5 Skills

```js
skills:['Unreal Engine','Unity','C++', ...],
```

Rendered as tags in the **Skills** window.

### 2.6 Experience, volunteering, education

```js
experience:[
  {role:'Lead Unreal Developer, Quest Designer', org:'ATTRITO – M7-PRODUCTION LTD', years:'2022 — Present',
   bullets:['Design and develop quests and missions', ...],
   sub:{title:'Zero Hour', bullets:['Ensure a standard player experience…', ...]}}
],
```

- `bullets` is required; `sub` (a sub-heading with its own bullets) is optional — delete the whole `sub:{...}` part if you don't need it.
- To add a second job, copy the whole `{ ... }` block, put a comma between the two, and edit.

```js
volunteering:[ {role:'Trainer', org:'Children Science Congress, Bangladesh', years:'2017 — 2020'}, ... ],
education:[ {role:'Computer Science and Engineering', org:'North South University', years:'2017 — 2021'}, ... ],
```

`org` can be an empty string `''` if there is nothing to add after the role.

### 2.7 Portfolio lists

```js
games:[     {title:'I Know', url:'https://rifff.itch.io/i-know'}, ... ],
slices:[    {title:'Pick Rotate Throw', url:'https://www.artstation.com/…'}, ... ],
materials:[ ... ],
modeling:[  ... ],
gallery:[   {title:'Travel', url:'https://www.canva.com/…'}, ... ],
```

Five plain lists of `{title, url}` (an optional `note` works here too). They become the **Games**, **Vertical slices**, **Materials**, **3D modeling** and **Gallery** boards in the Portfolio hall, and the Portfolio window on the plain site.

### 2.8 Music tracks (the jukebox)

```js
compose:[
  {title:'Droplet', id:'1965534851', url:'https://soundcloud.com/rifff_at/droplet'},
  ...
],
```

- `id` is the SoundCloud **track ID** (a number). To find it: open the track on soundcloud.com → Share → Embed → the embed code contains `api.soundcloud.com/tracks/1234567890` — that number is the id.
- `url` is the normal track page, used for the "Open on SoundCloud" links.
- The jukebox plays the tracks in this order and starts with the first one.

The Music room has **four wall boards**, one per track (`track0` … `track3`). If you add a fifth track it will play in the jukebox and appear on the plain site, but it won't have a board until you add one — see §4.

### 2.9 Guestbook

```js
guestbook:{enabled:true, repo:'rifatahammed/rifatahammed.github.io', label:'guestbook'},
```

**Turning it off:** change `enabled:true` to `enabled:false`. That removes the Guestbook board from the Profile room, the Guestbook window and taskbar link from the plain site, and the mention on the welcome board. Nothing else changes, and you can turn it back on any time.

How it works when it is on:

- Visitors type a name and a message. "Post on GitHub" opens a **new tab on github.com** with a pre-filled issue titled `Guestbook: <name>`; the visitor has to be logged in to GitHub and click "Submit" there themselves. "Send by email" opens the visitor's own mail app addressed to the first address in `emails`.
- The board then reads the open issues of the repo through GitHub's public API and shows the ones whose title starts with `Guestbook`.
- `repo` must be `owner/repository` of the repo that hosts the site, with Issues enabled (Settings → General → Features → Issues).
- `label` is optional. If you create a label with that name in the repo, notes get tagged with it; if the label doesn't exist GitHub silently ignores it and notes are still recognised by their title.

What it can and cannot do (so you know what you're allowing):

- The page never writes to your repository itself and holds no key or token. Every note is an ordinary GitHub issue created by the visitor's own account, so it is always clear who wrote it.
- Visitors cannot change the site, its files or its settings — issues are separate from the code.
- Notes contain plain text only. Anything a visitor types is shown as text, never as links, images or code, even if they paste HTML.
- You moderate it with normal GitHub tools: **close** an issue to hide a note, **delete** it to remove it for good, **lock** it to stop replies, or block the user. Only open issues are shown.
- Anyone can open issues on a public repo whether or not this site exists — so if you'd rather have none at all, disable Issues in the repo settings *and* set `enabled:false` here (otherwise the board would show "Could not load the notes").

---

## 3. Text that is *not* in `D`

A few strings are written directly where they are used. Search for these if you want to change them:

| What | Search for |
|---|---|
| Welcome board posts (intro, room list, how to walk) and their dates | `S.welcome=` |
| About post titles ("Hiiiiiiiiiiii", "We are all NPCs") | `S.about=` |
| Titles of every window and board (e.g. `'Vertical slices'`) | `port('slices'` etc., or `title:` on the `S.xxx=` lines |
| Guestbook form wording | `S.guestbook=` |
| Tips shown in the bottom notice box | `tips:` inside each room, and `const TIPS=` |
| Plain-site header sentence and status lines | `function buildPlain(` |
| Page title / description (browser tab, search engines) | `<title>` and `<meta name="description"` near the top |
| "1 here · RIFFF OS" under the location panel | `RIFFF OS` |
| Player name tag | `labelSprite('guest'` |

---

## 4. The rooms

Search for `const ROOMS={`. Each room looks like:

```js
profile:{name:'Profile room', w:22, d:16, H:5, indoor:true, tint:[178,150,118], floor:'wood',
  boards:[{key:'about',side:'N',t:-0.5},{key:'doing',side:'N',t:0.5},{key:'contact',side:'W',t:0},{key:'guestbook',side:'E',t:0}],
  torches:[['E',-0.7],['E',0.7],['S',-0.6],['S',0.6]],
  tips:['The guestbook is on the east wall — leave a note.', ...]},
```

- `w`, `d`, `H` — width, depth and wall height (metres, roughly; the player is about 2 tall).
- `tint` — RGB colour of the bricks; `floor` is `'wood'` or `'tile'`.
- `boards` — which windows hang on which wall. `key` is the section name (`about`, `doing`, `contact`, `guestbook`, `skills`, `experience`, `volunteering`, `education`, `games`, `slices`, `materials`, `modeling`, `gallery`, `track0`…). `side` is `N`/`E`/`S`/`W` and `t` is the position along that wall from `-1` (left end) to `1` (right end); keep boards at least `0.4` apart on the same wall.
- `torches` — decorative lights, same `side`/`t` format.
- Indoor rooms always get a "Back to lobby" door in the south wall, so don't put a board at `side:'S', t:0`.

**Adding a fifth track board:** add `{key:'track4',side:'W',t:0.6}` to the music room's `boards` and move the existing west board to `t:-0.6`.

**Adding a whole new room:** copy a room block, give it a new key (e.g. `lab:`), then add a doorway to the lobby's `doors` list and a button to the taskbar (search for `function buildBar(` and copy one of the `['profile','Profile',ICON.person]` entries). The lobby has one door per wall already, so a fifth room would need its door on a wall that has one (`at` is the position along that wall, like `t` for boards).

---

## 5. Look and feel

Colours are CSS variables at the top of the `<style>` block:

```css
:root{--win:#c9cec7; --win2:#e3e6e0; --hi:#ffffff; --sh:#6f766c; --sh2:#2e332c;
      --title:#3d4f47; --ink:#1d221c; --paper:#efeee5; --accent:#4fbf8b; --sky:#0d0b1c; --link:#1e3f8f}
```

- `--win` window grey, `--title` title-bar colour, `--paper` window background, `--accent` the green taskbar strip, `--sky` night sky.
- Font: change `Pixelify+Sans` in the Google Fonts `<link>` and the `font-family` line right below `:root`.
- Movement speed is `3.7` (walk) and `6.5` (run) in `const spd=`; default camera distance is `camDist:6.2` and pitch `camPitch:0.36`.

The 3D world is not made of image or model files: the character, the props and every texture are generated by code when the page loads. That keeps the site a single file and means most changes are just editing a number or a colour. The sections below tell you where.

### 5.1 The character

Search for `function makePlayer(`. The first line after it holds the palette (hex colours, `0xRRGGBB`):

| Material | Default | Used for |
|---|---|---|
| `skin` | `0xf2c9a6` | head |
| `tunic` | `0x5f8d3f` | body and arms |
| `hat` | `0xc03b3b` | the cone hat |
| `dark` | `0x4b3626` | legs / boots |
| beard (inline) | `0xeeeeee` | the beard cone |

Changing colours is enough for a "blue wizard" or "red knight". The following lines are the body parts, one per line, and each can be resized or deleted:

| Line starts with | Part | Numbers to play with |
|---|---|---|
| `const body=` | body | `CylinderGeometry(0.3,0.4,0.75,8)` = top radius, bottom radius, height, sides |
| `const head=` | head | `SphereGeometry(0.28,…)` = radius |
| `const beard=` | beard | delete this line for a beardless character |
| `const cone=` | hat | `ConeGeometry(0.34,0.9,8)` = radius, height, sides; `position.y=1.98` |
| `const eye=` | eyes | the `[-0.1,0.1]` pair is the eye spacing |
| `const arms=` / `const legs=` | limbs | these are animated when walking, keep their names |
| `const light=` | the warm glow around the player | `PointLight(0xffe0b0,0.7,7)` = colour, strength, reach |
| `const lbl=labelSprite('guest'` | the name tag | change `'guest'` or the `0.8` size |

The walk animation swings `arms` and `legs` (search `p.arms[0].rotation.x` if you want it to swing more or less). Wall collision uses `const PR=0.45` as the player's radius — raise it if you make the character wider.

### 5.2 The world

- **Room sizes, colours, floors, boards and torches** — `const ROOMS={`, explained in §4.
- **The courtyard** — search for `function lobbyExtras(`. In order: the four corner towers (`CylinderGeometry(2.6,2.8,H+4,10)` and their cone roofs `ConeGeometry(3.4,4.6,10)`), the merlons along the wall tops (the two `for` loops; `i+=3` is their spacing), the hedges (`hedge(side, from, to)` — positions along each wall), the four trees (`[[-12,-12],[12,-12],[-12,12],[12,12]]` are their x/z positions; add or remove pairs) and the welcome board (`function welcomeBoard(` — `bx=-5.2, bz=-2.6` is where it stands).
- **Doors** — `const DW=3,DH=3.4` are doorway width and height.
- **Sky** — `function makeSky(`: `n=800` stars, the moon at `(70,62,-100)`, the sun at `(-85,95,-75)`, and `i<9` blocky clouds with their drift speed. The stars spread over a 150-unit dome.
- **Lighting and fog** — `const DN={`. `night:{…}` and `day:{…}` each define: `bg` sky colour, `fog` colour with `fogN`/`fogF` (where fog starts and where it is solid), `amb`/`ambI` ambient colour and strength, `hsky`/`hgnd` sky/ground bounce colours with `hI`, `dir`/`dirI` the moon or sun light with `dx,dy,dz` its direction, and `iamb`/`iambI`/`ihI` the same for indoor rooms. Fog distances for a room are also set in `scene.fog=new THREE.Fog(` (indoor 14–46, outdoor 24–95).
- **Torch flicker** — the `for(const t of G.torches)` line in `function update(`; `0.78+0.22*…` is base brightness plus flicker amount.

### 5.3 Textures

All surfaces are small pixel-art canvases painted at load time. Search for `function makeCanvases(`:

```js
C.tile=tileCanvas([98,106,96],3);        // lobby floor tiles: base RGB, random seed
C.tileCool=tileCanvas([104,108,118],5);   // indoor tile floor (Portfolio hall)
C.hedge=noiseCanvas([52,92,40],26,9);     // hedges: RGB, grain amount, seed
C.roof=roofCanvas(11);                    // tower roofs
C.wood=woodCanvas(13);                    // wooden floors, frames, posts, trunks
C.dark=noiseCanvas([44,46,50],10,17);     // ceilings
```

- **Bricks** are not in that list: each room's walls use its own `tint` from `ROOMS` (RGB), so recolouring a room is a one-line change there. Brick size is in `function brickCanvas(`: `bh=16,bw=32` on a 128-pixel canvas.
- **Roof and wood base colours** are inside their functions: `base=[168,54,46]` in `roofCanvas`, `base=[118,80,46]` in `woodCanvas`.
- **Tile scale**: in `function buildRoom(`, `fr=R.floor==='wood'?3:4` is how many metres one texture repeat covers on the floor; wall bricks repeat every 2.5 m (`len/2.5,H/2.5` in `function wallSeg(`).
- Changing the seed number (the last argument) reshuffles the random grain without changing the colour.
- Textures are drawn with `magFilter=NearestFilter` so they stay crisp and pixelated; that is set in `function texFrom(` if you ever want smooth textures instead.

### 5.4 Going further (needs code, not just values)

These are not supported by editing values yet — ask for them if you want them:

- **Your own texture images** (pixel-art PNGs in the repo instead of generated ones).
- **A real character model** (a `.glb` from Blender with a walk animation, replacing the built-in gnome). Note this would make the site more than one file and heavier on phones.
- **New props** such as fountains, banners or statues in the courtyard — written in the same style as `tree(` and `welcomeBoard(`.

---

## 6. How visitors use it

- **Desktop** opens the castle. `W A S D` / arrows walk, `Shift` runs, drag the mouse to look, scroll to zoom, `E` (or Enter/Space) reads a board, `Esc` closes, `N` toggles day/night, `M` pauses the music.
- **Phones and tablets** open the plain site by default; "▶ Play" loads the castle with a touch stick and an E button.
- The bottom taskbar and the location dropdown teleport between rooms. "Plain site" (top right) switches to the readable version; "Enter the castle (3D)" switches back.
- URL hashes force a mode: `…/#play` always opens the castle, `…/#plain` always opens the plain site, `…/#p-portfolio` opens the plain site scrolled to a section (`p-about`, `p-skills`, `p-resume`, `p-portfolio`, `p-compose`, `p-contact`, and `p-guestbook` when the guestbook is enabled).
- The day/night choice and an unsent guestbook draft are remembered in the visitor's browser.

---

## 7. Handy things in the browser console (F12)

```js
RIFFF.go('music')        // teleport to a room: lobby, profile, resume, portfolio, music
RIFFF.open('skills')     // open any window by section key
RIFFF.day(true)          // day; RIFFF.day(false) for night
RIFFF.music.play(2)      // play track #2 (0-based); RIFFF.music.pause()
RIFFF.plain() / RIFFF.play()
RIFFF.state              // current room, player position, nearest board
```

---

## 8. Troubleshooting

- **Blank page after editing** → a syntax error in `D`. Check for a missing comma or an unescaped `'` inside a `'…'` string.
- **A link shows but the board is missing** → the section key in `boards` doesn't match a section name (see the key list in §4).
- **Guestbook says "Could not load the notes"** → the `repo` is wrong, Issues are disabled on the repo, or the visitor hit GitHub's limit of 60 anonymous requests per hour. "Read them on GitHub" always works.
- **Don't want the guestbook at all** → `guestbook:{enabled:false, …}` (see §2.9).
- **Jukebox says "Tap ▶ to play"** → the browser blocked autoplay until the visitor interacts; pressing the button starts it. iOS Safari does this every time.
- **Jukebox says "Player unavailable"** → w.soundcloud.com is blocked (ad blocker, corporate network) — the rest of the site keeps working.
- **Music plays but the wrong song** → check the `id` numbers in `compose`; the id is what plays, the `url` is only the link.
