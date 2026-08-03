<script lang="ts">
  import ThemeToggle from './lib/ThemeToggle.svelte'
  import NameRoller from './lib/NameRoller.svelte'

  const names = ['Ziqiang Zhu', '朱自强', 'Joe Zhu']
</script>

<header>
  <h1><NameRoller {names} /></h1>
  <ThemeToggle />
</header>

<style>
  header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
  }
</style>
