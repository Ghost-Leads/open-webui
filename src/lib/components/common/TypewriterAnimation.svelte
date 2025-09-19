<script>
  import { onMount } from 'svelte';
  import thinkingMessages from '$lib/data/thinking-messages.json';
  
  export let onComplete = () => {};
  export let speed = 50; // milliseconds per character
  export let pauseBetweenMessages = 800; // pause between messages
  
  let currentMessageIndex = 0;
  let currentText = '';
  let isComplete = false;
  
  const messages = thinkingMessages.messages;
  
  async function typeMessage(message) {
    currentText = '';
    for (let i = 0; i <= message.length; i++) {
      currentText = message.slice(0, i);
      await new Promise(resolve => setTimeout(resolve, speed));
    }
  }
  
  async function startAnimation() {
    for (let i = 0; i < messages.length; i++) {
      currentMessageIndex = i;
      await typeMessage(messages[i]);
      
      // Pause between messages (except for the last one)
      if (i < messages.length - 1) {
        await new Promise(resolve => setTimeout(resolve, pauseBetweenMessages));
      }
    }
    
    // Final pause before showing response
    await new Promise(resolve => setTimeout(resolve, 500));
    isComplete = true;
    onComplete();
  }
  
  onMount(() => {
    startAnimation();
  });
</script>

<div class="typewriter-container">
  <div class="thinking-indicator">
    <div class="thinking-dots">
      <span class="dot"></span>
      <span class="dot"></span>
      <span class="dot"></span>
    </div>
    <div class="thinking-text">
      {currentText}
      <span class="cursor">|</span>
    </div>
  </div>
</div>

<style>
  .typewriter-container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 1rem;
    background: rgba(0, 0, 0, 0.02);
    border-radius: 8px;
    margin: 0.5rem 0;
    border-left: 3px solid #3b82f6;
  }
  
  .thinking-indicator {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  
  .thinking-dots {
    display: flex;
    gap: 4px;
  }
  
  .dot {
    width: 6px;
    height: 6px;
    background: #3b82f6;
    border-radius: 50%;
    animation: pulse 1.5s ease-in-out infinite;
  }
  
  .dot:nth-child(1) {
    animation-delay: 0s;
  }
  
  .dot:nth-child(2) {
    animation-delay: 0.3s;
  }
  
  .dot:nth-child(3) {
    animation-delay: 0.6s;
  }
  
  .thinking-text {
    font-family: 'Courier New', monospace;
    color: #4b5563;
    font-size: 0.9rem;
    font-weight: 500;
  }
  
  .cursor {
    animation: blink 1s infinite;
    color: #3b82f6;
    font-weight: bold;
  }
  
  @keyframes pulse {
    0%, 100% {
      opacity: 0.3;
      transform: scale(1);
    }
    50% {
      opacity: 1;
      transform: scale(1.2);
    }
  }
  
  @keyframes blink {
    0%, 50% {
      opacity: 1;
    }
    51%, 100% {
      opacity: 0;
    }
  }
  
  /* Dark mode styles */
  :global(.dark) .typewriter-container {
    background: rgba(255, 255, 255, 0.05);
    border-left-color: #60a5fa;
  }
  
  :global(.dark) .dot {
    background: #60a5fa;
  }
  
  :global(.dark) .thinking-text {
    color: #d1d5db;
  }
  
  :global(.dark) .cursor {
    color: #60a5fa;
  }
</style>
