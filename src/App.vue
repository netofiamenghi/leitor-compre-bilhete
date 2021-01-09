<template>
  <div class="container">
    <div class="header">
      <img src="./assets/Logo.png">
    </div>
    <div class="div-titulo">
      <h1 class="titulo">Entrada/Bilheteria</h1>
    </div>
    <div class="camera">
      <qrcode-stream @decode="onDecode" @init="onInit" />
    </div>
    <div class="msg">
      <p class="error">{{ error }}</p>
      <h1 class="decode">{{ result }}</h1>
    </div>
    <div class="footer">
      <center><img src="./assets/Compre Bilhete Amarelo.png"/></center>
    </div>
  </div>
</template>

<script>

import { QrcodeStream } from 'vue-qrcode-reader'

export default {
  components: { QrcodeStream },
  data () {
    return {
      result: '',
      error: ''
    }
  },
  methods: {
    onDecode (result) {
      this.$http.get('https://comprebilhete.com.br/portaria/atualizar-bilhete.php?bilhete='+result)
      .then(res => this.result = res.body.resposta)
    },
    async onInit (promise) {
      try {
        await promise
      } catch (error) {
        if (error.name === 'NotAllowedError') {
          this.error = "ERROR: you need to grant camera access permisson"
        } else if (error.name === 'NotFoundError') {
          this.error = "ERROR: no camera on this device"
        } else if (error.name === 'NotSupportedError') {
          this.error = "ERROR: secure context required (HTTPS, localhost)"
        } else if (error.name === 'NotReadableError') {
          this.error = "ERROR: is the camera already in use?"
        } else if (error.name === 'OverconstrainedError') {
          this.error = "ERROR: installed cameras are not suitable"
        } else if (error.name === 'StreamApiNotSupportedError') {
          this.error = "ERROR: Stream API is not supported in this browser"
        }
      }
    }
  }
}
</script>

<style scoped>
  *{
    margin: 0;
    padding: 0;
  }
   .container{
    width: 100%;
  }
  .error {
    color: #FFC700;
  }
  .header {
    background-color: black;
    padding: 10px;
    border-radius: 5px;
    margin-top: 5px;
    margin-bottom: 10px;
  }
  .header img {
    width: 10%;
  }
  .footer {
    background-color: black;
    padding: 10px;
    border-radius: 5px;
    position: fixed;
    bottom: 0;
    margin-top: 10px;
  }
  .footer img {
    width: 30%;
  }
  .decode{
    background-color: #FFC700;
    border-radius: 10px;
    padding: 20px;
    font-size: 4em;
  }
  .div-titulo{
    margin-bottom: 10px;
  }
  .titulo{
    font-size: 4em;
    padding: 10px;
  }
  .camera{
    height: 700px;
  }
</style>