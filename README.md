[![Open Freestyle.sh](https://www.freestyle.sh/github-hero.png)](https://www.freestyle.sh/)
# freestyle-astro-svelte-template

[![Open in StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/fork/github/freestyle-sh/freestyle-astro-svelte-template)

[freestyle.sh](https://www.freestyle.sh) is a cloud platform in early development that replaces traditional databases with persistent javascript objects. Write code as if your data is in memory and never think about it's underlying storage mechanisms again. We'll statically and dynamically optimize your code to make efficient queries inside our cloudstate environment.
```svelte
<script context="module" lang="ts">
  import { cloudstate, useCloudState } from "freestyle-sh";

  // The @cloudstate decorator makes this class's data persistent.
  // Data can only be augmented and retrieved through the class's
  // methods. There's nothing you have to do for your data to be
  // saved, it's automatically saved for you.
  @cloudstate
  export class CloudCounter {
    value = 0;
    increment() {
      return ++this.value;
    }
    getCount() {
      return this.value;
    }
  }
</script>

<script lang="ts">
  export let count = 0;

  // Creates a singleton instance of Counter if one doesn't exist
  // and returns an async wrapper around it. Your methods are executed
  // and optimized inside your cloudstate database, not in your
  // web server or frontend app.
  const counter = useCloudState(CloudCounter);
</script>

<button
  on:click={() => {
    // Call a method on counter from anywhere in your backend or
    // frontend code to update the state in the cloud.
    // (Authentication for frontend coming soon.)
    counter.increment().then((c) => {
      count = c;
    });
  }}
>
  this button has been clicked {count} times
</button>
```

Astro is the web framework for building content-driven websites like blogs, marketing, and e-commerce. To learn more, visit [their documentation](https://docs.astro.build/en/concepts/why-astro/)

## Developing Locally
> Please note that cloudstate is not persistent during local development.

install dependencies
```
npm i
```

run the development server
```
npm run dev
```

## Ready to Deploy
build your project
```
npm run build
```

log into freestyle using github
```
npx freestyle-sh@latest login
```

deploy to freestyle.sh (beta)
```
npx freestyle-sh@latest deploy
```
