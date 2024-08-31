<script>
    import TaskContainer from "../lib/TaskContainer.svelte";

    let tasks;

    const toggleTask = (id, toggleCompleted) => {
        const taskIndex = tasks.indexOf(tasks.find(task => task._id === id));
        const newLast_updated = new Date().getTime();
        tasks[taskIndex].last_updated = newLast_updated;
        if(toggleCompleted) {
            const newCompleted = !tasks[taskIndex].completed
            tasks[taskIndex].completed = newCompleted;
            fetch(`https://task-manager-back-end-7gbe.onrender.com/api/tasks/update/${id}`, {
                method: "PATCH",
                body: JSON.stringify({
                    token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI2NmQwOWJkY2Q2MDIyYTZhOTc5OTY4YWYiLCJpYXQiOjE3MjQ5NjQ4NTMsImV4cCI6NDMxNjk2NDg1M30.0HquznnuvoYXtpZrtBsnpdCBZvPqcWpzS_vBTZx3v_Q",
                    completed: newCompleted,
                    last_updated: newLast_updated
                }),
                headers: {
                    "Content-type": "application/json; charset=UTF-8"
                }
            })
            .then(res => res.json())
            .then(json => console.log(json))
            .catch(err => console.log(err));
        }
        else {
            const newImportant = !tasks[taskIndex].important;
            tasks[taskIndex].important = newImportant;
            fetch(`https://task-manager-back-end-7gbe.onrender.com/api/tasks/update/${id}`, {
                method: "PATCH",
                body: JSON.stringify({
                    token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySWQiOiI2NmQwOWJkY2Q2MDIyYTZhOTc5OTY4YWYiLCJpYXQiOjE3MjQ5NjQ4NTMsImV4cCI6NDMxNjk2NDg1M30.0HquznnuvoYXtpZrtBsnpdCBZvPqcWpzS_vBTZx3v_Q",
                    important: newImportant,
                    last_updated: newLast_updated
                }),
                headers: {
                    "Content-type": "application/json; charset=UTF-8"
                }
            })
            .then(res => res.json())
            .then(json => console.log(json))
            .catch(err => console.log(err));
        }
    }

    const clickHandler = e => {
        const target = e.target;
        if(target.classList.contains("important-toggle")) {
            const id = target.parentElement.parentElement.id;
            toggleTask(id, false);
        }

        else if(target.classList.contains("completed-toggle")) {
            const id = target.parentElement.parentElement.id;
            toggleTask(id, true);
        }
    };

    let touchtestEl;
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div on:click={clickHandler}>
    <TaskContainer bind:tasks={tasks} />
</div>

v 1.0.3

<div bind:this={touchtestEl} class="touchtest" on:touchstart={
    touchtestEl.textContent = "touched"
}>Press</div>

<style>
    .touchtest {
        background-color: gray;
        width: 50%;
        height: 50vw;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: auto;
    }
</style>