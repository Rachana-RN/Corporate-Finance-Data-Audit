CREATE TABLE customers__(
customer_id INT PRIMARY KEY, 
customer_name VARCHAR(100), 
region VARCHAR(50),
signup_year INT
);
CREATE TABLE transactions__(
transaction_id INT PRIMARY KEY,
customer_id INT,
order_date DATE,
amount_usd NUMERIC(10,2),
status VARCHAR(20)
);
INSERT INTO customers__ VALUES
(1,'Alpha TEch SOlutions','North',2025),
(2,'Beta Retail Corp','South',2025),
(3,'Gamma Global Inc','North',2026),
(4,'Delta Logistics','East',2026),
(5,'Apex Financials','West',2024),
(6,'Quantam MedTech','South',2025),
(7,'Zenith E-Commerce','West',2026),
(8,'Beacon Agritech','East',2025),
(9,'Stellar EduTech','North',2026),
(10,'Matrix Legal System','South',2024),
(11,'Vortex Aviation','West',2025),
(12,'Summit Real Estate','East',2026);

INSERT INTO transactions__ VALUES
(1001,1,'2026-01-15',4500.00,'Completed'),
(1002,1,'2026-01-20',3200.00,'Completed'),
(1003,2,'2026-01-22',8900.00,'Disputed'),
(1004,3,'2026-01-28',1200.00,'Completed'),
(1005,4,'2026-02-02',5500.00,'Completed'),
(1006,2,'2026-02-11',2100.00,'Completed'),
(1007,5,'2026-02-14',12500.00,'Completed'),
(1008, 6, '2026-02-18', 6200.00, 'Completed'),
(1009, 7, '2026-02-22', 1500.00, 'Disputed'),
(1010, 8, '2026-02-25', 3400.00, 'Completed'),
(1011, 9, '2026-03-01', 7800.00, 'Completed'),
(1012, 10, '2026-03-04', 4100.00, 'Completed'),
(1013, 11, '2026-03-09', 9500.00, 'Completed'),
(1014, 12, '2026-03-12', 11000.00, 'Completed'),
(1015, 1, '2026-03-15', 2300.00, 'Completed'),
(1016, 3, '2026-03-22', 5400.00, 'Completed'),
(1017, 5, '2026-04-02', 13200.00, 'Completed'),
(1018, 6, '2026-04-05', 4800.00, 'Disputed'),
(1019, 2, '2026-04-10', 7100.00, 'Completed'),
(1020, 7, '2026-04-12', 890.00, 'Completed'),
(1021, 4, '2026-04-19', 6200.00, 'Completed'),
(1022, 8, '2026-04-25', 1500.00, 'Completed'),
(1023, 10, '2026-05-01', 3100.00, 'Completed'),
(1024, 12, '2026-05-04', 14500.00, 'Disputed'),
(1025, 9, '2026-05-08', 2200.00, 'Completed'),
(1026, 11, '2026-05-15', 8800.00, 'Completed'),
(1027, 1, '2026-05-19', 4100.00, 'Completed'),
(1028, 3, '2026-05-24', 6700.00, 'Completed'),
(1029, 5, '2026-06-01', 11500.00, 'Completed'),
(1030, 2, '2026-06-04', 5300.00, 'Completed'),
(1031, 6, '2026-06-10', 7400.00, 'Completed'),
(1032, 7, '2026-06-14', 3200.00, 'Completed'),
(1033, 8, '2026-06-20', 2900.00, 'Completed'),
(1034, 4, '2026-06-28', 4900.00, 'Completed'),
(1035, 10, '2026-07-02', 8200.00, 'Completed'),
(1036, 11, '2026-07-07', 10500.00, 'Completed'),
(1037, 12, '2026-07-12', 3600.00, 'Completed'),
(1038, 9, '2026-07-18', 1900.00, 'Disputed'),
(1039, 1, '2026-07-22', 6000.00, 'Completed'),
(1040, 3, '2026-07-29', 8100.00, 'Completed'),
(1041, 5, '2026-08-03', 14000.00, 'Completed'),
(1042, 6, '2026-08-09', 5100.00, 'Completed'),
(1043, 2, '2026-08-14', 6600.00, 'Completed'),
(1044, 7, '2026-08-19', 4300.00, 'Completed'),
(1045, 8, '2026-08-25', 1200.00, 'Completed'),
(1046, 4, '2026-09-01', 7300.00, 'Completed'),
(1047, 10, '2026-09-05', 2900.00, 'Completed'),
(1048, 11, '2026-09-11', 12000.00, 'Completed'),
(1049, 12, '2026-09-15', 9800.00, 'Completed'),
(1050, 9, '2026-09-21', 5200.00, 'Completed'),
(1051, 1, '2026-09-26', 3100.00, 'Completed'),
(1052, 2, '2026-10-01', 4400.00, 'Disputed'),
(1053, 3, '2026-10-05', 7200.00, 'Completed'),
(1054, 5, '2026-10-10', 16500.00, 'Completed'),
(1055, 6, '2026-10-15', 3900.00, 'Completed');  

WITH business_audit_cte AS(
SELECT
c.customer_name,
c.region,
COUNT(t.transaction_id) AS total_sales,
SUM(t.amount_usd) AS total_billed_revenue,
SUM(CASE WHEN t.status='Disputed' THEN t.amount_usd ELSE 0 END) AS total_disputed_value
FROM customers__ c
JOIN transactions__ t ON c.customer_id=t.customer_id
GROUP BY c.customer_name, c.region
)
SELECT
	customer_name,
region,
total_sales,
business_audit_cte.total_billed_revenue,
total_disputed_value,
ROUND((total_disputed_value/business_audit_cte.total_billed_revenue)*100,2) AS dispute_impact_percentage
FROM business_audit_cte
ORDER BY business_audit_cte.total_billed_revenue DESC;
DROP VIEW IF EXISTs enterprise_risk_tracker;
CREATE VIEW enterprise_risk_tracker AS
WITH business_audit_cte AS (
    SELECT 
        c.customer_name,
        c.region,
        COUNT(t.transaction_id) AS total_sales,
        SUM(t.amount_usd) AS total_billed_revenue,
        SUM(CASE WHEN t.status = 'Disputed' THEN t.amount_usd ELSE 0 END) AS 
total_disputed_value
    FROM customers__ c
    JOIN transactions__ t ON c.customer_id = t.customer_id
    GROUP BY c.customer_name, c.region
)
SELECT 
    customer_name,
    region,
    total_sales,
    total_billed_revenue,
    total_disputed_value,
    ROUND((total_disputed_value *100.0 / total_billed_revenue), 2) AS 
dispute_impact_percentage
FROM business_audit_cte;
SELECT * FROM enterprise_risk_tracker ORDER BY total_billed_revenue DESC;








