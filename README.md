# Release Watcher

A lightweight tool to fetch the latest release version from GitHub repositories. It prints the project name, URL to the release, and the most recent version tag in a clean format. Perfect for quick checks, scripts, or staying up to date with your dependencies. The results are compared to data from previous runs, and any changes are displayed in a human-readable format.

- [sommerfeld-io/release-watcher](https://hub.docker.com/r/sommerfeldio/release-watcher) on Docker Hub
- [Dockerfile source code](https://github.com/sommerfeld-io/release-watcher/tree/main/components) on GitHub
- [How to Contribute](https://github.com/sommerfeld-io/.github/blob/main/CONTRIBUTING.md)

![Project Logo](https://raw.githubusercontent.com/sommerfeld-io/release-watcher/refs/heads/main/.assets/logo.png)

**Example Output:** The versions overview for the [list of repos as specified in this repository](components/taskfile.yml) is written to [`components/output/versions.md`](components/output/versions.md).

## Supported Sources / Which objects can be watched?

- **Releases (= Git Tags) from GitHub:** The app queries the GitHub Releases API for the latest releases of the specified repositories. E.g. <https://api.github.com/repos/argoproj/argo-cd/releases/latest>. That means pre-releases are not included in the output.

## Software Tags and Versioning

Learn about our tagging policy and the difference between rolling tags and immutable tags [on our documentation page⁠](https://github.com/sommerfeld-io/.github/blob/main/docs/tags-and-versions.md).

## Usage

When running release-watcher, you will be asked to log in to GitHub using the GitHub CLI. This is necessary to access the GitHub API without running into rate limits. release-watcher uses the GitHub CLI for the login, so whether credentials are stored in the local cache or not is up to the GitHub CLI.

### How to use with Docker

You can run the release-watcher in a Docker container. The Docker image is built from the [Dockerfile](components/Dockerfile) in this repository.

The Dockerfile is also [available on Docker Hub as `sommerfeldio/release-watcher`](https://hub.docker.com/r/sommerfeldio/release-watcher).

Create a `.release-watcher.json` file in the current directory. This file contains the list of repositories to watch. The file should look like this:

```json
{
  "repositories": [
    "sommerfeld-io/release-watcher",
    "sommerfeld-io/container-images"
  ]
}
```

To run with Docker Compose, create a `docker-compose.yml` in the same directory as the `.release-watcher.json` file and run `docker compose up.

```docker-compose.yml
services:
  release-watcher:
    image: sommerfeldio/release-watcher:latest
    volumes:
      - .release-watcher.json:/app/.release-watcher.json
      - ./output:/app/output
    tty: true
```

To run with Docker directly, run the following command in directory where the `.release-watcher.json` file is located:

```bash
docker run --rm -it --volume ".release-watcher.json:/app/.release-watcher.json" --volume "output:/app/output" sommerfeldio/release-watcher:latest
```

### How to use with `task` (from this repsitory)

- Ensure you have [Task](https://taskfile.dev) installed on your system.
- Ensure you have [GitHub CLI](https://cli.github.com) installed on your system. This tool is mandatory because all GitHub API calls are made using the GitHub CLI.
- Ensure you have [JQ](https://stedolan.github.io/jq) installed on your system. This tool is used to parse the JSON output from the GitHub API.
- Ensure you have `diff` installed on your system. This tool is used to compare files (in this case the files containing the previous and current versions of the releases). Normally the tool is shipped with Ubuntu.

The [DevContainer Configuration](.devcontainer/Dockerfile) from this repository provides a ready-to-use environment with all necessary tools pre-installed. No additional setup is required other than VSCode with the DevContainer extension.

- Clone the repository to your local machine.

- Create a `components/.release-watcher.json` file next to `components/taskfile.yml`. This file contains the list of repositories to watch. The file should look like this:

  ```json
  {
    "repositories": [
      "sommerfeld-io/release-watcher",
      "sommerfeld-io/container-images"
    ]
  }
  ```

- Then simply run `task run` from [components/taskfile.yml](components/taskfile.yml).

  ```bash
  cd components
  task run # authenticate interactively with GitHub CLI
  ```

- This prints the version information to the console and writes it to a markdown file in the `components/output` directory.

### How to use with Docker (headless) in GitHub Actions

:zap: still work in progress ...

### How to use with `task` (headless) in GitHub Actions

You can provide a GitHub token as a secret in your GitHub repository.

- Clone the repository to your local machine.

- Remember to set up a `.release-watcher.json` file in the `components` directory as described above.

- Run the app with a token

  ```yaml
  - name: Run release watcher
    run: task run:${{ secrets.GITHUB_TOKEN }}
    shell: bash
  ```

## :zap: Baseline for Version Comparison

To be able to compare versions, the tool needs to have a baseline to compare against. The initial run will mark every repository as "updated" because there is no previous version to compare against. From the second run on, the tool will compare the current version with the previous version and only print the repositories that have changed.

This means the `components/output/versions.md` file should be persisted somehow (e.g. in a Git Repository) to act as the baseline for keeping the previous versions available for comparison.

## Building Blocks

The `taskfile.yml` file in the `components` directory defines the tasks to be executed. This is the main entry point for the application - regardless of whether you run it with Docker or `task` directly on your machine. When running inside Docker, the app does exactly the same as when running with `task` - just inside a container.

```ditaa
              +------------------+
              |    GitHub API    |
              |  (Release Info)  |
              +-------+----------+
                      ^
                      |
+---------------------|------------------------------------------------+
|                     |                                  taskfile.yml  |
|    +-----------+    |                                                |
|    |  gh auth  |    |                                                |
|    |   login   |    |                                                |
|    +-----+-----+    |                                                |
|          |          |                                                |
|          v          |                                                |
|       +--+----------+-----+      +------------------------+          |
|       |  Fetch data       +--+-->+    Console Output      |          |
|       |  from GitHub API  |  |   |  (Print release info)  |          |
|       +-------------------+  |   +------------------------+          |
|                              |                                       |
|                              |                                       |
|                              |         +------------------------+    |
|                              +-------->+    Markdown Output     |    |
|                                        |  (Write release info)  |    |
|                                        +-------------+----------+    |
|                                                      |               |
|                                                      v               |
|                                        +-------------+----------+    |
|                                        |     Compare changes    |    |
|                                        |           diff         |    |
|                                        +------------------------+    |
|                                                                      |
+----------------------------------------------------------------------+
```

## Risks and Technical Debts

All issues labeled as `risk` (= some sort of risk or a technical debt) or `security` (= disclosed security issues - e.g. CVEs) [are tracked as GitHub issue](https://github.com/sommerfeld-io/release-watcher/issues?q=is%3Aissue+label%3Asecurity%2Crisk+is%3Aopen) and carry the respective label.

## Contact

Feel free to contact me via <sebastian@sommerfeld.io> or [raise an issue in this repository](https://github.com/sommerfeld-io/release-watcher/issues).
