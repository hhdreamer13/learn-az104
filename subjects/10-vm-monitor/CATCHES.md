# Monitor (VM monitor / Log Analytics) — question catches

Hard/tricky points from real practice questions. One line each: the **catch**, then the **answer**.
Status tag: **✓ easy** (solid) · **⚠ reasoned** (got it but had to think) · **✗ missed** (real weak point — focus here).

## Alert rules vs action groups — the counting question
- ✗ **Count alert rules by SIGNALS; count action groups by UNIQUE RECIPIENT SETS.** One alert rule per signal/condition (a metric and an activity-log event = separate signals → separate rules). One action group per unique set of people-to-notify — and it's **reusable**, so two signals that notify the *same* users share **one** action group. Worked example: 4 signals where 2 notify identical users → **4 alert rules, 3 action groups**. (Alert rule = the condition; action group = who to notify + what to run. A rule can attach up to 5 action groups.) *(Now taught in the deck's "Alerts & action groups" facet.)*

## Alerts → external ITSM (obscure, but keyword-matchable)
- ✗ **"Send Azure alerts to an external ITSM tool (ServiceNow / System Center Service Manager) → create the IT Service Management Connector (ITSMC) FIRST"** — install ITSMC in the Log Analytics workspace, then wire alerts to it via an **action group**'s ITSM action. The connector is the prerequisite bridge. *(Yet another "do FIRST?" prerequisite trap — 4th one. The tell: an external ticketing/ITSM tool named in the question.)*
- **Distractors here:** *Data Collector Set* = a Windows on-prem perf tool, not Azure Monitor. *AzureActivity signal* = control-plane/Activity log (who created/deleted), NOT performance/memory. *Mail-enabled security group* = a notification detail, not "first."

## KQL (Kusto Query Language)
- ⚠ **`search in (TableName) "value"` searches ONE table; bare `search "value"` scans ALL tables** (slower). "Find X in table Y" → `search in (Y) "X"`. Distractors `| take 10` (just samples 10 rows) and `| sort by Col` (just orders) don't filter for a value. *(Reasoned it right, but had never seen KQL in action — see the deck's KQL primer.)*
