# TPlumber
TPlumber optimizes resources for Travis CI dependent projects by pushing commits to multiple forks, circumventing the limit of workers in Travis CI per github account.

Disclaimer: This tool only offers the above functionality. Whether you can/should do it depends on your contract with Travis CI.

## Getting Started

1. Setup multiple github accounts and fork your project into those accounts.

  * Depending how many workers are needed, Travis CI limits OSS projects to have 5 workers per account. E.g. 2 accounts for 10 workers.

2. [Obtain Github token](https://help.github.com/articles/creating-an-access-token-for-command-line-use/) with access `repo -> public repo` for each account, and save it for next step. 
  
3. Setup credentials

  In your project repo, touch `.token.json` with the following content. Make sure it is **NOT** added to git.
  ```
  [
    ["<github account>", "<repo name>", "<github token>", "<travis ci token>"],
    ...(additional accounts go here)
  ]

  ```
  Travis CI token can be obtained by:
  
  `curl -i -H 'Content-Type: application/json' -d '{"github_token":"<github token>"}' -H 'User-Agent: Travis/1.0' https://api.travis-ci.org/auth/github`

3. `git clone https://github.com/wisechengyi/travis_hack.git`

4. `cd travis_hack; ./pants binary src/python:main`

5. In your project directory, `<path/to/travis_hack>/dist/main.pex`

  Sample output:
  ```
  INFO:__EXP__:Starting branch: master
  Switched to a new branch 'tmp_0fdd071b32db87900dc123a4a082b1e5a87f0625_0_goobj'
   * [new branch]      head -> tmp_0fdd071b32db87900dc123a4a082b1e5a87f0625_0_goobj
  Switched to a new branch 'tmp_c7c650094b9accc8e1ff4e5db42f277b54c454a1_1_hkrje'
   * [new branch]      head -> tmp_c7c650094b9accc8e1ff4e5db42f277b54c454a1_1_hkrje
  INFO:__EXP__:Build found. Url: https://travis-ci.org/usernameA/repoA/builds/141947066
  INFO:__EXP__:Build found. Url: https://travis-ci.org/usernameB/repoA/builds/141947079
  Switched back to branch 'master'
  ```
