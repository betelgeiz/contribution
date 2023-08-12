<template>
    <div class="tooltip-container" @click="showTooltip" @mouseleave="hideTooltip">
      <slot></slot>
      <div v-if="visible" class="tooltip" :style="`left: ${leftPosition}%`">
        {{ content }} <br>
        <span>{{ dateTooltip }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        visible: false,
      };
    },
    props: {
      content: String,
      dateTooltip: String,
      leftPosition: {
        type: Number,
        default: 50
      }
    },
    methods: {
      showTooltip() {
        this.visible = true;
      },
      hideTooltip() {
        this.visible = false;
      },
    },
  };
  </script>
  
  <style scoped>
  .tooltip-container {
    position: relative;
    display: inline-block;
    width: 100%;
    height: 100%;
  }
  
  .tooltip {
    text-align: center;
    position: absolute;
    top: 100%;
    transform: translateX(-50%);
    padding: 0.5rem;
    background-color: rgba(0, 0, 0, 0.8);
    color: white;
    border-radius: 4px;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.2s ease-in-out;
    width: 150px;
  }

  .tooltip span {
    font-size: 13px;
    color: #949090;
  }
  
  .tooltip-container:hover .tooltip {
    opacity: 1;
  }
  </style>
  