<template>
  <div>
    <video ref="videoRef" autoplay></video>
    <canvas ref="canvasRef" style="display: none;"></canvas>
    <button @click="captureAndProcessImage">Do magic</button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const videoRef = ref(null);
const canvasRef = ref(null);

onMounted(async () => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true });
    videoRef.value.srcObject = stream;
  } catch (error) {
    console.error('Error accessing the camera:', error);
  }
});

const captureAndProcessImage = () => {
  const video = videoRef.value;
  const canvas = canvasRef.value;
  const context = canvas.getContext('2d');

  canvas.width = video.videoWidth;
  canvas.height = video.videoHeight;

  context.drawImage(video, 0, 0, canvas.width, canvas.height);

  const imageData = context.getImageData(0, 0, canvas.width, canvas.height);

  console.log(imageData);
};
</script>

<style scoped>
video {
  width: 100%;
  height: auto; 
}
</style>
