<script>
    import { onMount } from "svelte";
    import { tick } from "svelte";

    let time = $state(getTime());

    function getTime() {
        const now = new Date();
        return {
            hours: now.getHours().toString().padStart(2, "0"),
            minutes: now.getMinutes().toString().padStart(2, "0"),
            date: now.toLocaleDateString("nl-NL", {
                weekday: "short",
                day: "2-digit",
                month: "short",
            }),
        };
    }

    onMount(() => {
        const interval = setInterval(async () => {
            time = getTime();
            await tick();
        }, 1000);

        return () => clearInterval(interval);
    });
</script>

<div class="flex flex-col items-center justify-center p-6">
    <div class="text-6xl font-bold flex space-x-2">
        <span>{time.hours}</span>
        <span>:</span>
        <span>{time.minutes}</span>
    </div>
    <div class="text-lg text-gray-500 mt-2">{time.date}</div>
</div>

<style>
    @keyframes scale {
        0% {
            transform: scale(1);
            opacity: 1;
        }
        50% {
            transform: scale(1.3);
            opacity: 0.5;
        }
        100% {
            transform: scale(1);
            opacity: 1;
        }
    }
</style>
