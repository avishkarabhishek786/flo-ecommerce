# ecommerce
A city based e-commerce platform built on top of FLO Blockchain.


## Flow Process

When user makes a buy order req will go to Cashier
When Cashier receves money from buyer he will generate a obj for seller which 
shall also contain upi txid from buyer. seller will generate obj for courier conatining same 
upiid. When courier meets buyer, buyer shall present back same upi txid.


## Usage

SubAdmin creates the architecture object

Subadmin adds and certifies Sellers in master_configurations
Seller adds products and sends approval request to Subadmin 
Subadmins certifies Seller to sell that product 

Seller lists Item X for sale at certain price and quantity
Buyer makes a buy order 
Cashier receives payment from buyer. 
Cashier pays Seller. 
Buy order is live.

Situation 1:
Seller hands item to delivery person. 
Delivery person hands ite to buyer.
Order deleted after 14 days.

Situation 2:
Item is not available. 
Order status is updated to ITEM_UNAVAILABLE.
Seller must pay money back to Cashier. 
Cashier sends money back to Buyer.

To prevent Situation 2 system must force seller to update item availability 


## Example

// Create a copy of above object but with only constant values
nayaProductConstants = JSON.parse(JSON.stringify(nayaProduct));
delete nayaProductConstants.onSale;
delete nayaProductConstants.onSale;

nayaProduct.possession_history.push({
    signer_public_key: myPubKey,
    signature: floCrypto.signData(JSON.stringify(nayaProduct), myPrivKey),
    rank: 1
});


sell_obj =	{
	product_type: "DIGITAL",
    product: 'MOVIE_TICKETS',
    product_name: 'SHOLAY',
    product_description: 'Watch Movie Sholay starring Amitabh Bacchhan, Dharmendra, Amjad Khan.',
    image: false,
	product_category: 'ENTERTAINMENT',
	seller_flo_id: myFloID,
	selling_price: 25.00,
	product_location: "S.R.S. Cinemas, 4th Floor Sandhya Tower, Dangra Toli Chowk, Purulia Rd, Ajit Enclave, Lalpur, Ranchi, Jharkhand 834001",
	movie_datetime: '5 March, 2020 9PM'
}

sell_obj =	{
	product_type: "PHYSICAL",
    product: 'Cake',
	product_name: 'DARK CHOCOLATE MUMBO JUMBO',
    product_description: 'Chocolate flavour dark color cake. Contains egg.',
    image: false,
    product_category: 'Food',
	seller_flo_id: myFloID,
	selling_price: 300.00,
	product_location: 'Street 123 XYZ Colony Ranchi',
}


cu = new ecommerce.users.current_user({
    flo_id: myFloID,
})

cu.user_info.add_new_product(sell_obj)


Buyer

Add item 
ecommerce.loggedInUser.user_info.add_item_to_bucket("FSetoWanbxdwHHnj9oaQsF4Sf8wLoXPXXA", 2)
ecommerce.loggedInUser.user_info.preview_shopping_bill()
ecommerce.loggedInUser.user_info.checkout()


Cashier

receive all buy requests from buyers
here myFloID = cashier flo id
floCloudAPI.requestApplicationData(
    {
        type:ecommerce.master_configurations.DATA_TYPE.BUY_ORDERS,
        application:ecommerce.master_configurations.SUBJECT, 
        receiverID:my
    }
);



## Cash Flow 

1. F7HEAV9226tqXtRk34XrA9nZCb8ZcbVJt1 has 1 lac tokens curently
2. Transfer (say) 25k tokens to Cashier 1.
3. User X requests (say) 100 tokens from Cashier 1
4. Upi id of Cashier 1 pops up to X 
5. User pays 100 INR to upi id 
6. On receiving payment from X, Cashier 1 transfers 100 tokens to X 
 - transfer 100 rupee# to X_FLO_ADDRESS
7. X can now use tokens to buy items worth 100 INR 
8. On doing so, he will transfer Y (where Y<=100) tokens to vendor
 - transfer Y rupee# to Vendor_FLO_ADDRESS
9. Vendor can later redeem tokens in exchange of cash from Cashier 
 - transfer Y rupee# to CASHIER_FLO_ADDRESS
10. Cashier will send INR Y to Vendor.
