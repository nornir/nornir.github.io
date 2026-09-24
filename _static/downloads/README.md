# Pyre Windows installer (docs static asset)

Host the latest ``Pyre-<version>-Setup.exe`` here so Sphinx copies it to
``_static/downloads/`` on https://nornir.github.io/.

Refresh after building the installer::

    Copy-Item nornir-pyre\packaging\windows\dist\installer\Pyre-*-Setup.exe `
      docs\_static\downloads\ -Force

Track ``*.exe`` with Git LFS (see repo-root ``.gitattributes``). Sphinx creates a
stable ``Pyre-Setup.exe`` alias in the HTML output at build time.
