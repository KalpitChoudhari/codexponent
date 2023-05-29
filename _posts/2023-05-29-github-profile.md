---
layout: post
title:  "Streamlining GitHub Profile Switching: Simplify Your Workflow"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/github.png
---

**Working with multiple GitHub profiles and switching them continuously is a tedious task. Switching between multiple GitHub profiles requires the following steps:**

1. Removing existing identity
2. Add current identity (location to current identity file i.e. stored in `.ssh` directory)
3. Config user name and user email
4. Verify current profile.

Now, in order to add a new SSH identity, you can follow this blog: 

[Generating a new SSH key and adding it to the ssh-agent - GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

You can create new identity and add newly created identity in config file. I’ve added 2 identities and a sample config file will looks like this:

```yaml
# Account 1 (work) - the default config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/priamry_file_name
  
# Account 2 (personal) - the config we are adding
Host github.com-PersonalAccount
  HostName github.com/PersonalAccount
  User PersonalAccount
  IdentityFile ~/.ssh/secondary_file_name
```

Suppose you have multiple projects associated with different GitHub accounts. You can switch between accounts using the following commands:

```yaml
➜  ~ ssh-add -D
➜  ~ ssh-add ~/.ssh/YOUR_ID_SSH_FILE_NAME # This command will prompt for your GitHub password
➜  ~ git config user.name 'GITHUB_USER_NAME'
➜  ~ git config user.email 'GITHUB_EMAIL'
```

To check whether we’ve set proper GitHub account, we can fire following command in terminal:

```yaml
➜  ~ ssh -T git@github.com

# The output will look something like this:
# Hi GITHUB_USER_NAME! You've successfully authenticated, but GitHub does not provide shell access.
```

Manually executing all these commands every time you switch to a different GitHub account can be tiresome. However, we're developers 🧑🏼‍💻, and we can simplify this process.

I've created a bash function and stored it in my **`.zshrc`** file. This code allows you to effortlessly switch between profiles:

```bash
profile() {
  echo "Removing All Identities"
  ssh-add -D

  echo -n "Enter Profile (root / work): "
  read -n profile
  echo ""

  if [[ "$profile" == "personal" ]]; then
    ssh-add ~/.ssh/secondary_file_name
    git config user.name 'PERSONAL_GITHUB_USER_NAME'
    git config user.email "PERSONAL_GITHUB_EMAIL"
    echo ""
    ssh -T git@github.com
  elif [[ "$profile" == "work" ]]; then
    ssh-add ~/.ssh/priamry_file_name
    git config user.name 'WORK_GITHUB_USER_NAME'
    git config user.email "WORK_GITHUB_EMAIL"
    echo ""
    ssh -T git@github.com
  else
    echo "Wrong Profile!"
  fi
}
```

To use this function, simply modify the SSH file names and your GitHub configurations (username and email). Afterward, invoking the profile command will handle the rest. Cheers! 🍻
