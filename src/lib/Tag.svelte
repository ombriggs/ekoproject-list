<script>
  import { X } from "lucide-svelte";
  import { fade } from "svelte/transition";

</script>

<div class=" bg-white text-black rounded-lg flex items-center justify-center px-2 py-1 gap-2 mr-2 font-bold" in:fade out:fade>
  <slot />
  <button class=" hover:bg-gray-300 rounded-full p-1 flex items-center justify-center" on:click>
    <X size="20" />
  </button>
  
</div>
