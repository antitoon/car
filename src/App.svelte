<script lang="ts">
  import Clock from "./lib/Clock.svelte";
  import Map from "./lib/Map.svelte";

  function makeDraggable(node: HTMLElement) {
    let offsetX: number,
      offsetY: number,
      isDragging = false;

    node.addEventListener("mousedown", (e) => {
      isDragging = true;
      offsetX = e.clientX - node.getBoundingClientRect().left;
      offsetY = e.clientY - node.getBoundingClientRect().top;
      node.style.cursor = "grabbing";
    });

    document.addEventListener("mousemove", (e) => {
      if (!isDragging) return;
      node.style.left = `${e.clientX - offsetX}px`;
      node.style.top = `${e.clientY - offsetY}px`;
    });

    document.addEventListener("mouseup", () => {
      isDragging = false;
      node.style.cursor = "grab";
    });
  }
</script>

<main>
  <div class="m-0 p-0 h-full flex flex-row overflow-hidden bg-blue-100">
    <div class="flex-3 relative border-r border-gray-300">
      <Map />
    </div>
    <div class="flex-1 relative">
      <Clock />
    </div>
  </div>

  <div
    use:makeDraggable
    class="absolute bottom-5 left-5 w-2/5 h-1/4 bg-white shadow-md rounded-lg overflow-hidden cursor-grab"
  >
    <div class="bg-gray-300 p-2 text-center font-bold cursor-grab">
      Sleep hier om te verplaatsen
    </div>
    <iframe src="https://www.538.nl/player" title="538 player" class="w-full"
    ></iframe>
  </div>
</main>
