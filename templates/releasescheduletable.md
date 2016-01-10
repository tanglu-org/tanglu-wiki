<!-- ReleaseScheduleTable Template -->
<table class="table table-hover table-striped">
  <thead>
  <tr>
    <th width="146">Date</th>
    <th>Event</th>
    <th>Art tasks</th>
    <th>PR tasks</t>
  </tr>
  </thead>
  <tbody>
  {{#each schedule_events}}

  {{#if stage1}}
    <tr class="success">
  {{else if stage2}}
    <tr class="warning">
  {{else if stage3}}
    <tr class="danger">
  {{else if stage4}}
    <tr class="info">
  {{else}}
    <tr>
  {{/if}}
      <th width="146" scope="row">{{date}}</th>
      <td style="vertical-align: middle;">{{event}}</td>
      <td style="vertical-align: middle;">{{art-task}}</td>
      <td style="vertical-align: middle;">{{pr-task}}</td>
    </tr>
    
  {{/each}}
  </tbody>
</table>
