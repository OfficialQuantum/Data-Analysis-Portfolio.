# Nashville Housing Data Cleaning

<p align='center'>
  <img src='https://github.com/OfficialQuantum/assests/blob/main/assests/Nashville Housing Data Cleaning.png', alt='Welcome'>
</p>

**Code:** [Nashville Housing Data Cleaning](https://github.com/OfficialQuantum/Portfolio-Projects/blob/main/Nashville%20Housing%20%20Data%20Cleaning.sql)

**Goal:** To examine the sales history of the store and extract insights on its performance, as well as to identify potential improvements that can be implemented.

**Description:** The Nashville Housing dataset includes attributes such as property addresses, sale prices, dates of sale, property types, and owner details of houses that have been sold in Nashville between 2013 and 2019. The data cleaning process involves resolving missing values, correcting inaccuracies, and standardizing formats to ensure the dataset is reliable and ready for analysis.

**Skills:** Data cleaning, Data analysis, Hypothesis testing

**Technology:** SQL

---

## Query

```ruby
-- CLEANING THE DATA 
SELECT
    *
FROM
    PortfolioMain..Nashville 
---------------------------------------------------------------

-- 1. STANDARDIZING THE DATE FROMAT
ALTER TABLE
    Nashville
ADD
    SaleDateConverted DATE
UPDATE
    Nashville
SET
    SaleDateConverted = CONVERT(DATE, SaleDate)
SELECT
    SaleDate,
    SaleDateConverted
FROM
    PortfolioMain..Nashville 
---------------------------------------------------------------

-- 2. POPULATING THE PROPERTY ADDRESS COLUMN
SELECT
    a.ParcelID,
    a.PropertyAddress,
    b.ParcelID,
    b.PropertyAddress
FROM
    PortfolioMain..Nashville a
    JOIN PortfolioMain..Nashville b ON a.ParcelID = b.ParcelID
    AND a.UniqueID <> b.UniqueID
WHERE
    a.PropertyAddress IS NULL

-- Updating the Changes
UPDATE
    a
SET
    PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM
    PortfolioMain..Nashville a
    JOIN PortfolioMain..Nashville b ON a.ParcelID = b.ParcelID
    AND a.[ UniqueID ] <> b.[ UniqueID ] 
---------------------------------------------------------------

-- 3. BREAKING OUT ADDRESS INTO INDIVIDUAL COLUMNS (Address, City. State)
-- Method 1
-- Observing the table
SELECT
    Propertyaddress
FROM
    PortfolioMain..Nashville

SELECT
    PropertyAddress,
    SUBSTRING(
        PropertyAddress,
        1,
        CHARINDEX(',', PropertyAddress) -1
    ),
    SUBSTRING(
        PropertyAddress,
        CHARINDEX(',', PropertyAddress) + 1,
        LEN(PropertyAddress)
    )
FROM
    PortfolioMain..Nashville

-- Altering the table to accomodate the new columns
ALTER TABLE
    PortfolioMain..Nashville
ADD
    PropertyAddressSplit nvarchar(225),
    PropertyAddressCity nvarchar(255)

-- Updating the table with the new columns
UPDATE
    PortfolioMain..Nashville
SET
    PropertyAddressSplit = SUBSTRING(
        PropertyAddress,
        1,
        CHARINDEX(',', PropertyAddress) -1
    ),
    PropertyAddressCity = SUBSTRING(
        PropertyAddress,
        CHARINDEX(',', PropertyAddress) + 1,
        LEN(PropertyAddress)
    )
FROM
    PortfolioMain..Nashville
--------------------------------

-- Applying Alternative Method 
-- Observing the data
SELECT
    *
FROM
    PortfolioMain..Nashville 

-- Altering the table to accomodate the new columns
ALTER TABLE
    PortfolioMain..Nashville
ADD
    OwnerAddressSplit nvarchar(225),
    OwnerAddressCity nvarchar(255),
    OwnerAddressState nvarchar(255);

SELECT
	PARSENAME(REPLACE(OwnerAddress, ',' , '.'), 1),
	PARSENAME(REPLACE(OwnerAddress, ',' , '.'), 2),
	PARSENAME(REPLACE(OwnerAddress, ',' , '.'), 3)
FROM 
	PortfolioMain..Nashville
WHERE 
	OwnerAddress is not null 

-- Updating the table with the new columns
UPDATE
    PortfolioMain..Nashville
SET
    OwnerAddressSplit = PARSENAME(REPLACE(OwnerAddress, ',', '.'), 1),
    OwnerAddressCity = PARSENAME(REPLACE(OwnerAddress, ',', '.'), 2),
    OwnerAddressState = PARSENAME(REPLACE(OwnerAddress, ',', '.'), 3)
FROM
    PortfolioMain..Nashville     

-- Viewing the Split Address Columns
SELECT
    OwnerAddressSplit,
    OwnerAddressCity,
    OwnerAddressState
FROM
    PortfolioMain..Nashville
WHERE
    OwnerAddressSplit IS NOT NULL
---------------------------------------------------------------

-- 4. CHANGE 'Y' AND 'N' TO 'YES' AND 'NO' IN 'SoldAsVacant' COLUMN
-- Observing the table
SELECT
    *
FROM
    PortfolioMain..Nashville 

SELECT
    SoldAsVacant,
    COUNT(soldasvacant)
FROM
    PortfolioMain..Nashville
GROUP BY
    soldasvacant
ORDER BY
    2

SELECT
    SoldAsVacant,
    CASE
        WHEN soldasvacant = 'Y' THEN 'Yes'
        WHEN soldasvacant = 'N' THEN 'NO'
        ELSE Soldasvacant
    END
FROM
    PortfolioMain..Nashville

-- Updating the table 
UPDATE
    PortfolioMain..Nashville
SET
    SoldAsVacant = CASE
        WHEN soldasvacant = 'Y' THEN 'Yes'
        WHEN soldasvacant = 'N' THEN 'NO'
        ELSE Soldasvacant
    END
FROM
    PortfolioMain..Nashville 
---------------------------------------------------------------

-- 5. REMOVING DUPLICATE DATA
WITH RowNumCTE AS (
	SELECT
		*,
		ROW_NUMBER() OVER (
			PARTITION BY ParcelID,
			PropertyAddress,
			SaleDate,
			Saleprice,
			LegalReference
			ORDER BY
				UniqueID
		) DupliRows
	FROM
		PortfolioMain..Nashville --order by Newdata desc
)

-- Observing the Changes Made to the Data
SELECT
    *
FROM
    RowNumCTE
WHERE
    Duplirows > 1
---------------------------------------------------------------

-- 6. DELETE UNUSED/REDUNDANT COLUMNS
ALTER TABLE
    PortfolioMain..Nashville DROP COLUMN PropertyAddress,
    SaleDate,
    TaxDistrict,
    OwnerAddress
```
