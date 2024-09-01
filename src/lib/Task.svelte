<script>
    export let _id = "", content = "", date = 0, important = false, completed = false, last_updated = 0;

    $: console.log(important);
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="task" class:important class:completed on:click={e => {
    
}}>

    <div>
        <p class="content" contenteditable bind:textContent={content}></p>
        <p class="date">{date}</p>
    </div>
    <div class="toggle-container">
        <button class="circle important-toggle"
        on:click={() => important = !important}></button>
        <button class="circle completed-toggle"
        on:click={() => completed = !completed}></button>
    </div>
</div>

<style>
    .task {
        border: 1px solid #757575;
        display: flex;
        align-items: center;
        gap: 1rem;
        padding: .5rem;
        border-radius: .5rem;
        transition: all 100ms ease-in-out;
    }

    .task.important {
        border: 1px solid #FFC107;
    }

    .task.completed {
        border: 1px solid #00FF37;
    }

    .task.completed .content {
        text-decoration: line-through;
    }

    .date {
        color: #757575;
    }

    .circle {
        width: 40px;
        height: 40px;
        background-color: #1a1a1a;
        border-radius: 50%;
    }

    .toggle-container {
        display: flex;
        gap: .5rem;
        margin-left: auto;
    }

    .important-toggle {
        border: 1px solid #FFC107;
    }

    .completed-toggle {
        border: 1px solid #00FF37;
    }

    .task.important .important-toggle {
        background-color: #FFC107;
    }

    .task.completed .completed-toggle {
        background-color: #00FF37;
    }
</style>