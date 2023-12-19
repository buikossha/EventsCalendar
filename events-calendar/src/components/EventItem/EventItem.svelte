<script>
  import style from './EventItem.module.scss'
  import Modal from '../Modal/Modal.svelte'
  import moment from 'moment'
  export let event;

  const url = `https://${event.url}`
  const currentMonth = moment(event.date_start, 'MM/DD/YYYY').format('MMMM')
  const currentYear = moment(event.date_start, 'MM/DD/YYYY').format('YYYY')
  const currentWeekDayStart = moment(event.date_start, 'MM/DD/YYYY').format('ddd')
  const currentWeekDayEnd = moment(event.date_end, 'MM/DD/YYYY').format('ddd')

  let showModal = false;
</script>


<div class={moment(event.date_start, 'MM/DD/YYYY') > moment() ? style.event__container : style.event__container__expired}>
  <div class={style.event__date}>
    <span class={style.days__wrapper}>
      {event.date_start.slice(3, 5)} - {event.date_end.slice(3, 5)}
    </span>
    <span class={style.data__wrapper}>
      <p class={style.data__month}>{currentMonth}</p>
      <p class={style.data__gray}>{currentYear}, {currentWeekDayStart}-{currentWeekDayEnd}</p>
    </span>
  </div>
  <div class={style.event__title}>
    <button on:click={() => (showModal = true)}>{event.title}</button>
  </div>
  <p class={style.event__location}>{event.location}</p>

  <Modal bind:showModal>
      <h2 slot="title">
        {event.title}
      </h2>
  
    <div class={style.modal__block}>
      <span class={moment(event.date_start, 'MM/DD/YYYY') > moment() ? style.modal__days : style.modal__days__expired}>{event.date_start.slice(3, 5)} - {event.date_end.slice(3, 5)}</span>
      <p class={style.modal__month}>{currentMonth}</p>
      <p class={style.modal__yearday}>{currentYear}, {currentWeekDayStart}-{currentWeekDayEnd}</p>
      <p class={style.modal__location}>
        {event.location}
      </p>
    </div>
    <p class={style.modal__description}>{event.description}</p>
    <p class={style.modal__description}>{event.ticket_price}</p>
    <a class={style.modal__link} target="_blank" rel="noreferrer" href={url}>Visit event’s website</a>
  </Modal>
</div>
