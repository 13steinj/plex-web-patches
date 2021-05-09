# Plex Web Patches

These are patches for any stupid UI decisions I think Plex makes to their UI.

Started because (I swear) at one point, Plex moved the multi version badge on
posters and postercards from top right to top left.

And now, they've removed this badge altogether. So for the first patch, there's
a patch to bring this badge back (either on the left, or to the best of my
ability, the right (possibly off by a few pixels right-wise from the original,
I don't know what version last had it on the right).

But if you know CSS after applying these patches you can style them further
(to an extent). Even moreso if you know JS and you're willing to read obfuscated
[webpack](https://webpack.js.org/) code!

These patches are mutually exclusive. Generally, you cannot apply more than one.
If you wish to apply more than one, feel free to repeat step 6 below with
multiple 3-way merges either via `git apply -3` and `git mergetool` or some
other merge CLI/GUI / merge manually.

**N.b.** In some patches you may see the term "react_context", this is because
I assume Plex is using React and I didn't (and stil don't) care to actually
figure it out. They might be using Vue. It doesn't matter, both use the same
concept of virtual doms, fragments, and a `createElement` function in the same
manner.

**N.b.** The patches are as tight as possible, meaning, 0 lines of context if
possible. Sometimes the patching tools can't support that, so there's 1 line of
context. These patches are generated with `git diff -U<n>` where n starts at 0
and is incremented until the patch is successful (patches with "not enough"
context spuriously fail).

Steps to install patches on PMS or Plex Desktop (Windows / Linux, other
platforms untested and may need slight changes):

Prerequisites: [git](https://git-scm.com/downloads), a source control/code
management software. As of now, you may be able to get away with the simple
[patch](https://en.wikipedia.org/wiki/Patch_\(Unix\)) utility that comes
preinstalled on Mac and Linux (and other Unix-based OSes) but because I
can't make any guarantees on future patches (and on Windows `patch` comes
with `git`, so just get `git`, dammit. You're probably going to download
the patch using `git`!

1. Find your installation directory, `cd` into it.

2. ```bash
   # for PMS
   $ cd ./Resources/Plug-ins-*/WebClient.bundle/Contents/Resources/
   # for Plex Desktop
   $ cd ./web-client
   ```
   I recommend backing up this directory before proceeding.
3. Open `index.html` in a text editor, look for a `script` tag that references
   the `./js/` folder, and figure out what version of Plex Web is being used.
4. Download the relevant patch from this repository. If you're literally
   downloading using Github, download via the `Raw` button on once you view
   the file.
5. Open up the patch, to see which JS file(s) are being edited. All JS files
   need to be put through [PrettifyJS](https://www.prettifyjs.net/), and
   replaced with the pretty-fied version of the file. I'm working to replace
   this step with some bash command(s).
6. ```bash
   # using git
   $ git init && git apply /path/to/patch/you/downloaded.patch && rm -rf .git
   # using patch
   $ patch -p1 < /path/to/patch/you/downloaded.patch 
   ```

Note: The JS files will now be unminified. There are some claims that initial
load times will significantly suffer as a result (I have not experienced this
but it is *possible*. I am working on an optional step 7 to re-minify the
files).
