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
        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then((media) => {
            this.first =
              "Line 23: SET_CAMERA_PERMISSION (getUserMedia success)\nmedia.active: " +
              media.active;
          })
          .catch((err) => {
            this.second =
              "Line 28: getUserMedia error:\n" + err.name + " — " + err.message;
            this.third = "Line 29: SET_CAMERA_PERMISSION: false";
          });
      };

      if (!navigator.permissions || !navigator.permissions.query) {
        this.forth =
          "Line 35: Permissions API not supported. Fallback to getUserMedia.";
        askPermissions();
      } else {
        try {
          const result = await navigator.permissions.query({ name: "camera" });
          this.fifth =
            "Line 41: Permissions query result.state: " + result.state;

          if (result.state === "granted") {
            this.sixth = "Line 48: SET_CAMERA_PERMISSION: granted";
          } else if (result.state === "prompt") {
            this.seventh =
              "Line 51: SET_CAMERA_PERMISSION: prompt, requesting access...";
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
    <p v-if="isIOS">это iOS</p>
  </div>
</template>
