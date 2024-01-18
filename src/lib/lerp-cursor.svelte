<script lang="ts">
	import { lerp } from "./util";

  const width = 24, height = 24
  let cursorX: number | null = null, cursorY: number | null = null
  let mouseX = 0, mouseY = 0

  const handleMouseEnter = (e: MouseEvent) => {
    cursorX = e.clientX
    cursorY = e.clientY
  }
  const handleMouseLeave = () => {
    cursorX = null
    cursorY = null
  }
  const handleMouseMove = (e: MouseEvent) => {
    mouseX = e.clientX
    mouseY = e.clientY
  }

  function frame() {
    if(cursorX != null && cursorY != null) {
      cursorX = lerp(cursorX, mouseX, 0.3)
      cursorY = lerp(cursorY, mouseY, 0.3)
    }

    requestAnimationFrame(frame)
  }
  
  requestAnimationFrame(frame)
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
  on:mouseenter={handleMouseEnter}
  on:mouseleave={handleMouseLeave}
  on:mousemove={handleMouseMove}
 class="container">
  {#if cursorX != null && cursorY != null}
    <div
      style="--width: {width}px; --height: {height}px; --x: {cursorX - width / 2}px; --y: {cursorY - height / 2}px"
      class="cursor"></div>
  {/if}
</div>

<style>
  .container {
    width: 100%;
    height: calc(100vh - 24px);
  }
  
  .cursor {
    width: var(--width);
    height: var(--height);
    background: red;
    position: absolute;
    border-radius: 9999px;
    top: 0px;
    left: 0px;
    transform: translate(var(--x), var(--y));

  }

</style>