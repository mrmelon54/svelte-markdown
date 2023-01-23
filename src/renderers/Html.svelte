<script>
  import DOMPurify from 'dompurify'

  export let text
</script>

{@html DOMPurify.sanitize(text)}
