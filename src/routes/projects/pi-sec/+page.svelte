<script lang="ts">
  import data from '$lib/data/data.json';
  import ProjectPage from '$lib/components/projectPage.svelte';
</script>

<ProjectPage next_page="/projects/neovim" projects={[data.pisec]}>
  <span slot="next_page">Neovim</span>
</ProjectPage>
