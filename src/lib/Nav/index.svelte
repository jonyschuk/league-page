<script>
	import { tabs } from "$lib/utils/tabs";
	import NavSmall from "./NavSmall.svelte";
	import NavLarge from "./NavLarge.svelte";

	import IconButton from "@smui/icon-button";
	import { Icon } from "@smui/common";
	import Select, { Option } from "@smui/select";

	export let activeTab;

	$: active = tabs.find(
		(tab) =>
			tab.dest == activeTab ||
			(tab.nest &&
				tab.children.find((subTab) => subTab.dest == activeTab))
	);

	// toggle dark mode
	let lightTheme =
		typeof window === "undefined" ||
		window.matchMedia("(prefers-color-scheme: light)").matches;

	function switchTheme() {
		lightTheme = !lightTheme;
		let themeLink = document.head.querySelector("#theme");
		if (!themeLink) {
			themeLink = document.createElement("link");
			themeLink.rel = "stylesheet";
			themeLink.id = "theme";
		}
		themeLink.href = `/smui${lightTheme ? "" : "-dark"}.css`;
		document.head
			.querySelector('link[href="/smui-dark.css"]')
			.insertAdjacentElement("afterend", themeLink);
	}

	const league_Ids = ["814387382903218176", "moreids"];
	console.log("leagueIds", league_Ids);
</script>

<svelte:head>
	<title
		>{!activeTab[1]
			? "Home"
			: activeTab[1].toUpperCase() + activeTab.slice(2)} | League Page</title
	>
</svelte:head>

<nav>
	<a href="/"><img id="logo" alt="league logo" src="./badge.png" /></a>

	<div class="container">
		<Select
			class="shaped-outlined"
			variant="outlined"
			label="Your Sleeper League ID"
			size="6"
		>
			{#each league_Ids as id}
				<Option value={id}>{id}</Option>
			{/each}
		</Select>
		<IconButton
			toggle
			pressed={lightTheme}
			on:MDCIconButtonToggle:change={switchTheme}
			class="lightDark"
		>
			<Icon class="material-icons" on>dark_mode</Icon>
			<Icon class="material-icons">light_mode</Icon>
		</IconButton>
	</div>

	<div class="large">
		<NavLarge {tabs} bind:active />
	</div>

	<div class="small">
		<NavSmall {tabs} bind:active={activeTab} />
	</div>
</nav>

<style>
	a {
		display: table;
		margin: 0 auto;
	}
	nav {
		background-color: var(--fff);
		position: relative;
		z-index: 2;
		border-bottom: 1px solid #00316b;
		box-shadow: 0 0 8px 0 #00316b;
	}

	#logo {
		width: 80px;
		display: block;
		margin: 0 auto;
		padding: 10px;
	}

	.large {
		display: block;
	}

	.small {
		display: none;
	}

	.container {
		position: absolute;
		top: 0.25em;
		right: 0.25em;
	}

	:global(.lightDark) {
		color: var(--g555);
	}

	@media (max-width: 950px) {
		/* width of the large navBar */
		.large {
			display: none;
		}

		.small {
			display: block;
		}
	}
</style>
