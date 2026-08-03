<script lang="ts">
  let dark = $state(matchMedia('(prefers-color-scheme: dark)').matches)

  $effect(() => {
    document.documentElement.style.colorScheme = dark ? 'dark' : 'light'
  })
</script>

<button onclick={() => (dark = !dark)}>
  {dark ? '☀' : '☾'}
</button>

<style>
  button {
    background: none;
    border: none;
    color: inherit;
    font-size: 1rem;
    cursor: pointer;
    padding: 0;
  }
</style>
