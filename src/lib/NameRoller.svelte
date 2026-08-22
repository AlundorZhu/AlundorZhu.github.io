<script module lang="ts">
  export type Name = { text: string; lang?: string }
</script>

<script lang="ts">
  let { names }: { names: Name[] } = $props()

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

<button
  class="roller"
  type="button"
  aria-label={current.text}
  style="--turns: {turns}"
  onclick={() => turns++}
>
  <span class="sizer" aria-hidden="true">
    {#each names as name}
      <span lang={name.lang}>{name.text}</span>
    {/each}
  </span>
  <span class="cube" aria-hidden="true">
    {#each faces as name, f}
      <span class="face" style="--f: {f}" lang={name.lang}>{name.text}</span>
    {/each}
  </span>
</button>

<style>
  .roller {
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

  /* Invisible, but stays in flow so it gives the button its width, height, and
     baseline. Every name is stacked into the single grid cell below, so the
     cell measures the widest one and the width never changes as we roll. */
  .sizer {
    display: inline-grid;
    visibility: hidden;
    user-select: none;
  }
  .sizer > span {
    grid-area: 1 / 1;
    white-space: nowrap;
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
