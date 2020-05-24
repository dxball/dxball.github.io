# git ignore 替代方案


當有某個檔案或資料夾會因為測試需求而異動，但又不想簽入到 git repo 裡面 (例如 config 檔)，通常將路徑加入專案的 `.gitignore` 做排除

<!--more-->

# git 忽略異動

## .gitigore
就把不想要加到 git 的東西寫在 `.gitignore` 檔裡面

## .git/info/exclude

若不想修改專案的 `.gitignore`，又想排除異動的話，可以考慮放到 `.git/info/exclude`，語法與 `.gitignore` 相同

but...

`.git/info/exclude` 失效的時候怎麼辦？

## skip-worktree

1. 忽略追蹤異動
```
git update-index --skip-worktree [<file>...]
```

2. 解除忽略
```
git update-index --no-skip-worktree [<file>...]
```

## assume-unchanged

1. 忽略追蹤異動
```
git update-index --assume-unchanged [<file>...]
```

2. 解除忽略
```
git update-index --no-assume-unchanged [<file>...]
```

## Reference
- https://clouding.city/git/ignore-tracked/
- https://blog.miniasp.com/post/2014/12/23/Git-Advanced-Assume-Unchanged-Skip-worktree
- https://hashrocket.com/blog/posts/ignore-specific-file-changes-only-on-current-machine-in-git
- https://stackoverflow.com/questions/1753070/how-do-i-configure-git-to-ignore-some-files-locally

