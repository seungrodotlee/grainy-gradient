<template>
  <div
    class="grainy-gradient relative flex"
    :class="mode === 'text' && 'text-grainy-gradient'"
  >
    <div v-if="useText">
      <svg height="0" width="0">
        <defs>
          <clipPath id="svg-text-path">
            <text x="0" :y="calculatedTextY">{{ text }}</text>
          </clipPath>
        </defs>
      </svg>
      <span ref="textElement" class="invisible">
        {{ text }}
      </span>
    </div>
    <div class="noise absolute top-0 left-0 w-screen h-screen overflow-hidden">
      <div class="absolute" :style="noiseStyle"></div>
    </div>
    <div
      class="gradient absolute top-0 left-0 w-full h-full"
      :style="gradientStyle"
    ></div>
  </div>
</template>

<script>
import { onMounted, ref, computed } from "vue";

export default {
  props: {
    useText: Object,
    gradientStyle: Object,
    gradient: String,
  },
  setup(props) {
    const { text, useTyping } = props.useText;
    const noiseStyle = ref(null);
    const gradientStyle = computed(() => {
      let positionStyle;
      if (gradientStyle.isCenter) {
        positionStyle = {
          top: "50%",
          left: "50%",
          height: "100%",
          width: "100%",
          transform: "translate(-50%, -50%)",
        };
      } else {
        positionStyle = {
          top: props.gradientStyle.top || "0",
          left: props.gradientStyle.left || "0",
          height: props.gradientStyle.height || "100%",
          width: props.gradientStyle.width || "100%",
        };
      }

      return {
        background: props.gradient,
        ...positionStyle,
      };
    });
    const textElement = ref(null);
    const calculatedTextY = computed(() => {
      if (!textElement.value) return;

      // textElement의 폰트 크기 구해서 폰트크기 * 7/6 반환
      console.log(textElement.value);
    });

    onMounted(async () => {});

    return {
      text,
      useTyping,
    };
  },
};
</script>

<style scoped>
.text-grainy-gradient {
  clip-path: url(#svg-text-path);
}

.noise > div {
  top: -50%;
  left: -50%;
  height: 200vh;
  width: 200vw;
  mix-blend-mode: overlay;
  animation: bg-animation 0.2s infinite;
  z-index: 10;
}

@keyframes bg-animation {
  0% {
    transform: translate(0, 0);
  }
  10% {
    transform: translate(-5%, -5%);
  }
  20% {
    transform: translate(-10%, 5%);
  }
  30% {
    transform: translate(5%, -10%);
  }
  40% {
    transform: translate(-5%, 15%);
  }
  50% {
    transform: translate(-10%, 5%);
  }
  60% {
    transform: translate(15%, 0);
  }
  70% {
    transform: translate(0, 10%);
  }
  80% {
    transform: translate(-15%, 0);
  }
  90% {
    transform: translate(10%, 5%);
  }
  100% {
    transform: translate(5%, 0);
  }
}
</style>
