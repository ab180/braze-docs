---
nav_title: "SQL Segment Extensions"
permalink: "/sql_segments/"
hidden: true
---

# SQL Segment Extensions

You can generate a Segment Extension using Snowflake SQL queries of [Snowflake]({{site.baseurl}}/partners/data_and_infrastructure_agility/data_warehouses/snowflake/) data. SQL can help you unlock new segment use cases because it offers the flexibility to describe the relationships between data in ways that aren't achievable through other segmentation features.

{% alert important %}
The SQL editor is in early access. If you're interested in participating in the early access, reach out to your customer success manager.
{% endalert %}

## Creating Segment Extensions using SQL

To create a Segment Extension using SQL:

1. Go to **Segments** > **Segment Extensions**.
2. Click **Create New Extension** and select **SQL Editor**.<br><br>
   ![Dropdown button on the Segment Extension page to open the SQL editor.][1]{: style="max-width:40%" }<br><br>
3. Add a name for your Segment Extension and input your SQL. See the following sections for requirements and resources.<br><br>
   ![SQL editor showing an example SQL Segment Extension.][2]<br><br>
4. Save your Segment Extension.

{% alert note %}
SQL queries that take longer than 20 minutes to run will time out.
{% endalert %}

When the extension finishes processing, you can [create a segment]({{site.baseurl}}/user_guide/engagement_tools/segments/segment_extension#step-5-use-your-extension-in-a-segment) using your Segment Extension and target this new segment with your campaigns and Canvases.

### SQL rules

Your SQL must adhere to the following rules:

- Don't include a comment in the last line.
- Your SQL must select only one column: the `user_id` column. This means your SQL must contain:

```sql
SELECT user_id FROM “INSERT TABLE NAME”
```

## Managing SQL Segment Extensions

On the **Segment Extensions** page, segments generated using SQL are denoted with <i class="fas fa-code" alt="SQL Segment Extension"></i> next to their name.

Select a SQL Segment Extension to view where the extension is being used, archive the extension, or manually [refresh the segment membership](#refreshing-segment-membership).

![Messaging Use section of the SQL Editor showing where the SQL segment is being used.][3]

### Refreshing segment membership

To refresh the segment membership of any Segment Extension created using SQL, open the Segment Extension and select **Refresh**. Currently SQL Segment Extensions do not automatically regenerate.

{% alert tip %}
If you created a segment where you expect users to enter and exit regularly, manually refresh the Segment Extension it uses before targeting that segment in a campaign or Canvas.
{% endalert %}

## Troubleshooting

Your query may fail for any of the following reasons:

- Syntax errors in your SQL query
- SQL does not adhere to the SQL rules
- Processing timeout (after 20 minutes)

[1]: {% image_buster /assets/img_archive/sql_segments_create.png %}
[2]: {% image_buster /assets/img_archive/sql_segments_editor.png %}
[3]: {% image_buster /assets/img_archive/sql_segments_usage.png %}