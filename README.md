# JinwangMok 블로그

## Install & Test in Local

1. Install ruby(and gem) and github cli using brew.

```zsh
brew update
brew install ruby gh

# check ruby version
ruby --version

# login gh.
gh auth login
```

2. Clone current Blog repo.

```
gh repo clone JinwangMok/JinwangMok.github.io
cd JinwangMok.github.io
```

3. Install bundle and run local server.

```
bundle install
bundle exec jekyll serve
```