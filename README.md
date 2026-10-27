# grablogic.com

Static website for GrabLogic, served by GitHub Pages. Plain HTML and CSS — no build step,
no frameworks, no external fonts or scripts.

```
index.html                     Home: the list of apps
cryptoplayground.html          CryptoPlayground page
site.css                       All styles
cp-*.jpg, cp-diagram.png       CryptoPlayground screenshots
cryptoplayground-icon.png      CryptoPlayground icon
CryptoPlayground.dmg           The macOS download
CryptoPlayground-1.0-setup.exe The Windows installer
cpweb/                         CryptoPlayground running in the browser (WebAssembly)
CNAME                          Custom domain (grablogic.com)
.nojekyll                      Serve files as they are, without Jekyll
```

## Preview locally

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## Publish on GitHub Pages

1. This folder is the repository `github.com/grablogic/grablogicweb`. Commit and push:
   ```sh
   git add .
   git commit -m "GrabLogic website"
   git push -u origin main
   ```
2. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   branch `main`, folder `/ (root)`, Save. The site appears at `https://grablogic.github.io/grablogicweb/`
   within a minute or two.
3. Custom domain: the `CNAME` file already contains `grablogic.com`. At your domain registrar add
   - four `A` records for `grablogic.com`: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - a `CNAME` record for `www` pointing to `grablogic.github.io`

   Then in **Settings → Pages** check that the custom domain shows `grablogic.com` and tick
   **Enforce HTTPS** once the certificate is issued (can take up to an hour after DNS works).

## Updating the CryptoPlayground download

Build a signed and notarized release in the CryptoPlayground project (`scripts/build-release.sh`), then

```sh
cp ../cryptoplayground/dist/CryptoPlayground.dmg .
```

and update the version and size on `cryptoplayground.html`.

For Windows, build the installer on a Windows machine (`scripts\build-windows.ps1`), copy
`CryptoPlayground-<version>-setup.exe` here, and update its file name and size on
`cryptoplayground.html`.

For the browser version, build it in the CryptoPlayground project (`scripts/build-web.sh`), then

```sh
cp ../cryptoplayground/dist/web/* cpweb/
```

The disk image is committed to the repository (GitHub allows files up to 100 MB). To keep the
repository small over many releases, attach the `.dmg` to a GitHub Release instead and change the
download links to `https://github.com/grablogic/grablogicweb/releases/latest/download/CryptoPlayground.dmg`.
