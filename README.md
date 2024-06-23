-- Year wise loan amount Stats
SELECT
  YEAR(STR_TO_DATE(issue_d, "%b-%yy")) AS 'Years',
  SUM(loan_amnt) AS 'Loan Amount'
FROM Finance_1_u
GROUP BY Years
ORDER BY Years;


-- Grade and sub grade wise revol_bal
SELECT 
    finance_1_u.Sub_grade,
    SUM(finance_2_u.revol_bal) as 'Total Revol Bal'
FROM finance_1_u
JOIN finance_2_u ON finance_1_u.id = finance_2_u.id
GROUP BY finance_1_u.Sub_grade
ORDER BY finance_1_u.Sub_grade;

-- Total Payment for Verified Status Vs Total Payment for Non Verified Status
Select
    finance_1_u.Verification_status,
    ROUND(SUM(finance_2_u.total_pymnt)) as `Total Payment`
FROM finance_1_u
JOIN finance_2_u ON finance_1_u.id = finance_2_u.id
WHERE finance_1_u.verification_status IN ('Verified', 'Not Verified')
GROUP BY finance_1_u.Verification_status;


-- State wise and month wise loan status
SELECT addr_state, last_credit_pull_d, MAX(loan_status) AS loan_status
FROM Finance_1_u
INNER JOIN Finance_2_u ON Finance_1_u.id = Finance_2_u.id
GROUP BY addr_state, last_credit_pull_d
ORDER BY addr_state;

-- Home ownership Vs last payment date stats
Select
YEAR(STR_TO_DATE(finance_2_u.last_pymnt_d, "%b-%yy")) as 'Years'
,finance_1_u.home_ownership
,round(sum(finance_2_u.last_pymnt_amnt)) as 'Total_Payment'
from finance_1_u join finance_2_u on finance_1_u.id = finance_2_u.id
where finance_1_u.home_ownership in ('RENT', 'MORTGAGE', 'OWN')
group by finance_1_u.home_ownership, years
having round(sum(finance_2_u.last_pymnt_amnt)) != 0
order by YEAR(STR_TO_DATE(finance_2_u.last_pymnt_d, "%b-%yy")), round(sum(finance_2_u.last_pymnt_amnt)) desc;
