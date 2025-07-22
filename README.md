# Release Watcher

A lightweight tool to fetch the latest release version from GitHub repositories. It prints the project name, repo URL, and the most recent version tag in a clean format. Perfect for quick checks, scripts, or staying up to date with your dependencies.

The release-watcher tool is a single Ansible project designed to monitor GitHub releases for specified repositories. The list of projects to watch is defined in Ansible variables. When run, the tool uses HTTP requests to query the GitHub Releases API, retrieving release information for each project. This data is then processed and written to the console.

## Pre-requisites

- Ensure you have [Task](https://taskfile.dev) installed on your system.
- Ensure you have [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed on your system.
- Ensure you have [GitHub CLI](https://cli.github.com/) installed on your system. This tool is mandatory because all GitHub API calls are made using the GitHub CLI.
- To use an AI summarizer, you need to install an AI tool. In this case, the playbook uses [Gemini CLI](https://github.com/google-gemini/gemini-cli).

The [DevContainer Configuration](.devcontainer/Dockerfile) from this repository provides a ready-to-use environment with all necessary tools pre-installed. You can use it to run the playbook without additional setup.

## How to run the playbook locally

To run the ansible playbook, which checks the versions locally, simply run `task run` in the root directory of this repository.

```ditaa
            +--------------------+
            |    GitHub API      |
            | (Release Info)     |
            +---------+----------+
                      ^
                      |
+---------------------|-----------------------------------------------+
|                     |                                 taskfile.yml  |
|    +-----------+    |                                               |
|    |  gh auth  |    |                                               |
|    |   login   |    |                                               |
|    +-----+-----+    |                                               |
|          |          |                                               |
|          v          |                                               |
|    +-----+----------|------------------------------------------+    |
|    |                |                        Ansible Playbook  |    |
|    |                |                                          |    |
|    |  +-------------+-----+      +------------------------+    |    |
|    |  |  Fetch data       +--+-->+    Console Output      |    |    |
|    |  |  from GitHub API  |  |   |  (Print release info)  |    |    |
|    |  +-------------------+  |   +------------------------+    |    |
|    |                         |                                 |    |
|    |                         |                                 |    |
|    |                         |   +------------------------+    |    |
|    |                         +-->+    Markdown Output     |    |    |
|    |                             |  (Write release info)  |    |    |
|    |                             +-------------+----------+    |    |
|    |                                           |               |    |
|    |                                           v               |    |
|    |                             +-------------+----------+    |    |
|    |                             |     AI Summarizer      |    |    |
|    |                             |  (Parse + Summarizes)  |    |    |
|    |                             +------------------------+    |    |
|    |                                                           |    |
|    +-----------------------------------------------------------+    |
|                                                                     |
+---------------------------------------------------------------------+
```

## Risks and Technical Debts

All issues labeled as `risk` (= some sort of risk or a technical debt) or `security` (= disclosed security issues - e.g. CVEs) [are tracked as GitHub issue](https://github.com/sommerfeld-io/release-watcher/issues?q=is%3Aissue+label%3Asecurity%2Crisk+is%3Aopen) and carry the respective label.

## Contact

Feel free to contact me via <sebastian@sommerfeld.io> or [raise an issue in this repository](https://github.com/sommerfeld-io/release-watcher/issues).
