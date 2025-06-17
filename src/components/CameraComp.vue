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
            console.log(
              "SET_CAMERA_PERMISSION",
              "media.active: ",
              media.active
            );
          })
          .catch(() => {
            console.log("SET_CAMERA_PERMISSION", false);
          });
      };
      if (!navigator.permissions) {
        // Устройства без поддержки Permissions API
        askPermissions();
      } else {
        const result = await navigator.permissions.query({ name: "camera" });
        if (result.state === "granted") {
          console.log("SET_CAMERA_PERMISSION", "granted", true);
        } else {
          askPermissions();
        }
      }
    },
  },
};
</script>

<template>
  <div>
    <button @click="cameraRequest">Разрешить доступ к камере</button>
  </div>
</template>
