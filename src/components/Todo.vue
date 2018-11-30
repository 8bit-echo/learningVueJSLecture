<template>
  <!-- the $emit function allows you to send a message to the parent component with a custome name of the event and some data, in this case the prop called "index". See Home.vue: line 23 to see how this event is handled -->
  <li>
    <div class="text-wrapper">
      <span class="title">
        <slot name="title"></slot>
      </span>
      <br />
      <em> The prop received is {{ myPropName }}</em>
    </div>
    <button
      class="done"
      @click="$emit('itemWasClicked', index)"
    >✓</button>
  </li>
</template>

<script>
  //
  export default {
    name: "ToDo",
    props: ["myPropName", "index"] // this is how you tell this component what props to expect.
  };
</script>


<style lang="scss">
  li {
    padding: 1em;
    margin: 1em auto;
    border: solid 1px lightgrey;
    box-shadow: 0 0 0.05rem lightgrey;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .title {
      font-weight: bold;
      font-size: 125%;
    }

    button.done {
      background-color: #42b983;
      color: white;
      border: none;
      outline: none;
      font-size: 125%;
      padding: 1rem;
      border-radius: 0.5rem;
      transition: background-color 0.2s ease;
      cursor: pointer;

      &:hover {
        background-color: darken(#42b983, 10%);
        transition: background-color 0.2s ease;
      }
    }
  }
</style>
