<script>
  import { CircleX } from "lucide-svelte";
  import { fade } from "svelte/transition";

</script>

<div class=" bg-white text-black rounded-lg flex items-center justify-start px-2 py-1 gap-2 mr-2 font-bold" in:fade out:fade>
  <slot />
  <button class=" hover:text-red-600" on:click>
    <CircleX size="32" />
  </button>
  
</div>
