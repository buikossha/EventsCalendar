<script>
  import style from './EventList.module.scss'
  import EventItem from '../EventItem/EventItem.svelte'
  import { onMount } from 'svelte'
  import moment from 'moment'
  import axios from 'axios'

  let events = [];
  let sortedEvents = [];
  let sortInfo = '';
  let showAllEvents = false;
  let selected = '';

  onMount(async() => {
      try {
      const response = await axios.get('https://events.vortex.foundation/events');
      events = response.data.data
      events.sort((a, b) => moment(a.date_start, "MM/DD/YYYY") - moment(b.date_start, "MM/DD/YYYY"))
      sortedEvents = [...events]
    } catch (err) {
      console.log(err);
    }
  })

  $:sortedEvents = events.filter((el) => el.title.toLowerCase().includes(sortInfo.toLowerCase()))

  $:sortedEvents = events.filter((el) => el.location.includes(selected))
</script>


<div class={style.container}>
  <h2 class={style.title}>Upcoming crypto <br/> events calendar</h2>

  <div class={style.search__section}>
    <input name="search__input" bind:value={sortInfo} class={style.search__input} placeholder="Search event names"/>
    <select bind:value={selected} name="search__select" class={style.search__select}>
      <option value="">All</option>
      {#each events as event}
      <option value={event.location}>{event.location}</option>
    {/each}
    </select>
    <p class={style.search__text}>{events.length} events</p>
  </div>

  <div class={style.events}>
    <div class={style.events__header}>
      <p class={style.event__date}>Date</p>
      <p class={style.event__name}>Event Name</p>
      <p class={style.event__location}>Location</p>
    </div>
    {#each showAllEvents ? sortedEvents : sortedEvents.filter((el) => moment(el.date_start, 'MM/DD/YYYY') > moment()) as event (event.title)}
      <EventItem event={event} />
    {/each}
  </div>

  <button class={style.hide__button} on:click={() => showAllEvents = !showAllEvents}>
    {showAllEvents ? 'Hide past events' : 'See all events'}
  </button>
</div>
