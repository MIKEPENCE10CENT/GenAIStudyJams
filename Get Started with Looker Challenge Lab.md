### 1. Create a New View `users_region`
Create a new view named `users_region` in your Looker project and paste the following code:

```lkml
view: users_region {
  sql_table_name: cloud-training-demos.looker_ecomm.users ;;

  dimension: id {
    type: number
    sql: ${TABLE}.id ;;
    primary_key: yes
  }

  dimension: state {
    type: string
    sql: ${TABLE}.state ;;
  }

  dimension: country {
    type: string
    sql: ${TABLE}.country ;;
  }

  measure: count {
    type: count
    drill_fields: [id, state, country]
  }
}
```

### 2. Modify `training_ecommerce.model` File
In the `training_ecommerce.model` file, replace the existing content with the following:

```lkml
connection: "bigquery_public_data_looker"

# include all the views
include: "/views/*.view"
include: "/z_tests/*.lkml"
include: "/**/*.dashboard"

datagroup: training_ecommerce_default_datagroup {
  # sql_trigger: SELECT MAX(id) FROM etl_log;;
  max_cache_age: "1 hour"
}

persist_with: training_ecommerce_default_datagroup

label: "E-Commerce Training"

explore: order_items {
  join: users {
    type: left_outer
    sql_on: ${order_items.user_id} = ${users.id} ;;
    relationship: many_to_one
  }

  join: inventory_items {
    type: left_outer
    sql_on: ${order_items.inventory_item_id} = ${inventory_items.id} ;;
    relationship: many_to_one
  }

  join: products {
    type: left_outer
    sql_on: ${inventory_items.product_id} = ${products.id} ;;
    relationship: many_to_one
  }

  join: distribution_centers {
    type: left_outer
    sql_on: ${products.distribution_center_id} = ${distribution_centers.id} ;;
    relationship: many_to_one
  }
}

explore: events {
  join: event_session_facts {
    type: left_outer
    sql_on: ${events.session_id} = ${event_session_facts.session_id} ;;
    relationship: many_to_one
  }
  
  join: event_session_funnel {
    type: left_outer
    sql_on: ${events.session_id} = ${event_session_funnel.session_id} ;;
    relationship: many_to_one
  }
  
  join: users {
    type: left_outer
    sql_on: ${events.user_id} = ${users.id} ;;
    relationship: many_to_one
  }

  join: users_region {
    type: left_outer
    sql_on: ${events.user_id} = ${users_region.id} ;;
    relationship: many_to_one
  }
}
```

### 3. Create the Bar Chart
To create the bar chart in Looker:

1. **Go to the `events` explore**:
   - Select the `events` explore where the data for event types and users exists.
   
2. **Configure the chart**:
   - Select a dimension for `event type`.
   - Select a measure for `count of users` (from the `users` or `users_region` view).
   
3. **Sort and Limit**:
   - Sort the chart by the count of users in descending order.
   - Set a filter to show only the **Top 3 event types**.

4. **Create a Bar Chart**:
   - Change the visualization type to a **Bar Chart**.

5. **Save the Chart**:
   - Save the chart to a new dashboard by clicking **Save to Dashboard**.
   - Name the dashboard **User Events**.
