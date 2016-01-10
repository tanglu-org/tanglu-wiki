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
  {{#each tasks}}
  
  {{#if todo}}
    <tr class="danger">
      <th scope="row">Todo</th>
  {{else if inprogress}}
    <tr class="warning">
      <th scope="row">In Progress</th>
  {{else if done}}
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
    
  {{/each}}
  </tbody>
</table>
