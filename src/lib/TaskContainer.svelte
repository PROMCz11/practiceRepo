<script>
    import Task from "$lib/Task.svelte";
    import preloader from "$lib/assets/preloader.svg";

    export let tasks = [];

    let isLoadedFromServer = false;

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
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
{#await getTasks()}
    <img style="width: 50%; margin: auto;" src={preloader} alt="preloader">
{:then}
    {#each tasks as { content, date, important, completed, _id, last_updated }}
        <Task {isLoadedFromServer} {content} date={getFormattedLocalTime(date)} {important} {completed} {_id} last_updated={getFormattedLocalTime(last_updated)} />
    {/each}
{/await}