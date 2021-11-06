<script>
  import { onMount } from "svelte";

  export let projects;

  import { currentColorMode } from "../../stores/global.js";

  let value = 0;
  let video, source;
  let videoTransitioning = false;

  const updateVideo = function (url) {
    if (video && source) {
      videoTransitioning = true;

      setTimeout(() => {
        source.src = url;
        video.load();
        video.play();
        videoTransitioning = false;
      }, 200);
    }
  };

  $: currentProject = value ? projects[value] : projects[0];

  $: value || $currentColorMode,
    typeof value == "number"
      ? updateVideo(`./videos/${$currentColorMode}/${value}.mp4`)
      : null;

  onMount(() => {
    updateVideo(`./videos/${$currentColorMode}/${value}.mp4`);
  });
</script>

<div class="section-container">
  <div class="projects">
    {#each projects as project, i}
      <a
        href={currentProject.path.replace(/\.[^/.]+$/, "")}
        class="project-card no-underline {value === i ? 'active' : 'inactive'}"
      >
        <h1
          class="title-{i}"
          on:mouseover={() => {
            value = i;
          }}
          on:focus={() => {
            value = i;
          }}
        >
          {project.metadata.title}
        </h1>
        <!-- <h2>{project.metadata.description}</h2> -->
      </a>
    {/each}
  </div>
  <div class="absolute-container">
    <div class="transition-overlay" class:videoTransitioning />
    <video
      preload="metadata"
      autoplay
      muted
      playsinline
      id="video"
      bind:this={video}
    >
      <source bind:this={source} type="video/mp4" />
    </video>
  </div>
</div>

<style>
  .absolute-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: visible;
  }

  .projects {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    width: 95%;
    height: 100%;
    text-align: left;
    position: relative;
    z-index: 5;
    max-width: 1168px;
    margin: auto;
  }

  .project-card {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    transition: all 600ms ease;
  }

  .section-container {
    position: relative;
    height: 60vh;
    overflow: visible;
    /* overflow-x: hidden; */
  }

  h1 {
    margin-bottom: 0.25rem;
    font-size: 3rem;
    text-transform: uppercase;
    font-weight: 200;
    transition: opacity 600ms cubic-bezier(0.37, 0.35, 0.01, 0.99),
      color 600ms cubic-bezier(0.37, 0.35, 0.01, 0.99),
      text-shadow 600ms cubic-bezier(0.37, 0.35, 0.01, 0.99);
  }

  .active h1 {
    color: var(--accent-color);
  }

  .transition-overlay {
    background: var(--tertiary-color);
    width: 100%;
    height: 100%;
    display: block;
    margin: auto;
    max-width: 100vw;
    -o-object-fit: cover;
    object-fit: cover;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 4;
    pointer-events: none;
    opacity: 0;
    transition: opacity 200ms ease;
  }

  .videoTransitioning {
    opacity: 1;
  }

  video {
    height: 100%;
    display: block;
    -o-object-fit: cover;
    object-fit: cover;
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 3;
  }

  /* You can make this true on all desktop sizes for an overlay effect */
  .section-container {
    flex-direction: column-reverse;
  }

  .inactive {
    opacity: 0.3;
  }

  .inactive h1 {
    /* border-bottom: 1px dashed rgba(var(--text-color-rgb), 0.6); */
    /* border-bottom: 1px dashed transparent; */
    color: transparent;
    opacity: 0.5;
    text-shadow: 0 0 2px rgba(var(--text-color-rgb), 0.8);
  }

  @media screen and (min-width: 1269px) {
    .absolute-container {
      transform: translateX(54vw);
    }

    .title-0 {
      font-size: 3.75rem;
    }

    .title-1 {
      font-size: 4.05rem;
    }

    .title-2 {
      font-size: 4.8rem;
    }

    .title-3 {
      font-size: 9.1rem;
      transform: scaleY(0.95);
    }
  }
  @media screen and (max-width: 1268px) {
    .absolute-container {
      transform: translateX(54vw);
    }

    .title-0 {
      font-size: 4.5vw;
    }

    .title-1 {
      font-size: 4.8vw;
    }

    .title-2 {
      font-size: 5.78vw;
    }

    .title-3 {
      font-size: 11vw;
      transform: scaleY(0.95);
    }
  }

  @media screen and (max-width: 868px) {
    .section-container {
      overflow: visible;
    }

    .inactive {
      border-bottom: 1px solid transparent;
      border-top: 1px solid transparent;
    }

    .active {
      background: rgba(var(--tertiary-color-rgb), 0.8);
      border-bottom: 1px solid var(--accent-color);
      border-top: 1px solid var(--accent-color);
    }

    .absolute-container {
      transform: none;
    }

    video {
      width: 100%;
    }

    .projects {
      width: 100%;
    }

    .project-card {
      padding: 1rem;
    }

    .title-0 {
      font-size: 5.69vw;
    }

    .title-1 {
      font-size: 6.2vw;
    }

    .title-2 {
      font-size: 7.3vw;
    }

    .title-3 {
      font-size: 13.9vw;
    }
  }
</style>