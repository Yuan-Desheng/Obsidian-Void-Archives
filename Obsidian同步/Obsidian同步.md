[自動將 Obsidian 筆記備份到 Github 並和行動裝置同步](https://blog.poychang.net/obsidian-sync-between-desktop-and-mobile-with-git/)
### 手機端

這裡以 iOS 為例，首先在 App Store 中安裝 [Odsidian](https://apps.apple.com/us/app/obsidian-connected-notes/id1557175442) 和 [iSH](https://apps.apple.com/tw/app/ish-shell/id1436902243) 這兩個 App。

在開始之前，你可以使用 [Files](https://apps.apple.com/us/app/files/id1232058109) 這個 Apple 官方的檔案管理 App，來查看 Obsidian 存放 Vault 的資料夾是否存在：![[Pasted image 20240408223505.png]]確認好之後，就可以開啟在 iSH 並使用 `apk` 更新當前 Alpine Linux 已安裝的套件，並且安裝 Git 指令工具：

```
apk update & upgrade
apk add git
```

接著要在 iSH 中掛載 iOS 的指定資料夾，可以參考這份文件 [Mounting other file providers](https://github.com/ish-app/ish/wiki/Mounting-other-file-providers)，基本的操作指令如下：

```
mount -t ios . /mnt/obsidian
```

這指令會開啟 iOS 選擇資料夾的互動視窗，選擇好要掛載的資料夾，也就是 `Obsidian` 資料夾之後，就可以在 `/mnt` 底下看到 `Obsidian` 這個資料夾的內容，在使用 `cd /mnt` 移動到該目錄之後，可以使用 `git clone` 指令將我們存放在 GitHub 上的檔案復刻下來：
過程中會需要輸入 GitHub 的帳號密碼，如果你有啟動 2FA 的話，就需要產生一組 Personal Access Token 來使用，這裡有一份文件可以參考 [Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)。

最後回到 Obsidian App 中，在這裡可以看到稍早復刻下來的資料夾（demo-obsidian-sync）：
![[Pasted image 20240408223548.png]]開啟這個 Vault 的時候可能會跳出一些錯誤訊息，這是因為還沒有設定好 Git 相關設定，只要到設定頁面中，將 `Authentication/Commit Author` 段落中的設定填妥即可。![[Pasted image 20240408223607.png]]
[## [安卓使用Git同步Obsidian](https://blog.ibrowse.top/posts/using-git-to-sync-android-obsidian/)](https://blog.ibrowse.top/posts/using-git-to-sync-android-obsidian/)
- 从[F-Droid](https://f-droid.org/en/packages/com.termux/)上安装[Termux](https://github.com/termux/termux-app)，不要从Google Play Store安装，Play Store上的不再维护了
- 安装完成后，打开Termux，执行以下命令更新Termux中的软件：
    - `termux-change-repo` ：设置termux的repo源
    - `pkg update` ：更新apt数据库
    - `pkg upgrade`：更新所有安装的包
- 执行`termux-setup-storage` ，根据提示授予Termux对应的文件系统权限，设置完成后，Termux的Home目录下，会有一个storage文件夹，其中`~/storage/shared` 是我们要关注的，这个文件夹是`/storage/emulate/0` 的软链接
- 安装git：`pkg install git`
- 克隆你的Obsidian Vault到Android：`git clone https://gitlab/user/repo ~/storage/shared/repo`
- 打开Obsidian，选择repo文件夹做为Vault

# [termux不用root将里面的文件导出](https://www.cnblogs.com/arci/p/15379126.html)
让我们先输入查看当前绝对路径命令

pwd  
image.png

可见当前目录为/data/data/com.termux/files/home

再输入指令看看当前目录下的文件

ls  
image.png

可以看到的是这里有一个zphisher，我们要做的就是将他导出，让我们用户可以随意操作

既然是将termux里面的文件移动到我们可以操作的路径，那么一定需要访问内存的权限

termux-setup-storage  
将此命令输入并同意，就行了。

然后再介绍一个东西叫做文件软链接，文件软链接，就是termux可以访问的路径，用户也可以访问的路径，就叫软链接，其实说白了，就是给termux加了个快捷方式，更快的访问某个路径而已，然后把要导出的文件移动到该软链接下即可实现无root导出了。

那么如何创建软链接呢？此命令就是将手机该绝对路径 "/storage/emulated/0/Download/ " 设为软链接并取了个名字叫sharepath

ln -s /storage/emulated/0/Download/ sharepath  
顺便再跟一个 “ls” 命令查看一下当前目录下的文件，多了一个sharepath
