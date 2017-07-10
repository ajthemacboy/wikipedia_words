# wikipedia_words


### concat
find wikipedia -type f -exec cat {} \; | grep -v "<" > concat1.txt

#### ignore json errors
cat concat1.txt | jq -Rr 'fromjson? | .text' > concat1-text.txt

#### convert to lowercase
tr '[:upper:]' '[:lower:]' < concat1-text.txt > concat1-lowercase.txt

#### split into words
cat concat1-lowercase.txt | sed -e 's/[^a-zA-Z*0-9]/ /g;s/  */ /g' | tr " " "\n" | sed '/^\s*$/d' > concat1-words.txt

### de-dupe
sort --parallel=32 -uo concat1-dedupe.txt concat1-words.txt
