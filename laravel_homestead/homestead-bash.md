# Add "homestead" alias via Bash profile

1. Change to root directory by `cd ~/`
2. Run `touch .bash_profile`
3. Add following command to this file :
```bash
function homestead ( ) {
( cd ~/Homestead && vagrant $* )
}
```
then save and quit

4. Run `source ~/.bash_profile`
5. You can type `homestead up` and `homestead ssh` to start and access the vagrant box everywhere!

