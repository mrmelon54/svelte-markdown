<script>
  import DOMPurify from 'dompurify'

  export let options
  export let lang
  export let text
</script>

<pre class="{options.langPrefix}{lang}"><code>{@html DOMPurify.sanitize(options.highlight(text, lang))}</code></pre>
