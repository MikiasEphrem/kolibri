<template>

  <div class="page-status">
    <div class="pure-u-3-4">
      <div class="user-name-container">
        <mat-svg
          class="svg-item"
          category="action"
          name="face"
        />
        <h1 class="user-name">{{ $tr('title', {name: userName}) }}</h1>
      </div>
      <div class="questions">
        {{ $tr('overallScore', {score: score}) }}
      </div>
      <div class="questions">
        {{ $tr('questionsCorrect', { correct: questionsCorrect, total: questions.length }) }}
      </div>
    </div>
    <div class="pure-u-1-4">
      <div class="inner-column">
        <ProgressIcon class="svg-icon" :progress="progress" />
        <span v-if="completed">
          <strong> {{ $tr('completed') }} </strong>
          <br>
          <ElapsedTime :date="completionTimestamp" />
        </span>
        <span v-else-if="completed !== null">
          <strong> {{ $tr('inProgress') }} </strong>
        </span>
        <span v-else>
          <strong> {{ $tr('notStarted') }} </strong>
        </span>
      </div>
    </div>
  </div>

</template>


<script>

  import ProgressIcon from 'kolibri.coreVue.components.ProgressIcon';
  import ElapsedTime from 'kolibri.coreVue.components.ElapsedTime';

  export default {
    name: 'PageStatus',
    $trs: {
      title: '{name} - Exam performance',
      overallScore: 'Overall score: { score, number, percent }',
      questionsCorrect: 'Questions correct: {correct, number} of {total, number}',
      completed: 'Completed',
      inProgress: 'In progress',
      notStarted: 'Not started',
    },
    components: {
      ProgressIcon,
      ElapsedTime,
    },
    props: {
      userName: {
        type: String,
        required: true,
      },
      questions: {
        type: Array,
        default: () => [],
      },
      completed: {
        type: Boolean,
        default: false,
      },
      completionTimestamp: { type: Date },
    },
    computed: {
      questionsCorrect() {
        return this.questions.reduce((a, q) => a + (q.correct === 1 ? 1 : 0), 0);
      },
      score() {
        return this.questions.reduce((a, q) => a + q.correct, 0) / this.questions.length || 0;
      },
      progress() {
        // Either return in completed or in progress
        return this.completed ? 1 : 0.1;
      },
    },
  };

</script>


<style lang="scss" scoped>

  @import '~kolibri.styles.definitions';

  .page-status {
    background-color: $core-bg-light;
  }

  .user-name-container {
    display: block;
  }

  .svg-icon {
    font-size: 1.3em;
  }

  .questions {
    margin-top: 10px;
  }

  .svg-item {
    display: inline-block;
    vertical-align: middle;
  }

  .user-name {
    display: inline-block;
    margin: 0;
    vertical-align: middle;
  }

  .inner-column {
    float: right;
  }

</style>