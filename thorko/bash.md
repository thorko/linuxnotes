bash
========================

bash regex match

```
[[ $output =~ merge_requests/([0-9]+) ]]
echo ${BASH_REMATCH[1]}
```

getopts in bash

```
while getopts "k:nds:f:" opt; do
  case "$opt" in
    k)
      keywords=$OPTARG
      ;;
    n)
      noocr=1
      ;;
    d)
      dialog=1
      ;;
    s)
      pages=$OPTARG
      ;;
    f)
      file=$OPTARG
      ;;
  esac
done
```

substring of var

```
# from p 1-"count p"-2
device="${p:1:${#p}-2}
```
### read with autocomplete
```
read -e -p "What is your path: " path
```


### remove extension from filename
```
${filename%.*}
```

### read file line by line
```
while IFS= read -r line; do
done
```

### options
```
while getopts "f:t:i:x" opt; do
  case "$opt" in
    f)
      source_branch=$OPTARG
      ;;
    t)
      to_branch=$OPTARG
      ;;
    i)
      project_id=$OPTARG
      ;;
    x)
      bool=true
      ;;
  esac
done
```

### read file in array
```
IFS=$'\r\n' command eval 'array=($(cat file))'
```

### templating with make
```
# Makefile
all:
        eval "echo -e "$$(<template)""
---
# template
NAMESPACE=${var}
heute=${var2}
---
var=hallo var2=heute make
```

## increment number with sed

```bash
sed -i -r 's/^version: ([0-9]+\.[0-9]+\.)(.*)/echo "version: \1$((\2+1))"/ge' Chart.yaml
```

## compare two remote files

```bash
diff <(ssh user@remote-host1 'cat /path/to/file1') <(ssh user@remote-host2 'cat /path/to/file2')
```

# dynamic variable

```bash
d1="hallo"
d2="blbal"
echo ${!1}

./test d1
> hallo
./test d2
> blbal
```

## assoziative array

```bash
declare -A locs=(["DENIC"]="work" ["FRITZ!Box v2 630 Cable"]="home")
a="FRITZ!Box v2 630 Cable"
echo ${locs[$a]}
```

