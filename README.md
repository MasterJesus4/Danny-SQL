# Nashville Housing Project

## Project Overview
### Objective
The primary objective of this project  was to clean and preprocess the Nshville housing data to make it more usable and informative for company performance analysis
### Background
The Nashville housing data contained inconsistencies,missing values,and errors, making it challenging to extract valuable insights.

## Data Source
The Nashville housing data is a comprehensive dataset containing information on residential properties in Nashville, Tennessee. The data source includes a wide range of variable, such as :
- Property Characteristics: address, zip code, property type, number of bedrooms,etc
- Sales information : Sales price, sales date,etc

  ## Tools
  To ensure the quality and accuracy of the Nashville housing data, i performed thorough data cleaning using SQL.  The steps ibvolved:
  - Data inspection
  - Data standardization
  - Data validation
  - Data Transformation
  - Data duplication

   ### SQL Queries used are :
 
    Select *
			From [Project Portfolio].[dbo].NashvilleHousing
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 ``  Standardize DATE FORMAT

			Select *
			From [Project Portfolio].[dbo].NashvilleHousing


			Select SaleDate, CONVERT(Date,SaleDate)
			From [Project Portfolio].[dbo].NashvilleHousing

			ALTER TABLE NashvilleHousing
			Add SaleDateConverted Date;

			Update NashvilleHousing
			SET SaleDateConverted = CONVERT(Date,SaleDate)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-- Populate Property Address Data

			Select *
			From [Project Portfolio].[dbo].NashvilleHousing

			Select *
			From [Project Portfolio].[dbo].NashvilleHousing
			--Where PropertyAddress is null
			order by ParcelID

			Select a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)
			From [Project Portfolio].[dbo].NashvilleHousing a
			JOIN[Project Portfolio].[dbo].NashvilleHousing b
				on a.ParcelID = b.ParcelID
				AND a.[UniqueID ] <> b.[UniqueID ]
			Where a.PropertyAddress is null

			Update a
			SET PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
			From[Project Portfolio].[dbo].NashvilleHousing a
			JOIN[Project Portfolio].[dbo].NashvilleHousing b
				on a.ParcelID = b.ParcelID
				AND a.[UniqueID ] <> b.[UniqueID ]
			Where a.PropertyAddress is null

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 -- Breaking Out Address Into  Individual columns (Address, City And State)
		
      Select PropertyAddress
			From [Project Portfolio].[dbo].NashvilleHousing
			--Where PropertyAddress is null
			--order by ParcelID

			SELECT
			SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 ) as Address
			, SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress)) as Address

			From [Project Portfolio].[dbo].NashvilleHousing

			ALTER TABLE NashvilleHousing
			Add PropertySplitAddress Nvarchar(255);

			Update NashvilleHousing
			SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 )

			ALTER TABLE NashvilleHousing
			Add PropertySplitCity Nvarchar(255);

			Update NashvilleHousing
			SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress))


				Select OwnerAddress
				From [Project Portfolio].dbo.NashvilleHousing

				Select
				PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)
				,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)
				,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)
				From  [Project Portfolio].dbo.NashvilleHousing

				ALTER TABLE NashvilleHousing
				Add OwnerSplitAddress Nvarchar(255);

				Update NashvilleHousing
				SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)

				ALTER TABLE NashvilleHousing
				Add OwnerSplitCity Nvarchar(255)

				Update NashvilleHousing
				SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)

				ALTER TABLE NashvilleHousing
				Add OwnerSplitState Nvarchar(255);

				Update NashvilleHousing
				SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
    --Change Y and N To Yes And No in Sold as Vacant field

				Select *
				From [Project Portfolio].dbo.NashvilleHousing

				Select Distinct(SoldAsVacant), Count(SoldAsVacant)
				From [Project Portfolio].[dbo].NashvilleHousing
				Group by SoldAsVacant
				order by 2


				   Select SoldAsVacant,
				   CASE When SoldAsVacant = 'Y' THEN 'Yes'
					   When SoldAsVacant = 'N' THEN 'No'
					   ELSE SoldAsVacant
					   END
				From [Project Portfolio].[dbo].NashvilleHousing

				Update NashvilleHousing
				SET SoldAsVacant = CASE When SoldAsVacant = 'Y' THEN 'Yes'
					   When SoldAsVacant = 'N' THEN 'No'
					   ELSE SoldAsVacant
					   END
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

        -- REMOVE DUPLICATES

              WITH RowNumCTE AS(
						  Select *,
							ROW_NUMBER() OVER (
							PARTITION BY ParcelID,
										 PropertyAddress,
										 SalePrice,
										 SaleDate,
										 LegalReference
										 ORDER BY
											UniqueID
											) row_num

						  From  [Project Portfolio].dbo.NashvilleHousing
						--order by ParcelID
						)
						DELETE
						From RowNumCTE
						Where row_num > 1
            WITH RowNumCTE AS(
						Select *,
							ROW_NUMBER() OVER (
							PARTITION BY ParcelID,
										 PropertyAddress,
										 SalePrice,
										 SaleDate,
										 LegalReference
										 ORDER BY
											UniqueID
											) row_num
              From  [Project Portfolio].dbo.NashvilleHousing
						--order by ParcelID
						)
					    	Select *
					    	From RowNumCTE
					    	Where row_num > 1
						    Order by PropertyAddress
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
          
          --	DELETE unused Columns
			
      ALTER TABLE PortfolioProject.dbo.NashvilleHousing
			DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress, SaleDate


   --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Results/findings
The results of the data cleaning process using SQL on Nashville Housing include 
- Cleaned and standardized data
- Improved data quality
- Better data visualization
- Improved decision making

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   

























    `
