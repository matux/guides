Format
------
* **File Format:** PNG
* **Depth:**
  * 8-bit indexed for small or simple images.
  * 24-bit for photographs or color-rich graphics.
* **Alpha:** Save with Alpha/Transparency only if needed.
* **Metadata:** Avoid adding metadata except Color Profile information.

Color Space
-----------
* **Gamut:** sRGB
* **Profile:** sRGB IEC61966-2.1
  * Must Tag/Embed color profile in PNG.

Sizes and Naming
----------------
* All images must come in 3 distinct sizes.
  * 1x, 2x and 3x
  * Example:
    * 1x: 50x50
    * 2x: 100x100
    * 3x: 150x150
* Suffix PNG filename according to size.
  * @1x, @2x, @3x
  * Example:
    * `MyImage@1x.png`
    * `MyImage@2x.png`
    * `MyImage@3x.png`
* App icons and app launch images have special considerations, see below.

App Icons
---------

| Purpose           | Size @1x | Size @2x | Size @3x | Filename                  |
| ----------------- |:--------:|:--------:|:--------:|:-------------------------:|
| App Icon          | -        | 120x120 | 180x180  | `Icon-60(@2x/@3x).png`     |
| App iPad Icon     | 76x76    | 152x152 | -        | `Icon-76(@1x/@2x).png`     |
| App iPad Pro Icon | -        | 167x167 | -        | `Icon-83.5@2x.png`         |
| Notifications     | 20x20    | 40x40   | 60x60    | `Icon-20(@1x/@2x/@3x).png` |
| Settings          | 29x29    | 58x58   | 87x87    | `Icon-29(@1x/@2x/@3x).png` |
| Spotlight         | 40x40    | 80x80   | 120x120  | `Icon-40(@1x/@2x/@3x).png` |
| Carplay           | -        | 120x120 | 180x180  | `Icon-120(@2x/@3x).png`    |

Notes:
* Even if the app doesn't support iPad, it can still be downloaded on iPad, so providing iPad icons is advisable.

App Launch Image
----------------

| Device               | Size | Resolution  | Filename              |
| -------------------- |:----:|:-----------:|:---------------------:|
| iPhone 4/4S          | 3.5" | 640x960px   | `LaunchImage@3.5.png` |
| iPhone 5/5S/5C/SE    | 4.0" | 640x1136px  | `LaunchImage@4.0.png` |
| iPhone 6/6S/7/8      | 4.7" | 760x1334px  | `LaunchImage@4.7.png` |
| iPhone 6/6S/7/8 Plus | 5.5" | 1242x2208px | `LaunchImage@5.5.png` |
| iPhone X             | 5.8" | 1125x2436px | `LaunchImage@5.8.png` |
