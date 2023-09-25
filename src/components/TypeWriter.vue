<template>
  <div>
    <div class="typewriter text-4xl py-12 font-medium">
      {{ currentStatement }}<span :class="{ blink: !typing && !deleting }">_</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      brandingStatements: [
          "You have arrived.", "Step into the future.", "Elevate every step.",
          "Footprints of excellence.", "Stride with pride.", "Empower your journey.",
          "Unleash your potential.", "Crafted for champions.", 
          "Beyond boundaries.", "Redefine the road.", "Discover your pace.",
          "Elegance in every step.", "Born to lead.", "Dare to walk.",
          "Epic journeys start here.", "Fuel your ambitions.", "Trailblazers welcome.",
          "Find your footing.", "Pioneering paths.", "The road less traveled.",
          "Push past possible.", "Challenge every mile.", "From zero to hero.",
          "Leap into legends.", "Walk the talk.", "For world-changers.",
          "Master your marathon.", "Command the streets.", "Unparalleled. Unbeaten.",
          "Forge your path.", "Born to defy.", "Beyond the horizon.", "Shape the world."
      ],
      currentStatement: "",
      deleting: false,
      typing: true,
    };
  },
  mounted() {
    this.nextStatement();
  },
  methods: {
    nextStatement() {
      const randomIndex = Math.floor(Math.random() * this.brandingStatements.length);
      const statement = this.brandingStatements[randomIndex];
      this.typeStatement(statement);
    },
    typeStatement(statement) {
      let index = 0;
      const interval = setInterval(() => {
        this.currentStatement += statement[index];
        index++;
        if (index === statement.length) {
          clearInterval(interval);
          this.typing = false;
          
          setTimeout(() => {
            this.deleteStatement(statement);
          }, 3000); // Delay of 1.5 seconds before deleting.
        }
      }, 150); 
    },
    deleteStatement(statement) {
      this.deleting = true;
      const interval = setInterval(() => {
        this.currentStatement = this.currentStatement.slice(0, -1);
        if (this.currentStatement.length === 0) {
          clearInterval(interval);
          this.deleting = false;
          this.typing = true;
          this.nextStatement();
        }
      }, 100); 
    }
  }
};
</script>


<style scoped>

@keyframes blink {
  0%, 49% {
    opacity: 1;
  }
  50%, 100% {
    opacity: 0;
  }
}
.blink {
  animation: blink 1s infinite
}
</style>
