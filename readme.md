



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


```
convert_cbz()
{
mkdir "_converted"
cp "$1" "_converted"
cd "_converted"
clear
pdftoppm -jpeg "$1" "$1"
tar -caf "$1.cbz" *.jpg
find . -type f -iname \*.jpg -delete
cd ..
}
```

Convert a folder full of PDFs to CBZ
for filename requiring spaces, put in quotations
```
convert_all_cbz()
{
mkdir "_converted"
cp *.pdf "_converted"
cd "_converted"
clear
find . -maxdepth 1 -type f -name '*.pdf' -exec pdftoppm -jpeg {} {} \;
tar -caf "$1.cbz" *.jpg
find . -type f -iname \*.jpg -delete
cd ..
}

```

```
volume()
{
#when providing the name of the manga, double quote
#requires https://github.com/Kanjirito/simple-manga-downloader/blob/master/USAGE.md
for i in "$1"; do mkdir "$i"; cd "$i" ; done
SMD down "$2" -id "$(pwd)"
tar -caf "$1 Volume 01.cbz" */Chapter\ {1..10}
tar -caf "$1 Volume 02.cbz" */Chapter\ {11..21}
tar -caf "$1 Volume 03.cbz" */Chapter\ {21..30}
tar -caf "$1 Volume 04.cbz" */Chapter\ {31..40}
tar -caf "$1 Volume 05.cbz" */Chapter\ {41..50}
tar -caf "$1 Volume 06.cbz" */Chapter\ {51..60}
tar -caf "$1 Volume 07.cbz" */Chapter\ {61..70}
tar -caf "$1 Volume 08.cbz" */Chapter\ {71..80}
tar -caf "$1 Volume 09.cbz" */Chapter\ {81..90}
tar -caf "$1 Volume 10.cbz" */Chapter\ {91..100}
tar -caf "$1 Volume 11.cbz" */Chapter\ {101..111}
find . !  -name "*.cbz" -delete
ls *.cbz 
}

```
