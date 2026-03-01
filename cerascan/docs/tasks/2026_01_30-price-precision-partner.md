# Instruction: Partner Price Precision Configuration

## Feature

- **Summary**: Allow each partner to configure price display precision (0, 1, or 2 decimals) with immediate effect on all price displays across the application
- **Stack**: `Node.js`, `Express.js`, `MySQL`, `EJS`, `Alpine.js`, `PDFKit`
- **Branch name**: `feature/partner-price-precision`

## Existing files

- @code/src/database/schemas.js
- @code/src/models/partner.js
- @code/src/views/partials/admin/partenaires.ejs
- @code/src/views/partenaire.ejs
- @code/src/utils/templateHelpers.js
- @code/src/views/catalogue.ejs
- @code/src/views/product.ejs
- @code/src/views/quote.ejs
- @code/src/services/PDFManager.js

### New file to create

- code/migrations/XXXXXX_add_price_precision_to_partners.js

## Implementation phases

### Phase 1: Database Migration

> Add price_precision column to partners table

1. Create migration file with ALTER TABLE adding `price_precision` INT DEFAULT 2
2. Add CHECK constraint for values (0, 1, 2)
3. Execute migration with `pnpm run migrate`

### Phase 2: Backend Model and API

> Extend partner model to handle new field

1. Update PARTNERS_SCHEMA in schemas.js to include price_precision
2. Add price_precision to partner model validation
3. Include field in getAllPartners(), createPartner(), updatePartner() functions
4. Return price_precision in API responses

### Phase 3: Admin Interface

> Add precision selector in admin partner management

1. Add select field (0, 1, 2 decimals) in partenaires.ejs modal form
2. Include field in Alpine.js formData object
3. Display current value in partner list/details
4. Update save/update API calls to include new field

### Phase 4: Partner Self-Service Interface

> Allow partners to modify their own price precision

1. Add select field in partenaire.ejs edit form
2. Bind to existing Alpine.js form handling
3. Include in PUT /api/partner/update payload

### Phase 5: Price Formatting

> Modify formatPrice() to use dynamic precision

1. Update formatPrice(prix, decimales) in templateHelpers.js to use partner precision
2. Inject currentPartner.price_precision into template context
3. Update catalogue.ejs price displays to use partner precision
4. Update product.ejs price displays
5. Update quote.ejs price calculations and displays

### Phase 6: PDF Generation

> Apply partner precision to PDF price displays

1. Pass partner price_precision to PDFManager
2. Update PDF price formatting to use dynamic precision
3. Test PDF generation with different precision values

## Reviewed implementation

- [ ] Phase 1: Database Migration
- [ ] Phase 2: Backend Model and API
- [ ] Phase 3: Admin Interface
- [ ] Phase 4: Partner Self-Service Interface
- [ ] Phase 5: Price Formatting
- [ ] Phase 6: PDF Generation

## Validation flow

1. Create test partner with default precision (2 decimals) - verify prices show as "125.50"
2. Change partner precision to 0 decimals - verify prices show as "126" (rounded)
3. Change partner precision to 1 decimal - verify prices show as "125.5"
4. Verify catalogue page displays prices with correct precision
5. Verify product detail page displays prices with correct precision
6. Verify quote page displays all prices (unit, total, HT, TVA, TTC) with correct precision
7. Generate PDF and verify prices use partner precision
8. Test admin form saves precision correctly
9. Test partner self-service form saves precision correctly
10. Verify existing partners default to 2 decimals after migration

## Estimations

- **Confidence**: 9/10
  - Reasons for high confidence:
    - Clear, well-defined scope
    - Existing formatPrice() function already accepts decimals parameter
    - Standard CRUD operations on existing partner model
    - No complex business logic changes
  - Risks:
    - Need to ensure all price display locations are updated (may miss some edge cases)
    - PDF generation may have specific formatting requirements
