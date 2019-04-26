# My Development Environment
Little things that make me go faster


## CLI
- Homebrew
- [The Silver Searcher](https://github.com/ggreer/the_silver_searcher)
- [Z - jump around](https://github.com/rupa/z)
- brew install bash-completion

## Vim
- [Maximum Awesome](https://github.com/square/maximum-awesome)

## ~/.bash_profile
- Add a folder for adding scripts
```
# Add your bin folder to the path, if you have it.  It's a good place to add all your scripts
if [[ -d "$HOME/bin" ]]; then
  export PATH="$HOME/bin:$PATH"
fi
```

## ~/.bashrc

```
alias gb='git branch -v'
alias gcb='git checkout -b'
alias gd='git diff'
alias gl="git log --date=local"
alias gs='git status'
alias gss='git status --short --branch'

function gco {
  if [[ $# == 0 ]]; then
    git checkout master
  else
    git checkout "$@"
  fi
}
```


## ~/bin
- Git Open PR
```
#!/usr/bin/env bash
# Open the PR for the given commit

if [ $# == 0 ]; then
  # No args, use the current HEAD
  merge=$(git-find-merge)
elif [ $# == 1 ]; then
  # Passed a commit, try to find its merge
  merge=$(git-find-merge ${1})
  if [ "${merge}" == "" ]; then
    # Failing that, maybe the commit was a merge
    merge=${1}
  fi
else
  echo "Open the PR for the given commit"
  echo ""
  echo "Usage: $(basename "${0}") <sha>"
  echo "  sha: If empty, use the HEAD commit"
  echo ""
  echo "NOTE: The PR must have already been merged"
  exit 1
fi

repo_url=$(git config remote.origin.url | sed -e 's/^git@\(.*\):\(.*\)$/git:\/\/\1\/\2/' | sed 's/^.*\/\/\(.*\).git$/\1/' | sed -e 's/\git@//')
project=$(echo $repo_url | awk 'BEGIN { FS="/" } ; { print $2 }')
repo=$(echo $repo_url | awk 'BEGIN { FS="/" } ; { print $3 }')
pr=$(git log -n1 ${merge} | grep "Merge pull request #" | head -n 1 | sed  's%^.*Merge pull request #\([[:digit:]]*\).*%\1%')

if [ "$pr" == "" ]; then
  echo "Could not find PR # for ${merge}"
  exit 1
fi

url="https://blah/$project/repos/$repo/pull-requests/$pr/overview"
echo $url
open $url
exit 0
```
