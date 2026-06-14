# ERGO NEXT Insurance – GraphQL Schema

## Overview

This is a conceptual GraphQL schema for ERGO NEXT Insurance (formerly NEXT Insurance), the Munich Re/ERGO-owned digital commercial insurer focused on U.S. small and medium-sized businesses. NEXT does not publish a public GraphQL API or developer portal; partner access to the NEXT Connect embedded insurance platform is gated through a partnerships engagement. This schema models the logical domain of the platform based on publicly available documentation, partner integration guides, and the company's product surface.

## Schema Source

The schema covers the NEXT Connect embedded insurance platform and its REST/component-based partner API. Types are derived from:

- NEXT Connect partner documentation
- Embedded payroll integrations (Check, Gusto Embedded, Intuit QuickBooks, Square)
- NEXT Insurance product pages for all lines of business
- Public press releases and partner announcements

## Domain Coverage

### Policy Lifecycle
- `Policy`, `PolicyStatus`, `PolicyApplication`, `ApplicationStatus`
- `Binder`, `BinderRequest`, `BinderStatus`
- `Renewal`, `RenewalStatus`
- `Cancellation`, `CancellationInitiator`

### Coverage
- `Coverage`, `CoverageType`, `CoverageLimit`, `Deductible`
- `LineOfBusiness` enum (12 lines: GL, Workers Comp, BOP, Commercial Auto, Professional Liability, Commercial Property, Tools & Equipment, EPLI, Product Liability, Liquor Liability, Tech Coverage, Emergency Fund)
- `CoverageCategory`, `DeductibleType`

### Quoting
- `Quote`, `QuoteStatus`, `QuoteRequest`, `QuoteRequestInput`

### Premium & Payments
- `Premium`, `PremiumBreakdown`, `PaymentSchedule`
- `PremiumPayment`, `PaymentInput`, `PaymentStatus`, `PaymentMethod`
- Pay-as-you-go workers' comp via payroll integration modeled in `PremiumPayment.wages` and `PremiumPayment.payrollPeriod`

### Parties
- `PolicyHolder`, `BusinessInfo`, `EntityType`
- `Employee`, `EmployeeInput`, `WorkersCompClassification`
- `Location`, `LocationInput`, `PropertyValue`
- `Vehicle`, `VehicleInput`, `VehicleUse`
- `BusinessLicense`
- `IndustryClass`, `ProfessionType` (15 profession types across 1,300+ supported professions)

### Claims
- `Claim`, `ClaimType`, `ClaimStatus`, `ClaimDetails`
- `ClaimDocument`, `DocumentType`
- `Settlement`

### Certificates & Endorsements
- `Certificate`, `CertificateHolder`, `CertificateInput`
- `Endorsement`, `EndorsementType`, `EndorsementInput`

### Coverage Line Detail Types
- `GeneralLiability`
- `WorkersComp`
- `BOP` (Business Owners Policy)
- `CommercialAuto`
- `ProfessionalLiability`
- `TechCoverage`
- `EmergencyFund`
- `Tools`, `ToolItem`

### Profession Profiles
- `Contractor`, `Handyman`, `Cleaner`, `Consultant`, `FitnessTrainer`
- `TrainingLocation` enum

### Partner & Platform
- `Partner`, `PartnerType`, `PartnerStatus`, `PartnerInput`
- `Agency`, `Agent`
- `APIKey`, `Environment`
- `Webhook`, `WebhookEvent`, `WebhookInput`

## Type Count

The schema defines 65+ named types including objects, enums, and input types.

## Key Design Notes

- **Pay-as-you-go Workers Comp**: The `PremiumPayment` type includes `payrollPeriod` and `wages` fields to model NEXT's per-cycle premium calculation that integrates with partner payroll runs.
- **Multi-carrier**: The `Quote.carrier` field and `Binder.carrier` field acknowledge that NEXT Connect aggregates 15+ carriers (Travelers, Liberty Mutual, Chubb, CNA, The Hartford, and others) in addition to ERGO NEXT itself.
- **Partner-gated access**: The `Partner` and `APIKey` types model the gated onboarding flow; `Environment` distinguishes sandbox from production credentials.
- **Certificate issuance**: The `Certificate` mutation and types model NEXT's self-serve certificate-of-insurance capability, a key differentiator for small business owners.
- **Endorsements**: `EndorsementType` captures the common endorsements requested by certificate holders (additional insured, waiver of subrogation, primary non-contributory).

## References

- NEXT Connect partner program: https://www.nextinsurance.com/become-a-partner/
- NEXT Partner API docs: https://www.nextinsurance.com/partner-api/
- GitHub: https://github.com/next-insurance
- Check integration guide: https://docs.checkhq.com/docs/check-next-component-integration-guide
- Gusto Embedded: https://embedded.gusto.com/blog/next-insurance-workers-comp-via-gusto-embedded-payroll/
