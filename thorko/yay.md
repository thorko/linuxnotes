# error running yay
```
export XDG_CONFIG_HOME=/home/thorko/.config/yay/
```

# just download
```
yay -G <package>
```

## clean install

```bash
yay -S --rebuildall <package>
```

## only perform yay upgrades

```bash
yay -Qu
yay -S <package>
```

## overwrite conflicting files in package

```bash
yay -S --overwrite '*' <package>
pacman -S --overwrite '*' <package>
pacman -U --overwrite '*' <package>
```

