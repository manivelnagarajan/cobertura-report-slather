# Cobertura action for reports exported from Slather.

![](https://github.com/manivelnagarajan/cobertura-report-slather/workflows/Test/badge.svg)

## This is modified from [corbertura-action](https://github.com/5monkeys/cobertura-action) to support new input ignore_branch_rate. Ignore branch rate is useful for cobertura xml reports generated using [Slather](https://github.com/SlatherOrg/slather) for XCode projects.

GitHub Action which parse a [XML cobertura report](http://cobertura.github.io/cobertura/) and display the metrics in a GitHub Pull Request.

Many coverage tools can be configured to output cobertura reports:

* [coverage.py](https://coverage.readthedocs.io/en/latest/cmd.html#xml-reporting)
* [Istanbul](https://istanbul.js.org/docs/advanced/alternative-reporters/#cobertura)
* [Maven](https://www.mojohaus.org/cobertura-maven-plugin/)
* [simplecov](https://github.com/colszowka/simplecov/blob/master/doc/alternate-formatters.md#simplecov-cobertura)

Note that this action can only run on pull requests being opened from the same repository. This action will not currently work for pull requests from forks -- like is common in open source projects -- because the token for forked pull request workflows does not have write permissions. Hopefully GitHub will have a solution for this in the future.


## How it looks like

![alt text](img/comment.png "Pull request comment with metrics")

## Inputs

### `repo_token` **Required**

The GITHUB_TOKEN. See [details](https://help.github.com/en/articles/virtual-environments-for-github-actions#github_token-secret).

### `path`

The to the cobertura report. Defaults to `coverage.xml`. If a glob pattern is provided it will take the first match.

### `skip_covered`

If files with 100% coverage should be ignored. Defaults to `true`.

### `minimum_coverage`

The minimum allowed coverage percentage as an integer.

### `show_line`

Show line rate as specific column.

### `show_branch`

Show branch rate as specific column.

### `show_class_names`

Show class names instead of file names.

### `only_changed_files`

Only show coverage for changed files.

### `report_name`

Use a unique name for the report and comment.

### `ignore_branch_rate`

Default is false. You can set true if you don't want to count branch_rate while calcuating total coverage. If you have exported the report using Slather for XCode projects, then it is recommended to set true here.

### `pull_request_number` **Optional**

Pull request number associated with the report. This property should be used when workflow trigger is different than `pull_request`.

## Example usage

```yaml
on:
  pull_request:
    types: [opened]
    branches:
      - master
jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - uses: manivelnagarajan/cobertura-report-slather@master
        with:
          path: src/test.xml
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          minimum_coverage: 75
```
