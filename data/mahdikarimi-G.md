## you can store asset ids in offer struct instead of addresses to save gas 

everytime a user creates an offer the offer struct will contain two address of buy_gem and sell_gem since other users use these addresses for their trading , you can store address of different tokens with an id in a mapping and use that asset id instead of whole address to save gas
( everytime user creates an offer he will set two uint instead of two address in offer that saves gas ) . 