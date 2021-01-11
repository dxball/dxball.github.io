# git checkout 排除指定目錄/檔案


跟別人共用的專案，有時候 checkout 的時候不需要把部分的東西一併取出來，可以節省 compile 時間，git 有提供 sparse checkout 的功能，可以指定或排除特定目錄/檔案

<!--more-->

## 啟用 git sparse-checkout
1. 啟用設定

    ```shell
    git config core.sparseCheckout true
    ```

2. 新增或編輯 `.git/info/sparse-checkout`，語法跟 `.gitignore` 一樣

    ```bash
    # 所有目錄及檔案都要 checkout
    /*
    
    # 以下目錄不會被 checkout
    !/Assets/FolderNotCheckout
    !/Assets/FileNotCheckout.dat
    ```

3. 更新 git 工作目錄

    ```shell
    git read-tree -mu HEAD
    ```

## 停用 git sparse-checkout
1. 停用設定

    ```shell
    git config core.sparseCheckout false
    ```

2. 清空 `.git/info/sparse-checkout`

    ```shell
    echo "" > .git/info/sparse-checkout
    ```

3. 更新 git 工作目錄

    ```shell
    git read-tree -mu HEAD
    ```

## Reference
- https://git-scm.com/docs/git-read-tree#_sparse_checkout
- https://stackoverflow.com/a/33934496/1568102

