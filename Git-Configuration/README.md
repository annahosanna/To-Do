# Git Configuration
```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# Bad idea, but easiest way to work around a cert which is self signed or has an untrusted CA
git config --global http.sslVerify false

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

