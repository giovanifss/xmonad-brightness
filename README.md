# xmonad-brightness
XMonad plugin for brightness control (linux). Got the idea from [xmonad-vanessa](https://hackage.haskell.org/package/xmonad-vanessa).  

**Note**: This plugin was merged in [xmonad-extras](https://github.com/xmonad/xmonad-extras), you may consider using it instead of a manual installation.

### Installation
Copy `src/XMonad/Util/Brightness.hs` to `~/.xmonad/lib/XMonad/Util/Brightness.hs`.

## Usage
This package exports three functions: `increase`, `decrease` and `change`.
- **`increase`**: Call `change` to update the brightness by *+150*;
- **`decrease`**: Call `change` to update the brightness by *-150*;
- **`change`**: Receive a `(Int -> Int)` function and perform the needed `IO` to change brightness;  

Note that **`change`** will return `IO()`. If you want to call it directly, be sure to return `X()` if whitin XMonad.

#### Example
Inside `~/.xmonad/xmonad.hs` (Manual keybindings):
```haskell
import qualified Data.Map as M
import qualified Graphics.X11.ExtraTypes.XF86 as XF86
(...)
import qualified XMonad.Util.Brightness as Bright
(...)

myKeys conf@(XConfig {XMonad.modMask = modm}) = M.fromList $
(...)
++
[ ((0, XF86.xF86XK_MonBrightnessUp), Bright.increase)        -- Increase screen brightness
, ((0, XF86.xF86XK_MonBrightnessDown), Bright.decrease)]     -- Decrease screen brightness
```
### Permissions
In a normal linux environment, the file `/sys/class/backlight/intel_backlight/brightness` will only be writable by **root**.
To allow your user to write this file, you can:
- **Recomended**: Create a group with your user and `root` and give permissions to this group to write the file;
- **Small Security risk**: Allow anyone to write the file through `646` permissions: `-rw-r--rw-`;
