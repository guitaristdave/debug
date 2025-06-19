<script>
export default {
  name: "CameraComp",
  data() {
    return {
      first: null,
      second: null,
      third: null,
      forth: null,
      fifth: null,
      sixth: null,
      seventh: null,
      eighth: null,
      ninth: null,
      tenth: null,
    };
  },
  methods: {
    async cameraRequest() {
      await this.checkCameraPermission();
    },
    async checkCameraPermission() {
      const askPermissions = () => {
        this.add('askPermissions')
        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then((media) => {
            this.add('then', media)
          })
          .catch((err) => {
            this.add('catch', err)
            
          });
      };
      if (!navigator.permissions || !navigator.permissions.query) {
         this.add('navigator', navigator)
        askPermissions();
      } else {
        try {
          const result = await navigator.permissions.query({ name: "camera" });
          if (result.state === "granted") {
          } else if (result.state === "prompt") {
            askPermissions();
          } else if (result.state === "denied") {
            this.eighth =
              "Line 55: SET_CAMERA_PERMISSION: denied. Access blocked.";
          }
          result.onchange = () => {
            this.ninth = "Line 59: Permissions changed to: " + result.state;
          };
        } catch (err) {
          this.tenth = "Line 63: Permissions query error:\n" + err.message;
          askPermissions();
        }
      }
    },
    async add(...message) {
      document.body.append(document.createElement('div').innerHTML = message.join('<br>'))
    },
  },
  computed: {
    isIOS() {
      return /iPhone|iPad|iPod/i.test(navigator.userAgent);
    },
  },
};
</script>

<template>
  <div>
    <button @click="cameraRequest">Разрешить доступ к камере</button>
    <ol>
      <li>{{ first }}</li>
      <li>{{ second }}</li>
      <li>{{ third }}</li>
      <li>{{ forth }}</li>
      <li>{{ fifth }}</li>
      <li>{{ sixth }}</li>
      <li>{{ seventh }}</li>
      <li>{{ eighth }}</li>
      <li>{{ ninth }}</li>
      <li>{{ tenth }}</li>
    </ol>
    <p v-if="!isIOS">это iOS</p>
  </div>
</template>

<style>
button {
  border: 1px solid black;
  background-color: yellow;
  color: black;
  padding: 15px;
}
</style>
