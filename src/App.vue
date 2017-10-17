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
  </div>
    </div>  
</template>

<!-- Javascript -->
<script>
  const STATUS_INITIAL=0, STATUS_SAVING=1, STATUS_SUCCESS=2, STATUS_FAILED=3;

  /*
  
        <h1>Upload Starbucks Card</h1>
        <div class="dropbox">
          <input type="file" multiple :name="uploadFieldName" :disabled="isSaving" @change="filesChange($event.target.name, $event.target.files); fileCount = $event.target.files.length" accept="image/*" class="input-file">
          <p v-if="isInitial">
            Drag your file(s) here to begin <br> or click to browse
          </p>

          <p v-if="isSaving">
            Cargando {{ fileCount }} archivos de Arte de Starbucks Card.
          </p>
        </div>
      </form>  */

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
