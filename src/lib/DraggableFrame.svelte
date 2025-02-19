<script lang="ts">
  let { src, title } = $props();

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

<div
  use:makeDraggable
  class="absolute bottom-0 right-0 w-1/4 h-3/5 bg-white shadow-md rounded-lg overflow-hidden cursor-grab"
>
  <div class="flex flex-col h-full w-full">
    <div class="bg-gray-300 p-2 text-center font-bold cursor-grab">
      Sleep hier om te verplaatsen
    </div>
    <iframe
      {src}
      {title}
      class="flex-1 h-full w-full"
      allow="encrypted-media"
    ></iframe>
  </div>
</div>
