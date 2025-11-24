---
title: "Bảng dữ liệu"
---

---
# Bảng dữ liệu
```sql dataquery
    SELECT
    *
    FROM qlvt.getdata
    ORDER BY agency_name, company_name
```
<DataTable 
 data={dataquery} 
/>

<Checkbox
    title="button kiem tra uu tien" 
    name=date_today
  />

<DataTable title="getuser" data={getuser} />

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

```sql getuser
  SELECT DISTINCT ON (id) *
  FROM (
      ${checkToday(inputs.date_today, "db.getuser", "db.getuser2")}
  ) t
  ORDER BY id, priority;
```

```sql monthly_activity
    SELECT
        DATE_TRUNC('day', l.action_time) AS activity_day,
        COUNT(*) FILTER (WHERE l.action_type = 'login') AS login_count,
        COUNT(*) FILTER (WHERE l.action_type = 'logout') AS logout_count,
    FROM qlvt.logs l
    JOIN qlvt.getdata d 
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

```sql agency_name
    SELECT
    agency_name,
    company_name,
    user_name,
    login_count, logout_count
    FROM qlvt.getdata d 
    WHERE ${inputs.multi_dimensions} 
    GROUP BY all
    ORDER BY all
```
---

<DataTable data={agency_name} />