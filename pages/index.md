---
title: "SwallFin - 아콩이네 투자 관리"
fullWidth: true
neverShowQueries: true
hideTOC: true
---

```sql common
SELECT 
    MAX(CASE WHEN id = 'display_date' THEN value END) AS build_date,
    MAX(CASE WHEN id = 'today_usd_krw' THEN CAST(value AS DECIMAL) END) AS usd_krw,
    MAX(CASE WHEN id = 'today_usd_krw' THEN CAST(value AS DECIMAL) END)
      - MAX(CASE WHEN id = 'prev_usd_krw' THEN CAST(value AS DECIMAL) END) AS usd_krw_delta,
    MAX(CASE WHEN id = 'today_vix' THEN CAST(value AS DECIMAL) END) AS vix,
    MAX(CASE WHEN id = 'today_vix' THEN CAST(value AS DECIMAL) END)
      - MAX(CASE WHEN id = 'prev_vix' THEN CAST(value AS DECIMAL) END) AS vix_delta,
    MAX(CASE WHEN id = 'today_fear_greed' THEN CAST(value AS DECIMAL) END) AS fear_greed,
    MAX(CASE WHEN id = 'today_fear_greed' THEN CAST(value AS DECIMAL) END)
      - MAX(CASE WHEN id = 'prev_fear_greed' THEN CAST(value AS DECIMAL) END) AS fear_greed_delta
FROM swallow_sheets.swallow_sheets_for_evidence_common
WHERE id IN ('display_date', 'today_usd_krw', 'prev_usd_krw', 'today_vix', 'prev_vix', 'today_fear_greed', 'prev_fear_greed');
```
---

<BigValue 
  title="데이터 갱신 시간"
  data={common}
  value=build_date
/>
&nbsp; &nbsp; &nbsp; &nbsp;
<BigValue 
  title="1 미국 달러 ($) ="
  data={common}
  value=usd_krw
  fmt=krw2
  comparison=usd_krw_delta
  comparisonTitle="전일 대비"
  comparisonFmt=krw2
/>
&nbsp; &nbsp; &nbsp; &nbsp;
<BigValue 
  title="VIX"
  data={common}
  value=vix
  fmt=num2
  comparison=vix_delta
  comparisonTitle="전일 대비"
  comparisonFmt=num2
/>
&nbsp; &nbsp; &nbsp; &nbsp;
<BigValue 
  title="Fear & Greed Index"
  data={common}
  value=fear_greed
  fmt=num2
  comparison=fear_greed_delta
  comparisonTitle="전일 대비"
  comparisonFmt=num2
/>

---

```sql stock_dashboard
SELECT * FROM swallow_sheets.swallow_sheets_for_evidence_latest_raw;
```


# 증시 현황판
<DataTable data={stock_dashboard} rows="all">
  <Column id=종목이름/>
  <Column id=현재가 fmt=num2/>
  <Column id="전일 대비(%)" redNegatives=true fmt=pct2 contentType=colorscale colorScale=info/>
  <Column id="52주 고가" fmt=num2/>
  <Column id="52주 저가" fmt=num2/>
  <Column id="RSI(14)" fmt=num2 contentType=colorscale colorScale=negative/>
  <Column id="주당 배당금" fmt=num2/>
  <Column id=배당률 fmt=pct2/>
  <Column id="배당기준일"/>
  <Column id="배당지급일"/>
  <Column id="업데이트"/>
</DataTable>