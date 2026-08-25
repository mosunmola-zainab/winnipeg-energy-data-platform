# Requirements and System Design

## Problem Definition

The City of Winnipeg needs a reliable way to combine historical energy utility consumption, cost and weather data. The platforms will help data professionals and decision makers understand historical and current energy usage patterns, identify major cost and consumption drivers, analyze changes over time, forecast future consumption and cost to support budgeting and future energy planning.

## Consumers and Their Needs

**Data Analysts:** Reliable data to map out trends, KPIs, compare and contrast what has been and what currently is. Analysts can investigate patterns and provide explanations that other stakeholders can act on.

**Data Scientists / ML Engineers:** Feature-ready historical data to build predictive products that improves decision making, including demand and cost forecasting and anomaly detection.

**Finance Team:** Uses the platform to understand consumption, cost and forecast to plan and allocate resources.

**Energy and Operations Team:** Assess usage pattterns, high-consumption areas, anomalies and where opportunity for efficiency resides in the system.

**Policy / Strategy Team:** Platform provides patterns and evidence that helps them evaluate energy initiatives and policies.

**Urban Planning Team:** Where supported by location and building data, it can provide geographic and building- level insights to support energy-efficient urban planning.

## Business Questions

1. **Is something happening that shouldn't be?** Are there unusual changes or patterns in consumption or cost that warrant investigation?

2. **Why are we spending this much?** What are the major drivers of energy cost?

3. **How has our energy use changed over time?**

4. **Where is the energy going?** Which accounts/service locations/service types consume the most energy?

5. **Considering Winnipeg's extreme climate, how much does our weather affect energy consumption and cost?**

6. **How much energy are we likely to use in the future and how much might it cost us?**

7. **Where should we do better?** Which consumption and cost pattern show areas that may warrant investigation for improved efficiency?

## Data Products and What It Serves

### Product 1: Winnipeg Energy Intelligence Dataset

#### Purpose

This provides a trusted and analysis ready data that can be re-used by downstream consumers. The wider platform preserves raw source records so curated outputs can traceable to its lineage:

    - How has energy consumption changed?
    - Where is the energy being consumed?
    - Why is the cost changing?
    - How does the weather affect consumption hence cost?
    - Where are the unusual patterns?

#### Consumers

Primarily, the consumers are the data analysts along with the finance, energy/operations teams and eventually BI/reporting products that take in the outputs to give more refine output.

#### Grain

Based on the first look of data, one row represents one utility service record for a specific account/meter, service type, service location and billing period. This is provisional.

#### Dimensions

**When?**
- Time
    - billing period
    - month
    - quarter
    - year
    - season

**Where?**
- Location
    - service address
    - geographical area, if a reliable mapping can be established.

**What?**
- Service
    - utility/service type
    - account/meter

**Who?**
- Organization
    - department, if a reliable mapping can be established.

**Context?**
- Weather
    - Temperature
    - Precipitation
    - Other weather-derived variables where useful

#### Measures


**How much energy?**
- Energy Consumption
    - Billing units
    - Days of service

**How much money?**
- Cost
    - Amount due

    **Why that cost?**
    - Basic charges
    - Distribution charges
    - Transportation charges
    - Demand charges
    - Carbon charges
    - Taxes
    - Adjustments

#### Derived Metrics
This would include but not limited to:

- Cost per billing unit
- Consumption per service day
- Cost per service day
- Monthly and annual consumption
- Monthly and annual expenditure
- Period-over-period consumption change
- Period-over-period cost change
- Cost-component share of total expenditure

#### Freshness

The platform should check for new utility and weathher data daily because the platform is independent and doesn't control when new records are published. 

This is to pick up fresh data without assuming a specific schedule. Downstream datasets would be updated accordingly and maintain indempotency. 

This refresh strategy may be revised later after adequate profiling of publication frequency is done.

#### Quality Assurance

To check for and ensure this product is reliable and trustworthy for downstream users, the following quality is what is abided by:

- **Uniqueness and Indempotency:** Duplicate utility service records should be identified and prevented. 

- **Schema validation:** Incoming fields should conform to their expected data types and formats. Unexpected schema changes should be detected and prevented from silently propagating to downstream products. Where safe, known changes may be handled automatically; otherwise the pipeline should fail clearly and require review..

- **Completeness:** Unexpected missingness should be detected and reported. Records should be handled according to documented rules rather than silently discarded or imputed.

- **Validity:** Values, features and relationships whether pre-existing or derived should serve business rules and needs.

- **Freshness:** The platform should track when source data was last successfuly updated and identify and report unexpected delays and errors.

- **Consistency:** Relationships between records and downstream dimensions should remain valid the data moves through the platform.

- **Data Dictionary:** The product is accompanied by a document that details important fields, units, codes, transformations and known values. Unknown ones should be investigated and verified before documentation.