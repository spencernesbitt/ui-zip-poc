<script setup lang="ts">
import JSZip from 'jszip';
import JSZipUtils from 'jszip-utils';
//import { reactive } from 'vue';
import { ref } from 'vue';
import fs from 'file-saver'

const message = ref('');
const fileCount = ref(0);

function compressFiles() {
  if(fileCount.value > 0){
    message.value = "Retrieving files ...";
    const url = 'http://downloads.bbc.co.uk/learningenglish/towardsadvanced/unit_1/bbc_1_9_uses_of_time.pdf';
    const files = new Array(fileCount.value).fill(url);
    getAndZipFiles(files);
    message.value = "done";
  } else {
    message.value = "Please select a value > 0";
  }
}


function getAndZipFiles(urls:Array<string>)
{
  const zip = new JSZip();
  let downloading = 0;
  let downloaded = 0;
  const zipFilename = "zipFilename.zip";
  console.log("Files to get: " +urls.length);
  urls.forEach(function(url:string){
    downloading++;
    const filename = "bbc_1_9_uses_of_time_"+downloading+".pdf";
    // loading a file and add it in a zip file
    console.log("Downloading "+filename);
    JSZipUtils.getBinaryContent(url, function (err:Error, data:Uint8Array){
      if(err) {
        console.log(err);
        throw err; // or handle the error
      }

      console.log("Retrieved "+ filename);
      downloaded++;
      zip.file(filename, data, {binary:true});
      console.log("Downloaded " + downloaded)
      if (downloaded == urls.length){
        console.log("Saving Zip file");
        zip.generateAsync({type:'blob'}).then(function(content) {
          fs.saveAs(content, zipFilename);
        });
      }
    });
  });
}

function onFileCountInput(e:Event){
  if(e.target){
    fileCount.value = +(<HTMLInputElement>e.target).value;
  }
}
// function onFileUrlsInput(e:Event){
//   if (e.target){
//     fileUrls.value = (<HTMLTextAreaElement>e.target).value;
//   }
//}

</script>

<template>
  <!--<h1>{{ message.split('').reverse().join('') }}</h1>-->
  <h1>Input the number of files download</h1>
  <!-- <p>Count is: {{ counter.count }}</p> -->
  <form v-on:submit.prevent="compressFiles">
     <!-- <input type="file" name="zip_file" accept=".zip"></input> -->
     <!-- <textarea name="file_urls" cols="40" rows="10" @input="onFileUrlsInput"></textarea> -->
     <input name="fileCount" type="number" min="0" max="100" size="20" @input="onFileCountInput"/>
     <button type="submit">Compress files</button>
  </form>
  <h2>{{ message }}</h2>
  <!--
  <form v-on:submit.prevent="compressFiles">
     <input type="file" name="zip_file" accept=".zip"></input>
     <textarea name="file_urls" cols="40" rows="10"></textarea>
     <button type="submit">Compress files</button>
  </form>
-->

<!-- http://downloads.bbc.co.uk/learningenglish/towardsadvanced/unit_1/bbc_1_9_uses_of_time.pdf -->
</template>
