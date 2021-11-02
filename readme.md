



<h2>Misc</h2>
##generates keypair

##usage genkey username username@IP

```
 genkey()
{
ssh-keygen -f ~/.ssh/"$1"-ecdsa -t ecdsa -b 521
ssh-copy-id -i ~/.ssh/"$1"-ecdsa "$2"
echo "alias "conn_"$1"=\"ssh -i ~/.ssh/"$1"-ecdsa "$2"\" >> .bashrc
}
```


##runs 2 applications at the same time
#usage split_em command_1 command_2
```
split_em()
{
tmux new-session \; \
  send-keys "$1" C-m \; \
  split-window -v \; \
  send-keys "$2" C-m \;
}
```

##creates a backup of the file you're working with
```
bak()
{
cp -v $1{,.bak_$(date +%F)}
}
```
##restores that backup
```
unbak()
{
	mv -v $1{.bak_$(date +%F),}
}
```

##Todays status for all SystemD modules
```
(
clear
for i in $( ls /etc/systemd/system/) 
do 
echo $i
systemctl status $i | grep -i "$(date +%b)" 2>/dev/null
done
)
```


##Loop through existing screens 
```
(
 for i in $(screen -ls | awk '{print $1}') 
 do screen -x "$i"
 done
)
 ```
 
 
##pulls videos from a youtube playlist and combines them into a single file

syntax: merge_playlist playlist.url filename

```
merge_playlist() 
{
  mkdir temp."$2" ; cd "$_" || exit
  youtube-dl -o '%(playlist_index)s.%(ext)s'  "$1"
  for f in *.mkv
  do echo file \'"$f"\' >>fileList.txt; done
  ffmpeg -f concat -safe 0 -i fileList.txt -c copy "$2".mkv
  mv "$2".mkv ..
  cd ..
  find -delete temp."$2"
  ls "$2".mkv
}
```

##converts a single PDF to cbz
requires 

https://linux.die.net/man/1/pdftoppm
convert_cbz origin.pdf destination

```
convert_cbz()
{
mkdir "$2"_converted
cp "$1" "$2"_converted
cd "$2"_converted
clear
pdftoppm -jpeg "$1" "$2"
tar -caf "$2".cbz "$2"-*.jpg
find . -type f -iname \*.jpg -delete
cd ..
}
```
