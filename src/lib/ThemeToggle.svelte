<script lang="ts">
  type Theme = 'light' | 'dark'

  const system = matchMedia('(prefers-color-scheme: dark)')

  let override = $state(localStorage.getItem('theme') as Theme | null)
  let systemDark = $state(system.matches)

  const dark = $derived(override ? override === 'dark' : systemDark)

  // A MediaQueryList is an EventTarget, so we can follow the OS theme instead
  // of sampling it once at mount. The function $effect returns is its cleanup.
  $effect(() => {
    const onChange = (e: MediaQueryListEvent) => (systemDark = e.matches)
    system.addEventListener('change', onChange)
    return () => system.removeEventListener('change', onChange)
  })

  // Derive the DOM from state rather than patching it inside the handler, so
  // the two can't drift. '' clears the inline style, falling back to app.css.
  $effect(() => {
    document.documentElement.style.colorScheme = override ?? ''
  })

  function toggle() {
    override = dark ? 'light' : 'dark'
    localStorage.setItem('theme', override)
  }
</script>

<button
  type="button"
  aria-label={dark ? 'Switch to light theme' : 'Switch to dark theme'}
  onclick={toggle}
>
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
