<script>
    import marked from 'marked';
    import { afterUpdate } from 'svelte';

    let textpos;
    let value = `
# H1 heading
## H2 heading
### H3 heading
---
**bold text**
*italicized text*
---
1. First item
2. Second item
3. Third item
- First item
- Second item
- Third item

> First indent
>> Second indent

[Svelte](https://svelte.dev/)
`;

    let save;

    function handleKeyup(event) {

        if(save) {
            clearTimeout(save);
        }

        save = setTimeout(() => {
            console.log("keey up");

            textpos.focus();
            textpos.select();
            document.execCommand('copy');
            //window.getSelection().removeAllRanges()
        }, 400);


    }





</script>



<style>
    .container{
        background: #d0d0d0;
        padding: 20px;
    }
    .markdown-editor {
        width: 100%;
        display: flex;
        align-items:flex-start;
        justify-content: space-evenly;
    }
    .left-panel, .right-panel {
        width: 50%;
        height: 85vh;
        background: #ffffff;
    }
    .right-panel {
        overflow: auto;
    }
    .source {
        border: none;
        width: 100%;
        height: 100%;
        background: #2d2d2d;
        color: #ccb50a;
    }
    .source:focus {
        outline: none;
    }
    .output {
        width: 100%;
        padding: 0 2em;
    }
</style>

<main class="container">
    <div class="markdown-editor">
        <div class="left-panel">
            <textarea class="source" on:keyup={handleKeyup} bind:this={textpos} bind:value={value}></textarea>
        </div>

        <div class="right-panel">
            <div class="output">{@html marked(value)}</div>
        </div>
    </div>
</main>
