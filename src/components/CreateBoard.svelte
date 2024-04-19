<script context="module" lang="ts">
  import { cloudstate } from "freestyle-sh";
  import { CloudMessageBoard } from "./MessageBoard.svelte";

  @cloudstate
  export class CloudApp {
    boards: CloudMessageBoard[] = [];

    createBoard(board: { name: string }) {
      const newBoard = new CloudMessageBoard(board);
      this.boards.push(newBoard);
      return {
        id: newBoard.id,
      };
    }
  }
</script>

<script lang="ts">
  import { useCloudState } from "freestyle-sh";
  import { navigate } from "astro/virtual-modules/transitions-router.js";

  let name = "";
  const app = useCloudState(CloudApp);
</script>

<div>
  <div>
    <input bind:value={name} />
    <button
      on:click={() => {
        app.createBoard({ name }).then((board) => {
          navigate(`/boards/${board.id}`);
        });
      }}
    >
      Create Board
    </button>
  </div>
</div>
