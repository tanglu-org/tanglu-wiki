<!-- FeatureTable -->
<table class="table table-hover">
  <thead>
  <tr>
    <th>Status</th>
    <th>Component</th>
    <th>Description</th>
    <th>Contact</th>
  </tr>
  </thead>
  <tbody>
  {{#if status == "todo" }}
    <tr class="danger">
      <th scope="row">Todo</th>
  {{else if status == "inprogress"}}
    <tr class="warning">
      <th scope="row">In Progress</th>
  {{else if status == "done"}}
    <tr class="success">
      <th scope="row">Done</th>
  {{else}}
    <tr>
      <th scope="row">Unknown</th>
  {{/if}}
    <td>{{component}}</td>
    <td>{{description}}</td>
    <td>{{contact}}</td>
  </tr>
  </tbody>
</table>