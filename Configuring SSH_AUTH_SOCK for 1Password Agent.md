---
created: 2024-01-04T13:24
updated: 2024-01-04T13:27
---
On my Mac I was getting the error `The agent has no identities.` when running `ssh-add -l` even though I had SSH keys in my personal vault.

So I added this to my ~/.zshrc:
```
export SSH_AUTH_SOCK=~/Library/Group\ Containers/2BUA8C4S2C.com.1password/t/agent.sock
```