Tags:
- [[Quant Finance]]
---
## Basic Structure
2 fundamental concepts:
* Asset Classes: classification of financial instruments
    - debt
    - equity
    - money market
    - derivatives
    - indices
    - collective investments
* Data types: classification of the _information_ about the financial instruments
    - reference data
    - business data
    - static data

## Reference Data
Entities:
- Issuers
    - e.g. companies, governments
- Instruments
- Markets

Relationships
- issuers issue multiple instruments
- instruments are quoted on multiple markets

Identifiers
- Instrument identifiers
    - CUPSIP, ISIN, etc.
- Market identifiers
    - uniquely identify a (instrument, market) pair
    - RIC, SEDOL, ticker symbols, etc.
- Issuers
    - DUNS, Bloomberg Company identifier, etc.
## Business Data

#### Global Business Data
- Security descriptive
    - security details
    - asset class
    - security features
    - security classification schemes
    - third parties
    - conversion terms
- trading
    - pricing
    - time and sales
    - aggregared
    - market rules
- ratings
    - instrument ratings
    - issuer ratings
- issuer & corporate info
    - issuer details
    - performance data (of the issuer)
    - industry classifications
    - ownership hierarchies
- relations & constituents
    - issuer level: corporate ownership and holdings
    - instrument level: constituents and weightings (for things like funds and indices)

#### Asset-specific Business Data
- corporate actions
    - dividends
    - capital changes
    - earnings
    - shares outstanding
    - reorg notices
- terms & conditions
    - concepts
        - terms: legal conditions defining the security
        - details: further describes terms and conditions
        - events: time-based record where term of the bond has been invoked
    - subtypes
        - summary info
        - redemption info
        - floating rate notes
        - sinking funds
        - municipal bonds
        - structured products
- payment info
    - schedules
    - rules
    - historical records
- collective investment details
    - management
    - holdings
    - performance
    - risk measures
- clearing info
- tax info
    - geography
    - tax type
    - notes

## Static Data
- Global static data
    - currency codes
    - country codes
    - market & exchange codes
        - e.g. MIC
    - industry classifications
        - e.g. SIC, MSCI
    - security classifications
        - e.g. CFI
- Data type-specific

---
Source: https://www.goodreads.com/book/show/471105.Market_Data_Explained
