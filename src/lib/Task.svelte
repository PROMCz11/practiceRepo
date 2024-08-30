<script>
    export let completed = 0;
    export let important = 0;
    export let content = '';
    export let date = new Date().getTime();
    export let last_updated = date;
    export let _id = 0;
</script>

<div class="task" id="{_id}">
    <div>
        <p class="content">{content}</p>
        <p class="date">{date}</p>
        <p class="last_updated">{last_updated}</p>
    </div>
    <div class="toggle-container">
        <div class:active={important} class="circle important-toggle"></div>
        <div class:active={completed} class="circle completed-toggle"></div>
    </div>
</div>

<style>
    :root {
        --clr-accent: #A0153E;
        --clr-accent-rgb-for-alpha: 160, 21, 62;
    }

    .task {
        border-bottom: 1px solid #333333;
        padding: 1rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .toggle-container {
        display: flex;
        gap: .5rem;
    }

    .circle {
        width: 40px;
        height: 40px;
        border-radius: 50%;
        background-color: #222222;
    }

    .important-toggle {
        border: 1px solid var(--clr-accent);
    }

    .completed-toggle {
        border: .5px solid #00FF37;
    }

    .important-toggle.active {
        background-color: var(--clr-accent);
    }

    .completed-toggle.active {
        background-color: #00FF37;
    }

    .date {
        color: #757575;
    }
</style>