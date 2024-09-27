<script lang="ts">
  import { EllipsisVertical, Copy, Trash2, Forward } from "lucide-svelte";

  type Project = {
    projectName: string;
    raterOfRecord: string;
    builder: string;
    address: string;
    status: string;
    qaTypes: string;
    qaStatus: string;
    visible: boolean;
  };

  const projects: Project[] = [
    {
      projectName: "Real Project 1",
      raterOfRecord: "Owanari Briggs",
      builder: "Ekotrope",
      address: "1234 Real St.",
      status: "Registered",
      qaTypes: "",
      qaStatus: "",
      visible: false
    },
    {
      projectName: "Real Project 2",
      raterOfRecord: "Owanari Briggs",
      builder: "Ekotrope",
      address: "12345 Real St.",
      status: "Unregistered",
      qaTypes: "",
      qaStatus: "",
      visible: false
    },
    {
      projectName: "Real Project 3",
      raterOfRecord: "Owanari Briggs",
      builder: "Ekotrope",
      address: "4321 Real St.",
      status: "Registered",
      qaTypes: "",
      qaStatus: "",
      visible: false
    },
    {
      projectName: "Real Project 4",
      raterOfRecord: "Owanari Briggs",
      builder: "Ekotrope",
      address: "54321 Real St.",
      status: "Unregistered",
      qaTypes: "",
      qaStatus: "",
      visible: false
    },
    {
      projectName: "Longer Real Project 5",
      raterOfRecord: "Owanari Briggs",
      builder: "Ekotrope",
      address: "2431 Real St.",
      status: "Registered",
      qaTypes: "",
      qaStatus: "",
      visible: false
    }
  ]
</script>

<main class=" text-black w-full">
  <table class=" w-full mt-4 border-separate border-spacing-y-1 ">
    <thead>
      <tr class="bg-ekoblue text-white">
        <th></th>
        <th></th>
        <th class="text-start">Project Name</th>
        <th class="text-start">Builder</th>
        <th></th>
        <th></th>
      </tr>
    </thead>
    

    <tbody>
      {#each projects as project, indx}
      <tr class="text-start align-middle bg-gray-100">
        <td>
          <div class="flex items-center justify-center h-full">{indx+1}</div>
        </td>

        <td>
          <div class="flex items-center justify-center h-full">
            <input type="checkbox" class=" size-5" />
          </div>
        </td>

        <td>
          <div class="flex items-center justify-start h-full">{project.projectName}</div>
        </td>
        
        <td>
          <div class="flex items-center justify-start h-full">{project.builder}</div>
        </td>
        
        <td>
          <div class="flex items-center justify-center h-full">
            <button class="bg-white shadow-lg px-2 py-1 m-2 w-fit h-fit rounded-md text-ekoblue font-bold hover:text-white focus:text-white hover:bg-blue-300 focus:bg-blue-600">
              Open
            </button>
          </div>
        </td>
        
        <td>
          <div class="flex items-center justify-center h-full">
            <button on:click={() => project.visible = !project.visible}>
              <EllipsisVertical color="#005a76" />
            </button>
          </div>
        </td>
      </tr>

      <tr class="bg-gray-100" class:invisible={!project.visible}>
        <td colspan="100">
          <div class="pl-2 py-2">
            <div>
              <span class="font-bold">Rater of Record:</span>
              <span>{project.raterOfRecord}</span>
            </div>
            
            <div>
              <span class="font-bold">Street Address:</span>
              <span>{project.address}</span>
            </div>

            <div>
              <span class="font-bold">Status:</span>
              <span>{project.status}</span>
            </div>

            <div>
              <span class="font-bold">QA Types:</span>
              <span>{project.qaTypes}</span>
            </div>

            <div>
              <span class="font-bold">QA Status:</span>
              <span>{project.qaStatus}</span>
            </div>
            <div class="flex items-center justify-center gap-2">
              <button class="flex px-2 py-1 bg-white rounded-md shadow-lg items-center justify-center text-ekoblue font-bold hover:text-white focus:text-white hover:bg-blue-300 focus:bg-blue-600 gap-1">
                <Copy />
                <span class="font-bold">Copy</span>
              </button>
              <button class="flex px-2 py-1 bg-white rounded-md shadow-lg items-center justify-center text-ekoblue font-bold hover:text-white focus:text-white hover:bg-blue-300 focus:bg-blue-600 gap-1">
                <Forward />
                <span class="font-bold">Share</span>
              </button>
              <button class="flex px-2 py-1 bg-white rounded-md shadow-lg items-center justify-center text-red-500 hover:bg-red-400 hover:text-white focus:bg-red-500">
                <Trash2 />
              </button>
            </div>
          </div>
        </td>
      </tr>
    {/each}
    </tbody>    
  </table>
</main>

<style>
  th {
    padding: 8px 0px;
  }

  .invisible {
    display: none;
  }
</style>