<script>
    export let tasks = [];
    
	import Task from "./Task.svelte";

    const getTasks = async () => {
        const res = await fetch("https://task-manager-back-end-7gbe.onrender.com/api/tasks", {
            method: "POST",
            body: JSON.stringify({
                token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI2NmQwOWJkY2Q2MDIyYTZhOTc5OTY4YWYiLCJpYXQiOjE3MjQ5NjQ4NTMsImV4cCI6NDMxNjk2NDg1M30.0HquznnuvoYXtpZrtBsnpdCBZvPqcWpzS_vBTZx3v_Q"
            }),
            headers: {
                "Content-type": "application/json; charset=UTF-8"
            }
        })

        const json = await res.json();
        tasks = json.data.tasks;
        return tasks;
    }

</script>

<div class="task-container">
    {#await getTasks()}
    Loading...
    {:then tasks}
        {#each tasks as { _id, content, date, important, completed, last_updated }}
        <Task {_id} {content} {date} {important} {completed} {last_updated}/>
        {/each}
    {/await}
</div>

<style>
    .task-container {
        display: flex;
        flex-direction: column;
        gap: 1rem;

        padding: 1rem;
    }
</style>