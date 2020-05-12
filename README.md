Linux Audio base extension
==========================

This is a base extension for Flatpak Linux audio plugins to allow
building Linux audio plugins flatpaks.

**This package only provides a definition of the extension points which
are a prerequisite to build the plugins.** It doesn't provide anything
else, and applications that want to use audio plugins do not need to
use it. Just copy the extension point definitions to your manifest.

It currently supports:

- LV2 as a `org.freedesktop.LinuxAudio.Lv2Plugins` extension.
- DSSI as a `org.freedesktop.LinuxAudio.DssiPlugins` extension.
- LADSPA as a `org.freedesktop.LinuxAudio.LadspaPlugins` extension.
- VST (Linux) as a `org.freedesktop.LinuxAudio.VstPlugins` extension.
- VST3 as a `org.freedesktop.LinuxAudio.Vst3Plugins` extension.

Content
-------

This is empty, it only provide the needed declaration for the
extension points.

Application using audio plugins
-------------------------------

Your appication supports audio plugins?

Add the extension points for plugins. Below is an example for LV2 plugins:

```
"add-extensions": {
  "org.freedesktop.LinuxAudio.Lv2Plugins": {
    "directory": "extensions/Lv2Plugins",
    "version": "19.08",
    "add-ld-path": "lib",
    "merge-dirs": "lv2",
    "subdirectories": true,
    "no-autodownload": true
  }
}
```

The manifest of this flatpak has them all.

And make sure the application find the LV2 plugins by putting the
following finish argument:

```
"--env=LV2_PATH=/app/extensions/Lv2Plugins/lv2"
```

The manifest of this flatpak has them all.

For DSSI, LADSPA and VST it is the same change as above. It's actually
recommended to add them all if you support LV2 as using a LV2 plugin
like Carla, you can use the others formats.

The table below summarie the mount point and environment to set. The
mount point is a sub directory to `/app/extensions`. The subdir is a
subdirectory in the mount point that will have all the plugins as
needed by the application host.


| Format     | Ext point name | mount point | subdir | env          |
+------------+----------------+-------------+--------+--------------+
| LV2        | Lv2Plugins     | Lv2Plugins  | lv2    | `LV2_PATH`   |
| DSSI       | DssiPlugins    | DssiPlugins | dssi   | `DSSI_PATH`  |
| LADSPA     | LadspaPlugins  | LaspaPlugins| ladspa | `LADSPA_PATH`|
| VST (Linux)| VstPlugins     | VstPlugins  | lxvst  | `LXVST_PATH` and `VST_PATH` |
| VST3       | Vst3Plugins    | Vst3Plugins | vst3   | `VST3_PATH`  |


Runtime considerations
----------------------

Currently the base runtime is Freedesktop 19.08. Plugins and
applications have to use the same runtime so you should consider this
when upgrading the runtime on your application flatpak.

When moving to Freedestkop 20.08, branches will have to be created to
keep the older versions of the plugins available. As an application
you might want to consider when you want to do the upgrade.

The `version` specified for the extension point of the application has
to match the underlying freedesktop SDK it uses and plugins are built
using the `branch` with the same name. This will allow transitioning
to a new runtime without breaking applications using the older
runtime.

Plugins
-------

You want to provide a Plugin as a Flatpak package? Build a
an extension, using the base app.

LV2: `org.freedesktop.LinuxAudio.Lv2Plugins`
DSSI: `org.freedesktop.LinuxAudio.DssiPlugins`
LADSPA: `org.freedesktop.LinuxAudio.LadspaPlugins`
VST: `org.freedesktop.LinuxAudio.VstPlugins`
VST3: `org.freedesktop.LinuxAudio.Vst3Plugins`


