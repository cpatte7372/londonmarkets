# Claude AI for Excel — Prompt Library
## London Markets Insurance Data Engineering Series

Companion prompts for the six-episode series. Run each prompt in Claude AI for Excel
against the ClientClaimTxnPayments dataset (2,000 synthetic settlements included in
this pack), or adapt them to your own claims data built on the same data model.

Source data model: dbo.tovw_ClientClaimTransactionPayments

---

## Episode 1 — Claims Financial Reconciliation

**Prompt 1**
```
Summarise the total PaidClaim and Outstanding amounts across all
SettlementIDs. Highlight any settlements where Outstanding is greater
than PaidClaim and flag these as potentially under-reserved positions.
```

**Prompt 2**
```
Create a table showing SettlementID, PaidClaim, Outstanding, and a
calculated IncurredLoss column (PaidClaim + Outstanding). Sort by
IncurredLoss descending to identify the largest open exposures.
```

---

## Episode 2 — Apportionment & Co-insurance Transparency

**Prompt 1**
```
Compare ApportionmentPaidClaim against PaidClaim for each SettlementID.
Where ApportionmentPaidClaim exceeds PaidClaim, flag these rows and
provide a summary count and total value of such settlements.
```

**Prompt 2**
```
Produce a breakdown showing the split between direct client payments
(ApportionmentClientPaidClaim) and third-party payments (PaidClaimTP)
as a percentage of total paid claims per settlement.
```

---

## Episode 3 — Third-Party & Trust Account Monitoring

**Prompt 1**
```
Summarise all settlements where PaidClaimTP or PaidFeesTP is greater
than zero. Show total values and what proportion of overall paid claims
these represent. Label this the Trust Account Activity Summary.
```

**Prompt 2**
```
Identify any settlements where PaidClaimTP is populated but
ApportionmentCommissionTP is zero. List these as exceptions requiring
review for potential trust fund reconciliation gaps.
```

---

## Episode 4 — Commission & Funds Withheld Tracking

**Prompt 1**
```
Produce a summary table showing ApportionmentCommission,
ApportionmentCommissionAll, and ApportionmentFundsWithheld per
SettlementID. Calculate the ratio of FundsWithheld to CommissionAll
and flag any settlement where this ratio exceeds 50%.
```

**Prompt 2**
```
Show the total ApportionmentFundsWithheld across all settlements and
compare this to total ApportionmentCommissionAll. Provide a written
interpretation of what this ratio means for net settlement exposure.
```

---

## Episode 5 — Fee vs Loss Segregation

**Prompt 1**
```
For each SettlementID, calculate the fee loading ratio by dividing
PaidFees by PaidClaim. Highlight any settlements where fees represent
more than 20% of the paid claim amount as outliers for review.
```

**Prompt 2**
```
Produce a regulatory summary table showing total PaidClaim, PaidFees,
Outstanding, and OutstandingFees as separate line items. Format this
as an IFRS 17 disclosure-ready split between indemnity and loss
adjustment expense.
```

---

## Episode 6 — Downstream Analytics & Portfolio Reporting

**Prompt 1**
```
Create a portfolio summary showing total values for every column in
this dataset. Present this as an executive dashboard table with
clear column labels and GBP formatted values.
```

**Prompt 2**
```
Identify the top 10 SettlementIDs by total incurred loss (PaidClaim
plus Outstanding). For each, show all financial components side by
side and highlight which settlements have the highest apportionment
activity relative to direct payments.
```

**Prompt 3**
```
Produce a written client-facing claims portfolio summary based on
this data. Include total paid losses, outstanding reserves, fee
exposure, and apportionment activity. Write it in a professional
tone suitable for a renewal meeting with an insured.
```

---

## Bonus — General Utility Prompts

**Data quality audit**
```
Audit this dataset for any rows where all financial columns are zero.
List the SettlementIDs affected and flag them as potentially incomplete
or erroneous records.
```

**Sign convention check**
```
Check for any negative values across PaidClaim, Outstanding,
ApportionmentPaidClaim, and ApportionmentCommission. List affected
SettlementIDs and provide a possible explanation for each sign
convention in a London Markets context.
```

**Distribution analysis**
```
Create a chart showing the distribution of PaidClaim values across
all settlements, grouped into bands of equal size. Describe the
shape of the distribution and what it suggests about the claims
portfolio composition.
```

---

## The Data Model

The dataset is produced by a structured data layer (T-SQL view) joining seven
source tables. Each column is precisely defined at source:

| Column | Definition |
|---|---|
| SettlementID | Unique settlement reference (NetSettlementID) |
| PaidClaim | Direct paid losses — posting codes CM/RF, non-fee, no apportionment |
| PaidFees | Direct paid fees — posting codes CM/RF, fee-flagged |
| Outstanding | Loss reserves — posting code OS, non-fee |
| OutstandingFees | Fee reserves — posting code OS, fee-flagged |
| ApportionmentPaidClaim | Apportioned paid claims — TypeCode PC |
| ApportionmentPaidFees | Apportioned paid fees — TypeCode PFS |
| ApportionmentCommission | Commission on Credit accounts — TypeCode CS |
| ApportionmentFundsWithheld | Funds withheld on Credit accounts — TypeCode FW |
| ApportionmentCommissionAll | Commission across all account types — TypeCode CS |
| PaidClaimTP | Third-party paid claims — Trust accounts or settlement accounts |
| PaidFeesTP | Third-party paid fees — Trust accounts or settlement accounts |
| ApportionmentCommissionTP | Third-party commission apportionments |
| ApportionmentClientPaidClaim | Client-specific apportioned claims (CreditWho/DebitWho = 1) |
| ApportionmentClientPaidFees | Client-specific apportioned fees (CreditWho/DebitWho = 1) |

The prompt is easy. The column meaning exactly what it says, every time —
that is the hard part. That is data engineering.

---

*The dataset in this pack is synthetic — precisely modelled, insight-driven
data created for demonstration purposes. No real client, insurer, or
settlement information is included.*
