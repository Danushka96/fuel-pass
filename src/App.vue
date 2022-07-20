<template>
  <div>
    <div class="actions">
      <button
          :disabled="!qrInput"
          @click="generateReport"
          class="btn"
          style="background-color: #7aea7a"
      >
        Download
      </button>
      <button
          :disabled="!qrInput"
          @click="openFileInput"
          class="btn"
          style="background-color: #7aa1ea; margin-left: 10px"
      >
        Update
      </button>
    </div>
    <vue-html2pdf
        :show-layout="false"
        :float-layout="false"
        :enable-download="true"
        :preview-modal="false"
        :paginate-elements-by-height="1400"
        filename="pass"
        :pdf-quality="2"
        :manual-pagination="false"
        pdf-format="a4"
        pdf-orientation="portrait"
        pdf-content-width="350px"
        @progress="onProgress($event)"
        @hasStartedGeneration="hasStartedGeneration()"
        @hasGenerated="hasGenerated($event)"
        ref="html2Pdf"
    >
      <section slot="pdf-content">
        <div class="pass-content">
          <div class="row content-body">
            <div class="column">
              <img class="logo" alt="ceypetco-logo" :src="require('@/assets/ceypetco_logo.png')">
            </div>
            <div class="column">
              <img class="logo" alt="ioc-logo" :src="require('@/assets/ioc.png')">
            </div>
          </div>
          <div class="carpet">
            <div class="carpet-text">NATIONAL FUEL PASS</div>
          </div>
          <div class="qr-input-section" v-if="!qrInput">
            <div class="qr-input-container" @click="openFileInput">
              <img :src="require('@/assets/plus.png')" class="plus-btn">
            </div>
          </div>
          <div class="qr-section" v-if="qrInput">
            <img class="qr-code" alt="QR Code" :src="qrInput"/>
          </div>
        </div>
      </section>
    </vue-html2pdf>
    <input hidden type="file" class="qr-input" @change="setQr" ref="fileInput" accept="image/x-png">
  </div>
</template>

<script>
import VueHtml2pdf from 'vue-html2pdf'

export default {
  name: 'App',
  components: {
    VueHtml2pdf
  },
  data: () => ({
    qrInput: null,
  }),
  methods: {
    onProgress(event) {
      console.log(event);
    },
    hasStartedGeneration() {
      console.log('generation started')
    },
    hasGenerated(event) {
      console.log(event);
    },
    generateReport() {
      this.$refs.html2Pdf.generatePdf()
    },
    setQr(event) {
      this.qrInput = URL.createObjectURL(event.target.files[0]);
    },
    openFileInput() {
      this.$refs.fileInput.click()
    }
  }
}
</script>

<style>

.btn {
  cursor: pointer;
  width: 100px;
  height: 50px;
  border-radius: 20px;
  border: #7aea7a;
}

.actions {
  margin-left: 5px;
  width: 350px;
  display: flex;
  justify-content: space-between;
}

.pass-content {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 5px;
  border: 5px solid #3a3535;
  border-radius: 40px;
}

.content-body {
  padding-top: 10px;
  margin-top: 20px;
}

.column {
  float: left;
  width: 50%;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

.logo {
  width: 100px;
  height: 100px;
}

.carpet {
  margin-top: 10px;
  background-color: red;
}

.carpet-text {
  padding-top: 5px;
  padding-bottom: 5px;
  color: white;
  font-size: 20px;
  font-weight: bold;
}

.qr-section {
  margin-top: 10px;
  margin-bottom: 10px;
}

.qr-input-section {
  cursor: pointer;
  padding: 40px;
}

.qr-input-container {
  display: flex;
  align-items: center;
  justify-content: space-around;
  border: 3px dashed #868484;
  height: 280px;
}

.qr-code {
  width: 300px;
}

.plus-btn {
  width: 150px;
}
</style>
