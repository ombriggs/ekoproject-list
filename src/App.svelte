<script lang="ts">
  import { Info, EllipsisVertical, Copy, Trash2, Forward } from "lucide-svelte";

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
  <table class=" w-full mt-4 border-separate border-spacing-y-1">
    <thead>
      <tr class="bg-[#005a76] text-white">
        <th></th>
        <th></th>
        <th></th>
        <th>Project Name</th>
        <th>Builder</th>
        <th>Open</th>
        <th></th>
      </tr>
    </thead>
    

    <tbody>
      {#each projects as project, indx}
      <tr class="text-start align-middle bg-gray-100">
        <td>{indx+1}</td>
        <td>
          <Info />
        </td>
        <td>
          <input type="checkbox" />
        </td>
        <td>{project.projectName}</td>
        <td>{project.builder}</td>
        <td>
          <button class="bg-white shadow-lg px-2 py-1 m-2 w-fit h-fit rounded-md text-[#005a76] font-bold hover:text-white focus:text-white hover:bg-blue-300 focus:bg-blue-600">
            Open
          </button>
        </td>
        <td>
          <button on:click={() => project.visible = !project.visible}>
            <EllipsisVertical color="#005a76" />
          </button>
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
              <button class="flex px-2 py-1 bg-white rounded-md shadow-lg items-center justify-center text-[#005a76] font-bold hover:text-white focus:text-white hover:bg-blue-300 focus:bg-blue-600 gap-1">
                <Copy />
                <span class="font-bold">Copy</span>
              </button>
              <button class="flex px-2 py-1 bg-white rounded-md shadow-lg items-center justify-center text-[#005a76] font-bold hover:text-white focus:text-white hover:bg-blue-300 focus:bg-blue-600 gap-1">
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
    padding: 8px;
  }

  .invisible {
    display: none;
  }
</style>