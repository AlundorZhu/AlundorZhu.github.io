<script lang="ts">
  let dark = $state(matchMedia('(prefers-color-scheme: dark)').matches)

  function toggle() {
    dark = !dark
    document.documentElement.style.colorScheme = dark ? 'dark' : 'light'
  }
</script>

<button onclick={toggle}>
  {dark ? '☀' : '☾'}
</button>

<style>
  button {
    background: none;
    border: none;
    color: inherit;
    font-size: 1.2rem;
    cursor: pointer;
    padding: 0;
  }
</style>
