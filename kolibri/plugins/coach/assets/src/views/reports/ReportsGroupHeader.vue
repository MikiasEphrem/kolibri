<template>

  <div>
    <p>
      <BackLink
        :to="classRoute('ReportsGroupListPage')"
        :text="$tr('back')"
      />
    </p>
    <h1>
      <KLabeledIcon icon="group" :label="group.name" />
    </h1>

    <!-- TODO COACH
    <HeaderTable>
      <HeaderTableRow>
        <template slot="key">
          {{ coachCommon$tr('avgQuizScoreLabel') }}
        </template>
        <template slot="value">
          {{ coachCommon$tr('percentage', {value: avgScore}) }}
        </template>
      </HeaderTableRow>
    </HeaderTable>
    <HeaderTable>
      <HeaderTableRow>
        <template slot="key">{{ coachCommon$tr('learnersLabel') }}</template>
        <template slot="value">
          <TruncatedItemList
            :items="[
              'John',
              'Alice',
              'Julie',
              'Lucy',
              'Steve',
              'Mary',
              'Judy',
              'George',
              'Carol',
              'Ken'
            ]"
          />
        </template>
      </HeaderTableRow>
    </HeaderTable>
     -->

    <HeaderTabs>
      <HeaderTab
        :text="coachCommon$tr('reportsLabel')"
        :to="classRoute('ReportsGroupReportPage', {})"
      />
      <HeaderTab
        :text="coachCommon$tr('membersLabel')"
        :to="classRoute('ReportsGroupLearnerListPage', {})"
      />
      <HeaderTab
        :text="coachCommon$tr('activityLabel')"
        :to="classRoute('ReportsGroupActivityPage', {})"
      />
    </HeaderTabs>
  </div>

</template>


<script>

  import commonCoach from '../common';

  export default {
    name: 'ReportsGroupHeader',
    components: {},
    mixins: [commonCoach],
    computed: {
      group() {
        return this.groupMap[this.$route.params.groupId];
      },
      /** TODO COACH
      recipients() {
        return this.group.member_ids;
      },
      avgScore() {
        const examStatuses = this.examStatuses.filter(status =>
          this.recipients.includes(status.learner_id)
        );
        const statuses = examStatuses.filter(status => status.status === this.STATUSES.completed);
        if (!statuses.length) {
          return null;
        }
        return this._.meanBy(statuses, 'score');
      },
      */
    },
    $trs: {
      back: 'All groups',
    },
  };

</script>


<style lang="scss" scoped></style>
