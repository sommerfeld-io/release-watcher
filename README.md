# release-watcher

A lightweight tool to fetch the latest release version from GitHub repositories. It prints the project name, repo URL, and the most recent version tag in a clean format. Perfect for quick checks, scripts, or staying up to date with your dependencies.

## Pre-requisites

- Ensure you have Ansible installed on your system.
- To use an AI summarizer, you need to install an AI tool. In this case, the playbook uses [Gemini CLI](https://github.com/google-gemini/gemini-cli).

## How to run the playbook locally

todo ... how to run locally ... (which command to run and which prerequisites are needed)

```ditaa
       +--------------------+
       |    GitHub API      |
       | (Release Info)     |
       +---------+----------+
                 ^
                 |
+----------------+----------------+      +-------------------------+
|     Ansible Playbook (local)    +---- >+    Console Output       |
| (fetches data from GitHub API)  |      |  (Prints release info)  |
+---------------------------------+      +-------------+-----------+
                                                       |
                                                       v
                                         +-------------+-----------+
                                         |     AI Summarizer       |
                                         |  (Parses + Summarizes)  |
                                         +-------------------------+
```

## How to run the playbook in a GitHub Actions Workflow

todo ... summary ...

```ditaa
       +--------------------+
       |    GitHub API      |
       | (Release Info)     |
       +---------+----------+
                 ^
                 |
+----------------+----------------+      +-------------------------+
|     Ansible Playbook            +---- >+    Console Output       |
| (fetches data from GitHub API)  |      |  (Prints release info)  |
+---------------------------------+      +-------------+-----------+
                                                       |
                                                       v
                                         +-------------+-----------+
                                         |     AI Summarizer       |
                                         |  (Parses + Summarizes)  |
                                         +-------------+-----------+
                                                       |
                                                       v
                                            +----------+-----------+
                                            |   Google Chat Bot    |
                                            | (Sends update msg)   |
                                            +----------------------+
```

todo ... sample workflow.yml ...

## Risks and Technical Debts

All issues labeled as `risk` (= some sort of risk or a technical debt) or `security` (= disclosed security issues - e.g. CVEs) [are tracked as GitHub issue](https://github.com/sommerfeld-io/release-watcher/issues?q=is%3Aissue+label%3Asecurity%2Crisk+is%3Aopen) and carry the respective label.

## Contact

Feel free to contact me via <sebastian@sommerfeld.io> or [raise an issue in this repository](https://github.com/sommerfeld-io/release-watcher/issues).
