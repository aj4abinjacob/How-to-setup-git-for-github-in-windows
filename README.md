# How to setup git for github in windows

*use (shift+insert) to paste in git bash shell
### 1. Login to your github account via browser and go to https://github.com/settings/emails and add a primary email address this email will be used through out this tutorial 

### 2. Download git from https://git-scm.com/download/win

### 3. Install git with recommended settings


### 4. Open Git Bash by using search 

### 5. Add your name and email by typing the following 
```
git config --global user.name "your full name"
git config --global user.email "yourEmailAddress@gmail.com"
```

### 6. Let's create some files and folders
```
touch ~/.bashrc
mkdir ~/.ssh
touch ~/.ssh/
```

### 7. Change directory to .ssh
```
cd ~/.ssh
```

### 8. Create ssh key 
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Give the file a name, something like id_rsa.
Two files will be created id_rsa and id_rsa.pub . The file with .pub should be added to your github account.

Add a passphrase, it's like a password for a password

### 9. Run the ssh agent
```
eval `ssh-agent -s`
```

### 10. Add the key to shh agent
```
ssh-add id_rsa
```

### 11. Copy the public ssh key to your clipboard
```
cat id_rsa.pub | clip
```
### 12. Pase the clipboard to github by going to https://github.com/settings/keys

### 13. Test if it worked
```
ssh -T git@github.com
```
you will be asked to enter the passphrase
type yes after the entering this command

#### The steps from here are to disable git bash asking id_rsa passphrase everytime you try to access github
### 14. Edit the bashrc file
```
notepad ~/.bashrc
```
An empty notepad will be opened

### 15. Paste the following commands in the notepad and save it (ctrl+s)
```
SSH_ENV="$HOME/.ssh/environment"


function start_agent {
    echo "Initializing new SSH agent..."
    touch $SSH_ENV
    chmod 600 "${SSH_ENV}"
    /usr/bin/ssh-agent | sed 's/^echo/#echo/' >> "${SSH_ENV}"
    . "${SSH_ENV}" > /dev/null
    /usr/bin/ssh-add
}

# Source SSH settings, if applicable
if [ -f "${SSH_ENV}" ]; then
    . "${SSH_ENV}" > /dev/null
    kill -0 $SSH_AGENT_PID 2>/dev/null || {
        start_agent
    }
else
    start_agent
fi
```

### 16. Edit the config file 
```
notepad ~/.ssh/config
```
A notepad will be opened

### 17. Pase the following in the notepad and save it
```
Host github.com
    HostName github.com
    User yourUserName
    IdentityFile ~/.ssh/id_rsa
```

### 18. Finally run
```
source ~/.bashrc
```
There may be a warning about wrong bashrc but ignore it

### 19. Let's test it out
Create a folder<br>
Right click and click on "Git Bash here" <br>
Go to any of your repositories and click on the green box "code" and click on ssh and then copy the ssh link<br>
Now open the Git Bash and type 
```
git clone git@github.com:yourGithubUserName/CopiedRepository.git
```
you will asked to type the passphrase and click enter. The repository will be cloned to the folder.





