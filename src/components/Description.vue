<template>
  <div class="relative w-60 mx-auto my-4">
    <BaseInput type="text" name="description" id="description" label="Description:" v-model="description" @input="handleInput"/>
    <span>{{ textCondition }}</span>
  </div>
</template>

<script>
import BaseInput from "@/components/BaseInput.vue";

export default {
  components: {BaseInput},
  data() {
    return {
      description: "",
      textCondition: "",
      balancedMessage: 'The text is balanced.',
      unbalancedMessage: 'The text is not balanced.',
      timeoutId: null
    };
  },

  methods: {
    handleInput() {
       if (this.timeoutId) {
        clearTimeout(this.timeoutId);
      }
       this.timeoutId = setTimeout(() => {
        this.textCondition = this.isBalanced(this.description) ? this.balancedMessage : this.unbalancedMessage;
      }, 500);
    },

    isBalanced(text) {
      const stack = [];
      const openingCharacters = '([{';
      const closingCharacters = ')]}';

      for (const char of text) {
        if (openingCharacters.includes(char)) {
          stack.push(char);
        } else if (closingCharacters.includes(char)) {
          const lastOpeningCharacters = stack.pop();
          const expectedOpeningCharacters = openingCharacters[closingCharacters.indexOf(char)];
          if (lastOpeningCharacters !== expectedOpeningCharacters) {
            return false;
          }
        }
      }

      return stack.length === 0;
    }
  }
};
</script>
