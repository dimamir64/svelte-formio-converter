<script>
  import { onMount } from "svelte";
  import { Formio } from "formiojs";
  import schema from "./lib/schema.json";
  import options from "./lib/options.json";
  import { by639_1 } from "iso-language-codes";

  $: languages = ["en", ...Object.keys(options.i18n)];
  $: language_options = languages.map((key) => {
    const match = by639_1[key];

    if (match) {
      return {
        label: match.name,
        value: key,
      };
    } else {
      return {
        label: key,
        value: key,
      };
    }
  });

  let form;
  let formio;
  let selected_language = "en";

  $: {
    if (formio && selected_language) {
      formio.language = selected_language;
    }
  }

  onMount(() => {
    if (form) {
      Formio.createForm(form, schema, { ...options }).then((formioInstance) => {
        formio = formioInstance;
      });
    }
  });
</script>

<header class="py-3 border-bottom">
  <div class="container d-flex justify-content-between align-items-center">
    <div>Form Converter Demo</div>
    <div>
      <select
        bind:value={selected_language}
        class="form-select"
        aria-label="Select Language"
      >
        {#each language_options as opt}
          <option value={opt.value}>{opt.label}</option>
        {/each}
      </select>
    </div>
  </div>
</header>
<main>
  <div class="container pt-4">
    <div bind:this={form} id="form"></div>
  </div>
</main>
