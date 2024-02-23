<script lang="ts">
    export let author: string;
    export let message: string;
</script>

<div class="message">
    <p class="author">{author}</p>
    <p class="message">{message}</p>
</div>

<style lang="scss">
    div.message {
        background-color: #fff;
        padding: 1rem;
        margin: 0.5rem;
        border-radius: 0.5rem;
        box-shadow: 0 0 0.5rem rgba(0, 0, 0, 0.1);
        max-width: 50%;
        word-wrap: break-word;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-start;
        p.author {
            font-weight: bold;
            margin: 0;
        }
        p.message {
            margin: 0;
        }
    }
</style>