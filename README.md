
# 📚 CircleCI Github Changelog Generator

<p align="center">
    <a href="https://circleci.com/orbs/registry/orb/onimur/github-changelog-generator" title="Orb Version">
        <img src="https://img.shields.io/endpoint.svg?url=https://badges.circleci.io/orb/onimur/github-changelog-generator">
    </a>
    <a href="./LICENSE" title="License">
        <img src="https://img.shields.io/github/license/onimur/circleci-github-changelog-generator">
    </a>
    <a href="https://circleci.com/gh/onimur/circleci-github-changelog-generator" title="onimur">
        <img src="https://circleci.com/gh/onimur/circleci-github-changelog-generator.svg?style=shield">
    </a>
</p>

Orb for CircleCI that automatically generates the change log from your tags, issues, labels and pull requests on GitHub, using [github-changelog-generator](https://github.com/github-changelog-generator/github-changelog-generator)'s and based [action-github-changelog-generator](https://github.com/marketplace/actions/generate-changelog)'s

This orb is registered with CircleCI, see [here](https://circleci.com/orbs/registry/orb/onimur/github-changelog-generator).

<p align="center">
    <a href="https://circleci.com/orbs/registry/orb/onimur/github-changelog-generator" title="CircleCI Github Changelog Generator">
        <img width="45%" src=".github/resources/logo_git.png">
    </a>
</p>

## 💞 Support us

We are developing this structure in the open source community without financial planning.
If you like this project and would like to help us, make a donation:

<p align="center">
    <a href="https://www.patreon.com/onimur" target="_blank">
        <img width="30%" alt="Check my Patreon" src="https://raw.githubusercontent.com/onimur/.github/master/.resources/support-patreon.png"/>
    </a>
    <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=YUTBBKXR2XCPJ" target="_blank">
        <img width="30%" alt="Donate with Paypal" src="https://raw.githubusercontent.com/onimur/.github/master/.resources/support-paypal.png"/>
    </a>
    <a href="https://www.buymeacoffee.com/onimur" target="_blank">
        <img width="30%" alt="Buy me a coffee" src="https://raw.githubusercontent.com/onimur/.github/master/.resources/support-buy-coffee.png"/>
    </a>
</p>

## 📝 Content

- [Usage](#-usage)
    - [Simple Example](#simple-example)
        - [Push to remote repository](#push-to-remote-repository)
        - [Without push](#without-push)
    - [Custom Example]()
        - [Push to remote repository](#push-to-remote-repository-1)
        - [Without push](#without-push-1)
- [Commands](#%EF%B8%8F-commands)
    - [Job Default](#job-default)
    - [Job Custom](#job-default)
- [Main features](#-main-features)
- [Contributing](#-contributing)
- [License](#-license)

## 💡 Usage
In your config.yml

### Simple Example
For these examples you can use [these commands](#job-default).

#### Push to remote repository

```yml
version: 2.1

orbs:
  changelog: onimur/github-changelog-generator@x.y.z # Check the last version

orb_tagged_filters: &orb_tagged_filters
  branches:
    ignore: /.*/
  tags:
    only: /^\d+\.\d+\.\d+$/ #for tag 1.2.3

workflows:
  tagged-publish: # run job for all tags
    jobs:
      - changelog/changelog-default:
          steps:
            - checkout
          context: your-context           # Case contain your Github Token
          token: GITHUB_TOKEN             # Token as env_var
          git-push: true                  # Push changelog to remote repository
          filters: *orb_tagged_filters    # Trigger to only tag
```

#### Without push

```yml
version: 2.1

orbs:
  changelog: onimur/github-changelog-generator@x.y.z # Check the last version

orb_tagged_filters: &orb_tagged_filters
  branches:
    ignore: /.*/
  tags:
    only: /^\d+\.\d+\.\d+$/ #for tag 1.2.3

workflows:
  tagged-publish: # run job for all tags
    jobs:
      - changelog/changelog-default:
          steps:
            - checkout
          context: your-context           # Case contain your Github Token
          token: GITHUB_TOKEN             # Token as env_var
          git-push: false                 # The changelog will be save on store artifacts (Circle CI)
          filters: *orb_tagged_filters    # Trigger to only tag
```

### Custom Example

For these examples you can use [these commands](#job-custom).

#### Push to remote repository

```yml
version: 2.1

orbs:
  changelog: onimur/github-changelog-generator@x.y.z # Check the last version

orb_tagged_filters: &orb_tagged_filters
  branches:
    ignore: /.*/
  tags:
    only: /^\d+\.\d+\.\d+$/ #for tag 1.2.3

workflows:
  tagged-publish: # run job for all tags
    jobs:
      - changelog/changelog-custom:
          steps:
            - checkout
          context: your-context           # Case contain your Github Token
          token: GITHUB_TOKEN             # Token as env_var
          git-push: true                  # Push changelog to remote repository
          commit-message: Update changelog
          bot-name: my-bot
          bot-email: my-bot@email.com
          header-label: "# My Changelog"
          date-format: "%d-%m-%Y"
          output: CUSTOM-CHANGELOG.md
          breaking-label: "**Breaking**"
          bugs-label: "**Bugs**"
          filters: *orb_tagged_filters    # Trigger to only tag
```

### Without push

```yml
version: 2.1

orbs:
  changelog: onimur/github-changelog-generator@x.y.z # Check the last version

orb_tagged_filters: &orb_tagged_filters
  branches:
    ignore: /.*/
  tags:
    only: /^\d+\.\d+\.\d+$/ #for tag 1.2.3

workflows:
  tagged-publish: # run job for all tags
    jobs:
      - changelog/changelog-custom:
          steps:
            - checkout
          context: your-context           # Case contain your Github Token
          token: GITHUB_TOKEN             # Token as env_var
          git-push: false                 # The changelog will be save on store artifacts (Circle CI)
          header-label: "# My Changelog"
          date-format: "%d-%m-%Y"
          output: CUSTOM-CHANGELOG.md
          breaking-label: "**Breaking**"
          bugs-label: "**Bugs**"
          filters: *orb_tagged_filters    # Trigger to only tag
```



## 🛠️ Commands
You can check the list of all commands [here](https://github.com/github-changelog-generator/github-changelog-generator/wiki/Advanced-change-log-generation-examples#additional-options).

### Job Default
<details>
  <summary>Click to expand!</summary>
  
|    parameter   |                                           description                                         | required |          default         |     type     |
|:--------------:|:---------------------------------------------------------------------------------------------:|:--------:|:------------------------:|:------------:|
| bot-email      | Email to commit                                                                               |     -    | circleci@bot-noreply.com | string       |
| bot-name       | User name to commit                                                                           |     -    | CircleCI-bot             | string       |
| branch         | Branch to push changelog                                                                      |     -    | master                   | string       |
| commit-message | Commit message                                                                                |     -    | Auto Update changelog    | string       |
| executor       | Executor to use for this job. Defaults to this orb's default executor.                        |     -    | default                  | executor     |
| project        | Name of project on GitHub.                                                                    |     -    | CIRCLE_PROJECT_REPONAME  | env_var_name |
| git-push       | If true, Push the changelog to remote repository. Else store changelog to artifacts           |     -    | false                    | boolean      |
| steps          | Any step before execute job. It can be used to export or retrieve some environment variable.  |     -    | []                       | steps        |
| token          | To make more than 50 requests per hour your GitHub token is required.                         |     -    | GITHUB_TOKEN             | env_var_name |
| user           | Username of the owner of target GitHub repo.                                                  |     -    | CIRCLE_PROJECT_USERNAME  | env_var_name |
</details>

### Job Custom
<details>
    <summary>Click to expand!</summary>
    
|          parameter         |                                                           description                                                          | required |                       default                       |     type     |
|:--------------------------:|:------------------------------------------------------------------------------------------------------------------------------:|:--------:|:---------------------------------------------------:|:------------:|
| add-sections               | Add new sections but keep the default sections.                                                                                |     -    | ''                                                  | string       |
| author                     | Add author of pull request at the end. Default is true                                                                         |     -    | true                                                | boolean      |
| base                       | Optional base file to append generated changes to.                                                                             |     -    | ''                                                  | string       |
| bot-email                  | Email to commit                                                                                                                |     -    | circleci@bot-noreply.com                            | string       |
| bot-name                   | User name to commit                                                                                                            |     -    | CircleCI-bot                                        | string       |
| branch                     | Branch to push changelog                                                                                                       |     -    | master                                              | string       |
| breaking-label             | Set up custom label for breaking changes section. Default is "\*\*Implemented enhancements:\*\*".                              |     -    | '\*\*Implemented enhancements:\*\*'                 | string       |
| breaking-labels            | Issues with these labels will be added to a new section, called Breaking changes. Default is backwards-incompatible,breaking.  |     -    | 'backwards-incompatible,breaking'                   | string       |
| bug-labels                 | Issues with the specified labels will be added to Fixed bugs section. Default is bug,Bug.                                      |     -    | 'bug,Bug'                                           | string       |
| bugs-label                 | Set up custom label for bug-fixes section. Default is "\*\*Fixed bugs:\*\*".                                                   |     -    | '\*\*Fixed bugs:\*\*'                               | string       |
| cache-file                 | Filename to use for cache. Default is github-changelog-http-cache in a temporary directory.                                    |     -    | github-changelog-http-cache                         | string       |
| cache-log                  | Filename to use for cache log. Default is github-changelog-logger.log in a temporary directory.                                |     -    | github-changelog-logger.log                         | string       |
| commit-message             | Commit message                                                                                                                 |     -    | Auto Update changelog                               | string       |
| compare-link               | Include compare link (Full Changelog) between older version and newer version. Default is true.                                |     -    | true                                                | boolean      |
| configure-sections         | Define your own set of sections which overrides all default sections.                                                          |     -    | ''                                                  | string       |
| date-format                | Date format. Default is "%Y-%m-%d".                                                                                            |     -    | '%Y-%m-%d'                                          | string       |
| deprecated-label           | Set up custom label for deprecated section. Default is "\*\*Deprecated:\*\*".                                                  |     -    | '\*\*Deprecated:\*\*'                               | string       |
| deprecated-labels          | Issues with the specified labels will be added to a section called Deprecated. Default is deprecated,Deprecated.               |     -    | 'deprecated,Deprecated'                             | string       |
| due-tag                    | Changelog will end before specified tag.                                                                                       |     -    | ''                                                  | string       |
| enhancement-label          | Set up custom label for enhancements section. Default is "\*\*Implemented enhancements:\*\*".                                  |     -    | '\*\*Implemented enhancements:\*\*'                 | string       |
| enhancement-labels         | Issues with the specified labels will be added to 'mplemented enhancements section. Default is enhancement,Enhancement.        |     -    | 'enhancement,Enhancement'                           | string       |
| exclude-labels             | Issues with the specified labels will be excluded from changelog. Default is duplicate,question,invalid,wontfix.               |     -    | 'duplicate,question,invalid,wontfix'                | string       |
| exclude-tags               | Changelog will exclude specified tags.                                                                                         |     -    | ''                                                  | string       |
| exclude-tags-regex         | Apply a regular expression on tag names so that they can be excluded.                                                          |     -    | ''                                                  | string       |
| executor                   | Executor to use for this job. Defaults to this orb's default executor.                                                         |     -    | default                                             | executor     |
| filter-by-milestone        | Use milestone to detect when issue was resolved. Default is true.                                                              |     -    | true                                                | boolean      |
| front-matter               | Add YAML front matter. Formatted as JSON because it's easier to add on the command line.                                       |     -    | ''                                                  | string       |
| future-release             | Put the unreleased changes in the specified release number.                                                                    |     -    | ''                                                  | string       |
| github-api                 | The enterprise endpoint to use for your GitHub API.                                                                            |     -    | ''                                                  | string       |
| github-site                | The Enterprise GitHub site where your project is hosted.                                                                       |     -    | ''                                                  | string       |
| header-label               | Set up custom header label. Default is # Changelog.                                                                            |     -    | '# Changelog'                                       | string       |
| http-cache                 | Use HTTP Cache to cache GitHub API requests (useful for large repos). Default is true.                                         |     -    | true                                                | boolean      |
| include-labels             | Of the labeled issues, only include the ones with the specified labels.                                                        |     -    | ''                                                  | string       |
| issue-line-labels          | The specified labels will be shown in brackets next to each matching issue. Use ALL to show all labels. Default is [].         |     -    | ''                                                  | string       |
| issues                     | Include closed issues in changelog. Default is true.                                                                           |     -    | true                                                | boolean      |
| issues-label               | Set up custom label for closed-issues section. Default is "\*\*Closed issues:\*\*".                                            |     -    | '\*\*Closed issues:\*\*'                            | string       |
| issues-wo-labels           | Include closed issues without labels in changelog. Default is true.                                                            |     -    | true                                                | boolean      |
| max-issues                 | Maximum number of issues to fetch from GitHub. Default is "-1" - unlimited.                                                    |     -    | -1                                                  | integer      |
| only-last-tag              | Changelog will only show last tag.                                                                                             |     -    | false                                               | boolean      |
| output                     | Output file. Default is CHANGELOG.md.                                                                                          |     -    | CHANGELOG.md                                        | string       |
| pr-label                   | Set up custom label for pull requests section. Default is "\*\*Merged pull requests:\*\*".                                     |     -    | '\*\*Merged pull requests:\*\*'                     | string       |
| pr-wo-labels               | Include pull requests without labels in changelog. Default is true.                                                            |     -    | true                                                | boolean      |
| project                    | Name of project on GitHub.                                                                                                     |     -    | CIRCLE_PROJECT_REPONAME                             | env_var_name |
| pull-requests              | Include pull-requests in changelog. Default is true.                                                                           |     -    | true                                                | boolean      |
| git-push                   | If true, Push the changelog to remote repository. Else store changelog to artifacts                                            |     -    | false                                               | boolean      |
| release-branch             | Limit pull requests to the release branch, such as master or release.                                                          |     -    | ''                                                  | string       |
| release-url                | The URL to point to for release links, in printf format (with the tag as variable).                                            |     -    | ''                                                  | string       |
| removed-label              | Set up custom label for removed section. Default is "\*\*Removed:\*\*".                                                        |     -    | '\*\*Removed:\*\*'                                  | string       |
| removed-labels             | Issues with the specified labels will be added to a section called Removed. Default is removed,Removed.                        |     -    | 'removed,Removed'                                   | string       |
| security-label             | Set up custom label for security section. Default is "\*\*Security fixes:\*\*".                                                |     -    | '\*\*Security fixes:\*\*'                           | string       |
| security-labels            | Issues with the specified labels will be added to a section called Security fixes. Default is security,Security                |     -    | 'security,Security'                                 | string       |
| simple-list                | Create a simple list from issues and pull requests. Default is false.                                                          |     -    | false                                               | boolean      |
| since-tag                  | Changelog will start after specified tag.                                                                                      |     -    | ''                                                  | string       |
| ssl-ca-file                | Path to cacert.pem file. Default is a bundled lib/github_changelog_generator/ssl_certs/cacert.pem. Respects SSL_CA_PATH.       |     -    | lib/github_changelog_generator/ssl_certs/cacert.pem | string       |
| steps                      | Any step before execute job. It can be used to export or retrieve some environment variable.                                   |     -    | []                                                  | steps        |
| strip-generator-notice     | Strip generator reference.                                                                                                     |     -    | false                                               | boolean      |
| strip-headers              | Strip headers.                                                                                                                 |     -    | false                                               | boolean      |
| token                      | To make more than 50 requests per hour your GitHub token is required.                                                          |     -    | GITHUB_TOKEN                                        | env_var_name |
| unreleased                 | Add to log unreleased closed issues. Default is true.                                                                          |     -    | true                                                | boolean      |
| unreleased-label           | Set up custom label for unreleased closed issues section. Default is "\*\*Unreleased:\*\*".                                    |     -    | '\*\*Unreleased\*\*'                                | string       |
| unreleased-only            | Generate log from unreleased closed issues only.                                                                               |     -    | ''                                                  | string       |
| user                       | Username of the owner of target GitHub repo.                                                                                   |     -    | CIRCLE_PROJECT_USERNAME                             | env_var_name |
| usernames-as-github-logins | Use GitHub tags instead of Markdown links for the author of an issue or pull-request.                                          |     -    | ''                                                  | string       |
| verbose                    | Run verbosely. Default is true.                                                                                                |     -    | true                                                | boolean      |

</details>
  
## 🔍 Main features

- [Orb-CircleCI](https://circleci.com/orbs/) 
- [github-changelog-generator](https://github.com/github-changelog-generator/github-changelog-generator)
  
## 🧩 Contributing

This project is open-source, so feel free to fork, or to share your ideas and changes to improve the project, check with more details below.

- 💬 [Contributing](docs/CONTRIBUTING.md)
- 👮🏼 [Code of conduct](docs/CODE_OF_CONDUCT.md)
- 😷 [Support](docs/SUPPORT.md)

## 📃 License

    MIT License

    Copyright (c) 2020 Onimur

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.

  * [MIT License](./LICENSE)
