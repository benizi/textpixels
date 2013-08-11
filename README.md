# TextPixels

Generate an image of a repository, visualized as 1-pixel tall text.

# Installation

This requires ImageMagick, if you want images instead of raw pixels.

This requires that the Python Pygments `pygmentize` command is in your PATH.
If you don't have Pygments installed, just run:

```sh
pip install pygments
```

This also requires Ruby 2.0.  (The language detection libs require it.)

Then, just check out this repo and install the Ruby dependencies with Bundler:

```sh
bundle
```

# Usage

Simple example to render a repository at 1920x1080, 80-column lines, PNG output.

```sh
bundle exec bin/textpixels --repo /path/to/repository --out graphic.png
```

# Options

* [--files](#files) FILES / [--repo](#files) REPO
* [--cols](#cols) N
* [--transparent](#transparent) / [--alpha](#transparent)
* [--solid](#solid) / [--no-alpha](#solid)
* [--style](#style) STYLE
* [--finish](#finish) PHASE
* [--load](#load) FILE
* [--save](#save) FILE
* [--known](#known) / [--skip-unknown](#known)
* [--lines](#lines) MAX
* [--out](#out) IMAGEFILE
* [--size](#size) WxH
* [--crop](#crop) DIMS
* [--no-crop](#no-crop)
* [--raw](#raw)
* [--height](#height) N
* [--long](#long)
* [--fg](#fg) COLOR / [--default-fg](#default-fg) COLOR
* [--bg](#bg) COLOR / [--default-bg](#default-bg) COLOR
* [--lang-as](#lang-as) PROPS

## Option descriptions

* <a id="files"></a>`--files` FILES / [--repo](#files) REPO

What files to process.  If neither option is given, filenames will be read from
stdin.  A directory is treated as a git repository on which `git ls-files` will
be called to find filenames.

* <a id="cols"></a>`--cols` N

Number of columns per line of text.  Default: 80.

* <a id="transparent"></a>`--transparent` / [--alpha](#transparent)

Make the default background transparent.

* <a id="solid"></a>`--solid` / [--no-alpha](#solid)

Don't use transparency.  (Default is to not use transparency.)

* <a id="style"></a>`--style` STYLE

Name of a Pygments style to use for colorization.  [Try the Pygments
demo](http://pygments.org/demo) to see them.

The special value NONE will prevent any style from being used. Then you
probably also want to use [--lang-as](#lang-as) for language-based colors, or
[--fg](#fg) and [--bg](#bg) for two-color output.

* <a id="finish"></a>`--finish` PHASE

Only really useful with [--save](#save).

Stop after the named phase.  Mainly for debugging, but also for preprocessing a
set of files to colorize them in several different ways.

[See example below.](#preproc)

* <a id="load"></a>`--load` FILE

Load a file previously [--save](#save)'d.

* <a id="save"></a>`--save` FILE

Save the results of processing in the named file.  Mostly for debugging.  But
can be used for caching intermediate stages for multiple uses.  E.g.:

<a id="preproc"></a>

```sh
# save the results of parsing all the files into HTML
bundle exec bin/textpixels --repo some/large/repo --finish htmlize --save HTML

# generate output with two different styles
bundle exec bin/textpixels --load HTML --style fruity --out fruity.png
bundle exec bin/textpixels --load HTML --style monokai --out monokai.png
```

* <a id="known"></a>`--known` / [--skip-unknown](#known)

Ignore files with unknown filetype.

* <a id="lines"></a>`--lines` MAX

Only render the first MAX lines of output.

* <a id="out"></a>`--out` IMAGEFILE

Output to this file.  Can use anything valid as the target of an ImageMagick
command.  See [the explanation of Output
Filename](http://www.imagemagick.org/script/command-line-processing.php#output)
in the ImageMagick documentation.

* <a id="height"></a>`--height` N

Height of an image strip.  Default: 1080.

* <a id="crop"></a>`--crop` DIMS

Size of the final crop for the output image.  Default: 1920x+0+0.  See [the
ImageMagick description of
-crop](http://www.imagemagick.org/script/command-line-options.php#crop)

* <a id="no-crop"></a>`--no-crop`

Prevents the final image crop.

* <a id="long"></a>`--long`

Sets [--no-crop](#no-crop) and makes the height as tall as needed to fit all
file lines.

* <a id="size"></a>`--size` WxH

Sets [--height](#height) to H and [--crop](#crop) to Wx+0+0.

* <a id="raw"></a>`--raw`

Prints raw RGB or RGBA (depending on [--transparency](#transparent)) pixels.

* <a id="fg"></a>`--fg` COLOR / [--default-fg](#default-fg) COLOR

Default foreground color.  Must be specified as RRGGBB or RRGGBBAA.  (No leading hash.)

* <a id="bg"></a>`--bg` COLOR / [--default-bg](#default-bg) COLOR

Default background color.  Must be specified as RRGGBB or RRGGBBAA.  (No leading hash.)

* <a id="lang"></a>`--lang-as` PROPS

Use the GitHub language color as the specified comma-separated CSS properties.
Only `color` and `background-color` have any effect.

E.g.:

```sh
# generate a block of color for each file
bundle exec bin/textpixels --repo somerepo --style NONE --lang-as color,background-color
```
