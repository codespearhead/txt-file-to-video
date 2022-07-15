## Change prompt

instance=2 && echo "PS1=\"\[\033[01;32m\]\u@webscraper_$instance\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ \"" >> ~/.bashrc && source ~/.bashrc

## Install tmux and ffmpeg

sudo apt update & sudo apt install tmux ffmpeg

Ps: if it seems that the terminal is hung at "All packages are up to date", just tap ENTER

## Install yt-dlp

sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && sudo chmod a+rx /usr/local/bin/yt-dlp

## Make sure yt-dlp, ffmpeg and tmux

printf "\n- Tmux version: %s\n- FFMPEG version: %s\n- YT-DLP version: %s\n\n" "$(tmux -V | awk '{print $NF}')" "$(ffmpeg -version | sed -n "s/ffmpeg version \([-0-9.]*\).*/\1/p;")" "$(yt-dlp --version)"

## Create a folders inside your Amazon EFS. It's advisable each instance operate on a separate folder and the lists to be in a separate folder as well

mkdir /mnt/efs/fs1/1
mkdir /mnt/efs/fs1/lists

## Go to folder in each instance

instance=1 && cd /mnt/efs/fs1/$instance

## Open tmux

tmux

## Start downloading all videos in listed in file "1.txt"

instance=2 && list_path="../lists/$instance" && sudo yt-dlp --batch-file "$list_path.txt" -f 'bv*[height=1080]+ba' --embed-thumbnail --embed-metadata

## Detach the tmux session

Click CRTL+B+D

## Close ssh session

exit

Ps: important to note that every time you enter the session, you have to detach from it instead of running "exit" inside the session, or else the session will be closed.

Credits:

https://superuser.com/questions/1609806/get-ffmpeg-version-number
https://github.com/yt-dlp/yt-dlp
https://github.com/github/gitignore/blob/main/Global/macOS.gitignore
