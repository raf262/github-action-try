# Github Actions on a simple Symfony skeleton
[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

**Note:** 
This project is created on an R&D day to add github actions to our projects. It is based on a Symfony skeleton and allow
you to have basic Github Actions preconfigured for your project including :
- Linter checker on PR
- Adding reviewers on PR
- Stale and close PR / Issue if no activity > 30days

You might have to create new labels in your Github environment and change some variables in jobs for your projects.

## Structure

Github Actions are located under a folder at the project's root

```
./.github/workflow/        
```


## Install

Github actions are automatically loaded from your project's files. If you delete the file, the Action will be deleted.

## Usage

Create a new file in `./.github/workflow/` name `my_github_action.yml`.
It will contain :
- a node `name` displayed on the tab **Actions** on your Github repository
- a node `on` that configure when the actions will be triggered
- then a node `jobs` who contains the actions to run when doing the job

``` yaml
name: 'My super Github Action'
on:
  pull_request:
    types: [opened, ready_for_review]

jobs:
  add-reviews:
    runs-on: ubuntu-latest
    steps:
      - uses: package/action@version
        with:
          configuration-path: ".github/conf.yml"
```

## Jobs

### Linter checker
This job runs a linter checker on the project.
It is triggered on every push in the project excluding the __master__ and __main__ branches.
It use the [github/super-linter](https://github.com/github/super-linter) action.
Only new edited or added files are analysed.
The `./vendor` directory, files `@generated` and `.gitignore` are excluded from the analysis.

### Stale checker
This job act as a CRON job and check if a PR or an Issue is still active. If she's not, it will add a tag `stale` and
__close__ the PR or Issue in 5 or 10 days depending on your configuration.
You can set a predefined message for the PR or Issue that is stale. 
Change the number of day without activity before staled, exempt PR with some label and start analysing PR / Issue opened
after a certain date with `start_date`.
It use the [actions/stale@v4](https://github.com/marketplace/actions/close-stale-issues#exempt-pr-labels).

### Add reviewers
This job auto add reviewers to the PR using a configuration file located at `./github/teams/my-team.yml`.
It is triggered when a pull request is open and ready for review.
It is base on [kentaro-m/auto-assign-action@v1.2.0](https://github.com/kentaro-m/auto-assign-action).
In the configuration file of your team you can assign the PR directly to the creator (done by default).
The name of the reviewer that you set in :
``` yaml
reviewGroups:
    my-team:
        - reviewer_name_1
        - reviewer_name_2
```
will automatically be added to your PR.

## Security

If you discover any security related issues, please use the issue tracker.

## Credits
- [@raf262](https://github.com/raf262)
