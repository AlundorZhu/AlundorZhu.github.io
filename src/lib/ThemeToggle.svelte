<script lang="ts">
  type Theme = 'light' | 'dark'

  const system = matchMedia('(prefers-color-scheme: dark)')

  // Deliberately not persisted. The OS preference is the source of truth; a
  // click is a temporary divergence that lasts until the next OS change or the
  // next page load, whichever comes first.
  let override = $state<Theme | null>(null)
  let systemDark = $state(system.matches)

  const dark = $derived(override ? override === 'dark' : systemDark)

  // A MediaQueryList is an EventTarget, so we can follow the OS theme instead
  // of sampling it once at mount. Clearing the override here means a click is
  // only a temporary divergence: the next OS change takes the wheel back.
  // The function $effect returns is its cleanup.
  $effect(() => {
    const onChange = (e: MediaQueryListEvent) => {
      systemDark = e.matches
      override = null
    }
    system.addEventListener('change', onChange)
    return () => system.removeEventListener('change', onChange)
  })

  $effect(() => {
    document.documentElement.style.colorScheme = override ?? ''
  })

  function toggle() {
    override = dark ? 'light' : 'dark'
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
