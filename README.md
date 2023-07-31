# How to migrate the Accented Australian English

## Pre-requisites

0. This was run on mac/os the similar steps should be reproducible in linux or in a Windows Subsystem Linux (WSL)
1. Node
2. Docker
3. Siegfried
4. Git clone this repo
5. Transcripts Folder
6. Audio Folder

## Repos

1. git@github.com:Language-Research-Technology/corpus-tools-ro-crate.git
   1. cd corpus-tools-ro-crate
   2. npm install
2. git@github.com:Language-Research-Technology/ro-crate-excel
   1. cd ro-crate-excel
   2. npm install
   3. npm link

## Files

The default storage in the configuration is in /opt/storage/oni

```
sudo mkdir -p /opt/storage/oni
sudo chown $USER:$USER /opt/storage/oni
```

## Spreadsheet

Data has already been curated into a spreadsheet.

We need to create another spreadhseet called `additioal-ro-crate-metadata.xlsx` so that our tool can pick it up

## Spreadsheet to RO-Crate

create a `data` folder

If you are running from scratch. 

```
cd data
rocxl .
# a blank ro-crate will be bootstrapped
cp -r Audio_Anonymised data/
cp -r Transcripts_Anonymised data/
xlro -a .
# it will read additional-ro-crate-metadata.xlsx
```

## RO-Crate to OCFL

```
cd corpus-tools-ro-crate
```
Create a make_run.sh file; example:

Where the base is: BASE_DATA_DIR


```bash
make BASE_DATA_DIR=/Users/moises/source/github/Language-Research-Technology/corpus-tools-accented-australian-english/data \
 TEMPLATE_DIR=/Users/moises/source/github/Language-Research-Technology/corpus-tools-accented-australian-english/data \
 REPO_OUT_DIR=/opt/storage/oni/ocfl \
 REPO_SCRATCH_DIR=/opt/storage/oni/scratch-ocfl \
 BASE_TMP_DIR=./storage/temp \
 NAMESPACE=accented-australian-english \
 CORPUS_NAME=accented-australian-english \
 DATA_DIR=/Users/moises/source/github/Language-Research-Technology/corpus-tools-accented-australian-english/data
 ```

This will add the data into the OCFL repository
```
/opt/storage/oni
```

## OCFL-to-Oni

git clone https://github.com/Language-Research-Technology/oni-ui.git

```
cd oni-ui
npm install
docker-compose up
```

open new terminal window in oni-ui:

```
node structural-index.js
node elastic-ocfl-oni-index.js
```



