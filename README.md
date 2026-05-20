# Bruno CLI Docker Demo

A ready-to-run [Docker Compose](https://docs.docker.com/compose/) demo of the official [Bruno CLI](https://docs.usebruno.com/bru-cli/overview) Docker image. Clone, run one command, watch a real Bruno API collection execute inside a container.

Intended for trials, sales walkthroughs, prospect demos, and quick sanity checks of the `usebruno/cli` Docker image.

---

## Quick start

> Works identically on macOS, Linux, and Windows. The collection is mounted via Docker Compose using a relative path — no shell-specific `$(pwd)` / `${PWD}` / `%cd%` substitution needed.

```bash
git clone https://github.com/bruno-collections/bruno-cli-docker.git
cd bruno-cli-docker
docker compose run bruno-cli
```

That's it. Docker pulls the [`usebruno/cli:latest`](https://hub.docker.com/r/usebruno/cli) image, mounts the included collection, and runs the `posts` folder against the `Prod` environment. The CLI fires the bundled `posts` requests against [`mockdata.dev`](https://mockdata.dev) and prints a pass/fail summary at the end. JSON, JUnit XML, and HTML reports for the run land in [`reports/`](./reports) on your host (see [`reports/README.md`](./reports/README.md) for what each file is).

> **Note on `-r`:** Bruno CLI's `run` is non-recursive by default — it only looks at the target folder's direct children. If your collection has nested subfolders (most do, including the bundled one here), add `-r` to recurse: `bru run my-folder -r --env ci`. The `docker-compose.yml` shipped in this repo already passes `-r`. Targeting a single `.bru` or `.yml` file doesn't need `-r`.

> **Note on `--rm`:** Examples below omit `--rm`. Docker keeps stopped containers around after they exit, which lets you `docker logs` or `docker inspect` them later for debugging. If you'd rather have Docker auto-delete the container as soon as `bru` finishes — useful for CI runs or to avoid `docker ps -a` filling up with stale entries — append `--rm` to any `docker compose run` command:
>
> ```bash
> docker compose run --rm bruno-cli
> ```
>
> It's purely a cleanup convenience; it doesn't affect the image, mounts, stdout output, or exit code.

---

## What this demo does

Out of the box, `docker compose run bruno-cli` executes:

```
bru run posts -r --env Prod \
  --reporter-json /reports/results.json \
  --reporter-junit /reports/results.xml \
  --reporter-html /reports/results.html
```

inside a container, against the included `collection/` directory. The `posts` folder is a small slice covering:

- List / paginate / filter / search blog posts
- Manage post authors
- Read post comments
- Look up post statuses, tags, categories

All requests hit `https://mockdata.dev` — a free, public mock API service designed for development and testing. No accounts, no API keys, no setup required. After the run finishes, `reports/results.json`, `reports/results.xml`, and `reports/results.html` will be present on your host.

---

## What's in this repo

```
bruno-cli-docker/
├── README.md                    ← you are here
├── LICENSE                      ← MIT
├── docker-compose.yml           ← runs the demo
├── reports/                     ← generated reports land here (gitignored)
│   ├── README.md
│   └── .gitignore
└── collection/                  ← the Bruno collection itself
    ├── opencollection.yml       ← collection root marker (OpenCollection format)
    ├── readme.md                ← collection-specific docs
    ├── environments/
    │   └── Prod.yml             ← `baseUrl: https://mockdata.dev`
    ├── billing/                 ← customers, invoices, payments, subscriptions, lookups
    ├── flights/                 ← flight shop, orders, lookups
    ├── hotels/                  ← hotel shop, orders, lookups
    ├── posts/                   ← default demo target (posts, authors, comments, lookups)
    └── songs/                   ← song shop, orders, lookups
```

The collection uses Bruno's [OpenCollection](https://docs.usebruno.com) YAML format, fully supported by the Bruno CLI.

---

## Customize

### Run a different domain

Override the default `posts` target by passing CLI arguments after the service name. They replace the compose file's `command:`. Use `-r` so `bru` recurses into the folder's subfolders:

```bash
docker compose run bruno-cli run billing -r --env Prod
docker compose run bruno-cli run flights -r --env Prod
docker compose run bruno-cli run hotels -r --env Prod
docker compose run bruno-cli run songs -r --env Prod
```

### Run the entire collection

```bash
docker compose run bruno-cli run . -r --env Prod
```

(Runs every request across all five domains.)

### Run a single request file

```bash
docker compose run bruno-cli run "posts/posts/Get All Posts.yml" --env Prod
```

(`-r` is not needed when targeting a single file.)

### Customize report formats

The shipped `command:` in `docker-compose.yml` emits all three report formats (JSON, JUnit XML, HTML) into `/reports` on every run. To change that, override the command on the CLI and pass only the `--reporter-*` flags you want. Note that an override replaces the entire `command:` from the compose file, so re-state any flags you still need:

```bash
# JUnit XML only — useful when piping to CI test reporters
docker compose run bruno-cli run posts -r --env Prod \
  --reporter-junit /reports/results.xml

# JSON only
docker compose run bruno-cli run posts -r --env Prod \
  --reporter-json /reports/results.json

# Skip reports entirely — just the stdout summary
docker compose run bruno-cli run posts -r --env Prod
```

All `/reports/*` paths land in `reports/` on your host because of the `./reports:/reports` bind mount in the compose file.

### Pin a different image version

Edit `docker-compose.yml` and change `image: usebruno/cli:latest` to whatever you need:

```yaml
image: usebruno/cli:3.3.0        # exact version, immutable
image: usebruno/cli:3            # any 3.x.x, floats
image: usebruno/cli:3.3          # any 3.3.x, floats
image: usebruno/cli:3.3-debian   # debian variant (use for SSL / native-module compatibility)
```

See the [tag matrix](https://github.com/usebruno/bruno/tree/main/packages/bruno-cli/docker#tags) for all options.

### Swap the collection

The `collection/` directory is just bind-mounted — replace its contents with your own Bruno collection and re-run. As long as it has a valid `bruno.json` or `opencollection.yml` at its root, the CLI will run it.

---

## Where the image comes from

- **Docker Hub:** [`usebruno/cli`](https://hub.docker.com/r/usebruno/cli)
- **GitHub Container Registry:** [`ghcr.io/usebruno/cli`](https://github.com/usebruno/bruno/pkgs/container/cli)
- **Source Dockerfiles + smoke tests:** [`usebruno/bruno`](https://github.com/usebruno/bruno/tree/main/packages/bruno-cli/docker)
- **CLI documentation:** [docs.usebruno.com/bru-cli](https://docs.usebruno.com/bru-cli/overview)

To pull from GHCR instead of Docker Hub, change `docker-compose.yml`'s `image:` to `ghcr.io/usebruno/cli:latest`. Same image, mirrored.

---

## Collection attribution

The bundled collection is the **Bruno GenUI Collection**, originally maintained at [`teambruno/bruno-genui-collection`](https://github.com/teambruno/bruno-genui-collection) (private). It uses [mockdata.dev](https://mockdata.dev) as the upstream mock API. Used here with permission for demonstration purposes.

---

## License

MIT — see [LICENSE](./LICENSE).
