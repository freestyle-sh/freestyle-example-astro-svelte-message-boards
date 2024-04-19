<script context="module" lang="ts">
  import { cloudstate } from "freestyle-sh";

  @cloudstate
  export class CloudMessage {
    message = "";
    author = "";

    constructor(message: { message: string; author: string }) {
      this.message = message.message;
      this.author = message.author;
    }
  }
</script>

<script>
  export let message = "";
  export let author = "";
</script>

<div>
  <div>
    {author}
  </div>
  <div>
    {message}
  </div>
</div>
