<script lang="ts">
  let { names }: { names: string[] } = $props()

  const FACES = 4 // a cube has 4 faces around the X axis
  let turns = $state(0) // how many quarter-turns we've rolled

  const mod = (n: number, m: number) => ((n % m) + m) % m

  const current = $derived(names[mod(turns, names.length)])

  // Face f is facing the viewer whenever turns ≡ f (mod 4). So each face shows
  // the name for the next turn that will bring it to the front. The `turns - 1`
  // start means the face we just rolled away from keeps its old name until it
  // has spun out of sight, instead of swapping mid-animation.
  const faces = $derived(
    Array.from({ length: FACES }, (_, f) => {
      const t = turns - 1 + mod(f - (turns - 1), FACES)
      return names[mod(t, names.length)]
    }),
  )
</script>

<button class="roller" style="--turns: {turns}" onclick={() => turns++}>
  <span class="sizer">{current}</span>
  <span class="cube">
    {#each faces as name, f}
      <span class="face" style="--f: {f}">{name}</span>
    {/each}
  </span>
</button>

<style>
  .roller {
    /* strip the button chrome so it reads as plain text */
    font: inherit;
    color: inherit;
    background: none;
    border: none;
    padding: 0;
    text-align: left;
    cursor: pointer;

    position: relative;
    display: inline-block;
    line-height: 1.2;
    perspective: 1000px;
    --r: 0.6em; /* half the line height — the cube's radius */
  }

  /* Invisible, but stays in flow so it gives the button its width, height,
     and baseline. The visible faces are absolutely positioned on top. */
  .sizer {
    opacity: 0;
    white-space: nowrap;
    user-select: none;
  }

  .cube {
    position: absolute;
    inset: 0;
    transform-style: preserve-3d;
    transform: rotateX(calc(-90deg * var(--turns)));
    transition: transform 200ms ease-out;
  }

  .face {
    position: absolute;
    inset: 0;
    white-space: nowrap;
    backface-visibility: hidden;
    transform: rotateX(calc(90deg * var(--f))) translateZ(var(--r));
  }

  @media (prefers-reduced-motion: reduce) {
    .cube {
      transition-duration: 1ms;
    }
  }
</style>
