# Next.js Egg

This egg is designed to run any **Next.js application** on a Pterodactyl server. It supports pulling source code from a GitHub repository and running the app in either **development** or **production** mode using `npm` or `yarn`.

## Features

- Run Next.js in **development** (`next dev`) or **production** (`next build && next start`) mode
- Automatic dependency installation via `npm` or `yarn`
- Optional **auto-pull** from a GitHub repository on every startup
- Support for **private repositories** via GitHub username + personal access token
- Optional **logging** to file (`logs/next_dev.log` or `logs/next_start.log`)
- Install or uninstall **custom Node.js modules** on startup
- Configurable **host** and **port**
- Supports Node.js versions **18, 19, 20, 21**

## Docker Images

| Name       | Image                                          |
|------------|------------------------------------------------|
| Next.js 21 | `ghcr.io/ptero-eggs/yolks:nodejs_21`          |
| Next.js 20 | `ghcr.io/ptero-eggs/yolks:nodejs_20`          |
| Next.js 19 | `ghcr.io/ptero-eggs/yolks:nodejs_19`          |
| Next.js 18 | `ghcr.io/ptero-eggs/yolks:nodejs_18`          |

## Startup Detection

The server panel detects the server as **running** when it sees the following string in the console output:

```json
{
  "done": "Compiled successfully"
}
```

> This matches the default Next.js dev output. If you are running in production mode or your app outputs something different, update the **Start Configuration** in the egg settings accordingly. You can also use an array to support multiple output strings:
>
> ```json
> {
>   "done": [
>     "Compiled successfully",
>     "ready started server on"
>   ]
> }
> ```

## Configuration Variables

| Variable            | Env Key            | Default              | Description |
|---------------------|--------------------|----------------------|-------------|
| Node ENV            | `NODE_RUN_ENV`     | `dev`                | Run mode: `dev` (development) or `production` |
| Host Header         | `NEXT_HOST`        | `0.0.0.0`            | Hostname the server binds to |
| Server Port         | `NEXT_SERVER_PORT` | `3000`               | Port the Next.js app listens on (1024–65535) |
| Build Directory     | `BUILD_DIR`        | `/home/container`    | Path to your application root |
| GitHub URL          | `GITHUB_URL`       | *(empty)*            | Repository URL, e.g. `https://github.com/user/repo` |
| Branch              | `GIT_BRANCH`       | *(empty)*            | Git branch to pull, e.g. `main` |
| Auto Pull           | `AUTO_UPDATE`      | `0`                  | Set to `1` to pull from GitHub on every startup |
| Git Username        | `USERNAME`         | *(empty)*            | GitHub username for private repo auth |
| Git Access Token    | `ACCESS_TOKEN`     | *(empty)*            | GitHub personal access token ([create one here](https://github.com/settings/tokens)) |
| Logs Status         | `LOGS_STATUS`      | `0`                  | Set to `1` to enable file logging |
| Modules             | `NODE_PACKAGES`    | *(empty)*            | Comma-separated list of extra npm/yarn packages to install |
| Modules Uninstall   | `MODULES_UNINSTALL`| `0`                  | Set to `1` to **uninstall** the packages listed in Modules |
| Project Run         | `PROJECT_RUN`      | `npm`                | Package manager to use: `npm` or `yarn` |

## Getting Started

### Option 1 — Upload your files manually

1. Upload your Next.js project files via the Pterodactyl file manager or SFTP.
2. Make sure your project has a valid `package.json` in the **Build Directory**.
3. Set `NODE_RUN_ENV` to `dev` or `production`.
4. Start the server — dependencies will be installed automatically.

### Option 2 — Pull from GitHub

1. Set `GITHUB_URL` to your repository (e.g. `https://github.com/youruser/yourapp`).
2. Set `GIT_BRANCH` to the branch you want (e.g. `main`).
3. Set `AUTO_UPDATE` to `1`.
4. For private repos, fill in `USERNAME` and `ACCESS_TOKEN`.
5. Start the server — it will clone/pull the latest code and install dependencies.

## Logging

When `LOGS_STATUS` is set to `1`, logs are written to the `logs/` directory inside your Build Directory:

| Mode        | Log file               |
|-------------|------------------------|
| Development | `logs/next_dev.log`    |
| Production  | `logs/next_start.log`  |
| Auto-update | `logs/update.log`      |

## Notes

- Make sure your `package.json` includes a `dev` script (`next dev`) and a `start` script (`next start`) for the egg to work correctly.
- The default port is `3000`. Make sure this matches the port assigned by Pterodactyl.
- In production mode, the app is built first with `next build` before starting. This may take a moment on first run.
- The `node_modules` folder is placed inside your **Build Directory** to avoid permission issues.

## Author

**Software By Ricardo** — [hello@softwarebyricardo.xyz](mailto:hello@softwarebyricardo.xyz)  
Egg version: `v1.0` — Generated: `2026-04-22`
