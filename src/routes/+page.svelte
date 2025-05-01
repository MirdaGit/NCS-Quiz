<script lang="ts">
	import Sphere from "$lib/Sphere.svelte";
	import { correctItems } from "../lib/stores";

	const listItems: Array<{ authors: Array<string>; name: string; color: string }> = [
		{ authors: ["alanwalker"], name: "fade", color: "#f2ea02" },
		{ authors: ["syncole"], name: "feelgood", color: "#f2ea02" },
		{ authors: ["differentheaven"], name: "nekozilla", color: "#e3e3e3" },
		{ authors: ["itro", "tobu"], name: "cloud9", color: "#f2ea02" },
		{ authors: ["elektrolight"], name: "symbolism", color: "green" },
		{ authors: ["lazlo"], name: "falltothelight", color: "pink" },
		{ authors: ["distrion, elektrolight"], name: "rubik", color: "#f2ea02" },
		{ authors: ["tobu"], name: "seven", color: "#f2ea02" },
		{ authors: ["alexskrindo"], name: "jumbo", color: "#f2ea02" },
		{ authors: ["deafkev"], name: "invincible", color: "green" },
		{ authors: ["spektrem"], name: "shine", color: "#f2ea02" },
		{ authors: ["tobu"], name: "roots", color: "#f2ea02" },
		{ authors: ["alanwalker"], name: "force", color: "#f2ea02" },
		{ authors: ["unknownbrain"], name: "whydoi", color: "green" },
		{ authors: ["jimyosef"], name: "firefly", color: "#f2ea02" },
		{ authors: ["tobu"], name: "life", color: "#f2ea02" },
		{ authors: ["axol", "alexskrindo"], name: "you", color: "#f2ea02" },
		{ authors: ["lensko"], name: "letsgo", color: "#f2ea02" },
		{ authors: ["tobu", "itro"], name: "sunburst", color: "#f2ea02" },
		{ authors: ["ahrix"], name: "nova", color: "#f2ea02" },
		{ authors: ["elektromania"], name: "skyhigh", color: "#f2ea02" },
		{ authors: ["tobu", "syndec"], name: "dusk", color: "#f2ea02" },
		{ authors: ["jimyosef"], name: "linked", color: "red" },
		{ authors: ["lensko"], name: "cetus", color: "#f2ea02" },
		{ authors: ["alanwalker"], name: "spectre", color: "#f2ea02" },
		{ authors: ["tobu"], name: "hope", color: "#f2ea02" },
		{ authors: ["disfigure"], name: "blank", color: "cyan" },
		{ authors: ["tobu"], name: "candyland", color: "#f2ea02" },
		{ authors: ["differentheaven", "eh!de"], name: "myheart", color: "red" },
		{ authors: ["cartoon"], name: "másymás", color: "#e3e3e3" }
	];

	function checkInput(value: string, index: number, type: "authors" | "name") {
		const input = value.trim().replace(" ", "").replace("-", "").toLowerCase();
		if (input === "") {
			return;
		}

		if (type === "authors") {
			if (listItems[index][type].some((x) => x === input)) {
				$correctItems.correctAuthors[index] = true;
				$correctItems.authors[index] = value;
				correctItems.set($correctItems);
			}
		} else {
			if (listItems[index][type] === input) {
				$correctItems.correctNames[index] = true;
				$correctItems.names[index] = value;
				correctItems.set($correctItems);
			}
		}
	}

	let finished: boolean = false;
	$: if (
		$correctItems.correctAuthors.length === 30 &&
		$correctItems.correctNames.length === 30 &&
		$correctItems.correctAuthors.every((x) => x === true) &&
		$correctItems.correctNames.every((x) => x === true)
	) {
		finished = true;
	}
</script>

<div class="flex flex-col place-items-center gap-8 p-10">
	{#if !finished}
		<h1 class="text-6xl font-semibold text-white">🐟 🔥 Top Tobu NCS 30 kvíz 🔥 🤪</h1>
		<div class="flex w-5/6 flex-row flex-wrap gap-4">
			{#each listItems as item, idx}
				<div
					class="flex grow flex-row place-items-center rounded-sm bg-[#15191e] text-white shadow"
				>
					<span class="w-24 px-4 text-4xl font-semibold select-none">
						{(idx + 1).toString().padStart(2, "0")}.
					</span>
					<div class="flex grow flex-row place-items-center justify-between py-2">
						<div class="flex grow flex-col gap-4 pr-4">
							<div class="flex flex-row gap-2">
								<span class="w-12 select-none"> Autor </span>
								<input
									disabled={$correctItems.correctAuthors[idx]}
									class="grow py-[1px] pl-2"
									class:border={!$correctItems.correctAuthors[idx]}
									class:select-none={$correctItems.correctAuthors[idx]}
									value={$correctItems.authors?.[idx] ?? ""}
									onchange={(e) => {
										const val = (e.target as EventTarget & { value: string }).value;
										checkInput(val, idx, "authors");
									}}
								/>
							</div>
							<div class="flex flex-row gap-2">
								<span class="w-12 select-none"> Název </span>
								<input
									disabled={$correctItems.correctNames[idx]}
									class="grow py-[1px] pl-2"
									class:border={!$correctItems.correctNames[idx]}
									class:select-none={$correctItems.correctNames[idx]}
									value={$correctItems.names?.[idx] ?? ""}
									onchange={(e) => {
										const val = (e.target as EventTarget & { value: string }).value;
										checkInput(val, idx, "name");
									}}
								/>
							</div>
						</div>
						<div class="flex flex-col place-items-center gap-2 py-4 pr-4">
							{#if $correctItems.correctAuthors[idx] && $correctItems.correctNames[idx]}
								<svg
									xmlns="http://www.w3.org/2000/svg"
									width="105"
									height="75"
									fill={item.color}
									class="bi bi-check-lg"
									viewBox="0 0 16 16"
								>
									<path
										d="M12.736 3.97a.733.733 0 0 1 1.047 0c.286.289.29.756.01 1.05L7.88 12.01a.733.733 0 0 1-1.065.02L3.217 8.384a.757.757 0 0 1 0-1.06.733.733 0 0 1 1.047 0l3.052 3.093 5.4-6.425z"
									/>
								</svg>
							{:else}
								<Sphere stroke={item.color} height={75} />
								<button
									class="cursor-pointer border px-3 py-1 transition-all"
									disabled={$correctItems.freeSpaces < 1}
									class:cursor-pointer={$correctItems.freeSpaces > 0}
									class:hover:bg-white={$correctItems.freeSpaces > 0}
									class:hover:text-[#15191e]={$correctItems.freeSpaces > 0}
									class:cursor-not-allowed={$correctItems.freeSpaces < 1}
									onclick={() => {
										$correctItems.correctAuthors[idx] = true;
										$correctItems.correctNames[idx] = true;
										$correctItems.authors[idx] = listItems[idx].authors.join(" & ");
										$correctItems.names[idx] = listItems[idx].name;
										$correctItems.freeSpaces -= 1;
										correctItems.set($correctItems);
									}}
								>
									Skip ({$correctItems.freeSpaces} / 5)
								</button>
							{/if}
						</div>
					</div>
				</div>
			{/each}
		</div>
	{:else}
		<div class="flex grow flex-col place-content-center place-items-center gap-10">
			<h1 class="text-center text-4xl leading-[2] font-semibold text-white">
				🔥 Skvělá práce, podařilo se ti odhalit všech 30 songů! 🔥 <br /> 🐟 Čas si za odměnu pustit
				BONUS track! 🐟
			</h1>
			<!-- svelte-ignore a11y_media_has_caption -->
			<video width="640" height="480" controls autoplay>
				<source src="lefish.mp4" type="video/mp4" />
			</video>
		</div>
	{/if}
</div>
