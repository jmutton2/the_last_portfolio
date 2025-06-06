<script lang="ts">
  import temp from '$lib/images/temp.png';
  import languages from '$lib/data/language_urls.json';

  import AnimatedPreview from '$lib/components/AnimatedPreview.svelte';
  export let next_page: string = '';
  export let works = [];

  const allImages = import.meta.glob('$lib/images/**/*.png', { eager: true });

  // Function to filter images by work.resource
  function getImagesForWork(resource: string) {
    return Object.entries(allImages)
      .filter(([path]) => path.includes(`/${resource}/`)) // Match by folder
      .map(([, img]) => img.default);
  }
</script>

<div class="content">
  {#each works as work}
    <section class="py-28 px-5">
      <AnimatedPreview delay_offset={0}>
        <h1 class="font-extrabold text-7xl leading-[4.5rem]">
          {work.job_title}
        </h1>
      </AnimatedPreview>
    </section>

    <section class="py-28 px-5 w-4/5 mx-auto">
      <AnimatedPreview
        delay_offset={9}
        classes="justify-around w-full"
        inner_classes="flex justify-between flex-col sm:flex-row gap-8 "
      >
        <div>
          <p class="mt-4 text-black-200 mx-auto">Timeline</p>
          <p class="mt-4 text-black-200 mx-auto text-xl bolded">
            {work.job_timeline}
          </p>
        </div>
        <div>
          <p class="mt-4 text-black-200 mx-auto">Team Size</p>
          <p class="mt-4 text-black-200 mx-auto text-xl bolded text-nowrap">
            {work.job_team_size}
          </p>
          <p></p>
        </div>
        <div>
          <p class="mt-4 text-black-200 mx-auto">Role</p>
          <p class="mt-4 text-black-200 mx-auto text-xl bolded">
            {work.job_role}
          </p>
        </div>
      </AnimatedPreview>
    </section>

    <section class="py-28 px-5">
      <AnimatedPreview delay_offset={9}>
        <h1 class="font-extrabold text-kxl leading-[4.5rem]">Overview</h1>
        <p class="mt-4 text-black-200 mx-auto">
          {work.job_description}
        </p>

        <br />
        <br />

        <h1 class="font-extrabold text-kxl leading-[4.5rem]">
          Responsibilities
        </h1>

        <br />

        <ol class="pl-14 list-decimal">
          {#each work.job_tasks as task}
            <li>{task}</li>
            <br />
          {/each}
        </ol>
      </AnimatedPreview>
    </section>

    <section class="py-28 px-5">
      {#each getImagesForWork(work.resource) as src}
        <img class="w-full" {src} alt={`${work.resource} image`} />
      {/each}
    </section>

    <section class="py-14 px-5">
      <AnimatedPreview delay_offset={11}>
        <div class="flex gap-8 justify-between">
          {#each work.languages as language}
            {#await import(`$lib/images/${language}.png`) then { default: src }}
              <a class="small-tile" href={languages[language] || ''}>
                <img class="max-w-16 max-h-16" {src} alt={language} />
              </a>
            {/await}
          {/each}
        </div>
      </AnimatedPreview>
    </section>
  {/each}

  <section class="py-14 px-5">
    <AnimatedPreview delay_offset={13}>
      <a class="big-tile" href={next_page}>
        <div class="text-center">
          <h2 class="text-4xl">
            <slot name="next_page"></slot><span class="cursor"></span>
          </h2>
        </div>
      </a>
    </AnimatedPreview>
  </section>
</div>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
  }

  .tile {
    background: #242424;
    height: 20rem;
    border-radius: 64px;
    padding: 42px 64px 0;
    display: flex;
    flex-direction: column;
    max-height: 500px;
    height: 100%;
    overflow: hidden;
    filter: saturate(0);
    transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    cursor: pointer;
  }

  .small-tile {
    border-radius: 64px;
    transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    cursor: pointer;
  }

  .big-tile {
    width: 100%;
    height: 20rem;
    border-radius: 64px;
    padding: 84px 128px;
    display: flex;
    flex-direction: column;
    max-height: 500px;
    height: 100%;
    overflow: hidden;
    filter: saturate(0);
    transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    cursor: pointer;
  }

  .tile:hover {
    filter: saturate(1.2);
    transform: translate3d(0, -3px, 0);
  }

  .small-tile:hover {
    filter: saturate(1.2);
    transform: translate3d(0, -3px, 0);
  }

  .big-tile:hover {
    box-shadow: 0 0 0 3px #242424;
    filter: saturate(1.2);
    transform: translate3d(0, -3px, 0);
  }

  img {
    max-width: initial;
  }

  span,
  div,
  p {
    font-family: 'Source Code Pro', monospace;
    font-optical-sizing: auto;
    font-weight: 400;
    font-style: normal;
  }
</style>
