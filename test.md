sequenceDiagram
  autonumber

  participant U as End User (e.g., milind)
  participant T as Tableau Server/Cloud
  participant A as Azure AD (Entra ID)
  participant S as Snowflake (Service Account Session)
  participant G as GOV_DB Policy Store

  U->>T: Login (SSO via AAD)
  T->>A: OIDC/SAML auth
  A-->>T: Assertion/Token
  T->>S: Connect using service account credentials
  T->>S: Initial SQL → SET END_USER='milind@acme.com'; ALTER SESSION SET QUERY_TAG='Tableau|WB=...|User=milind@acme.com'
  T->>S: SELECT ... FROM ANALYTICS_DB.SALES.SALES_FACT
  S->>S: Row Access Policy triggers on column(s)
  S->>G: Call SECURE FUNCTION allowed_regions_upn($END_USER)
  G-->>S: Returns allowed set (e.g., ['US','CA'])
  S-->>T: Rows filtered to allowed set
  T-->>U: Dashboard renders filtered data
