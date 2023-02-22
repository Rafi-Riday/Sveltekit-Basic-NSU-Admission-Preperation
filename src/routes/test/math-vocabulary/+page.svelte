<script>
    import mathVocabulary from "./math-vocabulary.json";

    let page = "Test";
</script>

<main class="text-gray-900 flex flex-col gap-4 py-4 sm:px-4">
    <center>
        <button
            on:click={() => (page = "Test")}
            class="bg-sky-200 font-semibold px-2 py-1 underline">Test</button
        >
        or
        <button
            on:click={() => (page = "Read")}
            class="bg-sky-200 font-semibold px-2 py-1 underline">Read</button
        >
    </center>

    <center>
        <!-- Test -->
        <div class={page === "Read" ? "hidden" : ""}>
            <center class="mb-4">Select the answer</center>
            <div class="grid grid-cols-3 gap-4">
                {#each mathVocabulary as { n, en, bn }, idx (n)}
                    <fieldset
                        class="border border-solid border-slate-300 bg-slate-100 text-left p-2"
                    >
                        <legend
                            class="bg-slate-200 border border-slate-300 py-1 px-2"
                            ><span class="font-semibold">{idx + 1}. {en}</span
                            ></legend
                        >
                        <div class="flex flex-col gap-2">
                            <div
                                class="w-full flex flex-row items-center justify-evenly"
                            >
                                <label
                                    class="bg-white border border-slate-300 px-2 py-1"
                                    for="op_1_{n}"
                                >
                                    <input
                                        type="radio"
                                        name="q_{n}"
                                        id="op_1_{n}"
                                        value={bn}
                                    />
                                    {bn}
                                </label>
                                <label
                                    class="bg-white border border-slate-300 px-2 py-1"
                                    for="op_2_{n}"
                                >
                                    <input
                                        type="radio"
                                        name="q_{n}"
                                        id="op_2_{n}"
                                        value={bn}
                                    />
                                    {bn}
                                </label>
                            </div>
                            <div
                                class="w-full flex flex-row items-center justify-evenly"
                            >
                                <label
                                    class="bg-white border border-slate-300 px-2 py-1"
                                    for="op_3_{n}"
                                >
                                    <input
                                        type="radio"
                                        name="q_{n}"
                                        id="op_3_{n}"
                                        value={bn}
                                    />
                                    {bn}
                                </label>
                                <label
                                    class="bg-white border border-slate-300 px-2 py-1"
                                    for="op_4_{n}"
                                >
                                    <input
                                        type="radio"
                                        name="q_{n}"
                                        id="op_4_{n}"
                                        value={bn}
                                    />
                                    {bn}
                                </label>
                            </div>
                        </div>
                    </fieldset>
                {/each}
            </div>
        </div>
        <!-- Read -->
        <div class={page === "Test" ? "hidden" : ""}>
            <center class="mb-4"
                >Click <u class="cursor-pointer">Links</u> to view definitions</center
            >
            <table class="border-collapse border border-slate-400">
                <thead>
                    <tr>
                        {#each ["Serial No.", "English", "Bangla", "Reference to PDF"] as header (header)}
                            <th
                                class="bg-slate-300 border border-slate-400 px-3 py-1"
                                >{header}</th
                            >
                        {/each}
                    </tr>
                </thead>
                <tbody>
                    {#each mathVocabulary as { n, en, bn }, idx (n)}
                        <tr
                            class="{(idx + 1) % 2 === 0
                                ? 'bg-slate-100'
                                : 'bg-white'} hover:bg-slate-300"
                        >
                            <td
                                class="font-semibold border border-slate-400 px-3 py-1"
                                >{idx + 1}</td
                            >
                            <td
                                class="border border-slate-400 px-3 py-1 underline"
                            >
                                <a
                                    target="_blank"
                                    rel="noreferrer"
                                    href="https://www.google.com/search?q={en
                                        .split(' ')
                                        .join('+')}">{en}</a
                                >
                            </td>
                            <td
                                class="border border-slate-400 px-3 py-1 underline"
                            >
                                <a
                                    target="_blank"
                                    rel="noreferrer"
                                    href="https://www.google.com/search?q={bn
                                        .split(' ')
                                        .join('+')}">{bn}</a
                                >
                            </td>
                            <td
                                class="border border-slate-400 px-3 py-1 text-center"
                                >{n}</td
                            >
                        </tr>
                    {/each}
                </tbody>
            </table>
        </div>
    </center>
</main>
