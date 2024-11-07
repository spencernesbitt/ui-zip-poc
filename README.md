# zip-poc
This is a very simple POC to prove we can download multiple files in parallel and save them to a single zip file. The URL for the downloaded file is currently hardcoded but we can set the number of copies to get a feel for the performance.

## Creating from scratch
These are the steps I took to get this set up

## Scaffold the vue.js app
Uses Vite and Vue 3
```
npm create vue@latest
```

## Install JSZip
This handles the compression on the client

```
npm i jszip
```

## Install JSZip-Utils
This has utilities for downloading the document data. Need to also add \src\decs.d.ts as there is no type information included or available on definitely typed.
```
npm i jszip-utils
```

## Install FileSaver.Js
This allows us to save the file to the local machine.
```
npm i file-saver
``` 

Again, there is no type info included but some nice folks have made the types available so we can include them using:
```
npm i --save-dev @types/file-saver
```

## Start the server
```
npm run dev
```
