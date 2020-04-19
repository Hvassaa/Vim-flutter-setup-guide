# How to setup (neo)vim for Flutter / Dart with CoC and dart-vim plugin

On Fedora but probably most distros

## Initial downloads
There are 3 things that should be downloaded:

| Program        | Download |
|----------------|----------------------------------------------------|
| Dart           | https://flutter.dev/docs/get-started/install/linux |
| Flutter        | https://dart.dev/tools/sdk/archive                 |
| Android Studio | https://developer.android.com/studio               |

Depending on the distro you might be able to find it in the repos. I just download them from the webpages, except flutter which I git clone as the page suggests.

## Unzipping/extracting
I create a directory in my home directory called sdk. I just extract and stick everything in there, so I get something like the following under ~/sdk:

    android-studio-ide-192.6392135-linux
    dartsdk-linux-x64-release
    flutter

## Android studio setup
Now install and run android studio by running:

    ~/sdk/android-studio-ide-192.6392135-linux/android-studio/bin/studio.sh

Follow the instructions. When it's finished you might need to go into the SDK manager and enable "Android SDK Tools (Obsolete)" (see https://flutter.dev/docs/get-started/install/linux#install-android-studio) and any android SDKs you want.

## Flutter setup 
Next, make sure the flutter executable is accesible from $PATH. On my system ~/.local/bin is in $PATH, so I symlink it there:

    ln -s ~/sdk/flutter/bin/flutter ~/.local/bin/flutter

and make sure ~/.local/bin actually exists.

Run

    ~/sdk/flutter/bin/flutter doctor

And check if you have any big problems, you probably don't. Some licenses may need to be accepted so do that (follow the instructions from the output).

## Dart setup
Dart is needed for the LSP-server features that we will also want with flutter. You can also symlink this to somewhere in $PATH but it is not strictly needed for flutter. If you want to use dart on its' own it's probably a good idea.

## (Neo)vim setup

### Plugins
We will use two plugins to improve our experience with flutter/dart: 

* [dart-vim-plugin](https://github.com/dart-lang/dart-vim-plugin) *provides filetype detection, syntax highlighting, and indentation for Dart code in Vim*
* [coc.nvim](https://github.com/neoclide/coc.nvim) as a LSP client

provided you use [vim-plug](https://github.com/junegunn/vim-plug) add these plugs to get them:

    Plug 'neoclide/coc.nvim', {'branch': 'release'}
	Plug 'dart-lang/dart-vim-plugin'

### coc-flutter extension
Install the coc-flutter extension by executing :CocInstall coc-flutter inside (neo)vim. See the [coc-flutter page](https://github.com/iamcco/coc-flutter) for more info.

### Coc Config
Finally, you should configure the location of the dart LSP-server by adding the following to your coc config. make shure the paths are right:

    "languageserver": {
    "dart": {
      "command": "~/sdk/dartsdk-linux-x64-release/dart-sdk/bin/dart",
      "args": [
        " change this to the path of analysis_server
        "~/sdk/dartsdk-linux-x64-release/dart-sdk/bin/snapshots/analysis_server.dart.snapshot",
        "--lsp"
      ],
      "filetypes": ["dart"],
      "disableDynamicRegister": true,
      "trace.server": "verbose"
    }
    }

If this is the only thing in your config, enclose the whole thing in a set of {}'s. (You can open your config by executing :CocConfig inside (neo)vim)
