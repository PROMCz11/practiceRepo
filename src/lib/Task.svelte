<script>
    export let _id = "", content = "", date = 0, important = false, completed = false, last_updated = 0;

    import deleteIconSrc from "$lib/assets/delete-icon.svg";

    let expanded = false;

    const getFormattedLocalTime = (millisecondsSinceEpoch) => {
        const dateFromMilliseconds = new Date(millisecondsSinceEpoch);
        const options = {
            weekday: 'short',
            day: 'numeric',
            month: 'numeric',
            hour: 'numeric',
            minute: '2-digit',
            hour12: true
        };
        const formattedLocalTime = dateFromMilliseconds.toLocaleTimeString(undefined, options).split(",").join("");
        return formattedLocalTime;
    }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->

<div class="task" class:important class:completed class:expanded on:click={e => {
    if(e.target.classList.contains("task-top")) {
        expanded = !expanded
    }
}}>
    <div class="task-top">
        <div>
            <p class="content" contenteditable bind:textContent={content}></p>
            <p class="date">{getFormattedLocalTime(date)}</p>
        </div>
        <div class="toggle-container">
            <button class="circle important-toggle"
            on:click={() => important = !important}></button>
            <button class="circle completed-toggle"
            on:click={() => completed = !completed}></button>
        </div>
    </div>
    <div class="task-bottom">
        <p class="last_updated">Last Updated: {getFormattedLocalTime(last_updated)}</p>
        <button><img src={deleteIconSrc} alt=""></button>
    </div>
</div>


<style>
    .task {
        border: 1px solid #757575;
        display: flex;
        flex-direction: column;
        padding: .5rem;
        border-radius: .5rem;
        transition: all 100ms ease-in-out;
    }

    .task-top {
        display: flex;
        align-items: center;
        gap: 1rem;
    }

    .task-bottom {
        max-height: 0px;
        overflow: hidden;
        display: flex;
        justify-content: space-between;
        align-items: center;

        transition: max-height 200ms ease-in-out;
    }

    .task.expanded .task-bottom {
        max-height: 200px;
    }

    .task-bottom button {
        background-color: transparent;
        border: none;
        width: 40px;
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

    .date, .last_updated {
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