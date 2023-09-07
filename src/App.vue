<template>
  <div>
    <ThreeDObjectGenerator :geometries="geometries" :editableHeight="1000" />
  </div>
</template>

<script>
import ThreeDObjectGenerator from "./components/ThreeDObjectGenerator.vue";

export default {
  components: {
    ThreeDObjectGenerator,
  },
  data() {
    return {
      geometries: [
        [
          [0, 0],
          [0, 10],
          [15, 10],
          [7, 15],
          [7, 8],
          [7, 0],
        ],
        [
          [0, 0],
          [7, 12],
          [0, 14],
        ],
      ],
    };
  },
};
</script>
