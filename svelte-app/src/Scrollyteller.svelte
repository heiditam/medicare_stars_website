<script>
    import { onMount } from 'svelte';
  
    let progressBar;
  
    onMount(() => {
      const updateProgressBar = () => {
        const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
        const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
        const scrollPercent = (scrollTop / scrollHeight) * 100;
  
        // Update the width of the progress bar
        progressBar.style.width = scrollPercent + "%";
      };
  
      window.addEventListener("scroll", updateProgressBar);
  
      return () => {
        window.removeEventListener("scroll", updateProgressBar);
      };
    });
  </script>
  
  <style>
    .progress-container {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 10px;
      background-color: rgba(0,0,0,0.1);
      z-index: 99;
    }
  
    .progress-bar {
      height: 100%;
      width: 0;
      background-color: palevioletred;
      transition: width 0.2s ease;
    }
  </style>

<div class="progress-container">
    <div class="progress-bar" bind:this={progressBar}></div>
  </div>