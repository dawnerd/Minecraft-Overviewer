====================
Settings and Options
====================

Overviewer settings can be set in two places, on the command line when you run
the overviewer, or in a settings file. You specify a settings file to use with
the :option:`--settings` command line option.

.. note::
    Any command line option can optionally be set in the settings file. However,
    there are some settings that can only be set in the settings file.

.. note::
    Some options go by different names on the command line and the settings
    file. Those are noted in bold below.

The first section of this document covers command line options. The second part
covers the more advanced ways of customizing The Overviewer using settings
files.

.. contents::
    :local:

Command line options
====================

.. cmdoption:: -h, --help

    Shows the list of options and exits

.. cmdoption:: --advanced-help

    Display help - including advanced options

Useful Options
--------------

.. cmdoption:: --settings <PATH>

    Use this option to load settings from a file. For more information see the
    `Settings File`_ section below.

    **Not available in settings file (duh)**

.. cmdoption:: --north-direction <NORTH_DIRECTION>

    Specifies which corner of the screen north will point to.
    Valid options are: lower-left, upper-left, upper-right, lower-right.
    If you do not specify this option, it will default to whatever direction
    the existing map uses. For new maps, it defaults to lower-left for
    historical reasons.

    .. note::
        We define cardinal directions by the sun in game; the sun rises in the
        East and sets in the West.

    **Settings file:**
        Option name: ``north_direction``

        Format: One of the above strings.

.. cmdoption:: --rendermodes <MODE1>[,MODE2,...]

    Use this option to specify which render mode to use, such as lighting or
    night. Use :option:`--list-rendermodes` to get a list of available
    rendermodes, and a short description of each. If you provide more than one
    mode (separated by commas), Overviewer will render all of them at once, and
    provide a toggle on the resulting map to switch between them.
    
    If for some reason commas do not work for your shell (like if you're using
    Powershell on Windows), you can also use a colon ':' or a forward slash '/'
    to separate the modes.

    Incomplete list of common render-modes for your convenience:
    
    * normal,
    * lighting
    * night

    **Settings file:**
        Option name: ``rendermode`` **Note the lack of an s**

        Format: a list of strings.

        Default: ["normal"]

    See the `Render Modes`_ section for more information.

.. cmdoption:: --list-rendermodes

    List the available render modes, and a short description of each, and exit.

    **Not available in settings file**

Less Useful Options
-------------------

.. cmdoption:: --bg-color <color>

    Configures the background color for the Google Map output. Specify in
    #RRGGBB format.

    **Settings file:**
        Option name: ``bg_color``

        Format: A string in the above format.

        Default: "#1A1A1A"

.. cmdoption:: --changelist <filename>

    Outputs a list of changed tiles to the named file. If the file doesn't
    exist, it is created. If it does exist, its contents are overwritten.

    This could be useful for example in conjunction with a script to upload only
    changed tiles to your web server.

    **Settings file:**
        Option name: ``changelist``

        Format: String (path plus filename)

        Default: Not specified (no changelist outputted)

.. cmdoption:: --changelist-format <format>

    Chooses absolute or relative paths for the output with the
    :option:`--changelist` option. Valid values for format are "relative" or
    "absolute".

    **Settings file:**
        Option name: ``changelist_format``

        Format: A string (one of the above)

        Default: "relative"

.. cmdoption:: --check-terrain

    When this option appears on the command line, Overviewer prints the location
    and hash of the terrain.png it will use, and then exits.

    This is useful for debugging terrain.png path problems, especially with
    :option:`--textures-path`. Use this to see what terrain.png your current
    setup has selected.

    **Not available in settings file**

.. cmdoption:: --display-config

    Display the configuration parameters and exit. Doesn't render the map. This
    is useful to help validate a configuration setup.

    **Not available in settings file**

.. cmdoption:: --forcerender

    Force re-rendering the entire map (or the given regionlist). This
    is an easier way to completely re-render without deleting the map.

    This is useful if you change texture packs and want to re-render everything
    in the new textures, or if you're changing the :option:`--north-direction`.

    **Settings file:**
        Option name: ``forcerender``

        Format: A boolean

        Default: False

.. cmdoption:: --imgformat

    Specifies the output format for the tiles. Currently supported options are
    "png" or "jpg".

    **Settings file:**
        Option name: ``imgformat``

        Format: A string, either "png" or "jpg"

        Default: "png"

.. cmdoption:: --imgquality

    When using ":option:`--imgformat` jpg", this specifies the jpeg quality
    parameter. This can help save disk space for larger maps.

    For saving space with pngs, see :option:`--optimize-img`

    **Settings file:**
        Option name: ``imgquality``

        Format: An integer 1-100

        Default: 95

.. cmdoption:: --no-signs

    Doesn't output signs to markers.js. This has the effect of disabling signs
    on your map.

    **Settings file:**
        Option name: ``nosigns``

        Format: Boolean

        Default: False

.. cmdoption:: --optimize-img <level>

    When using ":option:`--imgformat` png" (the default), this performs file
    size optimizations on the output. The level parameter is an integer
    specifying one of the following:

    1. Run pngcrush on all tiles

    2. Run pngcrush plus advdef on all tiles

    3. Run pngcrush plus advdef with more aggressive settings.

    These options may double the time or worse it takes to render your map, and
    can be expected to give around 19-23% reduction in file size.

    These options also require the corresponding program(s) installed and in
    your system path ($PATH or %PATH% environment variable)

    **Settings file:**
        Option name: ``optimizeimg``

        Format: an integer

        Default: not set (no optimization)

.. cmdoption:: -p <PROCS>, --processes <PROCS>

    On multi-cored or multi-processor machines, The Overviewer will perform its
    work on *all* cores by default. If you want to manually specify how many
    workers to run in parallel, use this option.

    Example to run 5 worker processes in parallel::

        overviewer.py -p 5 <Path to World> <Output Directory>

    **Settings file:**
        Option name: ``procs``

        Format: an integer.

        Default: ``multiprocessing.cpu_count()``

.. cmdoption:: -q, --quiet

    Prints less output. You can specify this multiple times.

    **Settings file:**
        Option name: ``quiet``

        Format: an integer

        Default: 0

.. cmdoption:: --regionlist <regionlist>

    Use this option to specify manually a list of regions to consider for
    updating. In normal operation, every chunk in every region is checked for
    update and if necessary, re-rendered. With this option, only the specified
    chunks are checked.
    
    This option should name a file containing, 1 per line, the path to the
    region files to be considered for update.

    It's up to you to build such a list. On Linux or Mac, try using the "find"
    command. You could, for example, output all region files that are older than
    a certain date. Or perhaps you can incrementally update your map by passing
    in a subset of regions each time. It's up to you!

    **Settings file:**
        Option name: ``regionlist``

        Format: A string representing the region list file.

        Default: Scan all region files.

    .. note::
        See sample.settings.py for an example for how to build a region list
        file.

.. cmdoption:: --skip-js

    Skip the generation and output of markers.js and regions.js to the output
    directory.

    **Settings file:**
        Option name: ``skipjs``

        Format: Boolean

        Default: False

.. cmdoption:: --textures-path <path>

    Use this option to specify an alternate terrain.png to use for textures when
    rendering a world. ``path`` specifies the **containing directory** of
    terrain.png.

    The Overviewer will look for terrain.png in the following places in this
    order: path specified by this option, the program's directory, the
    overviewer_core/data/textures directory within the source directory, the
    default textures that come with Minecraft if it's installed.

    .. note::
    
        If you installed Overviewer from the Debian package, then there isn't a
        source directory; you must use this option to specify non-default
        textures.

    If you're having trouble getting The Overviewer to recognize your textures,
    see the :option:`--check-terrain` option.

    **Settings file:**
        Option name: ``textures_path``

        Format: A string (path to a dir with a terrain.png)

        Default: None

.. cmdoption:: -v, --verbose

    Prints more output. You can specify this multiple times.

    **Settings file:**
        Option name: ``verbose``

        Format: an integer

        Default: 0

.. cmdoption:: -V, --version

    Displays the version information and exits

    **Not available in settings file**

.. cmdoption:: --web-assets-path <path>

    When The Overviewer runs, it copies the files from the web_assets directory
    to the destination directory. If you wish to override these files with your
    own, for example, to make changes, you may put them in your own directory
    :make
    and specify the :option:`--web-assets-path` option.

    Files in the folder specified by ``path`` will override files from the
    web_assets directory, letting you customize the files.

    If you're running from source and are comfortable merging with Git, it may
    be better to edit the web_assets directly. If we update one of the files,
    you can use Git to merge in our changes with yours.

    If, however, you do not like Git, and don't mind having to manually update
    or merge web assets (or don't care for web asset updates at all), then copy
    all the web assets to a directory of your own and use this option.

    **Settings file:**
        Option name: ``web_assets_path``

        Format: A string (path to a directory to use for custom web assets)

        Default: Not set (no additional web assets used)

.. cmdoption:: -z <zoom>, --zoom <zoom>

    .. warning::

        This option does not do what you think it does. You almost certainly do
        not want to set this.

    This option effectively sets *how far the map can be zoomed out*. The
    Overviewer will by default determine how many *zoom levels* your map needs
    to show the entire map. This option overrides that; setting this option
    lower than automatically determined will *crop your map* and parts will be
    cut off. (We acknowledge that name zoom is misleading)

    To be precise, it sets the width and height of the highest zoom level, in
    tiles. A zoom level of z means the highest zoom level of your map will be
    2^z by 2^z tiles.

    This option map be useful if you have some outlier chunks causing your map
    to be too large, or you want to render a smaller portion of your map,
    instead of rendering everything.

    **Settings file:**
        Option name: ``zoom``

        Format: An integer.

        Default: Automatically calculated from the world size.

.. note::

    There are **more settings** that cannot be specified on the command line.
    See the section below!

Settings File
=============

You can optionally store settings in a file named settings.py (or really,
anything you want).  It is a regular python script, so you can use any python
functions or modules you want. To use a settings file, use the
:option:`--settings` command line option when you run the Overviewer.

For a sample settings file, look at 'sample.settings.py'. Note that this file is
not meant to be used directly, but instead it should be used as a collection of
examples to guide writing your own. It contains a number of examples to get you
started, but you almost certainly don't want to use it as-is.

You can specify *any of the above* options in your settings file *in addition to
the ones documented below*. For the command-line options, find its listed
"Option name" which is the Python identifier you will use. For example, if you
wanted to specify :option:`--bg-color`, you would look and see its option name
is "bg_color" (note the underscore) and you would put this line in your settings
file::

    bg_color = "#000000"

Settings file options
---------------------

In addition to the `Command line options`_, you can specify these options.


.. describe:: web_assets_hook

    This option lets you define a function to run after the web assets have
    been copied into the output directory, but before any tile rendering takes
    place. This is an ideal time to do any custom postprocessing for
    markers.js or other web assets.

    Set this identifier to a Python *function object* to be called.
    
    This function should accept one argument: a
    :class:`overviewer_core.googlemap.MapGen` object.

    .. warning::

        Currently, this option only works if the :option:`--skip-js` option is
        set

.. describe:: rendermode-options

    Different rendermodes have different options. This option is a dictionary
    that maps rendermode names to option dictionaries.

    See the `Render Modes`_ section for relevant options to the render modes.

.. describe:: custom_rendermodes

    You can also specify your own custom rendermodes with this option. This is a
    dictionary mapping your rendermode name to a dictionary of parameters to
    use.

    See the `Defining Custom Rendermodes`_ section for more information.

Render Modes
============

A rendermode is a unique way of rendering a Minecraft map. The normal render
mode was the original, and we've since added a render mode with proper lighting,
a rendermode for nighttime lighting, and we have a rendermode that only shows
caves.

Beyond that, there are also render "overlays" that can be toggled on or off,
overlaying a proper rendering. These can be used to show where minerals are and
such.

Specify your rendermodes with :option:`--rendermodes`. You can get a list of all
rendermodes installed with :option:`--list-rendermodes`.

Options and Rendermode Inheritance
----------------------------------

Each mode will accept its own options, as well as the options for
parent modes; for example the 'night' mode will also accept options
listed for 'lighting' and 'normal'. Also, if you set an option on a
mode, all its children will also have that option set. So, setting the
'edge_opacity' option on 'normal' will also set it on 'lighting' and
'night'.

Basically, each mode inherits available options and set options from
its parent.

Eventually the :option:`--list-rendermodes` option will show parent
relationships. Right now, it looks something like this:

* normal

  * lighting

    * night
    * cave

* overlay

  * spawn
  * mineral

How to Set Options
------------------

Available options for each mode are listed below, but once you know what to set
you'll have to edit your settings file to set them. Here's an example::

    rendermode_options = {
        'lighting': {
            'edge_opacity': 0.5,
        },

        'cave': {
            'lighting': True,
            'depth_tinting': False,
        },
    }

As you can see, each entry in ``rendermode_options`` starts with the mode name
you want to apply the options to, then a dictionary containing each option. So
in this example, 'lighting' mode has 'edge_opacity' set to 0.5, and 'cave' mode
has 'lighting' turned on and 'depth_tinting' turned off.

Option Listing
--------------

Soon there should be a way to pull out supported options from Overviewer
directly, but for right now, here's a reference of currently supported options.

normal
~~~~~~

* **edge_opacity** - darkness of the edge lines, from 0.0 to 1.0 (default: 0.15)
* **min_depth** - lowest level of blocks to render (default: 0)
* **max_depth** - highest level of blocks to render (default: 127)
* **height_fading** - darken or lighten blocks based on height (default: False)

lighting
~~~~~~~~

all the options available in 'normal', and...

* **shade_strength** - how dark to make the shadows, from 0.0 to 1.0 (default: 1.0)

night
~~~~~

'night' mode has no options of its own, but it inherits options from
'lighting'.

cave
~~~~

all the options available in 'normal', and...

* **depth_tinting** - tint caves based on how deep they are (default: True)
* **only_lit** - only render lit caves (default: False)
* **lighting** - render caves with lighting enabled (default: False)

mineral
~~~~~~~

The mineral overlay supports one option, **minerals**, that has a fairly
complicated format. **minerals** must be a list of ``(blockid, (r, g, b))``
tuples that tell the mineral overlay what blocks to look for. Whenever a block
with that block id is found underground, the surface is colored with the given
color.

See the *settings.py* example below for an example usage of **minerals**.

Defining Custom Rendermodes
---------------------------

Sometimes, you want to render two map layers with the same mode, but with two
different sets of options. For example, you way want to render a cave mode with
depth tinting, and another cave mode with lighting and no depth tinting. In this
case, you will want to define a 'custom' render mode that inherits from 'cave'
and uses the options you want. For example::

    custom_rendermodes = {
        'cave-lighting': {
            'parent': 'cave',
            'label': 'Lit Cave',
            'description': 'cave mode, with lighting',
            'options': {
                'depth_tinting': False,
                'lighting': True,
            }
        },
    }

    rendermode = ['cave', 'cave-lighting']

Each entry in ``custom_rendermodes`` starts with the mode name, and is followed
by a dictionary of mode information, such as the parent mode and description
(for your reference), a label for use on the map, as well as the options to
apply.

Every custom rendermode you define is on exactly equal footing with the built-in
modes: you can put them in the ``rendermode`` list to render them, you can
inherit from them in other custom modes, and you can even add options to them
with ``rendermode_options``, though that's a little redundant.

Example *settings.py*
---------------------

This *settings.py* will render three layers: a normal 'lighting' layer, a 'cave'
layer restricted to between levels 40 and 55 to show off a hypothetical subway
system, and a 'mineral' layer that has been modified to show underground rail
tracks instead of ore.

::

    rendermode = ['lighting', 'subway-cave', 'subway-overlay']

    custom_rendermodes = {
        'subway-cave' : {'parent' : 'cave', 
                         'label' : 'Subway',
                         'description' : 'a subway map, based on the cave rendermode',
                         'options' : {
                             'depth_tinting' : False,
                             'lighting' : True,
                             'only_lit' : True,
                             'min_depth' : 40,
                             'max_depth' : 55,
                         }
        },
        'subway-overlay' : {'parent' : 'mineral',
                            'label' : 'Subway Overlay',
                            'description' : 'an overlay showing the location of minecart tracks',
                            'options' : {'minerals' : [
                                (27, (255, 234, 0)),
                                (28, (255, 234, 0)),
                                (66, (255, 234, 0)),
                            ]}
        },
    }

    rendermode_options = {
        'lighting' : {'edge_opacity' : 0.5},
    #    'night' : {'shade_strength' : 0.5},
    #    'cave' : {'only_lit' : True, 'lighting' : True, 'depth_tinting' : False},
    }