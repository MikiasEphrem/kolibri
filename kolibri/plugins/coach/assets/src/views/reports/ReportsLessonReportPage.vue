<template>

  <CoreBase
    :immersivePage="false"
    :authorized="userIsAuthorized"
    authorizedRole="adminOrCoach"
    :showSubNav="true"
  >

    <TopNavbar slot="sub-nav" />

    <KPageContainer>

      <ReportsLessonHeader />

      <CoreTable :emptyMessage="emptyMessage">
        <thead slot="thead">
          <tr>
            <th>{{ coachCommon$tr('titleLabel') }}</th>
            <th>{{ coachCommon$tr('progressLabel') }}</th>
            <th>{{ coachCommon$tr('avgTimeSpentLabel') }}</th>
          </tr>
        </thead>
        <transition-group slot="tbody" tag="tbody" name="list">
          <tr v-for="tableRow in table" :key="tableRow.node_id">
            <td>
              <KLabeledIcon :icon="tableRow.kind">
                <KRouterLink
                  v-if="tableRow.kind === 'exercise' && tableRow.hasAssignments"
                  :text="tableRow.title"
                  :to="classRoute(
                    'ReportsLessonExerciseLearnerListPage',
                    { exerciseId: tableRow.content_id }
                  )"
                />
                <KRouterLink
                  v-else-if="tableRow.hasAssignments"
                  :text="tableRow.title"
                  :to="classRoute(
                    'ReportsLessonResourceLearnerListPage',
                    { resourceId: tableRow.content_id }
                  )"
                />
                <template v-else>
                  {{ tableRow.title }}
                </template>
              </KLabeledIcon>
            </td>
            <td>
              <StatusSummary
                :tally="tableRow.tally"
                :verbose="true"
              />
            </td>
            <td>
              <TimeDuration :seconds="tableRow.avgTimeSpent" />
            </td>
          </tr>
        </transition-group>
      </CoreTable>
    </KPageContainer>
  </CoreBase>

</template>


<script>

  import commonCoach from '../common';
  import ReportsLessonHeader from './ReportsLessonHeader';

  export default {
    name: 'ReportsLessonReportPage',
    components: {
      ReportsLessonHeader,
    },
    mixins: [commonCoach],
    computed: {
      emptyMessage() {
        return this.$tr('noResourcesInLesson');
      },
      lesson() {
        return this.lessonMap[this.$route.params.lessonId];
      },
      recipients() {
        return this.getLearnersForLesson(this.lesson);
      },
      table() {
        const contentArray = this.lesson.node_ids.map(node_id => this.contentNodeMap[node_id]);
        const sorted = this._.sortBy(contentArray, ['title']);
        return sorted.map(content => {
          const tally = this.getContentStatusTally(content.content_id, this.recipients);
          const tableRow = {
            avgTimeSpent: this.getContentAvgTimeSpent(content.content_id, this.recipients),
            tally,
            hasAssignments: Object.values(tally).reduce((a, b) => a + b, 0),
          };
          Object.assign(tableRow, content);
          return tableRow;
        });
      },
    },
    $trs: {
      back: 'All lessons',
      noResourcesInLesson: 'No resources in this lesson',
    },
  };

</script>


<style lang="scss" scoped></style>
