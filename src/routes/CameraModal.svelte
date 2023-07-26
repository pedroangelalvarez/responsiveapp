<!-- CameraModal.svelte -->
<script>
  let showModal = false;
  let photoDataUrl;
  let camera;
  let modal;
  import { createEventDispatcher, onMount } from 'svelte';

  const dispatch = createEventDispatcher();
  async function openCamera() {
    try {
      showModal = true;
      const stream = await navigator.mediaDevices.getUserMedia({ video: true });
      const videoElement = camera;
      if (videoElement) {
        videoElement.srcObject = stream;
      } else {
        console.error("Video element not found.");
        closeCamera();
      }
      
    } catch (error) {
      console.error("Error accessing the camera:", error);
    }
  }

  function closeCamera() {
    const videoElement = document.getElementById("camera");
    const stream = videoElement.srcObject;
    const tracks = stream.getTracks();
    tracks.forEach(track => track.stop());
    showModal = false;
  }

  function takePhoto() {
    const videoElement = document.getElementById("camera");
    const canvasElement = document.createElement("canvas");
    canvasElement.width = videoElement.videoWidth;
    canvasElement.height = videoElement.videoHeight;
    const ctx = canvasElement.getContext("2d");
    ctx.drawImage(videoElement, 0, 0, canvasElement.width, canvasElement.height);
    photoDataUrl = canvasElement.toDataURL("image/png");
    dispatch('photoTaken', { photoDataUrl });
    closeCamera();
  }
  onMount(() => {
    openCamera();
  });
</script>

{#if showModal}
  <div bind:this={modal} class="modal">
    <video bind:this={camera} id="camera" autoplay>
      <track kind="captions" /></video>
      
    <button on:click={takePhoto}>Tomar foto</button>
    <button on:click={closeCamera}>Cancelar</button>
   
  </div>
  {/if}

<style>
  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: black;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    z-index: 9999;
  }

  video {
    width: 100%;
    max-width: 400px;
  }
</style>