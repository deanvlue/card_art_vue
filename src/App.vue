<!-- App.vue -->

<!-- HTML teplate -->
<template>
  <div id="app">
    <div class="container">
      <!--UPLOAD-->
      <form enctype="multipart/form-data" novalidate v-if="isInitial || isSaving">
        <h1>Upload images</h1>
        <div class="dropbox">
          <input type="file" multiple :name="uploadFieldName" :disabled="isSaving" @change="filesChange($event.target.name, $event.target.files); fileCount = $event.target.files.length" accept="image/*" class="input-file">
            <p v-if="isInitial">
              Drag your file(s) here to begin<br> or click to browse
            </p>
            <p v-if="isSaving">
              Uploading {{ fileCount }} files...
            </p>
        </div>
      </form>

      <!-- success -->
      <div v-if="isSuccess">
        <h2>Uploaded {{ uploadedFiles.length }} file(s) succesfully</h2>
        <p>
          <a href="javascript:void(0)" @click="reset()">Upload Again</a>
        </p>
        <ul class="list-unstyled">
          <li v-for="item in uploadedFiles" v-bind:key="item">
            <img :src="item.url" :alt="item.originalName" class="img-responsive img-thumbnail">
          </li>
        </ul>
      </div>

      <!--failed-->
      <div v-if="isFailed">
        <h2>Uploaded Failed</h2>
        <p>
          <a href="javascript:void(0)" @click="reset()">try again</a>
        </p>
        <pre>{{ uploadError }}</pre>
      </div>

  </div>
    </div>  
</template>

<!-- Javascript -->
<script>

  import {upload} from './file-upload.fake.service';

  const STATUS_INITIAL=0, STATUS_SAVING=1, STATUS_SUCCESS=2, STATUS_FAILED=3;

  //https://sbxtstcard.blob.core.windows.net/cardart?st=2017-10-17T14%3A30%3A00Z&se=2110-10-18T14%3A30%3A00Z&sp=rwdl&sv=2016-05-31&sr=c&sig=cv5P0ggq1wOXAULokFD4T5z1NCjEv5hAfuNLLIzlwVo%3D
  //?st=2017-10-17T14%3A30%3A00Z&se=2110-10-18T14%3A30%3A00Z&sp=rwdl&sv=2016-05-31&sr=c&sig=cv5P0ggq1wOXAULokFD4T5z1NCjEv5hAfuNLLIzlwVo%3D

  export default{
    name: 'app',
    data(){
      return{
        uploadedFiles: [],
        uploadedError: null,
        currentStatus: null,
        uploadFieldName: 'photos'
      }
    },

    computed:{
      isInitial() {
        return this.currentStatus === STATUS_INITIAL;
      },
      isSaving() {
        return this.currentStatus === STATUS_SAVING;
      },
      isSuccess(){
        return this.currentStatus === STATUS_SUCCESS;
      },
      isFailed(){
        return this.currentStatus === STATUS_FAILED;
      }
    },
    methods:{
      reset(){
        this.currentStatus = STATUS_INITIAL;
        this.uploadedFiles = [];
        this.uploadedError = null;
      },
      save(formData){
        // upload data to the server
        this.currentStatus = STATUS_SAVING;

        upload(formData)
          .then(x=>{
            this.uploadedFiles = [].concat(x);
            this.currentStatus = STATUS_SUCCESS;
          })
          .catch(err=>{
            this.uploadError = err.response;
            this.currentStatus = STATUS_FAILED;
          });
      },

      filesChange(fieldName, fileList){
        //handle file changes
        const formData = new FormData();
        if (!fileList.length) return;

        // append the files to FormData
        Array
          .from(Array(fileList.length).keys())
          .map( x =>{
            formData.append(fieldName, fileList[x], fileList[x].name);
          });

        //save it
        this.save(formData);
      }
    },
    mounted(){
      this.reset();
    },
  }

</script>

<!-- SASS Styling -->
<style lang="scss">
  body{
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 0.9em;
  }

  h1{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 2.2em;
  }

  .dropbox{
    outline: 2px dashed grey; /* the dash box*/
    outline-offset: -10px;
    background: #0f5934;
    color: dimgray;
    padding: 10px 10px;
    min-height: 200px; /*minium height*/
    position: relative;
    cursor: pointer;
  }

  .input-file{
    opacity: 0; /* invisible pero allí está*/
    width: 100%;
    height: 200px;
    position: absolute;
    cursor: pointer;
  }

  .dropbox:hover{
    background: lightyellow; /*cambia color cuando estamos sobre el*/
  }

  .dropbox p{
    font-size: 1.2em;
    text-align: center;
    padding: 50px 0;
  }

</style>
