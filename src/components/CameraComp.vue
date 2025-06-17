<script>
export default {
  name: "CameraComp",
  methods: {
    async cameraRequest() {
      await this.checkCameraPermission();
    },
    async checkCameraPermission() {
      const askPermissions = () => {
        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then((media) => {
            alert(
              "SET_CAMERA_PERMISSION (getUserMedia success)\nmedia.active: " +
                media.active
            );
          })
          .catch((err) => {
            alert("getUserMedia error:\n" + err.name + " — " + err.message);
            alert("SET_CAMERA_PERMISSION: false");
          });
      };

      if (!navigator.permissions || !navigator.permissions.query) {
        alert("Permissions API not supported. Fallback to getUserMedia.");
        askPermissions();
      } else {
        try {
          const result = await navigator.permissions.query({ name: "camera" });
          alert("Permissions query result.state: " + result.state);

          if (result.state === "granted") {
            alert("SET_CAMERA_PERMISSION: granted");
          } else if (result.state === "prompt") {
            alert("SET_CAMERA_PERMISSION: prompt, requesting access...");
            askPermissions();
          } else if (result.state === "denied") {
            alert("SET_CAMERA_PERMISSION: denied. Access blocked.");
          }

          result.onchange = () => {
            alert("Permissions changed to: " + result.state);
          };
        } catch (err) {
          alert("Permissions query error:\n" + err.message);
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
    <p v-if="isIOS">это iOS</p>
  </div>
</template>
