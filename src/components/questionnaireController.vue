<template>
  <div>
  <!-- til debug-->
  {{ this.currentStepIndex }}
  {{ this.hasValidAnswer }}
 </div>
  <h2>{{currentStep.question}}</h2>
  <div v-for="answer in currentStep.answers" :key="answer.answer">
    <input type="radio" name="answer" v-on:change="setSelectedAnswer(answer)" :value="answer.value" :checked="answer.selected" /> {{ answer.answer}}
  </div>
  <div>
    <button v-on:click="prev()">Prev</button>
    <button v-on:click="next()">Next</button>
  </div>
   
   <div v-for="step in steps" :key="step.index">
      <div>{{step.index}}. {{ step.name }}</div>
   </div>
</template>
  
<script>
import { mapState, mapGetters, mapActions } from 'vuex';
export default {
      name: 'question-naire',
        
      computed: {
          
          ...mapGetters({ currentStepIndex:"getCurrentStepIndex", currentQuestionnaireStep: "getCurrentQuestionnaireStep", steps:"getStep", hasValidAnswers:"hasValidAnswers" }),

          currentStep()
          {
              return this.currentQuestionnaireStep;
          },

          hasValidAnswer()
          {
            return this.hasValidAnswers
          }
      },

      methods:{
        ...mapActions({
          clearStepSelections: "clearStepSelections",
          prevStep: "prevStep",
          nextStep : "nextStep"
            }),

            prev()
            {
               this.prevStep();
            },

            next()
            {
              if(this.hasValidAnswer)
              {
              this.nextStep();
            }
            },


        setSelectedAnswer(answer)
        {
          this.clearStepSelections(this.currentStepIndex);
          answer.selected = true;
        }
      }
    }
</script>