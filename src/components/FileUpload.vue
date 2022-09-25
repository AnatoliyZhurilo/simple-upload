<template>
  <button ref="uploaderRef" @click="selectFile">upload</button>
</template>

<script setup>
import qq from "fine-uploader/lib/s3";
import { onMounted, ref } from "vue";
let uploader_img = ref(null);
const uploaderRef = ref(null);

const g_bucket_name = "todoDefineName"; //todo

onMounted(() => {
  uploader_img.value = new qq.s3.FineUploaderBasic({
    debug: false,
    objectProperties: {
      key: function (id) {
        const fname = this.getName(id);
        console.log(fname);
        return fname;
      },
    },
    button: uploaderRef.value,
    request: {
      endpoint: `https://s3.amazonaws.com/${g_bucket_name}/`,
      accessKey: "AKIAWKUKPQ5NYHHEI5PB",
    },
    signature: {
      endpoint: "/S3/signature",
    },
    chunking: {
      enabled: false,
    },
    uploadSuccess: {
      endpoint: "/S3/success",
    },
    deleteFile: {
      enabled: true,
      forceConfirm: true,
      endpoint: "/S3/delete",
    },
    validation: {
      allowedExtensions: ["jpg", "jpeg", "png", "gif", "svg"],
      itemLimit: 10000,
    },
    callbacks: {
      onComplete: function (id, name, responseJSON) {
        if (responseJSON.success) {
          console.log(responseJSON);
        }
      },
      onUpload: function (id, name) {
        //
      },
    },
  });
});

const selectFile = () => {
  uploader_img.value.uploadStoredFiles();
};
</script>
