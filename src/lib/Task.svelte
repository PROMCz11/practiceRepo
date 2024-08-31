<script>
	import { blur } from "svelte/transition";

    export let completed = 0;
    export let important = 0;
    export let content = '';
    export let date = new Date().getTime();
    export let last_updated = date;
    export let _id = 0;

    const updateTaskContent = (content) => {
        fetch(`https://task-manager-back-end-7gbe.onrender.com/api/tasks/update/${_id}`, {
            method: "PATCH",
            body: JSON.stringify({
                token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI2NmQwOWJkY2Q2MDIyYTZhOTc5OTY4YWYiLCJpYXQiOjE3MjQ5NjQ4NTMsImV4cCI6NDMxNjk2NDg1M30.0HquznnuvoYXtpZrtBsnpdCBZvPqcWpzS_vBTZx3v_Q",
                content: content,
                last_updated: new Date().getTime()
            }),
            headers: {
                "Content-type": "application/json; charset=UTF-8"
            }
        })
        .then(res => res.json())
        .then(json => console.log(json))
        .catch(err => console.log(err))
    }

    let contentElement;

    const resizeTextarea = () => {
        contentElement.style.height = `1.5rem`;
        let scHeight = contentElement.scrollHeight;
        contentElement.style.height = `${scHeight}px`;
    }
</script>

<div class="task" id="{_id}">
    <div>
        <!-- <input class="content" type="text" bind:value={content} on:change={() => updateTaskContent(content)}> -->
         <textarea bind:this={contentElement} bind:value={content} class="content" placeholder="Type something here..." on:input={resizeTextarea} on:keyup={e => {
            if(e.key === "Enter") {
                e.target.value = e.target.value.trim();
                e.target.blur();
                resizeTextarea()
            }
         }} on:change={() => updateTaskContent(content)}></textarea>
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

    .content {
        background: transparent;
        border: none;
        color: white;
        padding: 0;
        height: 1.5rem;
        resize: none;
    }

    .content::-webkit-scrollbar {
        width: 0;
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