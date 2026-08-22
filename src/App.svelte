<script lang="ts">
  import ThemeToggle from './lib/ThemeToggle.svelte'
  import NameRoller from './lib/NameRoller.svelte'

  const names = [ 'Joe Zhu', 'Ziqiang Zhu', '朱自强']

  const links = [
    { label: 'github', href: 'https://github.com/AlundorZhu' },
    { label: 'linkedin', href: 'https://www.linkedin.com/in/joe-zhu-032153259/' },
    { label: 'resume', href: 'https://github.com/AlundorZhu/Resume/blob/main/main.pdf'}
  ]

  type Publication = {
    title: string
    href: string
    authors: string
    venue: string
  }

  const publications: Publication[] = [
    {
      title: 'A Principled Framework for Safe Algorithm Updates in Automated Insulin Delivery Systems',
      href: 'https://doi.org/10.48550/arXiv.2606.13882',
      authors: "Thomas Screven, Ziqiang (Joe) Zhu, Deniz Cengiz, Rayhan A. Lal, Korey K. Hood, Samuel T. King",
      venue: "preprint"
    },
    {
      title: 'Computer Vision for Movement Observation and Recovery Enhancement (C-MORE): Box and Blocks Test',
      href: 'https://doi.org/10.3390/bioengineering13060602',
      authors: 'Kim, Jun Min, Ziqiang (Joe) Zhu, Hari Venugopalan, Vicky Chan, Matthew K. Farrens, Samuel T. King, and Andria J. Farrens',
      venue: 'Bioengineering 2026, 13, 602.',
    },
    {
      title: 'Markerless Motion Capture Enhances Clinical Assessments: Preliminary Validation with the Box and Blocks Test.',
      href: 'https://doi.org/10.1109/icorr66766.2025.11063098',
      authors: 'Andria Farrens, Vicky Chan, Luis Garcia-Fernandez, Ziqiang (Joe) Zhu, Jun Min Kim, Matthew Karl Farrens, Samuel T King',
      venue: 'IEEE Int Conf Rehabil Robot (ICORR). 2025 May.',
    },
  ]
</script>

<header>
  <h1><NameRoller {names} /></h1>
  <nav>
    {#each links as { label, href }}
      <a {href}>{label}</a>
    {/each}
    <ThemeToggle />
  </nav>
</header>

<main>
  <p>
    I'm a [PhD student / undergrad] in [department] at
    <a href="https://www.ucdavis.edu">UC Davis</a>, where I work on
    [what you work on]. [One more sentence — what pulled you into it, or what
    you're building right now.]
  </p>

  <section>
    <h2>Publications</h2>
    <ul>
      {#each publications as pub}
        <li>
          <a href={pub.href}>{pub.title}</a>
          <div class="meta">{pub.authors}</div>
          <div class="meta">{pub.venue}</div>
        </li>
      {/each}
    </ul>
  </section>
</main>

<style>
  header {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: baseline;
    gap: 0.5rem 1.5rem;
  }

  h1 {
    margin: 0;
  }

  nav {
    display: flex;
    align-items: baseline;
    gap: 1rem;
    font-size: 0.9rem;
  }

  /* Underline only on hover — three underlined items next to the name roller
     is too much texture for a header. */
  nav a {
    text-decoration: none;
  }
  nav a:hover {
    text-decoration: underline;
  }

  main {
    margin-top: 2.5rem;
  }

  section {
    margin-top: 3rem;
  }

  h2 {
    font-size: 1rem;
    font-weight: 600;
    margin: 0 0 1rem;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  li + li {
    margin-top: 1.5rem;
  }

  .meta {
    font-size: 0.9rem;
    color: var(--muted);
  }
</style>
