# Git: Identity Profiles

A bunch of Git alias to allow manage user profiles, saving the profile parameters as global config.

## Prerequisites

1. Install [Git](https://git-scm.com/).
2. [Create GPG keys for each profile and remember it public id](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-gpg-key).

## Install / Terminal Setup

First of all, It's needed register the alias to set the parameters of any profile (**identity-set**). In the command line we can see that require one parameter (`profileName`) and read from console three more (`name`, `email` and GPG `signingKey`) to map profile settings under the key `user.identities`.

```sh
git config --global alias.identity-set '! read -p "user.name: " userName; read -p "user.email: " userEmail; read -p "user.signingKey: " userSigningKey; git config --global "user.identities.$1.name" "$userName"; git config --global "user.identities.$1.email" "$userEmail"; git config --global "user.identities.$1.signingKey" "$userSigningKey"; :'
```

The second alias will be used to unset profiles (**identity-unset**). The command line expect the `profileName` as parameter and removes all global settings related with that mapped key under `user.identities`.

```sh
git config --global alias.identity-unset '! git config --global --unset "user.identities.$1.name"; git config -global --unset "user.identities.$1.email"; git config -global --unset "user.identities.$1.signingKey"; :'
```

Next step. We register the alias used to require a desired profile (**identity-use**). In this terminal command, we can see that next user parameters (`name`, `email` and GPG `signingKey`) are setted in current Git repository with previous stored values (as global or local variables) under the mapped key `user.identities`.

```sh
git config --global alias.identity-use '! git config user.name "$(git config user.identities.$1.name)"; git config user.email "$(git config user.identities.$1.email)"; git config user.signingKey "$(git config user.identities.$1.signingKey)" :'
```

**This solution isn't automatic**, so as as last step, you should unsetting user and email in your global `~/.gitconfig` and setting `user.useConfigOnly` to `true` would force git to remind you to set them manually in each new or cloned repo. So execute:

```sh
git config --global --unset user.name
git config --global --unset user.email
git config --global user.useConfigOnly true
```

### How to use

To create a new profile or update it... execute:

```sh
git identity-set [profileName]
```
where `profileName` parameter is the desired profile key. Then, the command request you for it user settings like `name`, `email` and GPG `signingKey`.

To use an existent profile... execute:

```sh
git identity-use [profileName]
```

To remove an existent profile... execute:

```sh
git identity-unset [profileName]
```
