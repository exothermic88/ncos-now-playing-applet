# ncos-now-playing-applet

Packaging for [Now Playing](https://github.com/stldave314/cosmic-media-now-playing-applet)
(`cosmic-media-now-playing-applet`) — a COSMIC panel applet that shows the
currently playing media track via MPRIS, with playback controls — as the
`ncos-now-playing-applet` pacman package for the
[ncos repo](https://github.com/exothermic88/ncos-repo).

This repo holds only the PKGBUILD: the source is downloaded from the pinned
upstream release tag (`v$pkgver`).

## Build

```sh
makepkg -srf
```

## Update to a new upstream release

1. Bump `pkgver` in the PKGBUILD (and reset `pkgrel=1`).
2. `updpkgsums`
3. `makepkg -srf`

## Publish to the ncos repo

```sh
cp ncos-now-playing-applet-<ver>-<rel>-x86_64.pkg.tar.zst ~/claude_projects/ncos-repo/x86_64/
cd ~/claude_projects/ncos-repo
./rename   # repo-add + db rename
git add . && git commit -m "Add ncos-now-playing-applet <ver>-<rel>"
./push
```

Do not ship the `-debug` package.
