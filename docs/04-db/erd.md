# ERD — модель данных

```mermaid
erDiagram
 PROJECTS ||--o{ EXTERNAL_PROJECT_IDS : maps
 PROJECTS ||--o{ PLAN_VERSIONS : has
 PLAN_VERSIONS ||--o{ STAGES : contains
 PLAN_VERSIONS ||--o{ KPI_TARGETS : defines
 PROJECTS ||--o{ KPI_ACTUALS : measures
 PROJECTS ||--o{ FINANCIAL_ACTUALS : records
 PROJECTS ||--o{ DEVIATIONS : raises
 DEVIATIONS ||--o{ DEVIATION_HISTORY : tracks
 PROJECTS ||--o{ STATUS_PROPOSALS : proposes
 PROJECTS ||--o{ PROJECT_STATUS_HISTORY : records
 PROJECTS ||--o{ SYNC_RUNS : syncs
 USERS ||--o{ DEVIATIONS : assigned
 USERS ||--o{ AUDIT_LOG : creates
 PROJECTS { uuid project_id PK text name text enterprise_id text status }
 EXTERNAL_PROJECT_IDS { uuid external_id PK uuid project_id FK text source_code text external_project_id }
 PLAN_VERSIONS { uuid plan_version_id PK uuid project_id FK int version_number text approval_status }
 STAGES { uuid stage_id PK uuid plan_version_id FK int planned_duration_days int actual_duration_days }
 KPI_TARGETS { uuid kpi_target_id PK uuid plan_version_id FK text code numeric target_value text direction numeric threshold_percent }
 KPI_ACTUALS { uuid kpi_actual_id PK uuid project_id FK text source_code text kpi_code numeric actual_value }
 FINANCIAL_ACTUALS { uuid financial_actual_id PK uuid project_id FK numeric expense_amount numeric payment_amount text currency }
 DEVIATIONS { uuid deviation_id PK uuid project_id FK text type text status }
}
```

## Ограничения
- Уникальность `(source_code, external_project_id)`.
- Уникальность `(project_id, version_number)` и `(plan_version_id, KPI code)`.
- Идемпотентность фактов по ключу источника и ID записи.
- Расходы и платежи хранятся раздельно.
- История статусов, отклонений и аудит не удаляются обычными операциями.
