FontPro source distribution
===========================

FontPro provides scripts that automatically generate all necessary
files to use Adobe Minion Pro, Adobe Myriad Pro and Adobe Cronos Pro
with (pdf)latex.  In contrast to other solutions, the supplied LaTeX
packages provide extensive math support in the case of Minion and
Myriad.  The swash letters of CronosPro can be used as mathcal symbols
with MyriadPro's math (option 'crswash').


Requirements
------------

The scripts need the [LCDF Typetools](http://www.lcdf.org/type/),
version 2.85 or later, to run.  Furthermore, the following LaTeX
packages, which are currently included in a complete TeXLive or MikTeX
installation, are necessary:

* fontinst v1.925 or better
* fontaxes package
* MnSymbol for MinionPro, MdSymbol for MyriadPro
* fltpoint package
* tabfigures package (optional)


Building
--------

The following describes the build process. Here, `$` at the beginning
of the line indicates the shell command prompt. Under Windows,
[Cygwin](http://www.cygwin.com/) is required; either install the LCDF
type tools in Cygwin from source or install the
`texlive-font-fontutils` package.

1. Put the sources in a folder.
2. Create an `otf` directory in that folder and copy your OpenType
   font files into the `otf` directory:

        $ mkdir otf
        $ cp /some/path/*.otf otf

   MinionPro and MyriadPro font files normally follow the naming
   scheme assumed in the scripts. Make sure that the CronosPro files
   are of the form

        $FONT-$WEIGHT$SHAPE$OPTICAL.otf

   resulting in for example

        CronosPro-BoldCapt.otf
        CronosPro-BoldDisp.otf
        CronosPro-BoldItCapt.otf
        CronosPro-BoldItDisp.otf
        CronosPro-BoldIt.otf
        CronosPro-BoldItSubh.otf
        CronosPro-Bold.otf
        CronosPro-BoldSubh.otf
        CronosPro-Capt.otf
        CronosPro-Disp.otf
        CronosPro-ItCapt.otf
        CronosPro-ItDisp.otf
        CronosPro-It.otf
        CronosPro-ItSubh.otf
        CronosPro-Regular.otf
        CronosPro-SemiboldCapt.otf
        CronosPro-SemiboldDisp.otf
        CronosPro-SemiboldItCapt.otf
        CronosPro-SemiboldItDisp.otf
        CronosPro-SemiboldIt.otf
        CronosPro-SemiboldItSubh.otf
        CronosPro-Semibold.otf
        CronosPro-SemiboldSubh.otf
        CronosPro-Subh.otf 

3. Run the main script `makeall`:

        $ ./scripts/makeall $FONT

   Currently, `$FONT` can be MinionPro, MyriadPro or CronosPro.

   `makeall` has some options to specify which metrics to create:

        --verbose            show warning messages from otftotfm
        --quiet              omit these messages (default)
        --smallcaps          create small caps metrics (default)
        --nosmallcaps        turn --smallcaps off
        --swash              create swash metrics (default)
        --noswash            turn --swash off
        --greek              create greek metrics (default)
        --nogreek            turn --greek off
        --cyrillic           create cyrillic metrics (default)
        --nocyrillic         turn --cyrillic off
        --vietnamese         create vietnamese metrics (default)
        --novietnamese       turn --vietnamese off
        --expanded           create expanded metrics for pdftex
        --noexpanded         turn --expanded off (default)
        --kern               add some missing kerning pairs (default)
        --nokern             use the original Adobe kerning
        --wide-spacing       increase the sidebearings of quote glyphs
        --narrow-spacing     turn --wide-spacing off (default)
        --pack=<glyph-list>  explicitly name the glyph list

   The glyph list of the last option is used to reduce the size of the
   map file. The default behaviour without the `--pack` option is to
   use the glyph list that fits to your font version; if this glyph
   list is not available in the scripts folder, it can be generated
   using `generate-glyph-list.sh` _after_ a successful build.


Installing
----------

Under Linux-like environments, use the following commands to install
the font:

1. Run the `install` script. You can specify the directory where the
   files should be put. The default is `$TEXMFLOCAL`.

        $ ./scripts/install /usr/local/share/texmf

   (If you use a TeX distribution older than 2005 which does not
   follow TDS v1.1, uncomment the "old directory folders" in the
   install script!)

2. Add `$FONT.map` to the default font map files by

        $ updmap --enable Map=$FONT.map

   For a system-wide installation, use `updmap-sys` instead of
   `updmap`.

Under Windows with MikTeX, do the following: 

1. Run in the Cygwin Bash

        $ ./scripts/install /cydrive/c/path/to/Miktex

   where `c/path/to/Miktex` is the path to the Miktex installation
   tree of choice.

2. In the Windows prompt, issue

        initexmf -u
        initexmf --admin --edit-config-file updmap

   Add the following in the editor:

        Map $FONT.map

   and save. Afterwards, run

        initexmf --mkmaps


Cleaning up of the building folder
----------------------------------

If you want to install another font, it is recommended to do a

    $ ./scripts/clean

or to start from a fresh building folder.


Usage
-----

Use the FONT with

    \usepackage{FONT}

All valid options are included in the documentation, which is provided
in the `tex` folder and which can also be generated _after_ installing
with

    latex $FONT.dtx

If you get errors about insufficient memory you might need to increase
the values of `font_mem_size` and `font_max` in your TeX
configuration. For most distributions, these can be changed in the
file `texmf/web2c/texmf.cnf`. For MiKTeX the file is called
`miktex.ini`.


Maintainer and acknowledgement
------------------------------

The scripts and LaTeX packages are maintained by Sebastian Schubert
<schubert.seb@googlemail.com>.

They have been forked from the amazing [MinionPro scripts and
package](http://developer.berlios.de/projects/minionpro) written by
Achim Blumensath, Andreas Bühmann and Michael Zedler.

The part of this README concerning Cygwin is based on [this
entry](http://tex.stackexchange.com/a/87568/11605).

All files are public domain.
