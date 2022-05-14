# Blender Add-on: Reload Add-on

Quickly reload a single, configured add-on (*Ctrl+Alt+R*). It can also be used to easily reload all scripts (*Ctrl+Alt+Shift+R*).

While Blender provides the `script.reload()` operation to reload *all* scripts, it can be a bit slow to execute when testing simple changes and there is no hotkey provided for it. To speed up development a tiny bit, I wrote a Blender operation that quickly unregisters, reloads, and re-registers a configured add-on package. It's noticeably faster that a full script reload and will not affect the state of other add-ons.

## Installation

* Download the latest add-on build from [here](https://github.com/hextantstudios/hextant_reload_addon/releases/latest/download/hextant_reload_addon.zip) or clone it using *git* to your custom Blender `...\scripts\addons\` folder.
* From Blender's Main Menu:
  * *Edit / Preferences*
  * Click the *Install* button and select the downloaded zip file.
  * Check the check-box next to the addon to activate it.
  * Configure the package name for the add-on you wish to quick reload. (This is the add-on's directory name or single-file name.)
    * Note that this does not have to be done to simply use the reload *all* scripts hotkey.

## Use

* To reload the configured add-on package, press: *Ctrl+Alt+R*
* To reload all scripts: *Ctrl+Alt+Shift+R*

**Important:** Blender does *not* reload sub-modules automatically, so if you are developing a multi-module (multi-file) add-on, you will likely need to use the [`register_submodule_factory()`](https://docs.blender.org/api/current/bpy.utils.html#bpy.utils.register_submodule_factory) utility method for each module that may change during development. This modules *must* have a global `register()` and `unregister()` method (though they be empty) as the registration method attempts to call them. Something similar to the following in your `__init__.py` file:

```python
# __init__.py
import bpy

register, unregister = bpy.utils.register_submodule_factory(__package__, (
    'my_first_sub_module', 
    'my_second_sub_module'
))
```

## Known Issues

* If another add-on uses your add-on it is likely that you will need to reload *all* scripts  (*Ctrl+Alt+Shift+R*) so that the instance the other add-on sees is the correct (newly update) one.
* If a sub-module throws an exception during its `unregister()` call, you will likely need to fix the issue and reload *all* scripts or restart Blender as `register_submodule_factory()` does not continue unregistering other modules after the exception.

## License

This work is licensed under [GNU General Public License Version 3](https://download.blender.org/release/GPL3-license.txt).