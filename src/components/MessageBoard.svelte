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
      const newMessage = new CloudMessage(message);
      this.messages.push(newMessage);
      return {
        id: newMessage.id,
      };
    }

    getMessages() {
      return this.messages.map((message) => ({
        message: message.message,
        author: message.author,
        id: message.id,
      }));
    }

    async getBoard() {
      return {
        id: this.id,
        name: this.name,
        messages: this.getMessages(),
      };
    }
  }
</script>

<script lang="ts">
  import Message from "./Message.svelte";
  import { onDestroy, onMount } from "svelte";

  export let messages: { message: string; author: string; id: string }[] = [];
  export let name = "";
  export let id: string;

  const board = useCloudState(CloudMessageBoard, id);

  let message = "";
  let author = "";

  let interval: number;
  onMount(() => {
    // poll for messages every second
    setInterval(() => {
      board.getMessages().then((result) => {
        messages = result;
      });
    }, 1000);
  });

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

<div>
  <h1>
    {name}
  </h1>
  <div>
    <input placeholder="message" bind:value={message} />
    <input placeholder="author" bind:value={author} />
    <button
      on:click={() => {
        board.addMessage({ message, author }).then((result) => {
          messages = [
            ...messages,
            {
              message: message,
              author: author,
              id: result.id,
            },
          ];
          message = "";
        });
      }}
    >
      Add Message
    </button>
  </div>
  {#each messages as message}
    <Message message={message.message} author={message.author} />
  {/each}
</div>
