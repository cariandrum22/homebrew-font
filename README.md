homebrew-font
=============

# Installing Ricty fonts

Recommended installation by tapping the repository

```
$ brew tap sanemat/font
$ brew install ricty
# (generate)
$ cp -f /PATH/TO/RICTY/fonts/Ricty*.ttf ~/Library/Fonts/
$ fc-cache -vf
```

Alternate method to install without tapping the repository

```
$ brew install https://raw.github.com/sanemat/homebrew-font/master/ricty.rb
# (generate)
# After this, same as above.
```

# Troubleshooting

## The dependencies require that I install XQuartz

Fontforge's dependencies, Cairo and Pango, install with X11 support by default and depend on XQuartz, so you might get an error like the following:

```
cairo: XQuartz is required to install this formula.
You can install with Homebrew Cask:
  brew install Caskroom/cask/xquartz

You can download from:
  https://xquartz.macosforge.org
pango: XQuartz is required to install this formula.
You can install with Homebrew Cask:
  brew install Caskroom/cask/xquartz

You can download from:
  https://xquartz.macosforge.org
Error: Unsatisified requirements failed this build.
```

Cairo and Pango can be manually installed without X11 support, which will eliminate the need for XQuartz.
To install without XQuartz, execute the following:

```
brew install --without-x11 cairo && brew install --without-x11 pango
brew install ricty
```

## I get a Python error during `brew install fontforge`

If you get an error during `brew install fontforge`, like below:

```
==> Patching
==> ./bootstrap
==> ./configure --prefix=/Users/sane/.homebrew/Cellar/fontforge/20141230 --disable-silent-rules --without-x --without-giflib --without-libspiro
installed software in a non-standard prefix.

Alternatively, you may set the environment variables PYTHON_CFLAGS
and PYTHON_LIBS to avoid the need to call pkg-config.
See the pkg-config man page for more details.
```

### Quick fix (work around)

Execute the following:

```
# Install python before installing fontforge and ricty
brew install python
# Then install fontforge and ricty
brew install ricty
```

### Long help (my environment)

I use homebrew of course, but I don't use homebrew's python. I use pyenv via anyenv. In the homebrew build script, your `PKG_CONFIG_PATH` is not used; there is homebrew's `PKG_CONFIG_PATH`. You can try this `brew sh` and `printenv | grep PKG_CONFIG_PATH`.

The [fontforge configuration script](https://github.com/fontforge/fontforge/blob/7432f9a102f0f4268c5caabbb4f55d3ac33b0d0d/configure.ac#L217-L230) doesn't detect pyenv's python (I think, but I'm not famliar with autoconf yet, sorry). I don't understand why this doesn't detect the system python (after I execute `pyenv global system`).

The following is the debug output of `brew install fontforge --verbose --debug`:

```
detected a homebrew build environment
./configure: line 18044: /Users/sane/.homebrew/bin/python: No such file or directory
./configure: line 18045: cd: /../../pkgconfig: No such file or directory
found python pkg_config information:
./configure: line 18048: y: command not found
checking for a Python interpreter with version >= 2.7... python
checking for python... /Users/sane/.anyenv/envs/pyenv/versions/2.7.6/bin/python
checking for python version... 2.7
checking for python platform... darwin
checking for python script directory... ${prefix}/lib/python2.7/site-packages
checking for python extension module directory... ${exec_prefix}/lib/python2.7/site-packages
checking for PYTHON... no
configure: error: Package requirements (python-"2.7") were not met:

No package 'python-2.7' found

Consider adjusting the PKG_CONFIG_PATH environment variable if you
installed software in a non-standard prefix.

Alternatively, you may set the environment variables PYTHON_CFLAGS
and PYTHON_LIBS to avoid the need to call pkg-config.
See the pkg-config man page for more details.
```

The actual solution is detecting system python here, but I don't know how to detect this yet.
