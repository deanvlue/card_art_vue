<!-- App.vue -->

<!-- HTML teplate -->
<template>
  <div id="app">
    <div class="container">
      <form enctype="multipart/form-data" novalidate v-if="isInitial || isSaving">
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
      </form>  
    </div>
    </div>  
</template>

<!-- Javascript -->
<script>
  const STATUS_INITIAL = 0, STATUS_SAVING = 1, STATUS_SUCCESS = 2, STATUS_FAILED = 3;
  
</script>

<!-- SASS Styling -->
<style lang="sass">

</style>
