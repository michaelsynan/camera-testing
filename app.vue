<template>
  <div>
    <canvas ref="canvasRef" @click="getPixelColor" style="display: block; width: 100%; max-width: 600px; height: auto;"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const canvasRef = ref(null);
let imageRef;

onMounted(() => {
  // Ensure code runs only in the browser
  if (typeof window !== 'undefined') {
    imageRef = new Image();  // Initialize the Image object in the client-side
    imageRef.onload = () => drawImage();  // Set up the onload handler
    imageRef.src = '/hellsgate.jpg';  // Set the image source
  }
});

const loadImageOnCanvas = () => {
  if (typeof window !== 'undefined' && imageRef) {
    drawImage();  // Draw the image when the button is clicked
  }
};

const drawImage = () => {
  const canvas = canvasRef.value;
  if (canvas && imageRef) {
    const context = canvas.getContext('2d');
    canvas.width = imageRef.width;
    canvas.height = imageRef.height;
    context.drawImage(imageRef, 0, 0);
  }
};

const getPixelColor = (event) => {
  const canvas = canvasRef.value;
  if (canvas) {
    const context = canvas.getContext('2d');
    const rect = canvas.getBoundingClientRect();
    const x = Math.floor((event.clientX - rect.left) * (canvas.width / rect.width));
    const y = Math.floor((event.clientY - rect.top) * (canvas.height / rect.height));
    const imageData = context.getImageData(x, y, 1, 1).data;
    alert(`RGBA at (${x},${y}): ${imageData[0]}, ${imageData[1]}, ${imageData[2]}, ${imageData[3]}`);
  }
};
</script>

<style scoped>
canvas {
  max-width: 100%; /* Ensures the canvas is responsive but maintains the image's aspect ratio */
  display: block;
  height: auto;
}
</style>
