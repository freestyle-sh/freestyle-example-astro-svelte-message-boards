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

    getBoards() {
      return this.boards.map((board) => ({
        id: board.id,
        name: board.name,
      }));
    }
  }
</script>

<script lang="ts">
  import { useCloudState } from "freestyle-sh";

  export let boards: { id: string; name: string }[] = [];
  let name = "";
  const app = useCloudState(CloudApp);
</script>

<div>
  <div>
    <input placeholder="name" bind:value={name} />
    <button
      on:click={() => {
        app.createBoard({ name }).then((board) => {
          window.location.href = `/boards/${board.id}`;
        });
      }}
    >
      Create Board
    </button>
  </div>
  {#each boards as board}
    <div>
      <a href={`/boards/${board.id}`}>{board.name}</a>
    </div>
  {/each}
</div>
