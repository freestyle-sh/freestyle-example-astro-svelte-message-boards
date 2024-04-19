<script context="module" lang="ts">
  import { cloudstate, useCloudState } from "freestyle-sh";
  import { CloudMessage } from "./Message.svelte";

  @cloudstate
  export class CloudMessageBoard {
    id = crypto.randomUUID();
    name: string;
    messages: CloudMessage[] = [];

    constructor(board: { name: string }) {
      this.name = board.name;
    }

    addMessage(message: { message: string; author: string }) {
      this.messages.push(new CloudMessage(message));
    }

    getMessages() {
      return this.messages.map((message) => ({
        message: message.message,
        author: message.author,
      }));
    }

    getInfo() {
      return {
        id: this.id,
        name: this.name,
      };
    }
  }
</script>

<script lang="ts">
  import Message from "./Message.svelte";

  export let messages: { message: string; author: string }[] = [];
  export let name = "";

  const board = useCloudState(CloudMessageBoard, id);

  let message = "";
  let author = "";
</script>

<div>
  <h1>
    {name}
  </h1>
  <div>
    <input bind:value={message} />
    <input bind:value={author} />
    <button on:click={() => board.addMessage({ message, author })}>
      Add Message
    </button>
  </div>
  {#each messages as message}
    <Message message={message.message} author={message.author} />
  {/each}
</div>
