# Welcome to semantic release squash and merge testbed

This repository aims to test squash-and-merge based semantic release


## Overview

This repo will be a reference repository for a squash and merge strategy.
Each commit following the first one will be a squash and merge pull request commit from a new branch containing inner commmits.
The name of each pull request follow a strict format e.g. :

`Issue-to-solve (#3)`

Where the number "3" is the pull request number.
From that number, the inner commits will be retrieved to do a semantic release versioning.
Each inner commit must follow the semantic release notation e.g. :

`feat(func): add a function`

## Pull request strategy

The squash-and-merge strategy is efficient to keep a clean and lean history.
In a repo applying such a strategy, the first commit is the only one which is not a squash-and-merge one.
The following pull requests are a merge from another branch to the main branch, containing inner commits.
As the name of these branch must describe the issue they solved, it's the inner commits's message which contain
the release and type for the semantic versioning.
Hence, these messages must be retrieved. It is achieved using the GitHub's [API](https://docs.github.com/en/rest/pulls/pulls?apiVersion=2022-11-28#list-commits-on-a-pull-request).

This repository is available to test this function using the API.
The commits will be done using the `--allow-empty` option, except the first initial one.

To do this, the repository consists of :
- An initial seminal commit, including only the `README.md` file, with no tags.
- A second commit with a squash-and-merge pull request with 2 conventionnal commits in it :
    - The first's message is : `feat(func): add a function`
    - The second's message is : `fix: fix a script`
- As there is no tag, the semantic release of this commit will have the `v1.0.0` tag using `git tag "v1.0.0" <hash>`
- A third commit with a squash-and-merge pull request with 3 conventionnal commits in it :
    - The first's message is : `patch: some patch`
    - The second's message is : `feat(script): add a script`
    - The third's message is : `feat: add a feature`
- A fourth commit with a malformed message (not finishing by a `(#[number])`) which will be ignored and containing 2 commits :
    - The first' message is : `feat(func2): add a feature`
    - The second's message is : `fix: fix a func`

Therefore, the test must return :
- `commits.length = 3`
- `commits[0].message = "patch:some patch"`
- `commits[1].message = "feat(script): add a script"`
- `commits[2].message = "feat: add a feature"`

These returned commits have a similar structure as the commit retrieved by the semantic-release function `getCommits` from the 
[semantic-release/lib/get-commits.js](https://github.com/semantic-release/semantic-release/blob/master/lib/get-commits.js) file.