---
title: "아콩이네 투자 결산"
---

```sql portfolio
select * from swallow_sheets.swallow_sheets_portfolio_portfolio
```
```sql latest_raw
select * from swallow_sheets.swallow_sheets_portfolio_latest_raw
```
```sql portfolio_analysis
SELECT 
    p.주주,
    p.종목이름,
    p.보유수량,
    (lr.현재가 * p.보유수량 - p.매입금액) AS 평가손익, 
    ((lr.현재가 * p.보유수량 - p.매입금액) / p.매입금액) * 100 AS 수익률,
    (lr.현재가 * p.보유수량) AS 평가금액,
    p.매입금액,
    (p.매입금액 / p.보유수량) AS 평균단가,
    lr.현재가,
    (lr."주당 배당금" * p.보유수량 * lr."연간 배당회수") AS 예상_연간_배당금,
    (lr."주당 배당금" * lr."연간 배당회수" / lr.현재가) * 100 AS 배당률,
    lr."주당 배당금",
    lr.배당지급일
FROM swallow_sheets.swallow_sheets_portfolio_portfolio p
LEFT JOIN swallow_sheets.swallow_sheets_portfolio_latest_raw lr
ON p.종목코드 = lr.종목코드;
```
```sql portfolio_summary
SELECT 
    p.주주,
    SUM((lr.현재가 * p.보유수량 - p.매입금액)) AS 평가손익, 
    (SUM(lr.현재가 * p.보유수량 - p.매입금액) / SUM(p.매입금액)) AS 수익률,
    SUM(lr."주당 배당금" * p.보유수량 * lr."연간 배당회수") AS 연간_배당금,
    (SUM(lr."주당 배당금" * p.보유수량 * lr."연간 배당회수") / SUM(p.매입금액)) AS 배당률,
    SUM(lr.현재가 * p.보유수량) AS 평가금액,
    SUM(p.매입금액) AS 매입금액
FROM swallow_sheets.swallow_sheets_portfolio_portfolio p
LEFT JOIN swallow_sheets.swallow_sheets_portfolio_latest_raw lr
ON p.종목코드 = lr.종목코드
GROUP BY p.주주;
```

<!-- redNegatives=true -->
## 합산
<DataTable data={portfolio_summary} rows=all>
    <Column id=주주/>
    <Column id=평가손익 redNegatives=true fmt=krw0/>
    <Column id=수익률 redNegatives=true fmt=pct2/>
    <Column id="연간_배당금" redNegatives=true fmt=krw0/>
    <Column id=배당률 redNegatives=true fmt=pct2/>
    <Column id=평가금액 fmt=krw0/>
    <Column id=매입금액 fmt=krw0/>
</DataTable>

## 개별 종목
<DataTable data={portfolio_analysis} rows=all>
</DataTable>

