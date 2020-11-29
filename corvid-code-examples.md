# API Name: <updateInventoryVariantFieldsByProductId()>
## Simple Example
### Description: \<Update a variant's inventory information by product ID> 
```javascript
/*******************************
 * Backend code - inventory.jsw *
 *******************************/
import wixStoresBackend from 'wix-stores-backend';
export function updateInventoryVariantFieldsByProductId(productId, item) {
  return wixStoresBackend.updateInventoryVariantFieldsByProductId(productId, item);
}
/*************
 * Page code *
 *************/
import { updateInventoryVariantFieldsByProductId } from 'backend/inventory';
$w.onReady(async function () {
  const product = await $w('#productPage1').getProduct();
  let options = product.variants;
  let foundVariant = options.find((option) => {
    return option.choices.Color === "red" && option.choices.Size === "large";
  });
  let variantInfo = {
    trackQuantity: false,
    variants: [{
      inStock: true,
      variantId: foundVariant._id,
    }]
  };
  updateInventoryVariantFieldsByProductId(product._id, variantInfo);
});
/*  A sample inventory item with variants is:
 *
 * {
 * 	 "_id" : 133c6\-...-0fb2,
 * 	 "productId" : ecc3-...-1f04d,
 *   "trackQuantity" : true,
 * 	 "variants" : -[
 * 	  {
 * 	    "variantId" : 00000000-0000-0020-0005-9ec14dfb2270,
 * 	    "inStock" : false,
 * 	    "quantity" : 0
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0021-0005-9ec14dfb2270,
 * 	    "inStock" : false,
 * 	    "quantity" : 0
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0022-0005-9ec14dfb2270,
 * 	    "inStock" : true,
 * 	    "quantity" : 100
 *   	},
 * 	  {
 * 	    "variantId" : 00000000-0000-003f-0005-9ec14dfb2270,
 * 	    "inStock" : true,
 * 	    "quantity" : 123
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0040-0005-9ec14dfb2270,
 * 	    "inStock" : false,
 * 	    "quantity" : 0
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0041-0005-9ec14dfb2270,
 * 	    "inStock" : true,
 * 	    "quantity" : 1
 * 	  }
 * 	],
 * 	"_updatedDate" : 2020-02-17T08:25:57.734Z
 * 	}
 */
 ```
 
 ## Full Scenario
 ### Description: Update a variant's inventory information by product ID using user input
This example assumes the following elements exist on the page:

- Text boxes for entering the product ID and the inventory amount.
- Checkboxes for indicating if inventory is being tracked and if an item is in-stock.
- A drop-down for displaying a product's variants.
- Buttons for populating the drop-down and for performing the update.

```javascript
/*******************************
 * Backend code - inventory.jsw *
 *******************************/
import wixStoresBackend from 'wix-stores-backend';
export function updateInventoryVariantFieldsByProductId(id, item) {
	return wixStoresBackend.updateInventoryVariantFieldsByProductId(id, item);
}
/*************
 * Page code *
 *************/
import { updateInventoryVariantFieldsByProductId } from 'backend/inventory';
import wixData from 'wix-data';
export function updateByProductIdButton_click(event) {
	const productId = $w('#updateByProductIdInventoryId').value;
	const trackInventory = $w('#updateByProductIdTrackInventory').checked;
	const variantId = $w('#updateByProductIdVariantId').value;
	const inStock = $w('#updateByProductIdInStock').checked;
	const quantity = $w('#updateByProductIdQuantity').value;
	updateInventoryVariantFieldsByProductId(productId, {
		trackQuantity: trackInventory,
		variants: [{
			variantId,
			inStock,
			quantity
		}]
	}).then(console.log("Variant inventory updated."));
}
// Populate variant dropdown options 
// from which the user will choose.
export async function updateByProductIdRefreshVariantId_click(event) {
	const productId = $w('#updateByProductIdInventoryId').value;
	const variantRefreshResults = await wixData.query('Stores/InventoryItems').eq('productId', productId).find();
	const ids = variantRefreshResults.items[0].variants.map(item => {
		return { label: item.variantId, value: item.variantId };
	});
	$w('#updateByProductIdVariantId').options = ids;
}
/*  A sample inventory item with variants is:
 *
 * {
 * 	 "_id" : 133c6\-...-0fb2,
 * 	 "externalId" : ecc3-...-1f04d,
 *   "trackQuantity" : true,
 * 	 "variants" : -[
 * 	  {
 * 	    "variantId" : 00000000-0000-0020-0005-9ec14dfb2270,
 * 	    "inStock" : false,
 * 	    "quantity" : 0
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0021-0005-9ec14dfb2270,
 * 	    "inStock" : false,
 * 	    "quantity" : 0
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0022-0005-9ec14dfb2270,
 * 	    "inStock" : true,
 * 	    "quantity" : 100
 *   	},
 * 	  {
 * 	    "variantId" : 00000000-0000-003f-0005-9ec14dfb2270,
 * 	    "inStock" : true,
 * 	    "quantity" : 123
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0040-0005-9ec14dfb2270,
 * 	    "inStock" : false,
 * 	    "quantity" : 0
 * 	  },
 * 	  {
 * 	    "variantId" : 00000000-0000-0041-0005-9ec14dfb2270,
 * 	    "inStock" : true,
 * 	    "quantity" : 1
 * 	  }
 * 	],
 * 	"_updatedDate" : 2020-02-17T08:25:57.734Z
 * 	}
 */
 ```
