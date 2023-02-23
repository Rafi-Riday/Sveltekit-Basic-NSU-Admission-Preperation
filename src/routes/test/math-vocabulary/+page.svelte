<script>
    import mathVocabulary from "./math-vocabulary.json";

    // TODO: change default page value to "Read"
    let page = "Read";
    let questionLang = "en";
    const oneDirectionRange = 4;
    const returnQuestionSet = (idx) => {
        const optionSet = [];
        for (let i = 1; i <= oneDirectionRange; i++) {
            if (idx - i >= 0) {
                optionSet.push(mathVocabulary[idx - i]);
            } else {
                optionSet.push(mathVocabulary[mathVocabulary.length + idx - i]);
            }
            if (idx + i <= mathVocabulary.length - 1) {
                optionSet.push(mathVocabulary[idx + i]);
            } else {
                optionSet.push(mathVocabulary[idx + i - mathVocabulary.length]);
            }
        }
        return {
            optionSet,
            qSet: mathVocabulary[idx],
        };
    };
    const range = (max) => {
        return Math.floor(Math.random() * max);
    };
    const returnMCQ = (idx) => {
        const { optionSet, qSet } = returnQuestionSet(idx);
        const selectedWrongAns = [];
        const recursiveRandomOption = () => {
            const randomOption = optionSet[range(optionSet.length)];
            if (!selectedWrongAns.find((o) => o.n === randomOption.n)) {
                selectedWrongAns.push(randomOption);
            }
            if (selectedWrongAns.length < oneDirectionRange - 1) {
                recursiveRandomOption();
            }
        };
        recursiveRandomOption();
        const rightOptionNum = range(oneDirectionRange);
        const finalOptionSet = []
            .concat(selectedWrongAns.slice(0, rightOptionNum))
            .concat([qSet])
            .concat(
                selectedWrongAns.slice(rightOptionNum, selectedWrongAns.length)
            );
        return {
            finalOptionSet,
            qSet,
        };
    };

    const finalQuestionSet = [];
    mathVocabulary.forEach(({ n }, idx) => {
        finalQuestionSet.push(returnMCQ(idx));
    });

    const resultSet = {};
    let submitSection = "Submit";
    let markSheet;
    $: markSheet = {
        total: mathVocabulary.length,
        answered: Object.keys(resultSet).length,
        correct: Object.keys(resultSet).filter(
            (rIdx) => resultSet[rIdx].qSet.n === resultSet[rIdx].option.n
        ).length,
        wrong: Object.keys(resultSet).filter(
            (rIdx) => resultSet[rIdx].qSet.n !== resultSet[rIdx].option.n
        ).length,
    };
</script>

<main class="text-gray-900 flex flex-col gap-4 py-4 sm:px-4">
    <center>
        <button
            on:click={() => (page = "Read")}
            class="bg-sky-200 font-semibold px-2 py-1 underline">Read</button
        >
        or
        <button
            on:click={() => (page = "Test")}
            class="bg-sky-200 font-semibold px-2 py-1 underline">Test</button
        >
    </center>

    <center>
        <!-- Read -->
        {#if page === "Read"}
            <div id="Read">
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
        {/if}
        <!-- Test -->
        {#if page === "Test"}
            <div id="Test">
                <center class="mb-4">Select the answer</center>
                <center
                    class="mb-4 flex {questionLang === 'en'
                        ? 'flex-row'
                        : 'flex-row-reverse'} gap-2 justify-center items-center"
                >
                    <span>English</span>
                    <button
                        on:click={() =>
                            questionLang === "en"
                                ? (questionLang = "bn")
                                : (questionLang = "en")}
                        class="border border-sky-200 p-1 bg-sky-200 hover:bg-transparent"
                    >
                        <svg
                            class="w-5 h-5"
                            focusable="false"
                            xmlns="http://www.w3.org/2000/svg"
                            viewBox="0 0 24 24"
                        >
                            <path
                                d="M6.99 11L3 15l3.99 4v-3H14v-2H6.99v-3zM21 9l-3.99-4v3H10v2h7.01v3L21 9z"
                            />
                        </svg>
                    </button>
                    <span>Bangla</span>
                </center>
                <div
                    class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 px-2 sm:px-0"
                >
                    {#each finalQuestionSet as { finalOptionSet, qSet }, idx (qSet.n)}
                        <fieldset
                            class="border border-solid border-slate-300 bg-slate-100 text-left p-2"
                        >
                            <legend
                                class="bg-slate-200 border border-slate-300 py-1 px-2"
                                ><span class="font-semibold"
                                    >{idx + 1}. {qSet[questionLang]}</span
                                ></legend
                            >
                            <div class="grid grid-cols-2 gap-2">
                                {#each finalOptionSet as option, opIdx (opIdx)}
                                    <label
                                        class="bg-white border border-slate-300 px-2 py-1"
                                        for="op_{idx}_{opIdx}"
                                    >
                                        <input
                                            disabled={submitSection === "Submit"
                                                ? false
                                                : true}
                                            on:click={(e) => {
                                                resultSet[idx] = {
                                                    qSet,
                                                    option,
                                                };
                                                [
                                                    ...e.target.parentElement
                                                        .parentElement.children,
                                                ].forEach((c) =>
                                                    c.classList.add(
                                                        "bg-red-300"
                                                    )
                                                );
                                            }}
                                            type="radio"
                                            name="q_{idx}"
                                            id="op_{idx}_{opIdx}"
                                            value={option[
                                                questionLang === "en"
                                                    ? "bn"
                                                    : "en"
                                            ]}
                                        />
                                        {option[
                                            questionLang === "en" ? "bn" : "en"
                                        ]}
                                    </label>
                                {/each}
                            </div>
                        </fieldset>
                    {/each}
                </div>
                <div
                    class="fixed {submitSection === 'Reset'
                        ? '-bottom-6'
                        : '-bottom-3'} left-1/2 -translate-x-1/2 -translate-y-1/2"
                >
                    {#if submitSection === "Submit"}
                        <button
                            on:click={() => (submitSection = "Reset")}
                            class="bg-sky-400 shadow-custom font-semibold px-4 py-2 underline"
                        >
                            Submit
                        </button>
                    {:else}
                        <button
                            on:click={() => (submitSection = "Submit")}
                            class="bg-sky-400 shadow-custom font-semibold px-4 py-2 underline"
                        >
                            Reset
                        </button>
                        <div
                            class="bg-sky-200 mt-2 px-2 py-1 border border-slate-500 font-semibold shadow-custom"
                        >
                            Total: {markSheet?.total}, Answered: {markSheet?.answered},
                            Correct: {markSheet?.correct}, Wrong: {markSheet?.wrong}
                        </div>
                    {/if}
                </div>
            </div>
        {/if}
    </center>
</main>

<style>
    .shadow-custom {
        box-shadow: rgba(0, 0, 0, 0.3) 0px -10px 38px,
            rgba(0, 0, 0, 0.22) 0px 15px 12px;
    }
</style>
