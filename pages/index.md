---
title: "Bảng dữ liệu"
---

---
# Bảng dữ liệu
```sql dataquery
    SELECT
        *
    FROM mydatabase.getdata
    ORDER BY agency_name, company_name
```

<DataTable 
 data={dataquery} 
/>

## Dimension Grid
---
<DimensionGrid 
  data={dataquery}
  name=multi_dimensions 
  metrics={{
    "Login": "sum(login_count)",
    "Logout": "sum(logout_count)"
  }}
  multiple
/>

```sql monthly_activity
    SELECT
        DATE_TRUNC('day', l.action_time) AS activity_day,
        COUNT(*) FILTER (WHERE l.action_type = 'login') AS login_count,
        COUNT(*) FILTER (WHERE l.action_type = 'logout') AS logout_count
    FROM mydatabase.logs l
    JOIN mydatabase.getdata d 
    ON l.user_id = d.user_id
    WHERE ${inputs.multi_dimensions} 
    GROUP BY 1
    ORDER BY 1
```

<LineChart 
  data={monthly_activity}
  x="activity_day"
  y={["login_count", "logout_count"]}
  handleMissing="zero"
  title="Hoạt động đăng nhập / đăng xuất theo ngày"
/>

---