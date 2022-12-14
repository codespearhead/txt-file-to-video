<h1 align="center"><a href="https://paguiar.link/txt-file-to-video">TXT File to Video</a></h1>

<p align="center">
    <br>
  <a href="https://www.dreamstime.com/royalty-free-stock-photography-film-dvd-image10440737">
    <img src="https://thumbs.dreamstime.com/z/film-dvd-10440737.jpg" width="166px" height="120px"/>
  </a>
  <br><br>
    Learn to extract YouTube video links from the confort of your browser and download them from a text file
  <br>
</p>

<br>

## Introduction

TODO

## Define the number of the instance

```sh
instance=2
```

Ps: this can be any number. We're just assigning a different number to each instance so we don't lose track of which instance is download which text file with YouTube links.

## Change prompt

```sh
echo "PS1=\"\[\033[01;32m\]\u@webscraper_$instance\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ \"" >> ~/.bashrc && source ~/.bashrc
```

## Install yt-dlp

```sh
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && sudo chmod a+rx /usr/local/bin/yt-dlp
```

## Install tmux and ffmpeg and vim, then reboot

```sh
sudo apt update &&
sudo apt install tmux ffmpeg vim -y &&
sudo reboot
```

Ps: if it seems that the terminal is hung at "All packages are up to date", just tap ENTER

## Make sure yt-dlp, ffmpeg, tmux and vim are installed

```sh
printf "\n- Tmux version: %s\n- FFMPEG version: %s\n- YT-DLP version: %s\n- Vim version: %s\n\n" "$(tmux -V | awk '{print $NF}')" "$(ffmpeg -version | sed -n "s/ffmpeg version \([-0-9.]*\).*/\1/p;")" "$(yt-dlp --version)" "$(vim --version | head -n 1)"
```

# Section 2 - Buckle up!

## Open Tmux

```sh
tmux
```

## Define Instance variable (again)

```sh
instance=1
```

## Create a folders inside your Amazon EFS. It's advisable each instance operate on a separate folder and the lists to be in a separate folder as well

```sh
sudo mkdir /mnt/efs/fs1/$instance
```

## Create a text file and paste links into it

```sh
sudo vim "/mnt/efs/fs1/lists/$instance.txt"
```

Ps: press i to enter insert mode, right-click anywhere inside the terminal and click paste.

## Check if all lines are there

Press ESC to quit the Insert mode and type :set nu to show line numbers. If all lines are there, press EST again, type :x and press ENTER, to exit and saving changes.

## Go to folder in each instance

```sh
cd /mnt/efs/fs1/$instance
```

Ps: I suggest clearing the terminal after that. To do so, just run "clear" and press ENTER

# Section 3 - Go!

## Start downloading all videos in listed in file "1.txt"

```sh
list_path="../lists/$instance" &&
sudo yt-dlp --batch-file "$list_path.txt" -f 'bv*[height=1080]+ba' --embed-thumbnail --embed-metadata
```

## Detach the tmux session

Click CRTL+B+D

## Close ssh session

exit

Ps: important to note that every time you enter the session, you have to detach from it instead of running "exit" inside the session, or else the session will be closed.

## Mention code to extract all the links [TODO]

[3] ...

## Code to

```sh
ls | wc -l
```

## Mention code to compare files [TODO]

[4] ...

## Code to print out missing video links

[5]
[6]

```sh
instance=7
downloadedvideos=""
missingvideos=""

for file in *; do
  [[ "$file" =~ \[([^\)]+)\] ]]
  downloadedvideos+="https://www.youtube.com/watch?v=${BASH_REMATCH[1]}"$'\n'
done

missingvideos=$(grep -vFx "$downloadedvideos" "../lists/$instance.txt")

[TODO]
#print zero if there are no missing videos or the links if there are
printf "Missing videos: %s" "$(missingvideos)"
```

## Credits: [TODO]

[1] https://superuser.com/questions/1609806/get-ffmpeg-version-number
[2] https://github.com/yt-dlp/yt-dlp
[3] https://www.quora.com/How-do-I-grab-the-individual-urls-of-a-YouTube-playlist
[4] https://stackoverflow.com/questions/4078933/find-difference-between-two-text-files-with-one-item-per-line
[5] https://stackoverflow.com/questions/6208367/regex-to-match-stuff-between-parentheses
[6] https://superuser.com/questions/31464/looping-through-ls-results-in-bash-shell-script

# Disclaimers

> **Note**: We are not affiliated, associated, authorized, endorsed by or in any way officially connected to YouTube, LLC. (www.youtube.com).

> **Warning**: As of today [2022-12-12], YouTube's against video downloading, as explicited in the Permissions and Restrictions section of their [Terms of Service](https://www.youtube.com/t/terms): You are **not allowed to**: access, reproduce, **download**, distribute, transmit, broadcast, display, sell, license, alter, modify or otherwise use any part of the Service or any Content **except**: (a) as expressly authorized by the Service; or (b) **with prior written permission from YouTube** and, if applicable, the respective rights holders.

> **Note**:
> Copyright Disclaimer under section 107 of the Copyright Act 1976, allowance is made for “fair use” for purposes such as criticism, comment, news reporting, teaching, scholarship, education and research.
> Fair use is a use permitted by copyright statute that might otherwise be infringing.
> Non-profit, educational or personal use tips the balance in favor of fair use.
