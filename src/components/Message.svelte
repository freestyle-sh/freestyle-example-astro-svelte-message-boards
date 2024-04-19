<script context="module" lang="ts">
  import { cloudstate } from "freestyle-sh";

  @cloudstate
  export class CloudMessage {
    message: string;
    author: string;
    id = crypto.randomUUID();

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
    {author}: {message}
  </div>
</div>
