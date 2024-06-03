<script lang="ts">
  import "./app.css";
  import { Trash2, Plus, TriangleAlert, ChevronDown, ChevronUp, Check } from "lucide-svelte";
  import ac from "./assets/air-conditioning.jpg";
  import Tag from "./lib/Tag.svelte";
  import { scale, slide } from "svelte/transition";

  type Photo = {
    name: string;
    index: number;
    isSelected: boolean;
    showProductTags: boolean;
    showRebates: boolean;
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
      if (indx === selectedPhotos[0].index && photo.name !== newName) {
        photo = {...photo, name: newName}
      }
      return photo
    });
  };

  const showSidePanel = (currIndx: number) => {
    const selectedPhotosSize = photos.filter(photo => photo.isSelected).length;
    const oldVal: boolean = photos[currIndx].isSelected;
    photos.forEach(photo => photo.isSelected = false);
    photos = photos;
    photos[currIndx].isSelected = selectedPhotosSize > 1? true : !oldVal;
    selectedIndex = currIndx;
    newPhotoName = "";
  };

  const showSidePanelMulti = (currIndx: number) => {
    photos[currIndx].isSelected = !photos[currIndx].isSelected;
    selectedIndex = currIndx;
    newPhotoName = "";
  };

  const deletePhoto = (currIndx: number) => {
    photos = photos.filter((phto) => phto.index !== currIndx);
  };

  const addTag = (e: MouseEvent & { currentTarget: EventTarget & HTMLButtonElement}) => {
    const val = e.currentTarget.value;
    selectedPhotos.forEach(currPhoto => {
      let photoObj = photos.filter(photo => photo.index === currPhoto.index)[0];
      if (photoObj.productTags.indexOf(val) === -1) {
        photoObj.productTags = [...photoObj.productTags, val]
      }
    });
    selectedPhotos = selectedPhotos;
  }

  const removeTag = (tag: string) => {
    selectedPhotos.forEach(photo => {
      let productTags: string[] = photo.productTags;
      productTags = productTags.filter(currTag => currTag !== tag);
      photo.productTags = productTags;
    });
    selectedPhotos = selectedPhotos;
  }


  let photos: Photo[] = Array(27);
  let selectedIndex: number = -1;
  let newPhotoName: string = "";
  photos = photos.fill({
    name: "", 
    index: 0,
    isSelected: false, 
    showProductTags: false, 
    showRebates: false, 
    hersTags: [], 
    customTags: [], 
    productTags: []
  }).map((photo, indx) => {
    return {...photo, name: `air_conditioner_${indx}`, index: indx}
  });

  $: selectedPhotos = photos.filter(photo => photo.isSelected);
  $: showWaterHeaters = true;
  $: showExteriorInsulations = true;
</script>

<main class="flex flex-row  text-slate-200 px-4 h-screen items-start justify-evenly  overflow-y-auto">
  <div class="flex flex-wrap gap-4 items-center justify-center w-3/4 basis-2/4 pt-8">
    {#each photos as photo, indx (photo)}
      <button class="relative w-[250px] rounded-md p-1 m-0 overflow-hidden group transition-all hover:scale-110 hover:outline outline-2 outline-gray-400" on:click={() => showSidePanel(indx)}>
        <div class="relative rounded-md">
          <img alt="Air conditioner" src={ac} class="w-full rounded-md transition-opacity duration-300 group-hover:opacity-60" />
          <button class="absolute bg-white top-1 left-2 border-2 border-black rounded-md text-green-600 size-8" on:click|stopPropagation={() => showSidePanelMulti(indx)}>
            {#if photos[indx].isSelected}  
            <div class="w-fit h-fit" in:scale out:scale>
              <Check size="28"/>
            </div>
            {/if}
          </button>
          <div class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-50 opacity-0 group-hover:opacity-100 pointer-events-none">
            <span class="text-white pointer-events-none">$20 available rebates</span>
          </div>
          <button class="absolute bottom-2 right-2 bg-transparent border-none text-white cursor-pointer opacity-0 group-hover:opacity-100 transition-opacity duration-300 hover:text-red-700" on:click|stopPropagation={() => deletePhoto(photo.index)}>
            <Trash2 />
          </button>
        </div>
      </button>
      
    {/each}
  </div>
  
  
  {#if selectedPhotos.length > 0}
    <div class="flex flex-col pt-8 items-center justify-center gap-2 sticky top-0 w-[500px]" class:hidden={selectedPhotos.length === 0} in:slide={{axis: 'x', duration: 600}} out:slide={{axis: 'x', duration: 600}}>
      <div class=" text-xl font-bold self-start">Tags</div>
      <div class=" flex flex-col gap-2 self-start">
        <div class="flex flex-col gap-1" class:invisible={selectedPhotos.length > 1}>
          <img alt="An air conditioner" src={ac} class="rounded-lg h-fit w-[500px] self-center" />
          <span>{getDisplayName(selectedPhotos[0])}</span>
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
            <span class="font-semibold">Product Tags (Up to $20 in rebates available):</span>
            <div class="flex">
              <div class="flex gap-2 flex-wrap">
                {#each selectedPhotos[0].productTags as productTag (productTag)}
                  <Tag on:click={() => removeTag(productTag)}>{productTag}</Tag>
                {/each}
              </div>
              <button class="bg-slate-700 size-8 rounded-lg p-1 border-2 border-black hover:border-gray-500" on:click={showProductTagOptions}>
                <Plus size="20" />
              </button>
            </div>
            <div class:invisible={!photos[selectedIndex].showProductTags} class="mt-1 w-3/4">
              <div class=" bg-slate-50 flex flex-col text-black rounded-lg w-full text-lg p-4">
                  <button class="flex items-end justify-between font-bold hover:bg-gray-600 hover:bg-opacity-20 w-full p-1 rounded-md" on:click={() => showWaterHeaters = !showWaterHeaters}>
                    <span>Water Heaters</span>
                    {#if showWaterHeaters}
                    <ChevronUp />
                    {:else}
                    <ChevronDown />
                    {/if}
                  </button>
                  {#if showWaterHeaters}
                    <div class="flex flex-col w-full">
                      <button class="w-full hover:bg-green-600 hover:bg-opacity-20 p-1 rounded-md text-start truncate" value="Rinnai" on:click={addTag}>Rinnai Tankless Gas WH (1) $40</button>
                      <button class="w-full hover:bg-green-600 hover:bg-opacity-20 p-1 rounded-md text-start truncate" value="HPWH" on:click={addTag}>HPWH $20</button>
                    </div>
                  {/if}
                  <button class="flex items-end justify-between font-bold hover:bg-gray-600 hover:bg-opacity-20 w-full p-1 rounded-md" on:click={() => showExteriorInsulations = !showExteriorInsulations}>
                    <span>Exterior Insulation</span>
                    {#if showExteriorInsulations}
                    <ChevronUp />
                    {:else}
                    <ChevronDown />
                    {/if}
                  </button>
                  {#if showExteriorInsulations}
                  <div class="flex flex-col items-start text-start w-full">
                    <button class="w-full hover:bg-green-600 hover:bg-opacity-20 p-1 rounded-md text-start truncate" value="BASF" on:click={addTag}>BASF Rigid Insulation</button>
                    <button class="w-full hover:bg-green-600 hover:bg-opacity-20 p-1 rounded-md text-start truncate" value="OX" on:click={addTag}>OX Insulated Sheathing (1)</button>
                  </div>
                  {/if}
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

        <form class="flex items-center justify-between pt-4" class:invisible={selectedPhotos.length > 1} on:submit|preventDefault={onRenameFormSubmit}>
            <input type="text" placeholder="Rename photo..." id="rename" class="p-2 rounded-lg" bind:value={newPhotoName}>
            <button  type="submit" class="bg-green-600 py-[4.5px] px-2 rounded-lg">Submit</button>
        </form>
        
        <button class="self-end" on:click={() => selectedPhotos.forEach(photo => deletePhoto(photo.index))}>
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