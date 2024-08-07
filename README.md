# Tracking repository for Chromium

## Standard Operation Procedure

1. Download a source archive of Chromium from <https://commondatastorage.googleapis.com/chromium-browser-official/>. Normally the URL is <https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$VER.tar.xz>, replacing "$VER" with the version to be imported, like `127.0.6533.88`.
2. Switch to the `tarball` branch (`git checkout tarball`), then remove all files and directories in the working tree, including the hidden one, except the `.git` directory.
3. Extract all files from the archive, then move all files and directories, including the hidden one, from the `chromium-$VER` directory to the working tree.
4. Recursively remove `.gitattributes` files, `.gitignore` files, `.gitmodules` files and `.git` directories except the one in the top-level (`find . -name .gitattributes -o .gitignore -o .gitmodules -delete; find . -name .git`, note that `.git` directories have to be manually removed).
5. Update the index using all files in the entire working tree (`git add -A`), then create a new commit with message of "chore: import v$VER" (`git commit -m "chore: import v$VER"`).
6. Create a lightweight tag with name of "v$VER" (`git tag "v$VER"`).
7. Push the `tarball` branch and the tag just created to the remote (`git push; git push --tags`).
8. Create a new branch with name of "aosc/v$VER", pointing to the current `HEAD`, then switch to it (`git checkout -b "aosc/v$VER"`).
9. Apply all needed commits and/or patches.
10. Create a lightweight tag with name of "aosc/v$PKGEPOCH%$PKGVER-$PKGREL" (`git tag "aosc/v$PKGEPOCH%$PKGVER-$PKGREL"`). If any of these variables equals zero, omit the related section and the corresponding separator. For the definition of these variables, refer to [autobuild4](https://github.com/AOSC-Dev/autobuild4).
11. Push the `aosc/v$VER` branch and the tag just created to the remote (`git -c "push.autoSetupRemote=true" push; git push --tags`).
12. Prepare patches for the ABBS tree (`git format-patch "tags/v$VER..tags/aosc/v$PKGEPOCH%$PKGVER-$PKGREL"`).

## References

- [Download Chromium](https://www.chromium.org/getting-involved/download-chromium/)
