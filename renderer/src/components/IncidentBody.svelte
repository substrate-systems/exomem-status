<script>
  import DOMPurify from "dompurify";
  import snarkdown from "snarkdown";

  export let content;
  export let markdown = true;

  $: html = DOMPurify.isSupported
    ? DOMPurify.sanitize(markdown ? snarkdown(content) : content, {
        USE_PROFILES: { html: true },
        // These HTML protocol tokens exclude forms, styles, and page-selector redress.
        FORBID_TAGS: [
          "form",
          "input",
          "button",
          "select",
          "option",
          "textarea",
          "fieldset",
          "legend",
          "label",
          "style",
        ],
        FORBID_ATTR: ["style", "class", "id", "name", "hidden"],
        ALLOW_DATA_ATTR: false,
      })
    : null;
</script>

{#if html === null}
  {content}
{:else}
  {@html html}
{/if}
