# Pyre Windows installer (docs static asset)

Keep the latest ``Pyre-<version>-Setup.exe`` here for local Sphinx builds
(Git LFS; see repo-root ``.gitattributes``).

**GitHub Pages cannot host this file** (over the 100 MiB git limit; LFS is not
served). End-user download links in the monodoc point at the GitHub Release
asset created with the same version, for example::

    https://github.com/jamesra/nornir/releases/download/pyre-1.7.7/Pyre-1.7.7-Setup.exe

Refresh after building the installer::

    Copy-Item nornir-pyre\packaging\windows\dist\installer\Pyre-*-Setup.exe `
      docs\_static\downloads\ -Force

    gh release create pyre-<version> docs\_static\downloads\Pyre-<version>-Setup.exe `
      --repo jamesra/nornir `
      --title "Pyre <version> Windows installer" `
      --notes "Windows installer linked from nornir.github.io"

Sphinx also writes ``_static/downloads/index.html`` in the HTML output with a
redirect to that release asset.
