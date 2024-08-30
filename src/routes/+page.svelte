<script>
    import Task from "../lib/Task.svelte";

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

    let tasks = [];

    // tasks = [
    //     {
    //         content: "First",
    //         date: new Date().getTime(),
    //         important: 0,
    //         completed: 0,
    //         _id: "45"
    //     },
    //     {
    //         content: "Second",
    //         date: new Date().getTime(),
    //         important: 0,
    //         completed: 0,
    //         _id: "234"
    //     },
    //     {
    //         content: "Third",
    //         date: new Date().getTime(),
    //         important: 0,
    //         completed: 0,
    //         _id: "135"
    //     },
    //     {
    //         content: "Fourth",
    //         date: new Date().getTime(),
    //         important: 0,
    //         completed: 0,
    //         _id: "325"
    //     }
    // ]

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
        if(json.status) {
            tasks = json.data.tasks;
            return json.data.tasks;
        }

        else {
            console.log(json.message);
            return null;
        }
    }

    const clickHandler = e => {
        const target = e.target;
        if(target.classList.contains("important-toggle")) {
            const id = target.parentElement.parentElement.id;
            const taskIndex = tasks.indexOf(tasks.find(task => task._id === id));
            tasks[taskIndex].important = !tasks[taskIndex].important;
        }

        else if(target.classList.contains("completed-toggle")) {
            const id = target.parentElement.parentElement.id;
            const taskIndex = tasks.indexOf(tasks.find(task => task._id === id));
            tasks[taskIndex].completed = !tasks[taskIndex].completed;
        }
    };
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div on:click={clickHandler}>
    {#await getTasks()}
    <img style="width: 50%; margin: auto;" src="../src/assets/preloader.svg" alt="preloader">
    {:then}
        {#each tasks as { content, date, important, completed, _id, last_updated }}
            <Task {content} date={getFormattedLocalTime(date)} {important} {completed} {_id} last_updated={getFormattedLocalTime(last_updated)} />
        {/each}
    {/await}
</div>