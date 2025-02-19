<script lang="ts">
  import Clock from "./lib/Clock.svelte";
  import Map from "./lib/Map.svelte";

  let paused = $state(true);

  function togglePlay() {
    paused = !paused;
  }
</script>

<main>
  <div class="m-0 p-0 h-full flex flex-row overflow-hidden bg-blue-100">
    <div class="flex-3 relative border-r border-gray-300">
      <Map />
    </div>
    <div class="flex-1 relative">
      <div class="flex flex-col">
        <Clock />
        <div class="flex justify-center items-center">
          <audio bind:paused>
            <source
              src="https://22723.live.streamtheworld.com/RADIO538.mp3"
              type="audio/mpeg"
            />
          </audio>
          <button onclick={togglePlay} class="text-8xl">
            {paused ? "▶️" : "⏸️"}
          </button>
        </div>
      </div>
    </div>

    <!-- <DraggableFrame src="https://www.538.nl/player" title="538 player" /> -->
    <!-- <DraggableFrame src="https://open.spotify.com/" title="spotify" /> -->
  </div>
</main>
