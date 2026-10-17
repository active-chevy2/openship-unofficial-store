# Active Chevy 2's Unofficial Openship App Store

<a href="https://www.aihonestybadge.com" target="_blank" rel="noopener"><img src="https://www.aihonestybadge.com/badges/ai-generated-large.svg" alt="AI Generated" height="28"></a>

A community collection of JSON app definitions for [Openship](https://openship.io). Each file describes one self-hostable app: which Docker images to run, which settings the user needs to fill in, and what the user sees once the app is running.

This is an unofficial, community-maintained repository. It is not affiliated with or endorsed by the Openship project. Each app belongs to its own developers, and the logos, names and trademarks belong to their owners.

## What is included

The repository currently has 23 app definitions:

| File | App | Category | Purpose |
|---|---|---|---|
| `bento-pdf.json` | Bento PDF | other | Browser-based PDF tools with no uploads |
| `bluesky-pds.json` | Bluesky PDS | backend | Personal Data Server for the AT Protocol |
| `bookstack.json` | BookStack | cms | Wiki and documentation platform |
| `classicpress.json` | ClassicPress | cms | Lightweight WordPress-style CMS without the block editor |
| `codimd.json` | CodiMD | other | Realtime collaborative Markdown notes |
| `joplin.json` | Joplin Server | other | Sync server for the Joplin note app |
| `karakeep.json` | Karakeep | other | Bookmark manager with AI tagging and full-text search |
| `memos.json` | Memos | other | Lightweight note-taking and microblogging |
| `metube.json` | MeTube | other | Web GUI for downloading videos with youtube-dl |
| `navidrome.json` | Navidrome | other | Music streaming server with Subsonic-compatible clients |
| `ntfy.json` | ntfy | other | HTTP-based push notification service |
| `once-campfire.json` | ONCE Campfire | other | Simple group chat |
| `onetimesecret.json` | Onetime Secret | other | Share secrets through links that can be read once |
| `owncloud.json` | ownCloud | other | File sync and sharing |
| `pocket-id.json` | Pocket ID | other | OIDC provider with passkey sign-in |
| `pocketbase.json` | PocketBase | backend | Single-file backend for apps |
| `privatebin.json` | PrivateBin | other | Zero-knowledge pastebin |
| `searxng.json` | SearXNG | other | Privacy-respecting metasearch engine |
| `soju-and-gamja.json` | Soju & Gamja | other | IRC bouncer with a web client |
| `vert.json` | VERT | other | Local file converter using WebAssembly and FFmpeg |
| `wireguard-easy.json` | wg-easy | other | WireGuard VPN with a web admin UI |
| `zipline.json` | Zipline | other | ShareX and file upload server |

## How it works

Openship reads a JSON file for each app and uses it to:

1. Pull the Docker images listed under `services`.
2. Ask the user for the values in `configFields`. Fields marked `"generate": "secret"` get a random value automatically.
3. Start the containers, with health checks and restart rules.
4. Show the links and details in `connection` once the app is ready.

Every file uses the schema at `https://openship.io/app.schema.json`.

## File format

Each app file is a JSON object with these main fields:

| Field | Required | What it does |
|---|---|---|
| `$schema` | Yes | Points to the Openship schema |
| `id` | Yes | Unique app ID. This can differ from the file name, for example `bluesky-pds.json` uses `"pds"` |
| `name` | Yes | Name shown to users |
| `description` | Yes | One-line summary |
| `available` | Yes | `true` lists the app in the store, `false` hides it |
| `kind` | Yes | Currently `"template"` for every app here |
| `logo` | Yes | Logo name used by the store |
| `category` | Yes | One of `backend`, `cms`, `other` and similar groups |
| `tags` | Yes | Search keywords |
| `framework` | Yes | Currently `"docker-compose"` for every app here |
| `services` | Yes | The containers to run: image, ports, environment, volumes, health checks and restart policy |
| `configFields` | No | Settings the user fills in. Use `"secret": true` for passwords, and `"generate": "secret"` for values Openship should create |
| `connection` | Yes | Title, description and output links shown after install. Can also include a `firstLogin` note |
| `verified` | No | Marks an app that has been checked |
| `marketplace` | No | Feature highlights shown in the store |

### Example

```json
{
  "$schema": "https://openship.io/app.schema.json",
  "available": true,
  "id": "vert",
  "name": "VERT",
  "description": "The next-generation file converter. Open source, fully local and free forever.",
  "kind": "template",
  "logo": "vert",
  "category": "other",
  "tags": ["converter", "file", "documents"],
  "framework": "docker-compose",
  "services": [
    {
      "name": "vert",
      "image": "ghcr.io/vert-sh/vert:latest",
      "exposedPort": 80,
      "exposed": true,
      "restart": "unless-stopped"
    }
  ],
  "connection": {
    "title": "Open VERT",
    "description": "Your self-hosted local file converter is ready to use.",
    "outputs": [
      {
        "id": "url",
        "label": "Open VERT",
        "source": "publicUrl:vert",
        "kind": "url",
        "recommended": true
      }
    ]
  },
  "verified": true
}
```

## Adding a new app

1. Create a new file named after the app, for example `my-app.json`, in the repository root.
2. Fill in the required fields above. Use a unique `id`.
3. Pin the Docker image to a specific version tag where possible. Avoid `latest` for anything you want stable.
4. Add a `configFields` entry for every password, key or secret the app needs. Mark them with `"generate": "secret"` where Openship can create them.
5. Use `publicUrl:<service>` for links that should point to the app's public address.
6. Check the JSON is valid, for example with `python3 -m json.tool my-app.json`.
7. Open a pull request.

## Guidelines

- Use official or well-known upstream images, and say where an image comes from if it is not the project's own registry.
- Never put real passwords, keys or tokens in a file. Use `configFields` with `"generate": "secret"`.
- Keep descriptions short and factual.
- Only set `"verified": true` after you have actually tested the app install.
- If an app has a default login, say so in `connection.firstLogin` and tell the user to change it straight away.

## Licence

Add your chosen licence here. The app definitions in this repository are the work of their contributors. The apps themselves remain under their own licences.
