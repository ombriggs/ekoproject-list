<script lang="ts">
  import "./app.css";
  import { Trash2, Check, Plus, TriangleAlert, ChevronDown } from "lucide-svelte";
  import ac from "./assets/air-conditioning.jpg";
  import { fade } from "svelte/transition";

  type Photo = {
    name: string;
    isSelected: boolean;
    showProductTags: boolean;
    hersTags: string[];
    productTags: string[];
    customTags: string[];
  };

  const getDisplayName = (photo: Photo) => {
    return `C:\\\\work\\\\ekotrope\\\\photos\\\\${photo.name}`
  };

  const onRenameFormSubmit = () => {
    if (newPhotoName.toString().trim().length !== 0) {
      renamePhoto(newPhotoName.toString());
      newPhotoName = "";
      const elem = document.getElementById("rename");
      if (elem) {
        elem.focus();
      }
    }
  }

  const showProductTagOptions = () => {
    const currPhoto: Photo = photos[selectedIndex];
    currPhoto.showProductTags = !currPhoto.showProductTags;
    photos[selectedIndex] = currPhoto;
  }

  const renamePhoto = (newName: string) => {
    photos = photos.map((photo, indx) => {
      if (indx === selectedIndex && photo.name !== newName) {
        photo = {...photo, name: newName}
      }
      return photo
    });
  };

  const showSidePanel = (currIndx: number) => {
    isInvisible = selectedIndex === currIndx;
    selectedIndex = currIndx;
    newPhotoName = "";
  };

  const deletePhoto = (currIndx: number) => {
    photos = photos.filter((_, indx) => currIndx !== indx);
  }

  let photos: Photo[] = Array(20);
  let selectedIndex: number = -1;
  let isInvisible: boolean = true;
  let newPhotoName: string = "";
  photos = photos.fill({name: "", isSelected: false, showProductTags: false, hersTags: [], customTags: [], productTags: []}).map((photo, indx) => {
    return {...photo, name: `air_conditioner_${indx}`}
  } );
</script>

<main class="flex flex-row  text-slate-200 px-4 h-screen items-start justify-evenly">
  <div class="flex flex-wrap gap-4 items-center justify-center w-3/4 basis-2/4">
    {#each photos as photo, indx (photo)}
      <button class="rounded-md p-0 m-0 " out:fade on:click={() => showSidePanel(indx)}>
        <button class=" bg-slate-50 rounded-md border-2 border-black relative top-[2.5em] right-[6em]" on:click|stopPropagation={() => photo.isSelected = !photo.isSelected}>
            <div class:hidden={!photo.isSelected}>
              <Check color="#2e9e3b" />
            </div>
            <div class=" w-6 h-6"  class:hidden={photo.isSelected}/>
        </button>
        <img alt="An air conditioner" src={ac} class="rounded-lg h-fit w-[250px] p-1 hover:outline hover:outline-stone-700" />
        <button class="hover:text-red-600 relative text-white bottom-[2em] left-[6.5em]" on:click|stopPropagation={() => deletePhoto(indx)}>
          <Trash2 />
        </button>
      </button>
    {/each}
  </div>
  
  
  {#if !isInvisible}
    <div class="flex flex-col pt-8 items-center justify-center gap-2" class:hidden={isInvisible}>
      <div class=" text-xl font-bold self-start">Tags</div>
      <div class=" flex flex-col gap-2">
        <div class="flex flex-col gap-1">
          <img alt="An air conditioner" src={ac} class="rounded-lg h-fit w-[500px] self-center" />
          <span>{getDisplayName(photos[selectedIndex])}</span>
        </div>

        <div class="flex flex-col gap-4">
          <div class="flex flex-col gap-1">
            <div class="flex items-center gap-1">
              <span class="font-semibold">HERS Required Tags (5/10):</span>
              <TriangleAlert color="#BF9000" />
            </div>
            <button class=" bg-slate-700 w-fit rounded-lg p-1 border-2 border-black hover:border-gray-500">
              <Plus size="20" />
            </button>
          </div>

          <div class="flex flex-col gap-1">
            <span class="font-semibold">Product Tags:</span>
            <button class=" bg-slate-700 w-fit rounded-lg p-1 border-2 border-black hover:border-gray-500" on:click={showProductTagOptions}>
              <Plus size="20" />
            </button>
            <div class:invisible={!photos[selectedIndex].showProductTags} class="pt-1">
              <div class=" bg-slate-50 flex flex-col text-black rounded-lg w-fit px-4 py-2 items-start justify-center text-center">
                <button class="flex justify-start items-center text-center hover:bg-gray-500 w-full hover:rounded-lg pl-2">
                  <div class="font-semibold">Water Heaters</div>
                  <ChevronDown />
                </button>
                <button class="flex justify-start items-center text-center hover:bg-gray-500 w-full hover:rounded-lg pl-2">
                  <div class="font-semibold">Exterior Insulation</div>
                  <ChevronDown />
                </button>
              </div>
            </div>
          </div>

          <div class="flex flex-col gap-1">
            <span class="font-semibold">Custom Tags:</span>
            <button class=" bg-slate-700 w-fit rounded-lg p-1 border-2 border-black hover:border-gray-500">
              <Plus size="20" />
            </button>
          </div>
        </div>

        <div class="flex items-center justify-between pt-4">
            <input type="text" placeholder="Rename photo..." id="rename" class="p-2 rounded-lg" bind:value={newPhotoName}>
            <button class="bg-green-600 py-[4.5px] px-2 rounded-lg" on:click={onRenameFormSubmit}>Submit</button>
        </div>
        
        <button class="self-end" on:click={() => deletePhoto(selectedIndex)}>
          <div class="flex gap-1 bg-red-600 rounded-lg items-center justify-center py-[4.5px] px-2 font-semibold text-center">
            <span class="text-lg">Delete</span>
            <Trash2 />
          </div>
        </button>
      </div>
    </div>
  {/if}
</main>

<style>  
  .hidden {
    visibility: hidden;
  }

  .invisible {
    display: none;
  }
</style>