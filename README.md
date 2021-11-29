# RAID: Refactoring-aware and Intelligent Diffs

[![Tests and Lint](https://github.com/rodrigo-brito/refactoring-aware-diff/workflows/Tests%20and%20Lint/badge.svg)](https://github.com/rodrigo-brito/refactoring-aware-diff/actions)
[![Chrome Web Store](https://img.shields.io/chrome-web-store/v/bclbegekihgpelanbbleaceefgmekjdd)](https://chrome.google.com/webstore/detail/refactoring-aware-diff/bclbegekihgpelanbbleaceefgmekjdd)
[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

- Published at 29th IEEE/ACM International Conference on Program Comprehension (ICPC), 2021 
- ArXiv: https://arxiv.org/abs/2103.11453
- IEEE Library: https://www.computer.org/csdl/proceedings-article/icpc/2021/140300a265/1tB7wfFt3ji

Code review is a key practice in modern software development. Currently, this practice is performed using textual diff tools, such as the one provided by GitHub. However, diff results can easily become complex and hard to understand. Refactorings, for example, are not represented at all in diff results. This makes diff understanding more complex because reviewers need to infer themselves that a given group of added and removed lines of code represent, for example, an extract method.

![result](https://user-images.githubusercontent.com/7620947/83893436-4ff0d300-a726-11ea-8a4d-48d34c36d115.gif)

RAID is a tool pipeline that seamlessly enriches GitHub diff results with refactoring information. See the following figure:

<p align="center">
    <img src="https://user-images.githubusercontent.com/7620947/76000414-706c7f80-5ee2-11ea-8f21-06bfb2646b36.png" width= "400px" />
</p>

As presented in this figure, this pipeline has three key components:

-   [RefDiff](https://github.com/aserg-ufmg/RefDiff): a tool to detect refactoring operations in commits. We rely on GitHub Actions to automatically execute RefDiff after each Pull Request.

-   An [external server](https://github.com/rodrigo-brito/refactoring-aware-diff/tree/server), that collects the list of refactorings produced by RefDiff.

-   A [Chrome plug-in](https://chrome.google.com/webstore/detail/refactoring-aware-review/bclbegekihgpelanbbleaceefgmekjdd) that seamlessly annotates diff chunks with refactoring data (see the following snapshot).

#### Example of Move Method

![Move Method](https://user-images.githubusercontent.com/7620947/83657276-058d1c00-a597-11ea-811b-6761bdf2d58e.png)

#### Supported Refactorings

RAID relies on RefDiff and support the following refactorings

| Language   | Language Refactorings                                                                  |
|------------|----------------------------------------------------------------------------------------|
| Java       | Move, Extract Function, Inline Function, Rename, Change Signature, Pull Up, Push Down  |
| JavaScript |  Move, Extract Function, Inline Function, Rename, Change Signature, Pull Up, Push Down |
| C          | Move, Extract Function, Inline Function, Rename, Change Signature                      |
| Go         | Move, Extract Function, Inline Function, Rename, Change Signature                      |

## Installation

To detect refactorings in your repository, you must first install this [Chrome plug-in](https://chrome.google.com/webstore/detail/refactoring-aware-review/bclbegekihgpelanbbleaceefgmekjdd) and configure RefDiff on [Github Actions](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow).

<a href="https://chrome.google.com/webstore/detail/refactoring-aware-diff/bclbegekihgpelanbbleaceefgmekjdd">
    <img width="200px" src="https://user-images.githubusercontent.com/7620947/75341916-26e1bc00-5874-11ea-9526-463ddf1e7f82.png" />
</a>

To execute RefDiff as Github Actions, you need to create a workflow file in `.github/workflows` folder with `.yaml` extension. Example of configuration: `.github/workflows/workflow.yml`:

```yaml
on: [pull_request] # Trigger RefDiff after each pull request

jobs:
    main:
        runs-on: ubuntu-latest
        name: Refactoring Detection
        steps:
            - name: Git Checkout
              uses: actions/checkout@v2
              with:
                  fetch-depth: 0

            - name: raid
              env:
                  LANGUAGE: ${{ github.event.repository.language }} # Supports Java, Javascrit, C and Go.
                  REV_BEFORE: ${{ github.event.pull_request.base.sha }}
                  REV_AFTER: ${{ github.event.pull_request.head.sha }}
              uses: rodrigo-brito/refactoring-aware-diff@action
```

## Simple Example

We created a simple pull request with commits including toy refactorings, to illustrate the usage of our plug-in. See [here](https://github.com/rodrigo-brito/refactoring-aware-diff/pull/5/commits) (but first you need to install our plug in).

## How cite RAID

```bibtex
@inproceedings{brito2021, 
  author = {Rodrigo Brito and Marco Tulio Valente}, 
  title = {{RAID}: Tool Support for Refactoring-Aware Code Reviews}, 
  booktitle = {29th International Conference on Program Comprehension (ICPC)}, 
  year = {2021}, 
  pages = {265--275}
}
``` 

DOI: https://doi.ieeecomputersociety.org/10.1109/ICPC52881.2021.00033

## License

Distributed under the MIT License. See [LICENSE](LICENSE) for more information.
"# testRAID" 
