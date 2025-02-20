https://blog.csdn.net/weixin_40922744/article/details/107576748

1. 打开终端。
    
2. 粘贴以下文本，将示例中使用的电子邮件替换为 GitHub 电子邮件地址。
    
    ```shell
    ssh-keygen -t ed25519 -C "your_email@example.com"
    ```

3. 这将以提供的电子邮件地址为标签创建新 SSH 密钥。
    
    ```shell
    > Generating public/private ALGORITHM key pair.
    ```
    
    当系统提示您“Enter a file in which to save the key（输入要保存密钥的文件）”时，可以按 Enter 键接受默认文件位置。 请注意，如果以前创建了 SSH 密钥，则 ssh-keygen 可能会要求重写另一个密钥，在这种情况下，我们建议创建自定义命名的 SSH 密钥。 为此，请键入默认文件位置，并将 id_ALGORITHM 替换为自定义密钥名称。
    
    ```shell
    > Enter a file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
    ```
    
4. 在提示符下，键入安全密码。 有关详细信息，请参阅“[使用 SSH 密钥密码](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases)”。
    
    ```shell
    > Enter passphrase (empty for no passphrase): [Type a passphrase]
    > Enter same passphrase again: [Type passphrase again]
    ```
5. 将 SSH 公钥复制到剪贴板。
    
    如果您的 SSH 公钥文件与示例代码不同，请修改文件名以匹配您当前的设置。 在复制密钥时，请勿添加任何新行或空格。
    

```shell
$ cat ~/.ssh/id_ed25519.pub
```

将新增到GitHub setting ->SSH keys

ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI<公钥部分> <注释部分>

## 测试 SSH 连接

```shell
ssh -T git@github.com
```

# linux
要测试通过 HTTPS 端口的 SSH 是否可行，请运行以下 SSH 命令：

```bash
$ ssh -T -p 443 git@ssh.github.com
# Hi USERNAME! You've successfully authenticated, but GitHub does not
# provide shell access.
```

如果这样有效，万事大吉！ 否则，可能需要[遵循我们的故障排除指南](https://docs.github.com/zh/authentication/troubleshooting-ssh/error-permission-denied-publickey)。

Note

端口 443 的主机名为 `ssh.github.com`，而不是 `github.com`。

现在，若要克隆存储库，可以运行以下命令：

```shell
git clone ssh://git@ssh.github.com:443/YOUR-USERNAME/YOUR-REPOSITORY.git
```

## [启用通过 HTTPS 的 SSH 连接](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https)

如果你能在端口 443 上通过 SSH 连接到 `git@ssh.github.com`，则可覆盖你的 SSH 设置来强制与 GitHub.com 的任何连接均通过该服务器和端口运行。

要在 SSH 配置文件中设置此行为，请在 `~/.ssh/config` 编辑该文件，并添加以下部分：

```text
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

你可以通过再次连接到 GitHub.com 来测试这是否有效：

```bash
$ ssh -T git@github.com
# Hi USERNAME! You've successfully authenticated, but GitHub does not
# provide shell access.
```


## windows

```shell
$ ssh -T git@github.com
```


# 问题
## 1、

`kex_exchange_identification: Connection closed by remote host`
`Connection closed by 198.18.3.41 port 22`

[启用通过 HTTPS 的 SSH 连接](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https)
如果你能在端口 443 上通过 SSH 连接到 `git@ssh.github.com`，则可覆盖你的 SSH 设置来强制与 GitHub.com 的任何连接均通过该服务器和端口运行。

要在 SSH 配置文件中设置此行为，请在 `~/.ssh/config` 编辑该文件，并添加以下部分：

```text
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

## 2、
`Please make sure you have the correct access rights`
`and the repository exists.`

为了正确配置.ssh目录及其内涵的文件权限，可以按照以下步骤操作： 
将.ssh目录的权限设置为700：
运行以下命令：
chmod 700 ~/.ssh
将私钥文件的权限设置为600：
运行以下命令：
chmod 600 ~/.ssh/id_rsa
将公钥文件的权限设置为644：
运行以下命令：
chmod 644 ~/.ssh/id_rsa.pub
这样做的目的是，只允许当前用户读写.ssh目录和私钥文件，而其他用户只能读取公钥文件，从而保证.ssh目录和密钥文件的安全性。 当然，你可以使用如下命令批量化的处理.ssh权限问题： 
```shell
chmod 600 ~/.ssh/* && chmod 644 ~/.ssh/*.pub && chmod 700 ~/.ssh
```