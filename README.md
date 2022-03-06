# Ansible Role: github_download

Ansible role to download binary files from a GitHub Repository release to a
desired location.

## Requirements

This role has been tested on Ansible 2.10.0+ and should run against Linux/UNIX
based platforms.

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

## Role Variables

Below are a list of variables. For a more detailed description please refer to
[defaults/main.yml](defaults/main.yml)

| Variable                       | Short Description                                | Default Value    |
|--------------------------------|--------------------------------------------------|------------------|
| `github_download_packages`     | List of binaries to download. See example below. | `[]`             |
| `github_download_location`     | Location to download to.                         | `$HOME/bin`      |
| `github_download_pat_token`    | PAT Token for querying the API                   | None             |
| `github_download_hostname`     | Hostname for GitHub.                             | `github.com`     |
| `github_download_api_hostname` | Hostname for GitHub API.                         | `api.github.com` |

## Filename Templates

The following variables can be used in filenames and will be replaced with
values discovered by Ansible facts.

| Variable          | Description                                              | Example            |
|-------------------|----------------------------------------------------------|--------------------|
| `%ARCH%`          | OS Architecture                                          | `amd64`            |
| `%ARCHRAW%`       | OS Architecture (does not replace `x86_64` with `amd64`) | `x86_64`           |
| `%OS%`            | OS Family (lowercase)                                    | `linux` / `darwin` |
| `%OSCAPITALIZED%` | OS Family (Capitalized)                                  | `Linux` / `Darwin` |
| `%VERSION%`       | Package Version                                          | `v1.20.0`          |
| `%NVERSION%`      | Package Version (without `v` prefix)                     | `1.20.0`           |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for creating a list of users from GitHub

```yaml
- hosts: all
  become: true
  vars:
    github_download_packages:
      - name: package_name
        repo: owner/repo
        version: 1.0.2  # Can be 'latest'
        filename: package_name-%VERSION%-%OS%-%ARCH%.tar.gz  # See filename templates in README.md
        extracted_filename: package_name-%VERSION%
        command: package_name install
        command_become: false  # become root for this command
  roles:
    - role: xanmanning.github_download
```

## License

[BSD 3-clause](LICENSE.txt)

## Author Information

[Xan Manning](https://xan.manning.io/)
