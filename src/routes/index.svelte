<script context="module">
    export async function preload(page, session) {
        const res = await this.fetch(
            `https://quiet-bastion-27219.herokuapp.com/fixtures?first=20`,
        )
        const json = await res.json()
        const fixtures = json.fixtures.fixtures
        return { fixtures: json.fixtures }
    }
</script>

<script>
    import Header from "../components/Header.svelte"
    import SectionHeader from "../components/SectionHeader.svelte"
    import SectionSubHeader from "../components/SectionSubHeader.svelte"
    import FlatButton from "../components/FlatButton.svelte"
    import FixtureGroup from "../components/FixtureGroup.svelte"
    import HomeAwayHeader from "../components/HomeAwayHeader.svelte"
    import Advert from "../components/Advert.svelte"
    import SideMenu from "../components/SideMenu.svelte"
    import Fixtures from "../components/Fixtures.svelte"

    import { isOperaMini } from "../components/stores.js"

    let menuOpen = false
    export let fixtures
</script>

<style>
    main {
        top: 45px;
        position: absolute;
        width: 100%;
        max-width: 100%;
    }
    .button-row {
        display: flex;
    }
    .menu-open-opera-mini {
        display: none;
    }
</style>

<svelte:head>
    <title>Sapper project template</title>
</svelte:head>

{#if menuOpen}
    <SideMenu
        on:close-click={_ => {
            menuOpen = false
        }} />
{/if}

<div class:menu-open-opera-mini={isOperaMini && menuOpen}>
    <Header
        on:menu-click={_ => {
            menuOpen = true
        }} />
    <main>
        <Advert />
        <SectionHeader name="Popular Leagues" />
        <div class="button-row">
            <FlatButton amount="34">Premier League</FlatButton>
            <FlatButton amount="42">Bundesliga</FlatButton>
        </div>
        <div class="button-row">
            <FlatButton amount="14">La Liga</FlatButton>
            <FlatButton amount="7">France Ligue</FlatButton>
        </div>
        <SectionHeader name="All Matches" />
        <SectionSubHeader>
            <HomeAwayHeader title="Bundesliga 1" />
        </SectionSubHeader>
        <Fixtures data={fixtures} />
    </main>
</div>
