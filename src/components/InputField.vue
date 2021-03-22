<template>
  <div class="inputField">
    <span> Name </span>
    <input
      label="Name"
      class="input-box"
      :class="[errors && errors.length > 0 ? 'error' : 'normal']"
      type="text"
      v-model="input"
    />
    <ul v-if="errors && errors.length > 0">
      <li v-for="(error, idx) in errors" :key="idx" class="textError">
        {{ errors[idx] }}
      </li>
    </ul>
  </div>
</template>
<script>

export default {
  name: "input-field",
  data() {
    return {
      input: "",
    };
  },
  props: {
    /**
     * Array of errors that will be rendered if provided input doesn't satisfy validation
     */
    errors: {
      type: Array,
      default: () => []
    },
    /**
     * Function that will take in the user typed input and apply some validation logic
     */
    validationFunc: {
      type: Function,
      default: () => {}
    },
  },
  methods: {},
  watch: {
    input: function() {
      this.validationFunc(this.input);
    },
  },
};
</script>

<style scoped>
.input-box {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  -webkit-transition: 0.5s;
  transition: 0.5s;
}
.normal {
  border: 3px solid #ccc;
}
.error {
  outline: 3px solid red;
}
.textError {
  color: red;
}
</style>

<docs>

## Examples

Input Field With Validation:

```vue
  <template>
    <div class="wrapper">
      <input-field :validationFunc="validation" :errors="errors"></input-field>
    </div>
  </template>

  <script>
    export default {
      data() {
        return {
          errors: [],
          errorMessages: {
            insufficientLength: {
              text: "Name must be at least 5 characters long",
              isInvalid: (input) => input && input.length < 5
            },
            containsDisallowedCharacters: {
              text: "Name can only contain alphanumeric characters",
              isInvalid: (input) => !input.match(/^\w+$/g)
            }
          }
        }
      },
      methods: {
        validation: function(input) {
          this.errors = []
          const insufficientLength = this.errorMessages["insufficientLength"]
          const containsDisallowedCharacters = this.errorMessages["containsDisallowedCharacters"]
          if(insufficientLength.isInvalid(input)) {
            this.errors.push(insufficientLength.text)
          } else {
            this.errors = this.errors.filter((er) => er === insufficientLength.text)
          }
          if(containsDisallowedCharacters.isInvalid(input)) {
            this.errors.push(containsDisallowedCharacters.text)
          } else {
            this.errors = this.errors.filter((er) => er === containsDisallowedCharacters.text)
          }
        }
      }
    }
  </script>
```
</docs>
