# TextPixels

Generate an image of a repository, visualized as 1-pixel tall text.

![TextPixels output](/example.png "Example output")

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

* [--files](#--files-files----repo-repo) FILES / [--repo](#files) REPO
* [--cols](#--cols-n) N
* [--transparent](#--transparent----alpha) / [--alpha](#--transparent----alpha)
* [--solid](#--solid----no-alpha) / [--no-alpha](#--solid----no-alpha)
* [--style](#--style-style) STYLE
* [--finish](#--finish-phase) PHASE
* [--load](#--load-file) FILE
* [--save](#--save-file) FILE
* [--known](#--known--skip-unknown) / [--skip-unknown](#--known--skip-unknown)
* [--lines](#--lines-max) MAX
* [--out](#--out-imagefile) IMAGEFILE
* [--size](#--size-wxh) WxH
* [--crop](#--crop-dims) DIMS
* [--no-crop](#--no-crop)
* [--raw](#--raw)
* [--height](#--height-n) N
* [--long](#--long)
* [--fg](#--fg-color----default-fg-color) COLOR / [--default-fg](#--fg-color----default-fg-color) COLOR
* [--bg](#--bg-color----default-bg-color) COLOR / [--default-bg](#--bg-color----default-bg-color) COLOR
* [--lang-as](#--lang-as-props) PROPS

## Option descriptions

##### `--files` FILES / `--repo` REPO

What files to process.  If neither option is given, filenames will be read from
stdin.  A directory is treated as a git repository on which `git ls-files` will
be called to find filenames.

##### `--cols` N

Number of columns per line of text.  Default: 80.

##### `--transparent` / `--alpha`

Make the default background transparent.

##### `--solid` / `--no-alpha`

Don't use transparency.  (Default is to not use transparency.)

##### `--style` STYLE

Name of a Pygments style to use for colorization.  [Try the Pygments
demo](http://pygments.org/demo) to see them.

The special value NONE will prevent any style from being used. Then you
probably also want to use [--lang-as](#--lang-as-props) for language-based colors, or
[--fg](#--fg-color----default-fg-color) and [--bg](#--bg-color----default-bg-color) for two-color output.

##### `--finish` PHASE

Only really useful with [--save](#save).

Stop after the named phase.  Mainly for debugging, but also for preprocessing a
set of files to colorize them in several different ways.

[See example below.](#preproc)

##### `--load` FILE

Load a file previously [--save](#save)'d.

##### `--save` FILE

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

##### `--known`/`--skip-unknown`

Ignore files with unknown filetype.

##### `--lines` MAX

Only render the first MAX lines of output.

##### `--out` IMAGEFILE

Output to this file.  Can use anything valid as the target of an ImageMagick
command.  See [the explanation of Output
Filename](http://www.imagemagick.org/script/command-line-processing.php#output)
in the ImageMagick documentation.

##### `--height` N

Height of an image strip.  Default: 1080.

##### `--crop` DIMS

Size of the final crop for the output image.  Default: 1920x+0+0.  See [the
ImageMagick description of
-crop](http://www.imagemagick.org/script/command-line-options.php#crop)

##### `--no-crop`

Prevents the final image crop.

##### `--long`

Sets [--no-crop](#--no-crop) and makes the height as tall as needed to fit all
file lines.

##### `--size` WxH

Sets [--height](#height) to H and [--crop](#--crop-dims) to Wx+0+0.

##### `--raw`

Prints raw RGB or RGBA (depending on [--transparency](#transparent)) pixels.

##### `--fg` COLOR / `--default-fg` COLOR

Default foreground color.  Must be specified as RRGGBB or RRGGBBAA.  (No leading hash.)

##### `--bg` COLOR / `--default-bg` COLOR

Default background color.  Must be specified as RRGGBB or RRGGBBAA.  (No leading hash.)

##### `--lang-as` PROPS

Use the GitHub language color as the specified comma-separated CSS properties.
Only `color` and `background-color` have any effect.

E.g.:

```sh
# generate a block of color for each file
bundle exec bin/textpixels --repo somerepo --style NONE --lang-as color,background-color
```
