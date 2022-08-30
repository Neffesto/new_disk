<template>
  <section class="main">
    <div class="container">
      <div class="top">
        <input type="file" id="files" ref="files" accept="image/*" multiple v-on:change="handleFilesUpload()"/>
<!--        <div class="upload">Files-->
<!--          <input type="file" id="files" ref="files" accept="image/*" multiple v-on:change="handleFilesUpload()"/>-->
<!--        </div>-->
      </div>
      <div class="bot">
          <div v-for="(file, key) in files" :key="key" class="file-listing" @click="getImageModal">
            <div class="wrapper" @click="showModal">
              <img class="preview" v-bind:ref="'image'+parseInt( key )" :class="{activeHorizon: checked[key].horizon, activeVertical: checked[key].vertical}" alt="img" src=""/>
            </div>
            <div class="check-box">
              <input type="checkbox" v-model="checked[key].horizon" @click="checkTaskDone(key)"> вписывание по горизонтали <br/>
              <input type="checkbox" v-model="checked[key].vertical" @click="checkTaskDone(key)"> вписывание по вертикали <br/>
            </div>
          </div>
      </div>
    </div>
  </section>

  <ModalWindow v-show="isModalVisible" @close="closeModal">
    <template v-slot:body>
      <div v-for="(file, key) in files" :key="key" class="file-listing" >
        <div class="wrapper wrapper-modal">
          <img class="preview" v-bind:src="url[key]" :class="{activeHorizon: checked[key].horizon, activeVertical: checked[key].vertical}" alt="img"/>
        </div>
        <div class="check-box">
          <input type="checkbox" v-model="checked[key].horizon" @click="checkTaskDone(key)"> вписывание по горизонтали <br/>
          <input type="checkbox" v-model="checked[key].vertical" @click="checkTaskDone(key)"> вписывание по вертикали <br/>
        </div>
      </div>
    </template>
  </ModalWindow>
</template>
<script>

import ModalWindow from "@/components/newdisk/ModalWindow";
export default {
  name: "NewDisk",
  components: {ModalWindow},
  data(){
    return {
      files: [],
      checked: [],
      url: [],
      isModalVisible: false,
    }
  },
  methods: {
    handleFilesUpload(){
      let uploadedFiles = this.$refs.files.files;
      for( let i = 0; i < uploadedFiles.length; i++ ){
        this.files.push( uploadedFiles[i] )
        //console.log(this.files);
        this.checked.push(
            {[i]: [
                {horizon: false},
                {vertical: false}
              ]},
        );
        //console.log(this.checked);
      }
      this.getImagePreviews();
    },
    getImagePreviews(){
      for( let i = 0; i < this.files.length; i++ ){
        if ( /\.(jpe?g|png|gif)$/i.test( this.files[i].name ) ) {
          let reader = new FileReader();
          reader.addEventListener("load", function(){
            this.$refs['image'+parseInt( i )][0].src = reader.result;
          }.bind(this), false);
          reader.readAsDataURL( this.files[i] );
        }
      }
    },

    checkTaskDone(index) {
      if (this.checked[index].horizon === true) {
        this.checked[index].horizon = false;
      }
      if (this.checked[index].vertical === true) {
        this.checked[index].vertical = false;
      }
    },
//Модальное окно
    getImageModal() {
      for (let i = 0; i < this.files.length; i++) {
        this.url.push(URL.createObjectURL(this.files[i]))
        //console.log(URL.createObjectURL(this.files[i]))
      }
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
//Пагинация
  }
}
</script>

<style scoped>
.main {
  display: flex;
  justify-content: center;
}
.container {
  max-width: 1920px;
  display: flex;
  flex-direction: column;
}
.top {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;}
/*.upload {}*/
.bot {
  display: grid;
  grid-template-columns: 320px 320px 320px;
}
.file-listing {
  margin-right: 5px;
  margin-left: 5px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
/*.file-listing:not(:last-child) {*/
/*  margin-right: 10px;*/
/*}*/
.wrapper {
  width: 300px;
  height: 300px;
  margin-bottom: 5px;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid black;
}
.wrapper-modal {
  width: 900px;
  height: 506.25px;
}
.preview {}
.check-box {
  margin-bottom: 20px;
  text-align: left;
}
.activeHorizon {
  width: 100%;
  height: auto;
}
.activeVertical {
  width: auto;
  height: 100%;
}

@media (max-width: 1024px) {
  .container {
    max-width: 1024px;
  }
  .bot {
    grid-template-columns: 320px 320px;
  }
  .wrapper-modal {
    width: 640px;
    height: 360px;
  }
}

@media (max-width: 768px) {
  .container {
    max-width: 768px;
  }
  .bot {
    grid-template-columns: 320px;
  }
  .wrapper-modal {
    width: 260px;
    height: 146.25px;
  }
}


</style>