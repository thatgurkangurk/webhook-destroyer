<script lang="ts">
    $: webhook = "";
    $: isBtnDisabled = false;
    $: btnText = "Delete";
    let form: HTMLFormElement;

    async function onSubmit(
        e: Event & {
            readonly submitter: HTMLElement | null;
        } & {
            currentTarget: EventTarget & HTMLFormElement;
        }
    ) {
        if (
            !webhook.startsWith(
                "https://discord.com/api/webhooks/" ||
                    "https://canary.discord.com/api/webhooks/" ||
                    "https://ptb.discord.com/api/webhooks"
            )
        ) {
            alert("Please enter a valid Discord webhook URL!");
            form.reset();
            return;
        }

        isBtnDisabled = true;
        btnText = "Deleting...";

        fetch(webhook, {
            method: "DELETE",
        })
            .catch(() => alert("An error occurred!"))
            .then((res) => {
                form.reset();

                isBtnDisabled = false;
                btnText = "Delete";

                if (res) {
                    if (res.status === 204) {
                        alert("Webhook has been deleted!");
                    } else {
                        alert("Webhook does not exist!");
                    }
                }
            });
    }
</script>

<h1>{webhook}</h1>

<form bind:this={form} on:submit|preventDefault={onSubmit}>
    <label
        for="webhook"
        class="block mb-2 text-left text-sm font-medium text-gray-400"
        >Webhook URL</label
    >
    <input
        bind:value={webhook}
        type="url"
        id="webhook"
        class="block p-3 w-full rounded-md text-sm bg-gray-700 border-gray-600 outline-none focus:outline-2 focus:outline-blue-600 mb-4"
        placeholder="https://discord.com/api/webhooks/..."
        required
    />
    <button
        disabled={isBtnDisabled}
        type="submit"
        id="btn"
        class="bg-orange-600 hover:bg-orange-700 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5"
        >{btnText}</button
    >
</form>
