<template>

  <div>
    <h1 dir="auto">
      <KLabeledIcon icon="lesson" :label="title" />
    </h1>

    <HeaderTable>
      <HeaderTableRow :keyText="coachCommon$tr('statusLabel')">
        <LessonActive slot="value" :active="active" />
      </HeaderTableRow>
      <HeaderTableRow :keyText="coachCommon$tr('recipientsLabel')">
        <template slot="value">
          <Recipients
            :groupNames="groupNames"
            :hasAssignments="recipients.length > 0"
          />
        </template>
      </HeaderTableRow>
      <HeaderTableRow
        :keyText="coachCommon$tr('descriptionLabel')"
        :valueText="description || coachCommon$tr('descriptionMissingLabel')"
      />
    </HeaderTable>

  </div>

</template>


<script>

  import KLabeledIcon from 'kolibri.coreVue.components.KLabeledIcon';
  import HeaderTable from '../../common/HeaderTable';
  import HeaderTableRow from '../../common/HeaderTable/HeaderTableRow';
  import Recipients from '../../common/Recipients';
  import LessonActive from '../../common/LessonActive';
  import { coachStringsMixin } from '../../common/commonCoachStrings';

  // This is actually only used on the LessonSummaryPage, so Assignment type is
  // implicitly 'lesson'
  export default {
    name: 'AssignmentSummary',
    components: {
      LessonActive,
      KLabeledIcon,
      HeaderTable,
      HeaderTableRow,
      Recipients,
    },
    mixins: [coachStringsMixin],
    props: {
      title: {
        type: String,
        required: true,
      },
      active: {
        type: Boolean,
        required: true,
      },
      description: {
        type: String,
        required: false,
        default: null,
      },
      recipients: {
        type: Array,
        required: true,
      },
      groups: {
        type: Array,
        required: true,
      },
    },
    computed: {
      groupNames() {
        const names = [];
        this.recipients.forEach(r => {
          const match = this.groups.find(({ id }) => id === r.collection);
          if (match) {
            names.push(match.name);
          }
        });
        return names;
      },
    },
    $trs: {
      noOne: 'No one',
    },
  };

</script>


<style lang="scss" scoped></style>
