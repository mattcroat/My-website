<script>
  import { onMount } from 'svelte';

  let isDarkMode = false; // Updated default value
  let fontSize = 16;
  let contrast = 'default';
  let color = 'default';

  function disableTransitionsTemporarily() {
    document.documentElement.classList.add('transition-none');
    window.setTimeout(() => {
      document.documentElement.classList.remove('transition-none');
    }, 0);
  }

  function increaseFontSize() {
    fontSize += 2;
    document.documentElement.style.fontSize = `${fontSize}px`;
  }

  function decreaseFontSize() {
    fontSize -= 2;
    document.documentElement.style.fontSize = `${fontSize}px`;
  }

  function increaseContrast() {
    contrast = 'high';
    document.documentElement.classList.add('high-contrast');
  }

  function decreaseContrast() {
    contrast = 'default';
    document.documentElement.classList.remove('high-contrast');
  }

  function changeColor() {
    color = 'red';
    document.documentElement.style.color = 'red';
  }

  onMount(() => {
    const storedFontSize = localStorage.getItem('fontSize');
    if (storedFontSize) {
      fontSize = parseInt(storedFontSize, 10);
      document.documentElement.style.fontSize = `${fontSize}px`;
    }

    const storedIsDarkMode = localStorage.getItem('isDarkMode');
    if (storedIsDarkMode) {
      isDarkMode = storedIsDarkMode === 'true';
      if (isDarkMode) {
        document.documentElement.classList.add('dark');
      }
    }
  });
</script>

<button
  type="button"
  class="w-5 h-5 sm:h-8 sm:w-8 sm:p-1"
  on:click={() => {
    isDarkMode = !isDarkMode;
    localStorage.setItem('isDarkMode', isDarkMode.toString());
    disableTransitionsTemporarily();
    if (isDarkMode) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }}
>
  <span class="hidden text-zinc-500 dark:block">🌒</span>
  <span class="block text-zinc-400 dark:hidden">☀️</span>
</button>

<button type="button" on:click={increaseFontSize}>Increase Font Size</button>
<button type="button" on:click={decreaseFontSize}>Decrease Font Size</button>
<button type="button" on:click={increaseContrast}>High Contrast</button>
<button type="button" on:click={decreaseContrast}>Low Contrast</button>
<button type="button" on:click={changeColor}>Change Color</button>

<style>
  .transition-none {
    transition: none !important;
  }

  .high-contrast {
    filter: contrast(125%);
  }
</style>
