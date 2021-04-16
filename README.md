# GitHub Demo

## Setting up SSH keys
---

Using the SSH protocol, you can connect and authenticate to remote servers and services. With SSH keys, you can connect to GitHub without supplying your username and personal access token at each visit.

### SSH Key Types
---

| Algorithm | Public key | Private key |
| --------- | ---------- | ----------- |
|  ED25519 (preferred)  | `id_ed25519.pub` | `id_ed25519` |
|  RSA (at least 2048-bit key size)     | `id_rsa.pub` | `id_rsa` |
|  DSA (deprecated)      | `id_dsa.pub` | `id_dsa` |
|  ECDSA    | `id_ecdsa.pub` | `id_ecdsa` |

### Check for existing SSH keys

1. Open Terminal.

1. Enter ls -al ~/.ssh to see if existing SSH keys are present:

    ```shell
    $ ls -la ~/.ssh
    # Lists the files in your .ssh directory, if they exist
    ```

1. Check the directory listing to see if you already have a public SSH key. By default, the filenames of the public keys are one of the following:

    - id_rsa.pub
    - id_ecdsa.pub
    - id_ed25519.pub

### Generate SSH Key
---

1. Open a terminal.
1. Type `ssh-keygen -t` followed by the key type and an optional comment.
   This comment is included in the `.pub` file that's created.
   You may want to use an email address for the comment, but it is optional.

   For example, for ED25519:

   ```shell
   ssh-keygen -t ed25519 -C "<comment>"
   ```

1. Press Enter. Output similar to the following is displayed:

   ```plaintext
   Generating public/private ed25519 key pair.
   Enter file in which to save the key (/home/user/.ssh/id_ed25519):
   ```

1. Accept the suggested filename and directory, unless you want to save in a specific directory where you store other keys.

1. Specify a passphrase, or leave empty to not be prompted for a password on every commit (less secure, but more convenient)

   ```plaintext
   Enter passphrase (empty for no passphrase):
   Enter same passphrase again:
   ```

1. A confirmation is displayed, including information about where your files are stored.


## Add an SSH key to your GitHub account

To use SSH with GitHub, copy your public key to your GitHub account.

1. Copy the contents of your public key file. You can do this manually or use a script.
   For example, to copy an ED25519 key to the clipboard:

   **macOS:**

   ```shell
   tr -d '\n' < ~/.ssh/id_ed25519.pub | pbcopy
   ```

   **Linux** (requires the `xclip` package):

   ```shell
   xclip -sel clip < ~/.ssh/id_ed25519.pub
   ```

   **Git Bash on Windows:**

   ```shell
   cat ~/.ssh/id_ed25519.pub | clip
   ```

   Replace `id_ed25519.pub` with your filename. For example, use `id_rsa.pub` for RSA.

1. Sign in to GitHub.
1. In the top right corner, select your avatar.
1. Select **Settings**.
1. From the left sidebar, select **SSH and GPG Keys**.
1. In the top right, select **New SSH key**
1. In the **Key** box, paste the contents of your public key.
   If you manually copied the key, make sure you copy the entire key,
   which starts with `ssh-ed25519` or `ssh-rsa`, and may end with a comment.
1. In the **Title** text box, type a description, like _Work Laptop_ or
   _Home Workstation_.
1. Select **Add SSH key**.

### Proxy passing SSH

1. Open the terminal.
1. Change directories to where your ssh keys are stored.
    ```shell
    cd ~/.ssh/
    ```
1. Create a `config` file and insert this into it.
    ```plaintext
    Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519
        PreferredAuthentications publickey
        PasswordAuthentication no
        IdentitiesOnly yes
        ProxyCommand /usr/bin/nc -x 127.0.0.1:3129 %h %p
    ```
1. Save the file and exit
