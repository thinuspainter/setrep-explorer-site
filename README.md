# SETREP Settlement Config Explorer — site

One GitHub Pages site, one folder per configuration / config manager. Each folder is a
self-contained, password-protected dependency explorer. Configs are parsed entirely in the
browser — no data is uploaded anywhere.

```
.
├── index.html          ← landing page (links to each config)
├── default/
│   └── index.html      ← locked explorer, sample config preloaded
└── <new-config>/
    └── index.html      ← copy of the explorer for the next manager
```

## Deploy to GitHub Pages

1. Create a repository (keep it **private** if the configs are sensitive — see Security below).
2. Upload these files preserving the folder structure (`index.html` at the root, `default/index.html`, etc.).
3. Repo **Settings → Pages → Build and deployment → Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. After ~1 minute the site is live at `https://<user-or-org>.github.io/<repo>/`.
   - Landing page: `…/<repo>/`
   - Default explorer: `…/<repo>/default/`

## Add a new configuration / manager

1. Copy the `default/` folder to a new lowercase folder, e.g. `config-a/`.
2. Replace `config-a/index.html` with a locked build for that config:
   - **Preloaded** (recommended): provide that config's four tables and a preloaded+locked
     build is generated for you, then dropped in as `config-a/index.html`.
   - **Upload-only**: just reuse the existing locked build — the manager loads their own
     export via the **Upload config** button each visit.
3. Duplicate a card in the root `index.html` with `href="config-a/"`.
4. Commit & push.

## Passwords

The explorers are encrypted with [StaticCrypt](https://github.com/robinmoisson/staticrypt)
(AES + PBKDF2). The password is **not** recoverable from the file.

- Default build password: stored separately (ask the tool owner).
- Use one shared password, or a different password per folder if managers should not see
  each other's configs.

## Security note

GitHub Pages on a **public** repo serves files to anyone with the URL. StaticCrypt protects
the content at rest, but the encrypted payload is still downloadable and a determined viewer
could attempt an offline password attack. For sensitive settlement configs, use a **private**
repo with Pages (requires GitHub Team/Enterprise) or an access-controlled internal host.
