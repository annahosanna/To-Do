# Git Configuration

People keep asking me this so I decided to write it down here.

- For Windows check that you already have ssh installed, if not then you can download it from `https://github.com/PowerShell/Win32-OpenSSH`

1. In your home directory (`~`) check that you already have a `.ssh` directory. If not create it. If on Linux or MacOS set the directory permissions to `700`.
2. Use `ssh-keygen -t rsa`, and `ssh-keygen -t ed25519` to create public and private keys in that directory for each of those. Set the private key permission to `600` and the public key permision to `644`. So Git repositories require RSA keys while others do not allow RSA keys.

- For Windows Git can be installed from `https://git-scm.com/`, for MacOS you can install the Xcode command line tools by running `xcode-select --install`, for Linux use the package manager. (I recommend installing command line Git since it is more flexible than GUI Git)

- Install your editor of choice such as VSCode, Zed, or Eclipse. I recommend Zed if it is supported by your platform because of its strong AI integration. (`https://github.com/Aider-AI/aider` sounds interesting too)

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# A good merge conflict resolution tool is Perforce p4merge
# https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration

# On a Windows system
# Convert to LF on checkin
# Convert to CRLF on checkout
git config --global core.autocrlf true

# On a macos or linux system
# Convert to LF on checkin, just in case
# Do not change it (since it should be LF already) on checkout
git config --global core.autocrlf input
```
