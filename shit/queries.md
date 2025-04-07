```sql
WITH refund_payments AS (
  SELECT 
    order_id,
    SUM(amount_electronic + amount_loyalty + amount_cash) AS refund_sum
  FROM payments.payments
  GROUP BY order_id
  HAVING SUM(amount_electronic + amount_loyalty + amount_cash) >= 0
),
order_match AS (
  SELECT 
    oi.order_id,
    e.title ->> 'ru' AS match_title
  FROM tickets.order_items oi
  JOIN tickets.tickets t ON t.id = oi.item_id
  JOIN events.events e ON e.id = t.event_id
  GROUP BY oi.order_id, e.title
)
SELECT 
  o.id,
  CONCAT(u.person->'name'->>'ru', ' ', u.person->'surname'->>'ru') AS "Имя Фамилия",
  om.match_title AS "Матч",
  o.update_time AS "Дата возврата",
  rp.refund_sum AS "Сумма возврата"
FROM tickets.orders o
JOIN refund_payments rp ON rp.order_id = o.id
JOIN auth.users u ON u.id::TEXT = o.user_id
JOIN order_match om ON om.order_id = o.id
WHERE o.status = 'PARTIAL_REFUND' or o.status = 'REFUND';
```
